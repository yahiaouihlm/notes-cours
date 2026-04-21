# Asynchronous System


Les systèmes de distribution **asynchrones** reposent souvent sur un **broker** : un outil intermédiaire chargé de gérer la communication entre plusieurs applications.

### Fonctionnement

1. L'**application A** (le producteur) envoie un message au **broker**.
2. Le broker stocke ce message dans un **topic** (mode publication/abonnement) ou une **queue** (file d’attente).
3. Le broker vérifie si l'**application B** (le consommateur) est disponible :
   - ✅ Si elle est disponible, le message est immédiatement transmis.
   - ❌ Sinon, le message est conservé jusqu’à ce que l’application B soit prête à le recevoir.

### Avantages

- 🔄 **Découplage** entre producteurs et consommateurs.
- 💡 **Résilience** : les messages ne sont pas perdus si le consommateur est temporairement indisponible.
- 📈 **Scalabilité** : plusieurs consommateurs peuvent traiter les messages en parallèle.

### Exemples de brokers

- Apache Kafka
- RabbitMQ
- ActiveMQ
- Redis Streams


<p align="center">
    <img src="./sources/asynchronous.png" alt="soap request">
</p>


## Différence entre **Queue** et **Topic**

- **Queue** (file d’attente) :  
  Un message envoyé dans une queue est reçu par **un seul consommateur**.  
  Exemple : Une tâche à faire, qu’un seul worker doit exécuter.

- **Topic** (sujet/abonnement) :  
  Un message envoyé sur un topic est reçu par **tous les abonnés**.  
  Exemple : Une info météo envoyée à tous les utilisateurs abonnés.

---

### En résumé :

- **Queue** = un message → un consommateur  
- **Topic** = un message → plusieurs consommateurs


## KAFKA 
Apache Kafka est une plateforme de diffusion (streaming) distribuée, développée en Scala et Java.

Une plateforme de streaming comme Kafka offre trois fonctionnalités principales :

- Publication et abonnement aux flux de données :
Elle permet aux applications clientes de publier et de s'abonner à des flux d'enregistrements, de manière similaire à une file d'attente de messages ou à un système de messagerie d'entreprise comme les brokers JMS (par exemple ActiveMQ) ou AMQP (comme RabbitMQ).

- Stockage durable et tolérant aux pannes :
Kafka permet de stocker les flux d’enregistrements de manière fiable, en assurant la durabilité des données et la tolérance aux pannes.

- Traitement en temps réel des flux :
Il est possible de traiter les flux d’enregistrements au fur et à mesure de leur arrivée, ce qu’on appelle le traitement de flux en temps réel (real-time stream processing).