# MAVEN (build tool)
- pour  générer un  projet Maven 
 ```bash
  mvn archetype:generate \
    -DgroupId=fr.sqli \                       # ✅ Important : identifie ton organisation ou ton package racine
    -DartifactId=HalimApp \                   # ✅ Important : nom du projet (nom du dossier et du .jar)
    -Dversion=1.0.0 \                         # (Optionnel : sinon = 1.0-SNAPSHOT par défaut)
    -Dpackage=fr.sqli.demo \                  # (Optionnel : sinon = même valeur que groupId)
    -DarchetypeGroupId=org.apache.maven.archetypes \   # (Souvent implicite, mais préférable de le définir)
    -DarchetypeArtifactId=maven-archetype-quickstart \ # ✅ Important : modèle de projet à utiliser
    -DarchetypeVersion=1.4 \                  # (Optionnel : sinon Maven choisit la dernière version disponible)
    -DinteractiveMode=false                   # ✅ Important : permet une génération sans intervention manuelle

 ```
- ## Note : sur powerShell window il faut utiliser "" pour  encadrer les option
   ```bash
   mvn archetype:generate "-DgroupId=fr.sqli.testapp" "-DartifactId=testapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DarchetypeVersion=1.5" "-DinteractiveMode=false"
   ```
   **pareil pour  la  commande de lancement :**
   ```bash
      mvn compile exec:java "-Dexec.mainClass=org.halim.aop.App"
   ```



 - `-DgroupeId=fr.sqli`: Identifiant unique représentant l’organisation ou   le groupe de projets.
Il correspond généralement au nom de domaine inversé de l’entreprise (ex : com.google, fr.sqli).
Il définit aussi la structure de package Java par défaut (fr.sqli).

 - `-DartifactId=HalimApp` : Nom du projet ou de l’application.
C’est aussi le nom du répertoire créé et celui du fichier .jar généré (ex: HalimApp-1.0.0.jar).

- `-Dversion=1.0.0`:Version initiale du projet.
Par convention, les versions de développement utilisent le suffixe -SNAPSHOT (ex: 1.0.0-SNAPSHOT), mais ici, on utilise une version stable 1.0.0.

- `-Dpackage=fr.sqli.demo` :
Définit le nom du package racine dans les fichiers Java générés.
Par défaut, Maven utilise groupId, mais tu peux le personnaliser ici.
Exemple : les fichiers seront générés dans src/main/java/fr/sqli/demo.


