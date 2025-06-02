# `Annotation` en java 
Une annotation en Java est une métadonnée (c’est-à-dire une information supplémentaire) qu’on ajoute à du code source pour :
-   décrire un comportement
-  fournir des instructions au compilateur, aux outils ou au framework (comme Spring)
-  ou injecter du comportement automatique (via réflexion, AOP, etc.)

## 🎯 Implémenter une Annotation Personnalisée en Java

### ✅ Étapes clés

1. **Créer l'annotation**
2. **Configurer la portée (`@Retention`) et la cible (`@Target`)**
3. **Lire l'annotation à l'exécution (via réflexion)**
4. *(Optionnel)* Ajouter de la logique (par exemple via AOP)

---

## 🧩 1. Créer une annotation simple

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME) // visible à l'exécution
@Target(ElementType.METHOD)         // applicable aux méthodes
public @interface MonAnnotation {
    String valeur() default "inconnu";
}
```
| Élément               | Rôle                                               |
| --------------------- | -------------------------------------------------- |
| `@Retention(RUNTIME)` | Rend l'annotation accessible à l'exécution         |
| `@Target(METHOD)`     | L'annotation ne peut s'appliquer qu'à des méthodes |
| `valeur()`            | Élément personnalisable (comme une propriété)      |


__`@Retention`__ est une annotation spéciale qui sert à définir combien de temps l’annotation personnalisée est conservée par le compilateur et la JVM,Plus précisément, elle indique le niveau de visibilité de l'annotation :

| Valeur possible           | Signification                                                                                                                                                                                                                               |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `RetentionPolicy.SOURCE`  | L’annotation est **visible uniquement dans le code source** et **supprimée au moment de la compilation**. Elle ne sera donc pas dans le bytecode (.class). Utile pour la documentation ou l’analyse statique.                               |
| `RetentionPolicy.CLASS`   | L’annotation est **conservée dans le bytecode** (fichier `.class`), mais **pas accessible à l’exécution** (pas via réflexion). C’est la valeur par défaut si tu ne mets rien.                                                               |
| `RetentionPolicy.RUNTIME` | L’annotation est **conservée au moment de l’exécution**, donc **accessible via la réflexion** (`Reflection API`). C’est indispensable si tu veux lire l’annotation pendant que ton programme tourne (par exemple, Spring ou JUnit font ça). |


__`@Target`__ est une métadonnée utilisée pour indiquer sur quels éléments de code une annotation personnalisée peut être appliquée.

| Valeur             | Utilisation                                               | Exemple                                                  |
| ------------------ | --------------------------------------------------------- | -------------------------------------------------------- |
| `TYPE`             | Classe, interface, enum, record                           | `@MonAnnotation class MaClasse {}`                       |
| `METHOD`           | Méthodes                                                  | `@MonAnnotation void faire() {}`                         |
| `FIELD`            | Attributs / champs                                        | `@MonAnnotation private int age;`                        |
| `PARAMETER`        | Paramètres d’une méthode                                  | `void saluer(@MonAnnotation String nom)`                 |
| `CONSTRUCTOR`      | Constructeurs                                             | `@MonAnnotation public MonObjet() {}`                    |
| `LOCAL_VARIABLE`   | Variables locales                                         | `@MonAnnotation int temp = 0;`                           |
| `ANNOTATION_TYPE`  | Une annotation sur une autre annotation (meta-annotation) | `@MonAnnotation public @interface NouvelleAnnotation {}` |
| `PACKAGE`          | Paquetage                                                 | `package-info.java` avec `@MonAnnotation`                |
| `RECORD_COMPONENT` | Composants d’un `record` (Java 16+)                       | `record User(@MonAnnotation String name) {}`             |


## Exemple 
```java
public @interface MonAnnotation {
    String valeur(); // obligatoire à remplir
    int count() default 1; // optionnel, avec une valeur par défaut
}

@MonAnnotation(valeur = "Exemple", count = 5) //  des  annotation avec des  arguments 
public class MaClasse {}
```




---
<br><br>


# `sealed interface` en Java

Depuis Java 17, Java permet de déclarer des interfaces ou classes comme **scellées** grâce au mot-clé `sealed`. Cela permet de **restreindre** quelles classes peuvent hériter ou implémenter une interface.

---

## 🔹 Qu’est-ce qu’une interface `sealed` ?

Une **interface sealed** est une interface **fermée** à l'héritage libre. Seules les classes listées avec le mot-clé `permits` peuvent l’implémenter.

---

## 🔸 Exemple de déclaration

```java
public sealed interface Seller permits IndividualSeller, CompanySeller {
    String getName();
    void sell();
}
```

# `record` en Java

Depuis **Java 14** (preview) et **Java 16** (stable), Java introduit un nouveau type de classe : le `record`. Il permet de **déclarer des classes immuables** de manière concise, idéales pour représenter des **données**.

---

## 🔹 Qu’est-ce qu’un `record` ?

Un `record` est une **classe spéciale** :
- Immuable par défaut
- Génère automatiquement :
  - `constructeur`
  - `getters`
  - `equals()`
  - `hashCode()`
  - `toString()`

---

## 🔸 Syntaxe de base

```java
public record Product(String name, double price) {}
```
equivalent à :
```java
public final class Product {
    private final String name;
    private final double price;

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String name() { return name; }
    public double price() { return price; }

    // equals(), hashCode(), toString() générés automatiquement
}
```
🔸 Utilisation
```java
Product p = new Product("Clavier", 49.99);

System.out.println(p.name());     // Clavier
System.out.println(p.price());    // 49.99
System.out.println(p);            // Product[name=Clavier, price=49.99]
 
```


