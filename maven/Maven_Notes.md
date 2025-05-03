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