- `-DarchetypeGroupId`=org.apache.maven.archetypes :
Le groupId de l’archétype (modèle de projet) à utiliser.
Ici, on choisit les archétypes officiels proposés par Apache Maven.


 - `- DarchetypeArtifactId` : Identifie le modèle de projet à utiliser.
    - `maven-quickstart`: génère une structure de projet Java basique avec une classe principale (App.java) et un test (AppTest.java).
    - `maven-archetype-webapp` (projet web Java avec structure JSP/Servlet 
    - `spring-boot-quickstart` 


- `-DarchetypeVersion=1.4`:    Version de l’archétype maven-archetype-quickstart à utiliser.
La version 1.4 est souvent utilisée pour les projets simples. 

- `DInteractiveMode=false` : Exécute la commande sans poser de questions à l'utilisateur.
Cela permet une génération automatisée du projet (pratique pour les scripts ou l’apprentissage).

Après l'exécution de la commande, il faut ensuite créer le package (par exemple `HalimApp`), qui va englober l'ensemble de l'application

__Maven place-t-il les fichiers JAR téléchargés ?__ 
Lorsque Maven télécharge une dépendance, il la place dans son dépôt local, qui se trouve dans ton répertoire utilisateur.
Par défaut, ce dépôt est situé dans :
-   `Sous Windows` : C:\Users\<nom_utilisateur>\.m2\repository

- ``Sous macOS/Linux`` : /Users/<nom_utilisateur>/.m2/repository ou /home/<nom_utilisateur>/.m2/repository



## commandes maven
Pour utiliser correctement les commandes `maven`, il faut se placer dans la racine du projet, là où se trouve le fichier `pom.xml`  



- ✅`mvn compile` :  Analyse le fichier pom.xml, télécharge les dépendances nécessaires (si elles ne sont pas encore dans le dépôt local .m2/repository), puis compile les fichiers source Java (src/main/java) et place les fichiers .class générés dans le dossier target/classes

- ✅`mvn test` : Exécute les tests unitaires situés dans src/test/java à l’aide de JUnit ou d’autres frameworks de test, après compilation des classes de test et des classes principales

-  ✅`mvn  clean` :Supprime le dossier `target/ `du projet,Ce dossier contient les fichiers compilés, les classes .class, les JAR/WAR générés, etc.

- ✅`mvn install`: Compile le projet (compile),Exécute les tests (test).Génère les fichiers .jar ou .war dans target/ (package),  Installe le JAR/WAR résultant dans ton dépôt Maven local (~/.m2/repository), ce qui le rend réutilisable par d'autres projets Maven sur ta machine.

- ✅`mvn package` : il va  d'abord `compiler`, lancer  les  `test` et au  finale  générer le jar 

- ✅ `mvn dependency:go-offline`: est une commande Maven qui sert à pré-télécharger toutes les dépendances de ton projet pour pouvoir compiler ou lancer Maven sans connexion Internet par la suite.

- ✅`mvn dependency: tree` , `mvn dependency:lsit` :  afficher  toutes les  depndences  externe d'un projet 

- ✅`mvn dependency:build-classpath`:afficher toues l'emplacement  de toutes les  dependences .jar  utilisé 

- ✅`D<nom_de_la_propriété>=<valeur_à passer>` :L’option -D dans Maven sert à passer un paramètre système (une propriété) à l'exécution de la commande. Elle a cette forme : exp `mvn test -Dtest=*ServiceTest` executer  tous les  test  qui  termine  par ServiceTest

- `mvn clean dependency:copy-dependencies package` une commande combinée qui exécute plusieurs étapes successives dans un projet Maven afin de nettoyer, préparer et construire le projet.

## mvn  dependency:< option >
# Commandes Maven Dependency Plugin

Le plugin **Maven Dependency Plugin** propose plusieurs objectifs (`goals`) pour gérer les dépendances d’un projet Maven.

---

## 1. `dependency:analyze`
- **Description** : Analyse les dépendances du projet pour détecter celles utilisées mais non déclarées et celles déclarées mais non utilisées.
- **Utilisation** : `mvn dependency:analyze`

## 2. `dependency:analyze-dep-mgt`
- **Description** : Analyse la section `dependencyManagement` pour détecter les incohérences dans la gestion des versions des dépendances.
- **Utilisation** : `mvn dependency:analyze-dep-mgt`

## 3. `dependency:analyze-exclusions`
- **Description** : Vérifie si les exclusions de dépendances sont bien nécessaires.
- **Utilisation** : `mvn dependency:analyze-exclusions`

## 4. `dependency:copy`
- **Description** : Copie une ou plusieurs dépendances spécifiques vers un dossier donné.
- **Utilisation** : `mvn dependency:copy -Dartifact=groupId:artifactId:version -DoutputDirectory=chemin`

## 5. `dependency:copy-dependencies`
- **Description** : Copie toutes les dépendances du projet dans un dossier spécifique (par défaut `target/dependency`).
- **Utilisation** : `mvn dependency:copy-dependencies -DoutputDirectory=target/dependency`

## 6. `dependency:copy-resources`
- **Description** : Copie les ressources associées aux dépendances (ex : sources, javadoc) vers un dossier.
- **Utilisation** : `mvn dependency:copy-resources`

## 7. `dependency:go-offline`
- **Description** : Télécharge toutes les dépendances nécessaires pour pouvoir travailler hors ligne.
- **Utilisation** : `mvn dependency:go-offline`

## 8. `dependency:resolve`
- **Description** : Résout les dépendances du projet et affiche celles qui seront utilisées.
- **Utilisation** : `mvn dependency:resolve`

## 9. `dependency:resolve-plugins`
- **Description** : Résout les plugins Maven utilisés dans le projet.
- **Utilisation** : `mvn dependency:resolve-plugins`

## 10. `dependency:tree`
- **Description** : Affiche l’arbre des dépendances, utile pour visualiser les relations entre dépendances.
- **Utilisation** : `mvn dependency:tree`

---

## Options communes

- `-DoutputDirectory=chemin` : Spécifie le dossier où copier les fichiers.
- `-DincludeScope=scope` : Inclut uniquement les dépendances d’un scope donné (`compile`, `runtime`, `test`…).
- `-DexcludeScope=scope` : Exclut les dépendances d’un scope donné.
- `-Dclassifier=type` : Inclut les dépendances avec un classificateur spécifique (`sources`, `javadoc`…).
- `-DoverWriteIfNewer=true|false` : Permet d’écraser uniquement si la version est plus récente.

---

## Documentation officielle

Pour plus de détails, consultez la documentation officielle :  
[https://maven.apache.org/plugins/maven-dependency-plugin/](https://maven.apache.org/plugins/maven-dependency-plugin/)
