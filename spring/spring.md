# Sring 

## Sring Configuration 

### Configuration spring logger
 propriétés utiles pour configurer les logs dans une application Spring Boot via le fichier application.properties.

 - __1. Définir le fichier de log__ 
    - Description : Définit le nom (et chemin) du fichier de log. Tous les logs y seront écrits  si activé.
    
    - Par défaut : Console uniquement.

        ```properties
        logging.file.name=logs/app.log
        ```
 - __2. Définir un dossier (et non un fichier unique)__ 
    - Description : Crée un fichier par défaut dans ce dossier (spring.log).
    
    - ⚠️ Si `logging.file.name` est défini, cette propriété est ignorée.

        ```properties
        logging.file.name=logs/app.log
        ```

 - __📊 3. Contrôle du niveau de log__ 
    - Description : Définit les niveaux de log disponibles : `trace`,`debug`,`info`,`warn` `error`, `fatal`, `off`
    - Personnalisable par package : pour loguer uniquement certains modules.

        ```properties
        logging.level.root=warn
        logging.level.com.HalimApp=error
        ```
                       
 - __🗃️ 4. Format de log (Pattern console ou fichier)__ 
    - Description : Description : Personnalise l’apparence des logs (date, thread, niveau, message...).
    - Personnalisable par package : pour loguer uniquement certains modules.

        ```properties
         #Pour la console
         logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss} - %msg%n 
         # Pour le fichier
         logging.pattern.file=%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n
        ```
### logger de spring, hibernet                               
 - __🛑 1. Réduction du bruit dans la console__ 
    - Description : Description : Réduit les logs de Spring ou Hibernate au strict minimum.
        ```properties
            logging.level.org.springframework=error
            logging.level.org.hibernate.SQL=off
        ```
 - __🛑 2. Afficher les logs SQL (utile en développeme__ 
    - Description : Affiche les requêtes SQL exécutées par JPA/Hibernate.
        ```properties
            spring.jpa.show-sql=true
            logging.level.org.hibernate.SQL=debug
        ```        
✅ Exemple complet recommandé: 
```properties
    #Rediriger les logs vers un fichier
    logging.file.name=logs/app.log

    # Contrôle des niveaux de log
    logging.level.root=warn
    logging.level.com.HalimApp=error
    logging.level.org.springframework=error

    # Format du log console
    logging.pattern.console=%d{HH:mm:ss} %-5level %logger{36} - %msg%n
``` 