# <h1 align="center">Thread</h1>

Un **thread** (ou fil d'exécution) est une unité d'exécution au sein d’un **processus**.  
Autrement dit, c’est un sous-ensemble d’un processus qui peut s'exécuter indépendamment et en parallèle avec d’autres threads du même processus.

## Un peu de système

Un système d'exploitation (OS) est capable de gérer plusieurs processus grâce au **scheduling** (ordonnancement), une fonctionnalité intégrée au **noyau** du système.

Le scheduling permet de décider **quel processus sera exécuté à quel moment**, en fonction de plusieurs critères tels que :

- La priorité des processus
- Le temps processeur déjà utilisé
- Le type de tâche (par exemple : tâche utilisateur ou tâche système)
- Les ressources disponibles
- L'algorithme d'ordonnancement utilisé (ex. : Round Robin, First Come First Served, etc.)

Ce mécanisme est essentiel pour permettre le **multitâche** et optimiser l'utilisation du processeur dans un système informatique moderne.


## Les threads ou processus légers

Les **threads** (ou **processus légers**) sont des unités d'exécution prises en charge par le système d'exploitation (OS). Cela signifie que c'est le **système** qui se charge de gérer leur **création**, leur **planification**, leur **synchronisation** et leur **destruction**.

Contrairement aux processus classiques, les threads **partagent le même espace mémoire** au sein d’un processus parent, ce qui les rend plus légers et plus rapides à créer. Cette architecture permet un traitement parallèle efficace, tout en facilitant la communication entre threads.

Cependant, cette proximité rend aussi la programmation multithreadée plus complexe à gérer, en raison des problèmes potentiels de **concurrence** (accès simultané aux mêmes données).


## 🧵 Classe `Thread` en Java

La classe `Thread` est l'objet de base permettant d’implémenter le **parallélisme** en Java. Elle permet d’exécuter plusieurs tâches en parallèle, dans des **threads** séparés.

---

### 🔧 Méthodes principales de la classe `Thread`

| Méthode           | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| `start()`         | Démarre l'exécution du thread (appelle automatiquement `run()`)            |
| `sleep(ms)`       | Met le thread en pause pendant un temps donné (en millisecondes)           |
| `yield()`         | Propose de céder temporairement l'utilisation du CPU                       |
| `wait()`          | Met le thread en attente jusqu’à ce qu’il soit réveillé                    |
| `notify()`        | Réveille un thread en attente sur le même objet                            |
| `notifyAll()`     | Réveille tous les threads en attente sur le même objet                     |

---

### ⚙️ Comportement de `start()`

Lorsque tu appelles la méthode `start()` sur un thread :

- Le thread passe de l’état **"nouveau"** à l’état **"prêt"**.
- Cela signifie qu’il est **en attente d’exécution**.
- Il **n’est pas exécuté immédiatement** : il est placé dans la **file d’attente** du planificateur de tâches du système.
- Le système d'exploitation décidera **quand** ce thread pourra effectivement être **exécuté**, en fonction de la charge du processeur, de la priorité, etc.

> 🧠 **Important** : il ne faut **jamais appeler directement `run()`** pour démarrer un thread, sinon il s’exécutera comme une méthode normale, sans parallélisme.
---



<p align="center">
    <img src="./thead.png " alt="one to one mapping">
</p>


## 🧵 Que fait `Thread.yield()` en Java ?

Lorsque plusieurs **threads** s'exécutent dans un programme Java, le processeur ne peut en exécuter **qu'un seul à la fois** (par cœur). C’est le **système d’exploitation** qui décide quel thread s’exécute et quand — c’est ce qu’on appelle le **scheduling**.

---

### 🎯 Rôle de `Thread.yield()`

La méthode `Thread.yield()` permet à un thread de **signaler au système qu’il est prêt à céder momentanément sa place** au profit d’autres threads de **priorité égale ou supérieure**.

> En d'autres termes :  
> **"Je peux attendre un peu, laisse quelqu’un d’autre passer s’il veut."**

---

### 🧠 Important à retenir :

- Ce **n’est pas une pause forcée** (contrairement à `Thread.sleep()`).
- C’est juste une **suggestion** faite au planificateur du système.
- Le **système peut ignorer** cette suggestion et continuer à exécuter le même thread.

---

### 🧪 Comparaison rapide

| Méthode              | Comportement                                    |
|----------------------|--------------------------------------------------|
| `Thread.sleep(1000)` | Pause forcée pendant 1 seconde                   |
| `Thread.yield()`     | Propose de céder le CPU, mais pas garanti       |

---

### ✅ Exemple simple

```java
for (int i = 0; i < 5; i++) {
    System.out.println("Thread courant : " + i);
    Thread.yield(); // Le thread propose de laisser la main aux autres
}
```
## Interrompre un thread

Pour interrompre un thread, on utilise la méthode `interrupt()`.

En Java, un thread **n’est jamais arrêté de force**. L’interruption fonctionne uniquement si **le thread coopère**, c’est-à-dire s’il vérifie régulièrement s’il a été interrompu via `Thread.interrupted()` ou `isInterrupted()`. Il doit ensuite **interrompre proprement son exécution**.

> ⚠️ Appeler `interrupt()` **ne stoppe pas directement** le thread.  
> Cela **positionne un flag d’interruption** (un indicateur) dans le thread.  
> Ce n’est que lorsque le thread **consulte ce flag** qu’il peut décider d’agir :
> - en sortant d’une boucle,
> - ou en réagissant à une `InterruptedException` (si le thread est bloqué dans `sleep()`, `wait()`, `join()`, etc.).

exemple :
```java
  class Traitement {
     while (!Thread.interrupted()) // consulter  le  flag provoque interrupt
  }
```

```java
   Thread th,th1;
   th=new Thread (new Traitement()) // contient un boucle infini
   th1 = new Thead(new Traitement()) // contient une  boucle  infini
   th.start();
   th1.start();
   th.interrupt() ⚠️ le  thread s arrette pas car le flag nest jamais cunsulter
```
  ⚠️ : L’appel à `Thread.interrupted()` **réinitialise le flag d’interruption**.  
Autrement dit, après cet appel, le flag repasse à `false`,  
et il faudra un nouvel appel à `interrupt()` pour le repositionner à `true`.



`stop()` à etait  abodonner par  java  car  il  ete  non deterministe  il  puvait avoutir a  des  comporement innatendu  (par ce que  c'etais  quelque chose  d'imediat)  "on peut  tuer un thread au movais moment  notamenet quand  il  manipule,  recupére  une resource partagé"



---

## 🧵 Attente d’un Thread grâce à join()
`thread.join`  permet de bloquer le thread appelant jusqu'à ce que le thread ciblé ait terminé son exécution.
C'est un mécanisme très utile en programmation multithread pour synchroniser l'exécution et éviter que le programme principal ne se termine avant les threads secondaire
```java 
    Thread t = new Thread(() -> {
        System.out.println("Thread en cours...");
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {}
        System.out.println("Thread terminé !");
    });

    t.start();

    try {
        t.join(); // attend que 't' se termine
    } catch (InterruptedException e) {}

    System.out.println("Main continue après la fin du thread.");
```


<h2 align="center">Concurrence et Partage de Données</h2>
Dans un programme multithreadé, les accès aux valeurs partagées (lecture/écriture) peuvent entraîner des comportements inattendus, notamment à cause d’opérations non atomiques comme value++ ou des instructions conditionnelles (if (...) { ... }).

Bien que ces instructions semblent simples en Java, elles sont en réalité composées de plusieurs instructions en assembleur, ce qui ouvre la porte à des entrelacements (interleavings) entre threads.

Autrement dit, pendant qu’un thread effectue une opération sur une donnée partagée (comme une copie en cache de la variable), un autre thread peut modifier cette même donnée. Cela peut provoquer une incohérence, car les modifications ne sont pas visibles entre les threads si elles ne sont pas correctement synchronisées.

## 🛠️ Solution avec synchronized
  Pour résoudre ce problème, Java propose le mot-clé synchronized, qui peut être appliqué sur une méthode ou un bloc de code, afin de :

  - Garantir un accès exclusif à la ressource partagée par un seul thread à la fois.

  - Assurer que les écritures sont visibles aux autres threads (effet mémoire).

  Cela permet de protéger les sections critiques du code et de garantir une exécution cohérente.

```java
public class Compteur {
    private int valeur = 0;

    public synchronized void incrementer() {
        valeur++;
    }

    public synchronized int getValeur() {
        return valeur;
    }
}
```

## Bloc de Synchronisation en Java

En plus de synchroniser **toute une méthode** avec le mot-clé `synchronized`, Java permet de **synchroniser uniquement une portion spécifique du code**, appelée **bloc de synchronisation**.

Cette approche est utile lorsque **seule une partie du code accède à une ressource partagée**, ce qui permet de :

- **Limiter la zone critique** pour de meilleures performances.
- **Réduire les blocages** entre threads.
- Garder **plus de contrôle** sur ce qui est synchronisé.

---

### 🔧 Syntaxe

```java
synchronized (objetVerrou) {
    // section critique : accès à la ressource partagée
}
```

L’objet utilisé comme verrou (ou "lock") peut être this, une variable dédiée, ou n’importe quel objet non nul. Un seul thread peut entrer dans le bloc synchronisé sur un même verrou à un instant donné.















## 📝 NOTE SUR LES  PROCESS,THREAD, CORE

### 🔹 La méthode `run()` dans la classe `Thread`

- `run()` est une méthode de type **`void`**, ce qui signifie qu'elle **ne retourne aucun résultat**.
- Elle ne déclare **aucune exception** avec `throws`, donc elle **ne peut pas propager d'exception** explicitement à l’extérieur.
  
---

### 🎯 Pourquoi `run()` ne lève-t-elle pas d'exception ?

- Le framework des threads en Java repose sur un **niveau d’abstraction élevé**.
- À ce niveau, il est **impossible de savoir à l’avance** quel type d’exception pourrait être levé dans le code exécuté par un thread.
- Par conséquent, Java impose que **toutes les exceptions doivent être gérées à l’intérieur de `run()`** si besoin.

> Cela encourage les développeurs à encapsuler la logique sensible dans des blocs `try-catch` à l’intérieur de la méthode `run()`.

---


## 🧠 La différence entre Cœur, Thread et Processus

Une machine moderne est conçue pour être **multi-cœurs** et **multi-processus**, ce qui permet d’exécuter plusieurs programmes en parallèle ou en alternance.

---

### 🔹 Qu'est-ce qu’un cœur (core) ?

- Un **cœur** est une unité **matérielle** du processeur.
- Il exécute les instructions **logiques** et **arithmétiques** d’un programme.
- Grâce à des technologies comme **l'hyper-threading**, un cœur peut **gérer plusieurs threads** de manière quasi simultanée.

---

### 🔹 Qu’est-ce qu’un processus ?

- Un **processus** est une **instance d’un programme** en cours d'exécution.
- Chaque processus a :
  - **Sa propre mémoire**
  - **Ses propres ressources système**
  - **Un ou plusieurs threads**
- Les processus sont **isolés** les uns des autres.

---


### 🔄 Comment fonctionne l’exécution parallèle ?

- Une machine peut avoir **plus de threads actifs que de cœurs physiques**.
- Le **système d’exploitation (OS)** utilise un **ordonnanceur (scheduler)** pour :
  - Décider quel thread ou processus sera exécuté à un instant donné.
  - **Allouer un cœur** à un thread pour une courte période (appelée *quantum*).
  - **Interrompre** un thread pour en exécuter un autre — on parle de **multitâche préemptif**.

➡️ Cela permet de faire **tourner de nombreux threads ou processus** sur un nombre limité de cœurs, en **créant une illusion de simultanéité**.

---

### ✅ Résumé comparatif

| Élément       | Nature     | Description                                          |
|---------------|------------|------------------------------------------------------|
| **Cœur**      | Matériel   | Unité physique du CPU qui exécute les instructions   |
| **Processus** | Logiciel   | Programme isolé avec sa mémoire propre               |
| **Thread**    | Logiciel   | Tâche d'exécution dans un processus, partage la mémoire |
| **Scheduler** | Logiciel (OS) | Répartit le temps CPU entre les threads et processus |

---
```java
      /* afficher le nombre  de coeurs disponible  dans une machine */
     int nbCoeurs = Runtime.getRuntime().availableProcessors();

        // Affichage du résultat
        System.out.println("Nombre de cœurs disponibles : " + nbCoeurs);
```



## ⚙️ Comment l'OS choisit quel thread ou processus exécuter

Le **scheduler** (ordonnanceur) du système d’exploitation décide **quel processus ou thread sera exécuté** à un moment donné selon plusieurs critères importants.

---

### 📌 1. La priorité

- Chaque **thread ou processus** possède une **priorité**.
- Plus la priorité est **élevée**, plus il sera favorisé par le système.
- Exemple :
  - Sous **Linux**, les priorités peuvent aller de `0` (haute) à `39` (basse) pour les tâches normales.
  - Les **tâches en temps réel** ont des politiques spéciales (`SCHED_FIFO`, `SCHED_RR`).
  - En **Java**, on peut définir la priorité avec :
    ```java
    thread.setPriority(Thread.MIN_PRIORITY);  // 1
    thread.setPriority(Thread.NORM_PRIORITY); // 5 (par défaut)
    thread.setPriority(Thread.MAX_PRIORITY);  // 10
    ```

--- 

### 📌 2. L’état du processus ou du thread

Le scheduler ne peut exécuter que les **threads "prêts" (ready)**. Voici les états typiques :
- `Ready` → En attente de temps CPU.
- `Running` → En cours d'exécution.
- `Blocked` → En attente d’un événement (I/O, verrou...).
- `Terminated` → Exécution terminée.

---

### 📌 3. Le temps CPU utilisé

- Le système suit combien de temps **chaque processus a utilisé le processeur**.
- Les threads/processus ayant **peu utilisé le CPU récemment** peuvent être favorisés pour garantir une meilleure répartition (équité).

---

### 📌 4. La politique d’ordonnancement (scheduling policy)

Le système peut utiliser plusieurs **algorithmes d’ordonnancement** :

| Politique              | Description                                                 |
|------------------------|-------------------------------------------------------------|
| `Round Robin`          | Chaque thread reçoit un court "tour" de CPU tour à tour.   |
| `First-Come First-Served` | Les processus sont exécutés dans l’ordre d’arrivée.      |
| `Priority Scheduling`  | Les processus avec une priorité plus élevée passent d’abord.|
| `Multilevel Queue`     | Divise les threads en files selon leur nature/priorité.    |
| `Shortest Job First`   | Exécute les tâches les plus rapides en premier.            |

---

### ✅ Résumé général

| Facteur                 | Rôle dans le choix d’exécution                  |
|-------------------------|-------------------------------------------------|
| Priorité                | Plus elle est haute, plus le thread est favorisé |
| État                    | Seuls les threads `Ready` peuvent être choisis   |
| Temps CPU utilisé       | Ceux qui ont peu tourné récemment sont favorisés |
| Politique du système    | Règles d’ordonnancement utilisées par l’OS       |

> 📎 Note : Même si une application définit des priorités pour ses threads, **c’est toujours le système d’exploitation** qui décide de leur exécution effective.

---



# Différence entre processus et thread : sont-ils dans la même "fil" ?

## Processus

- Un **processus** est une instance d'un programme en cours d'exécution.
- Chaque processus possède son propre **espace mémoire** (variables, code, heap, pile).
- Les processus sont **isolés** les uns des autres par le système d'exploitation.
- Plusieurs processus peuvent tourner en parallèle, chacun avec un ou plusieurs threads.

## Thread (fil d'exécution)

- Un **thread** est une unité d'exécution à l'intérieur d'un processus.
- Tous les threads d’un même processus **partagent le même espace mémoire**.
- Les threads s’exécutent **concurremment** et peuvent s'interrompre et reprendre.
- Chaque processus a au moins un thread principal (`main`).

## Est-ce qu’ils sont dans la même "fil" ?

- Non, un **processus n’est pas un thread**, mais il contient au moins un thread.
- On peut voir les threads comme des "sous-processus légers" qui partagent des ressources.
- Les threads d’un même processus courent dans le même espace mémoire mais chaque thread a son propre contexte d’exécution (pile, registre).

---

## En résumé

| Concept   | Espace mémoire       | Nombre possible par processus | Isolation     |
|-----------|---------------------|------------------------------|--------------|
| Processus | Oui (séparé)         | 1 ou plusieurs               | Oui          |
| Thread    | Partagé avec processus| Plusieurs                   | Non (partagé)|

---
# Fonctionnement du scheduler avec processus et threads

- Un **processus** comprend :
  - Son propre espace mémoire (code, données, heap, pile)
  - Un ou plusieurs **threads** d'exécution

- Le **scheduler** du système d'exploitation ne gère pas seulement les processus, mais bien **tous les threads actifs de tous les processus**.

- Chaque thread est une unité d’exécution que le scheduler peut :
  - Mettre en état **Running** (exécuté)
  - Le suspendre (état **Waiting/Blocked**)
  - Le remettre en état prêt (**Ready**) pour l’exécution

- Le scheduler décide **l’ordre et le temps d’exécution** de chaque thread selon :
  - Sa priorité
  - Son état
  - La politique d’ordonnancement (round-robin, priorité, etc.)

- Cela permet au système d’exécuter en **concurrence** plusieurs tâches même avec un nombre limité de cœurs.

---

En résumé :  
**Le scheduler "voit" tous les threads de tous les processus et les organise pour une exécution efficace.**

