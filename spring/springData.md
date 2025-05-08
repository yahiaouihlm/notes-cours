# Spring Data




## Gestion des Transactions avec `@Transactional` dans Spring

## 1. **Comportement de la transaction**
Lorsque nous annotons une méthode ou une classe avec `@Transactional`, cela signifie que Spring gère automatiquement le début et la fin de la transaction.

- **Commit** : Si aucune exception n'est levée pendant l'exécution de la méthode, la transaction est validée (commit).
- **Rollback** : Si une exception est levée pendant l'exécution, la transaction est annulée (rollback).

---

## 2. **Propagation des Transactions**
La **propagation** définit le comportement de la transaction en fonction de l'existence d'une transaction parente. Spring propose plusieurs types de propagation pour gérer les transactions de manière fine :

- **`REQUIRED`** (par défaut) : Si une transaction existe déjà, la méthode utilise cette transaction. Sinon, elle en crée une nouvelle.
- **`REQUIRES_NEW`** : La méthode crée une nouvelle transaction, indépendamment de toute transaction existante. Cela peut être utile pour s'assurer qu'une certaine opération soit réalisée indépendamment des autres (comme dans le cas des services qui manipulent des entités critiques).

---

## 3. **Gestion des Exceptions**
Les transactions en Spring peuvent être configurées pour effectuer un rollback sur certaines exceptions. Par défaut, Spring effectue un rollback pour les exceptions **non vérifiées** (subclass `RuntimeException`), mais il est possible de personnaliser ce comportement en indiquant explicitement les exceptions pour lesquelles un rollback doit être effectué.

```java
@Transactional(rollbackFor = Exception.class)
public void someMethod() throws Exception {
    // Toute exception entraînera un rollback
}
```

---

## 4. **Transactions et Services**
Les transactions peuvent être configurées à différents niveaux dans une application Spring, en particulier au niveau des **services** :

- **Service A** : Si un service manipule des données sensibles et fait appel à plusieurs méthodes qui doivent être validées ou annulées ensemble, on peut annoter la méthode du service avec `@Transactional`.
  
  Exemple :
  ```java
  @Transactional(rollbackFor = Exception.class)
  public void updateUserData(UserData data) {
      // Plusieurs étapes qui doivent être traitées dans une seule transaction
  }
  ```

- **Service B** : Si un autre service doit traiter des données de manière indépendante (par exemple, le traitement d'images ou l'enregistrement de fichiers), il peut avoir sa propre gestion transactionnelle en utilisant `@Transactional` dans ses méthodes.

---

## 5. **Exemple Pratique**
Imaginons une méthode de mise à jour d'un utilisateur, où nous mettons à jour les informations de l'utilisateur et ses images. Cette opération peut échouer pour plusieurs raisons, et nous voulons nous assurer que si une partie échoue, toutes les modifications soient annulées.

```java
@Transactional(rollbackFor = Exception.class)
public void updateUser(UserDto userDto) {
    // Vérification et validation des données utilisateur
    userDao.save(userDto);
    
    // Mise à jour des images, en assurant que cette partie fasse partie de la même transaction
    imageService.updateImage(userDto.getProfilePicture(), userDto.getCoverPicture());
}
```

Dans cet exemple :
- **`UserService`** et **`ImageService`** font partie de la même transaction, et si une exception survient dans l'une des étapes, tout sera annulé.
  
---

## 6. **Gestion des Transactions indépendantes**
Si nous voulons que certaines opérations fonctionnent **de manière totalement indépendante**, nous pouvons utiliser `@Transactional(propagation = Propagation.REQUIRES_NEW)` pour démarrer une nouvelle transaction même si une autre est déjà en cours.

Cela permet à certaines opérations (comme la gestion des fichiers d'image) d’être enregistrées séparément, sans interférer avec d'autres transactions dans l'application.

---

## 7. **Résumé des Types de Propagation**

| **Type de Propagation** | **Comportement** |
|-------------------------|------------------|
| `REQUIRED` (par défaut)  | Utilise une transaction existante ou en crée une nouvelle si nécessaire. |
| `REQUIRES_NEW`           | Toujours crée une nouvelle transaction indépendante. |
| `SUPPORTS`               | Si une transaction existe, elle est utilisée. Sinon, aucune transaction n'est créée. |
| `MANDATORY`              | Lève une exception si aucune transaction n'existe. |
| `NEVER`                  | Lève une exception si une transaction existe déjà. |
| `NESTED`                 | Crée une transaction imbriquée (dans une transaction existante). |

---

## 8. **Quand utiliser `@Transactional` ?**
- Lorsqu'il est nécessaire d'assurer **l'intégrité des données** dans des opérations complexes.
- Lorsqu'un service ou une méthode effectue des **modifications multiples** qui doivent être validées ou annulées ensemble.
- Lorsqu'il y a des **appels à d'autres services** et que l’on veut garantir qu'ils partagent la même transaction.

---
Gestion des Transactions avec @Transactional dans Spring