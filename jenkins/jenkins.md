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