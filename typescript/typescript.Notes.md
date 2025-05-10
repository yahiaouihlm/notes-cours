# Type Script


## 🧱 1 .types primitifs 
| Type        | Description                           | Exemple                            |
| ----------- | ------------------------------------- | ---------------------------------- |
| `string`    | Chaîne de caractères                  | `"Hello"` ou `'world'`             |
| `number`    | Nombres (entiers, décimaux, etc.)     | `42`, `3.14`, `-0.5`               |
| `boolean`   | Vrai ou faux                          | `true`, `false`                    |
| `bigint`    | Nombres très grands entiers           | `123n`, `BigInt(9007199254740991)` |
| `symbol`    | Valeur unique, utilisée comme clé     | `Symbol('id')`                     |
| `undefined` | Valeur d'une variable non initialisée | `let x: undefined = undefined`     |
| `null`      | Absence intentionnelle de valeur      | `let x: null = null`               |


```typescript
 let  value: string = "hello" //declarer une variable  de type string 
```
## 🧩 2. Types objets 
Tout ce qui est non primitif :

* Object

* {} (objet vide)

* Tableaux : string[], Array<number>, etc.

* Fonctions : () => void, (x: number) => string

* Classes : class User {}


##  🎯 3. Types spéciaux
| Type                 | Description                                                 |
| -------------------- | ----------------------------------------------------------- |
| `any`                | Désactive le typage (accepte tout)                          |
| `unknown`            | Comme `any`, mais plus sûr (nécessite vérification)         |
| `never`              | Valeur qui ne se produit jamais (`throw`, boucle infinie)   |
| `void`               | Absence de valeur (ex. retour d'une fonction sans `return`) |
| `null` / `undefined` | Primitifs, mais aussi considérés comme spéciaux             |
| `literal` types      | `"ok"`, `42`, `true` → valeurs constantes                   |


## 🧠 4. Types composés (combinés)
| Type             | Description                                 | Exemple                     |
| ---------------- | ------------------------------------------- | --------------------------- |
| **Union**        | Valeur de plusieurs types possibles         | `string \| number`          |
| **Intersection** | Combine plusieurs types ensemble            | `TypeA & TypeB`             |
| **Tuple**        | Tableau typé avec un nombre fixe d’éléments | `[string, number]`          |
| **Enum**         | Enumérations                                | `enum Color { Red, Green }` |
| **Literal**      | Types littéraux                             | `"active"`, `true`, `0`     |


```typescript 
Types en TypeScript
├── Primitifs
│   └── string, number, boolean, etc.
├── Objets
│   └── {}, Array, Function, Class, etc.
├── Spéciaux
│   └── any, unknown, never, void
├── Composés
    └── union, intersection, tuple, enum, literal
```



## type  personnalisé 
Pour déclarer des types personnalisés, similaires aux struct en langage C, on utilise le mot-clé type en TypeScript.
Exemple :

```typescript
    type Person = {
        name: string;
        age: number;
        isMale: boolean; // "isMal" corrigé en "isMale"
    };

// Création de personnes
    const personne1: Person = {
        name: "Alice",
        age: 30,
        isMale: false
    };

```

## mettre une variable  optionnelle dans un object  
- ? pour  dire  que la  valeur d'un object est optionnelle 
```typescript
    // Le `?` sert à indiquer qu'une propriété d'un objet est optionnelle
    type Person = {
    name: string;
    age: number;
    isStudent?: boolean; // optionnelle
    nature : "male" | "female"
    };

    // Exemple d'utilisation
    const personne1: Person = {
    name: "Alice",
    age: 30,
    natre :  "male" 
    // isStudent n'est pas requis ici
    };

    // ❌ Ceci est une erreur : on ne peut pas utiliser `?` dans une variable seule
    // let value?: string; // Erreur

    // ✅ Correction : si tu veux qu'une variable soit optionnelle, tu écris :
    let value: string | undefined;

    // ❌ Ceci est une erreur d'orthographe : "undefine" n'existe pas
    // let value: null | undefine | string; // Erreur

    // ✅ Correction :
    let value2: string | null | undefined;

    const object : {name : "test" , age : 75,nature : "male"}; 
    let tab : [] Personne;
    tab.push(object) // ❌ il  faut  que object soit declarer de type personne a cause du  type nature car "male" | "female" est un binome oe "male" est de type  générique <string>


```

# arrays types 
pour déclarer un type  array en `ts`
```typescript
   let ages :number [] = [100, 101] // ages  et  de tableau de number 
   let persone : Person [] // déclarer un tableau de Person 
   let peronne : Array <Person>  //  déclarer un tableau de Person 

```

## fonctions 
une  fonction en  `ts` peut retourné un type 
```typescript
     function getUser (id : number) : User | Undefine {} // return un  User ou undefine  
```
__`Object.assign()`__ est une méthode intégrée en JavaScript (et donc utilisable en TypeScript) qui copie les propriétés d’un ou plusieurs objets source(s) dans un objet cible, et retourne l’objet cible.
exemple :
```typescript
    const cible = { a: 1 };
    const source = { b: 2, c: 3 };

    Object.assign(cible, source);

    console.log(cible); // { a: 1, b: 2, c: 3 }
```



````
````





## let vs var 
📍 1. Portée (Scope)
- `var` a une portée de fonction (function scope).

- `let` a une portée de bloc (block scope), c’est-à-dire limitée aux {}.
    ```javascript
        function test() {
        if (true) {
            var x = 10;
            let y = 20;
        }
        console.log(x); // ✅ 10
        console.log(y); // ❌ ReferenceError: y is not defined
        }
    ```
2. Hoisting (levée de déclaration)    
Les deux (var et let) sont hoistés (levés en haut du scope), mais :

    -   var est initialisé à undefined pendant le hoisting.

    - let est non initialisé, il reste en "zone morte" jusqu'à la ligne où il est défini.
    ```javascript
            console.log(a); // ✅ undefined
            var a = 5;

            console.log(b); // ❌ ReferenceError
            let b = 5;
    ```
🔁 3. Redéclaration

- var peut être redéclaré dans le même scope.

- let ne peut pas être redéclaré dans le même scope.    
    ```javascript
            var a = 1;
            var a = 2; // ✅ aucun problème

            let b = 1;
            let b = 2; // ❌ SyntaxError

    ```

 ```
 ```   
 ## programmation asynchrone