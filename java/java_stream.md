# Les Streams en  java 
Les __`streams`__ sont une `API` introduite dans Java 8 qui permet d’adopter une approche de programmation fonctionnelle, même si Java reste un langage orienté objet. Cette API permet de traiter des collections de données de manière déclarative (sans boucles explicites), en enchaînant des opérations comme `filter`, `map`, ou `collect`.

Il est important de ne pas confondre les streams de Java 8 avec les classes de l’API I/O telles que `InputStream` ou ``OutputStream.``

-   Les streams I/O manipulent des flux d’octets ou de caractères (pour lire ou écrire des données, comme dans des fichiers ou des sockets).

- `Les streams de l’API Java 8`, quant à eux, manipulent un flux d’objets issus de collections (comme des listes ou des ensembles), permettant des traitements fonctionnels comme le filtrage, la transformation ou l’agrégation. 

## Declaration 
stream  est une  interface  java  `Interface Stream<T>`

```java 
  Stream<T> stream = collection.stream();
```
.steam()  s'applique  a  toutes  les  classes,  interfaces  qui  implements  stream