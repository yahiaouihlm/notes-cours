# 🔧 Logging
 
Le logging est un concept de journalisation qui consiste à enregistrer toutes les informations nécessaires au traitement d’un programme.
Cela peut inclure des informations simples, des erreurs, des avertissements (warnings), etc.

## ❓ Pourquoi utiliser des logs et non simplement System.out.println ?
- Les logs permettent d’assigner un niveau de sévérité (par exemple : ``INFO``, ``WARNING``, ``ERROR``, etc.), ce qui permet de filtrer rapidement et efficacement les informations pertinentes.
Contrairement à ``System.out.println``, qui affiche tout de manière brute.

- Les ``logs`` peuvent être :
    -   Configurés dynamiquement (via un fichier XML, YAML, ou propriétés),
    - Redirigés vers plusieurs destinations (fichiers, bases de données, consoles, serveurs distants, etc.)
    - Archivés automatiquement selon des règles (taille, date, etc.).

Il existe plusieurs frameworks de logging en Java, tels que __Log4j__, __Logback__, etc.
Tous ces frameworks implémentent (ou s'intègrent via) des interfaces communes comme celles fournies par __SLF4J__ ou __java.util.logging__, et proposent chacun une manière différente de gérer le logging.

## ⚙️ Concepts clés de Log4j et Logback 
Les frameworks de logging comme __Log4j__ ou __Logback__ reposent principalement sur deux concepts fondamentaux :
1. __Loggers__ 
    - Les loggers sont responsables de collecter les messages de log depuis le code.
    - Chaque classe ou module peut posséder son propre logger.
    - Ils permettent de définir le niveau de log (__INFO__, __DEBUG__, __ERROR__, etc.).

2. __Appenders__

    Les appenders (ou destinations) indiquent où les logs doivent être envoyés :__Fichiers (.log)__,__Console__,__Bases de données__ ..Ect

    Un logger peut être associé à plusieurs appenders. 

## 🧪 Filtres et niveaux de logs
Les ``loggers`` et les ``appenders`` peuvent tous deux avoir des filtres de niveau (ex. : n’enregistrer que les messages ERROR).
```
[MyLogger] ---> [FileAppender] (niveau = INFO)
           ---> [ConsoleAppender] (niveau = DEBUG)
```

## 🔥 Logback
__Logback__ est un framework de journalisation (logging) pour Java, conçu par les créateurs de Log4j. Il est aujourd’hui largement utilisé dans les applications Spring car il est :

- Performant

- Flexible

- Intégré par défaut dans Spring Boot

### 🛠️ Utilisation de Logback
La configuration de ``Logback`` se fait via des fichiers XML. Voici les options disponibles :
- ✅ __logback.xml__ : Fichier de configuration standard pour Logback.

    👉 Utilisé automatiquement s’il est présent dans src/main/resources

- 🧪 __logback-test.xml__ : Fichier de configuration prioritaire lors des tests.

- 🌱 __logback-spring.xml__ : Spécifique à Spring Boot : permet d'utiliser les expressions comme ${property.name} et d’injecter des variables du contexte Spring.

 

```xml
<configuration>

  <!-- Définir un appender qui écrit dans la console -->
  <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
  </appender>

  <!-- Définir un appender fichier avec rotation -->
  <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>logs/app.log</file>
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
      <fileNamePattern>logs/app.%d{yyyy-MM-dd}.log</fileNamePattern>
      <maxHistory>7</maxHistory>
    </rollingPolicy>
    <encoder>
      <pattern>%date %-5level %logger - %msg%n</pattern>
    </encoder>
  </appender>

  <!-- Racine des logs : INFO et au-dessus, va à la console et au fichier -->
  <root level="INFO">
    <appender-ref ref="CONSOLE" />
    <appender-ref ref="FILE" />
  </root>

  <!-- Logger spécifique à ton package -->
  <logger name="com.halimyahiaoui.myapp" level="DEBUG"/>

</configuration>
```

- __``<configuration>``__ : Balise racine obligatoire pour toute configuration Logback,Tout le contenu (appenders, loggers, root, etc.) doit être à l'intérieur.

- __``<appender>``__ : Définit un destinataire des logs (console, fichier, etc.). Attributs 
    - ``name``  : nom unique de l'appender (référence dans <root> ou <logger>)
    - ``class`` : classe Java implémentant l'appender (ex: ``ConsoleAppender``, ``RollingFileAppender``)

        ```xml
        <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
                ...
        </appender>
        ```
- __``<encoder>``__:    Définit le format des messages de log, Utilisé à l'intérieur d'un appender.
    ```xml
       <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    ```
    ``%d`` : date/heure

    ``%thread`` : thread Java

    ``%level`` : niveau de log (INFO, DEBUG...)

    ``%logger`` : nom du logger (classe Java)

    ``%msg`` : message de log

    ``%n ``: saut de ligne
            

- __``<rollingPolicy>``__ : Définit la politique de rotation des fichiers de log, C'est-à-dire que les fichiers de log seront archivés périodiquement selon une base temporelle. Utilisé avec ``RollingFileAppender`` , Attribut : class=``"TimeBasedRollingPolicy"`` pour une rotation basée sur la date.

    ```xml
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
        <fileNamePattern>logs/app.%d{yyyy-MM-dd}.log</fileNamePattern>
        <maxHistory>7</maxHistory>
    </rollingPolicy>
    ```
    - ``fileNamePattern`` : format de nom pour les fichiers archivés
    - ``maxHistory`` : nombre de fichiers archivés à conserver (ex : 7 jours) (Tout fichier de log archivé (par date) sera supprimé après 7 jours.)

- __``<root>``__ : Logger principal (obligatoire),Attribut : ``level`` (niveau minimal de log à enregistrer), Contient des références à des ``<appender>`` via ``<appender-ref>``
    ```xml
        <root level="INFO">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="FILE" />
        </root>
    ```

-  __``<logger>``__: Logger spécifique à une classe ou à un package,  Attributs : 
    
    - ``name`` : nom du package ou de la classe ciblée
    - ``level`` : niveau de log spécifique
        ```xml
         <logger name="com.halimyahiaoui.myapp" level="DEBUG" />
         <!-- Ce logger affichera les logs DEBUG et plus pour toutes les classes de com.halimyahiaoui.myapp. -->
        ```

- __``<appender-ref>``__  : Utilisé dans ``<root>`` ou ``<logger>`` pour lier un appender défini ailleurs.
    ```xml
         <appender-ref ref="CONSOLE" />
    ```
