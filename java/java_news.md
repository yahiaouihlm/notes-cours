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


