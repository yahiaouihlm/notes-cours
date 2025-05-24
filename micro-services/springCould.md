# Spring  Could : 

## 🌩️ Qu’est-ce que Spring Cloud ?

**Spring Cloud** est un ensemble de **bibliothèques** fournies par **Spring** pour faciliter la création et la gestion d’**applications distribuées**, comme les **microservices**.

---

## 🔧 À quoi sert Spring Cloud ?

Spring Cloud aide à résoudre des problèmes courants dans une architecture de microservices :

| Fonctionnalité                   | Description |
|----------------------------------|-------------|
| 🔎 **Service Discovery**         | Permet aux services de se découvrir automatiquement (**Eureka**) |
| 📦 **Configuration centralisée** | Centralise la configuration (**Spring Cloud Config**) |
| 🚦 **Load Balancing**            | Répartit les requêtes entre services (**Spring Cloud LoadBalancer**) |
| 📡 **API Gateway**               | Point d’entrée unique (**Spring Cloud Gateway**) |
| 🔁 **Resilience / Circuit Breaker** | Gère les pannes réseau (**Hystrix**, **Resilience4j**) |
| 📊 **Monitoring et Tracing**     | Trace les appels entre services (**Sleuth**, **Zipkin**) |

---

## 🔁 Exemple d’architecture avec Spring Cloud

```plaintext
[Gateway] → [Service A] → [Service B]
           ↘︎ Config Server
           ↘︎ Eureka Discovery
           ↘︎ Zipkin Tracing
```

---

## 📦 Modules principaux de Spring Cloud

| Module                      | Utilité principale |
|----------------------------|--------------------|
| `spring-cloud-config`       | Configuration centralisée |
| `spring-cloud-eureka`       | Service Discovery |
| `spring-cloud-gateway`      | API Gateway |
| `spring-cloud-sleuth`       | Tracing distribué |
| `spring-cloud-zipkin`       | Visualisation des traces |
| `spring-cloud-openfeign`    | Appels HTTP déclaratifs |
| `spring-cloud-bus`          | Propagation des mises à jour de config |

---

## 📚 Exemple de dépendance Maven (Eureka client)

```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

---

## ✅ Résumé

> **Spring Cloud** fournit les outils nécessaires pour construire des **microservices robustes, évolutifs et résilients**, en s’appuyant sur **Spring Boot**.

---




## <h2 align="center"> ⚙️ Configuration de la Gateway avec Spring Cloud Gateway </h2>

**Spring Cloud Gateway** est un composant de Spring Cloud qui agit comme une **passerelle intelligente (API Gateway)** entre les clients et les services backend.

La **Gateway** joue le rôle de **router intelligent**, permettant d’acheminer une requête HTTP vers le **bon service cible** selon des règles bien définies.

---

### 🛣️ Définition d'une route

Pour définir une route, on a besoin de **trois éléments** principaux :

---

### 1. `URI` de destination
C'est l’**adresse du service** vers lequel la requête sera redirigée (par exemple : `http://localhost:8081` ou un nom de service dans un registre Eureka).

---

### 2. `Predicate` (conditions)

Ce sont des **conditions** qui doivent être **vérifiées** pour qu’une requête soit acheminée.  
Elles peuvent porter sur :

- `Host`
- `Path`
- `Method` (GET, POST…)
- `After`, `Before`, `Between` (dates)
- `Cookie`, `Header`, `Query`
- `RemoteAddr`
- etc.

👉 Exemple :
```yaml
predicates:
  - Path=/users/**
  - Method=GET
```

---

### 3. `Filters` (filtres)

Les **filtres** permettent de **modifier les requêtes ou les réponses HTTP**.  
Ils interviennent **avant** ou **après** le routage, pour appliquer des transformations, du logging, ou de la résilience.

Filtres possibles :

- `AddRequestHeader` : Ajoute un en-tête à la requête
- `AddRequestParameter` : Ajoute un paramètre à la requête
- `AddResponseHeader` : Ajoute un en-tête à la réponse
- `DedupeResponseHeader` : Supprime les doublons dans les en-têtes
- `Hystrix` / `CircuitBreaker` : Gestion de la tolérance aux pannes
- `RewritePath` : Réécriture du chemin de l’URL
- etc.

👉 Exemple :
```yaml
filters:
  - AddRequestHeader=X-Request-ID, 1234
  - RewritePath=/old-path/(?<segment>.*), /new-path/${segment}
```

---

### 📌 Résumé

| Élément     | Rôle                                                         |
|-------------|--------------------------------------------------------------|
| `URI`       | Destination du routage                                       |
| `Predicate` | Condition d’activation de la route                           |
| `Filter`    | Traitements avant/après la redirection                      |


## 🔧 Propriétés Spring Cloud : 



### 1. `spring.cloud.config.enabled=false`

Cette propriété permet de **désactiver le client Spring Cloud Config**.

- Par défaut, si tu utilises Spring Cloud Config Client, l'application essaiera de se connecter à un **serveur de configuration centralisé** (`Spring Cloud Config Server`) pour récupérer ses fichiers de configuration (`application.yml`, etc.).
- En mettant cette propriété à `false`, tu **forces l'application à ignorer** toute tentative de connexion au Config Server.
- ✅ Utile si tu veux que l’application **démarre en local avec ses propres fichiers de configuration**.

> **Exemple :**
  ```properties
  spring.cloud.config.enabled=false
  ```

### 2. `spring.cloud.discovery.enabled=false`
  Cette propriété permet de désactiver le service discovery (découverte de services).

- Spring Cloud Discovery utilise généralement Eureka, Consul, ou Zookeeper pour que les microservices puissent s’enregistrer automatiquement et se découvrir entre eux.

- En mettant cette propriété à false, tu désactives cette fonctionnalité.



## ⚙️ Configuration statique de Spring Cloud Gateway avec un fichier YAML ou un Bean

Spring Cloud Gateway permet de configurer des **routes statiques** soit via un **fichier de configuration (`application.yml`)**, soit en **définissant un bean Java**.

---

### 📄 Configuration statique via `application.yml`

On peut déclarer les routes directement dans le fichier `application.yml` :

```yaml
spring:
  cloud:
    gateway:
      mvc:
        routes:
          - id: r1
            uri: http://localhost:8081
            predicates:
              - Path=/customers/**

          - id: r2
            uri: http://localhost:8082
            predicates:
              - Path=/products/**

```
### ☕ Configuration programmatique via un @Bean
```java
    import org.springframework.cloud.gateway.route.RouteLocator;
    import org.springframework.cloud.gateway.route.builder.RouteLocatorBuilder;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;

    @Configuration
    public class GatewayConfig {

        @Bean
        public RouteLocator gatewayRoutes(RouteLocatorBuilder builder) {
            return builder.routes()
                    .route("customers-route", r -> r
                            .path("/customers/**")
                            .uri("http://localhost:8081"))
                    .route("products-route", r -> r
                            .path("/products/**")
                            .uri("http://localhost:8082"))
                    .build();
        }
    }
```