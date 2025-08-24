<h1 align="center">jenkins</h1>

Jenkins est un outil open-source  __d’intégration continue (CI)__ et de  __déploiement continu (CD)__ écrit en Java. Il est largement utilisé dans le développement logiciel pour automatiser
 telque 
1. Automatiser le build (la compilation)
2. Lancer les tests automatiquement
3. Déployer l’application automatiquement
4. Intégrer des modifications de code fréquemment
5. Surveiller les changements dans un dépôt Git (ou autre)


## installation  et utilisation 
Jenkins peut être installé directement sur une machine (Linux, Windows, macOS), ou utilisé plus facilement via Docker, ce qui est recommandé pour un déploiement rapide et isolé.
```bash 
docker run -d --name jenkins -p 8080:8080 -p 50000:50000  -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts


-p 8010:8080 : L’interface Web de Jenkins sera accessible à l’adresse http://localhost:8010.
-p 5040:50000 : Port utilisé pour la communication avec les agents Jenkins (JNLP).
-v jenkins_home:/var/jenkins_home : Volume Docker pour la persistance des données Jenkins.
```
Chaque job Jenkins possède son propre espace de travail (workspace),  dans /var/jenkins_home


## infrastrucre de jenkins

<p align="center">
    <img src="jenkins-infrastucture.png" alt="architecture de hibernet">
</p>

L’architecture de ``Jenkins`` peut varier selon les besoins, mais elle repose généralement sur une structure maître/agents :

1. 🧠 Jenkins Master (Contrôleur) Le serveur principal qui :
    - Gère l'interface web
    - Planifie l'exécution des jobs
    - Attribue les tâches aux agents
    - Gère la configuration, les plugins, les utilisateurs, etc. 

    Répertoire principal : /var/jenkins_home Contient :
    - La configuration des jobs
    - Les plugins
    - Les logs
    - Les utilisateurs
    - Les workspaces


2. 🖥️ Jenkins Agents (ou Slaves)

    Machines (physiques ou virtuelles) auxquelles le contrôleur peut déléguer l'exécution des tâches.
    - Utiles pour répartir la charge
    - Peuvent être spécialisés (ex. : un agent pour Java, un autre pour Node.js, etc.)
    - Communiquent avec le contrôleur via SSH, JNLP, ou Docke


3. 🔌 Plugins Jenkins

    Jenkins est hautement extensible grâce à plus de 1 800 plugins permettant :
    1. L’intégration avec Git, Docker, Kubernetes, Slack, Jira, etc.
    2. L’ajout de types de jobs spécifiques (Pipeline, Maven, etc.)
    3. La gestion de la sécurité, des credentials, des notifications, etc.


4. 🧱 Pipeline Jenkins (Jenkinsfile)

    Un fichier texte écrit en ``Groovy`` qui décrit les étapes (stages) d’un processus CI/CD.


## Différents types de job jenkins

## 📌 Différents types de jobs Jenkins

### 1. 🧱 Freestyle Job
C’est le type de job **classique** dans Jenkins.  
Il offre une interface graphique simple pour configurer les étapes (build, test, déploiement).

**Caractéristiques :**
- Configuration via l’interface graphique.
- Simple à mettre en place mais peu flexible.
- Pas de gestion du pipeline sous forme de code.

---

### 2. 🔁 Pipeline Job (Scripted ou Declarative)
Un job **Pipeline** permet de décrire l’ensemble du workflow de CI/CD **sous forme de code**, souvent dans un fichier `Jenkinsfile`.

**Caractéristiques :**
- Supporte des étapes complexes (conditions, parallélisme, etc.).
- Code versionné avec le projet.
- Deux syntaxes possibles :
  - **Declarative** (recommandée)
  - **Scripted** (plus ancienne et flexible)

---

### 3. 🌿 Multibranch Pipeline
Extension du **Pipeline** qui permet de créer automatiquement des jobs pour **chaque branche** d’un dépôt Git contenant un `Jenkinsfile`.

**Caractéristiques :**
- Détection automatique des branches.
- Chaque branche possède son pipeline dédié.
- Idéal pour les projets avec plusieurs environnements (dev, staging, prod).

---

### 4. 🏗️ Pipeline Multiconfiguration (Matrix Job)
Permet d’exécuter un job sur **plusieurs combinaisons** d’axes (versions de Java, systèmes d’exploitation, bases de données, etc.).

**Caractéristiques :**
- Idéal pour les **tests croisés**.
- Possibilité de lancer les builds en parallèle.
- Moins utilisé depuis l’apparition des pipelines parallélisés.


⚙️ Récapitulatif des différences

| Type de Job          | Code / Scripté      | Auto-détection des branches | Complexité       | Cas d’usage typique  |
| -------------------- | ------------------- | --------------------------- | ---------------- | -------------------- |
| Freestyle            | Non                 | Non                         | Faible           | Tâches simples       |
| Pipeline             | Oui (`Jenkinsfile`) | Non                         | Moyenne à élevée | CI/CD scripté        |
| Multibranch Pipeline | Oui                 | Oui                         | Élevée           | CI/CD multi-branches |
| Matrix (Multiconfig) | Optionnel           | Non                         | Moyenne          | Tests croisés        |



## Agent
Dans Jenkins, un `agent` (aussi appelé ``node``) est une machine ou un conteneur qui exécute des jobs Jenkins à la place du master.

### 1. Master / Controller Jenkins
- Serveur principal de Jenkins
- Orchestre les jobs, gère l’interface web et les plugins.
- Peut exécuter des builds lui-même, mais ce n’est pas recommandé pour la production.

### 2. Agent
- Machine ou conteneur qui exécute réellement les jobs.
- Reçoit des instructions du master pour :
    * Compiler le code
    * Exécuter des tests
    * Déployer des applications
- Peut être configuré pour :
    * Exécuter un ou plusieurs jobs en parallèle via # of executors
    * Avoir des labels pour cibler certains jobs (ex : java, docker, linux)

###  3. Types d’agents
- ``Static agents`` : machines fixes enregistrées dans Jenkins.
- ``Dynamic agents ``: conteneurs ou machines provisionnés à la volée (via Kubernetes, Docker, AWS EC2, etc.).

### 4. Exemple concret
Supposons que tu as deux jobs :
- Job A -> besoin de Maven
- Job B -> besoin de Node.js

tu peux créer 
* Agent `maven-agent` avec Maven installé
* Agent `node-agent` avec Node.js installé



# Jenkins configuration
## 1. Configuration globale (Manage Jenkins → Configure System)
- #of executors :  nombre de thread à  utiliser en paralèle  autement dit  le  nombre de build à  lancer en paralèle