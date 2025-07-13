<h1 align="center"> Spring  Security </h1>

__Spring Security__ est un module du framework Spring dédié à la sécurité des applications Java. Il fournit un ensemble complet de fonctionnalités pour :
- L'authentification (vérification de l'identité d'un utilisateur)
- L'autorisation (gestion des droits d'accès aux ressources)

1. Dependences  
    ```xml
        <!-- Spring Security -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-security</artifactId>
            </dependency>
    ```

Pour activer et configurer Spring Security, il faut créer une classe de configuration.

```java
@Configuration // 👉 Indique à Spring qu’il s’agit d’une classe de configuration
@EnableWebSecurity // 👉 Active Spring Security
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity) throws Exception {
        return httpSecurity.build();
    }

}
```
`Spring Security` renvoie un filtre de type SecurityFilterChain.
Ce filtre est automatiquement injecté dans la chaîne de filtres de Tomcat (ou tout autre conteneur servlet),
et il est chargé de gérer l’authentification et l’autorisation dans l’application.

les  filtes  spring Security 

- indiquer  à  spring  que  toutes  les requete  devrait  etre  authentifier 
    ```java
        formLogin(Customizer.withDefaults()) //-> ajouter  un formulaire  d'authentification  
         .authorizeHttpRequests(auth ->auth.anyRequest().authenticated());
    ```

## 🔐 Source d'authentification avec Spring Security
`spring Security` prend en charge plusieurs sources d'authentification. L'une des plus simples est l'utilisation d'utilisateurs en mémoire via la classe . `InMemoryUserDetailsManager`

1. Authentification en mémoire (InMemoryUserDetailsManager)s : Il s'agit d'une implémentation de Spring Security qui permet de stocker des utilisateurs directement en mémoire, ce qui est pratique pour les tests ou les applications simples sans base de données.  

```java
@Bean
public InMemoryUserDetailsManager inMemoryUserDetailsManager() {
    return new InMemoryUserDetailsManager(
        User.withUsername("halim")
            .password("{noop}halim") // mot de passe en clair
            .roles("USER")
            .build()
    );
}
```
⚠️ **Remarque importante sur le mot de passe**
- Si un encodeur de mot de passe est défini dans le contexte Spring (par exemple BCryptPasswordEncoder), alors Spring Security l’utilisera automatiquement.

- Dans ce cas, ne pas utiliser {noop}. Le mot de passe doit être encodé en utilisant cet encodeur.

`Spring Security` offre  un  object qui s'appelle SecurityContextHolder qui  contient  l'object Authnitication 
<p align="center">
  <img src="sources/springContextHolder.png" alt="architecture spring mvc ">
</p>

__Le Principal__ est l’objet qui contient les informations d’authentification de l’utilisateur, tandis que les __Authorities__ représentent ses rôles ou ses autorisations

## Filtres
Spring Security repose sur un mécanisme de filtres pour gérer l’authentification.
Lorsqu’une requête HTTP arrive sur un serveur d’application comme Tomcat, celui-ci applique une série de filtres prédéfinis. Spring Security s’intègre dans cette chaîne de filtres (Filter Chain) en y injectant ses propres filtres de sécurité. Ainsi, il intercepte la requête et applique ses mécanismes d’authentification et d’autorisation avant de la transmettre à l’application.

<p align="center">
  <img src="sources/filterChain.png" alt="architecture spring Security ">
</p>



# 🔐 Spring Security – AuthenticationManager vs AuthenticationProvider

## 1. Structure de base

```
+-------------------------+
| AuthenticationManager   | <--- appelé par le système de sécurité
+-------------------------+
            |
            | délègue à
            v
+-------------------------+
| AuthenticationProvider  | <--- ta classe personnalisée
+-------------------------+
```

---

## 2. C’est quoi `AuthenticationProvider` ?

Une interface qui contient la **logique métier pour authentifier un utilisateur**.

Méthode clé :
```java
Authentication authenticate(Authentication authentication) throws AuthenticationException;
```

➡️ Ta classe personnalisée implémente cette méthode pour valider login/mot de passe.

---

## 3. C’est quoi `AuthenticationManager` ?

Un **composite** qui contient une **liste de `AuthenticationProvider`**.

Il essaie chaque provider dans l’ordre jusqu’à ce qu’un réussisse ou que tous échouent.

---

## 4. Pourquoi injecter le `AuthenticationProvider` ?

Parce que Spring ne le fait **pas automatiquement**.  
Tu dois dire explicitement au `AuthenticationManager` :  
👉 "Utilise **ma** stratégie d'authentification".

---

## 5. Comment faire ?

### ✅ Avec Spring Security moderne (Java config)

```java
@Bean
public AuthenticationManager authenticationManager(AuthenticationConfiguration config) throws Exception {
    return config.getAuthenticationManager();
}

@Bean
public AuthenticationProvider customAuthenticationProvider() {
    return new MyCustomAuthenticationProvider();
}

@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authenticationProvider(customAuthenticationProvider())
        .authorizeHttpRequests(...)
        .formLogin();

    return http.build();
}
```

### Autre méthode avec `AuthenticationManagerBuilder` (ancienne façon)

```java
@Autowired
public void configure(AuthenticationManagerBuilder auth) {
    auth.authenticationProvider(myCustomAuthProvider);
}
```

---

## 6. Pourquoi `getAuthenticationManager()` retourne ton provider ?

Quand tu fais :
```java
AuthenticationManager manager = authenticationConfiguration.getAuthenticationManager();
```

Spring Security :

1. Cherche tous les `AuthenticationProvider` déclarés comme `@Bean`.
2. Crée automatiquement un `ProviderManager` qui les contient.
3. Te retourne ce `AuthenticationManager`.

➡️ Donc si tu as fait :

```java
@Bean
public AuthenticationProvider authenticationProvider() {
    return new MyCustomAuthenticationProvider();
}
```

Alors **il est utilisé automatiquement**.

---

## 7. Que fait Spring en coulisse ?

```java
List<AuthenticationProvider> providers = getAllProvidersFromContext();
return new ProviderManager(providers);
```

---

## 8. Et avec plusieurs `AuthenticationProvider` ?

Spring les essaie **dans l'ordre**.  
Chaque provider a une méthode :
```java
boolean supports(Class<?> authenticationClass);
```

Le premier qui retourne `true` est utilisé.

---

## ✅ Résumé

| Élément | Rôle |
|--------|------|
| `AuthenticationProvider` | Contient la logique d'authentification personnalisée |
| `AuthenticationManager` | Gère la liste des providers et les appelle |
| Pourquoi injecter ? | Sinon Spring n'utilise pas ton provider |
| Pourquoi `getAuthenticationManager()` fonctionne ? | Car ton provider est un `@Bean` dans le contexte |

---

## 🔍 Bonus : Vérifier s’il est utilisé

Ajoute un log dans ta méthode `authenticate()` :
```java
@Override
public Authentication authenticate(Authentication authentication) throws AuthenticationException {
    System.out.println(">>> Appel de MonAuthenticationProvider !");
    // logique...
}
```

S’il n’est pas appelé → ton provider n’est pas bien enregistré.
