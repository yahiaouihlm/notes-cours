# La généricité en Java
7
## La généricité dans les classes Java
La généricité en Java est un mécanisme qui permet à une classe de manipuler plusieurs types à la fois. Une classe générique est une classe qui peut fonctionner avec n’importe quel type d'objet, tout en permettant au compilateur de vérifier les types lors de la compilation.

On utilise la syntaxe `<T>` pour définir un type générique. Cela permet d'obtenir plusieurs avantages importants :

- Le contrôle de type à la compilation : le compilateur s’assure que seuls les bons types sont utilisés.

- Moins de castings manuels : on évite les erreurs de conversion de type.

- Réutilisabilité du code : on peut écrire une seule fois une logique qui fonctionne pour plusieurs types.

Autrement dit, une fois qu'on définit un type pour une instance d'une classe générique, ce type est fixe pour cette instance. Cela n’est pas le cas lorsqu'on utilise un simple Object, qui accepte tous les types mais sans vérification stricte.
les types génrique  ne fonctionne  pas  avec les types primitifs `box<int>` =>  erreur

exemple : 
```java
    public class Box<T> {
        private T value;

        public void setValue(T value) {
            this.value = value;
        }

        public T getValue() {
            return value;
        }
    }

```
Et pour l’utiliser :

```java
    Box<String> stringBox = new Box<>();
    stringBox.setValue("Bonjour");
    String message = stringBox.getValue(); // Pas besoin de cast
```

## La généricité dans les méthodes Java
En Java, la généricité ne s’applique pas uniquement aux classes, mais aussi aux méthodes. Cela permet de créer des méthodes flexibles, réutilisables et sûres au niveau des types, sans rendre toute la classe générique.__une meéhode générique  n'a pas  besoin d'etre dans  une  class générique__

Une méthode générique déclare le ou les types génériques avant le type de retour :
```java
public <T> T identite(T valeur) {
    return valeur;
}
```
En Java, pour indiquer que T est un type générique dans une méthode, on doit le spécifier avant le type de retour de la méthode.
```java
public <T> void afficher(T variable) // T dans les paramètre est un type générique grace au <T> de void 
```

```java
public class Utils {
    public static <T, U> void afficherDeux(T a, U b) {
        System.out.println("Premier : " + a + ", Deuxième : " + b);
    }
}
//utilisation 

Utils.afficherDeux("Nom", 25); // ==>  le premier type retourné est String , 2 est integer
Utils.afficherDeux(12, 45.6); // ==>  le premier type retourné est Integer , 2 est float
```


## La généricité borné Bounded generics
Un type générique borné en Java est un type générique qui impose une contrainte sur le type que l’on peut utiliser. En d'autres termes, tu limites les types possibles à une certaine classe de base ou interface.
- exempel : 
    ```java
    <T extends SomeClass>   
    // autre exemple 
     public <T extends Comparable<T>> T getMax(T a, T b) {
      return a.compareTo(b) > 0 ? a : b;
      }
    ```
    Cela signifie que T doit être SomeClass ou une de ses sous-classes (ou une classe qui implémente une interface, si SomeClass est une interface).

| Syntaxe                 | Signification                            |
| ----------------------- | ---------------------------------------- |
| `<T>`                   | T peut être n’importe quel type          |
| `<T extends Class>`     | T doit hériter de `Class`                |
| `<T extends Interface>` | T doit implémenter `Interface`           |
| `<T extends A & B>`     | T doit hériter de `A` et implémenter `B` |

### Note 
En Java, une classe générique peut être bornée par une seule classe mais plusieurs interfaces — __`mais pas plusieurs classes`__.

✅ Syntaxe correcte (une classe + interfaces) :
```java
public class Box<T extends Animal & Serializable & Comparable<T>> {
    // ...
}
/*
 * Ici Animal est une classe.
 * Serializable et Comparable sont des interfaces.
 * T doit hériter de Animal et implémenter Serializable et Comparable.
 * 
*/
```
❌ Syntaxe incorrecte (plusieurs classes) :
```java
    // Ne fonctionne pas !
    public class Box<T extends Animal & Human> { }
```
- Pour les classes :Tu peux mettre une seule classe après extends.
-  Pour les interfaces :Tu peux en mettre autant que tu veux, mais tu dois les enchaîner avec &.
__`📌 Ordre obligatoire :`__ : La classe en premier, suivie des interfaces.

```
```


## Généricité  avec type inconnue <?> wildcard générique
En Java, `<?>` est une wildcard générique qui signifie "un type inconnu". Elle est très utilisée avec les collections (ou d’autres types génériques) quand tu ne veux pas ou ne peux pas spécifier précisément le type générique.

__🔍 Signification de <?>__
- <?> est équivalent à <? extends Object> → n’importe quel type.
- Elle est utilisée en lecture seule : tu peux lire des éléments, mais pas en ajouter (sauf null).

```java
  public static void printAnimalSounds(List<? extends Animal> animals) {
        for (Animal animal : animals) {
            animal.makeSound();  // Peu importe si c'est un Dog, Cat ou autre sous-type d'Animal
        }
    }
    
```

Explication :   

__`List<? extends Animal>`__ permet de passer n'importe quel sous-type d'Animal à une méthode. C'est flexible et ça permet de traiter des listes de types variés comme List<Dog>, List<Cat>, etc. dans une même méthode.

__`List<T extends Animal>`__ est un type générique qui nécessite de spécifier un type concret lors de l'appel de la méthode, comme List<Dog> ou List<Cat>, et chaque appel de méthode doit être lié à un type spécifique. Cela ne fonctionne pas pour des types mixtes dans un seul appel (tu ne peux pas passer une liste contenant à la fois des Dog et des Cat en utilisant cette signature).