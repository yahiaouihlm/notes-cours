# Les lambdas  Java 
Une `lambda représente une fonction` que tu peux passer comme paramètre, stocker dans une variable, ou utiliser dans des API comme les streams.
## pourquoi  les lambdas : 
- impossible  de définir une  fonction  en  dehors d'une  class 
- impossible de passer  une fonction  en paramètre d'une  autre 

__💡 Syntaxe de base :__
```java
(param1, param2) -> { corps de la fonction }
```

## 🔹 Exemple 1 : Remplacer une classe anonyme

### Avant (classe anonyme) :

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Bonjour depuis un thread !");
    }
};
new Thread(r).start();
```

### Après (lambda) :

```java
Runnable r = () -> System.out.println("Bonjour depuis un thread !");
new Thread(r).start();
```

---

## 🔹 Exemple 2 : Trier une liste avec `Comparator`

### Avant :

```java
List<String> noms = Arrays.asList("Alice", "Bob", "Charlie");
Collections.sort(noms, new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return s1.compareTo(s2);
    }
});
```

### Avec lambda :

```java
Collections.sort(noms, (s1, s2) -> s1.compareTo(s2));
```

### Encore plus court (référence de méthode) :

```java
Collections.sort(noms, String::compareTo);
```

---

## 🔹 Exemple 3 : Parcourir une liste avec `forEach`

```java
List<String> fruits = Arrays.asList("Pomme", "Banane", "Orange");
fruits.forEach(fruit -> System.out.println(fruit));
```

---

## 🔹 Exemple 4 : Utiliser `Stream` et `filter`

```java
List<Integer> nombres = Arrays.asList(1, 2, 3, 4, 5, 6);

nombres.stream()
       .filter(n -> n % 2 == 0)
       .forEach(n -> System.out.println("Pair : " + n));
```

---

## 🔹 Exemple 5 : Interface fonctionnelle personnalisée

```java
@FunctionalInterface
interface Calculateur {
    int calcul(int a, int b);
}

Calculateur addition = (a, b) -> a + b;
System.out.println(addition.calcul(5, 3));  // Affiche : 8
```

---

## 🧠 Résumé : Syntaxes de base

| Syntaxe Lambda        | Signification                   |
|-----------------------|----------------------------------|
| `() -> ...`           | Lambda sans paramètre           |
| `(x) -> ...`          | Lambda avec un seul paramètre   |
| `(x, y) -> ...`       | Lambda avec plusieurs paramètres|
| `(x, y) -> { ... }`   | Lambda avec bloc d'instructions |
