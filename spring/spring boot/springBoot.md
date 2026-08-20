# Sring Boot




```java
@SpringBootApplication // Enclenche automatiquement un @ComponentScan("lePackageOùCeTrouveCetteClasse")
```

## Commandes pour lancer Spring Boot avec Maven

Avec le plugin `spring-boot-maven-plugin` :

* **`mvn spring-boot:run`** => lancer l'application Spring Boot.
* **`mvn spring-boot:start`** => lancer l'application en mode non bloquant.
* **`mvn spring-boot:stop`** => arrêter l'application.
* **`mvn spring-boot:build-image`** => construire une image Docker à partir de l'application.
    * ```-Dspring-boot.build-image.imageName=myorg/myapp``` : pour nommer l'image docker 

### Lancer une application Spring Boot avec un fichier de configuration

On peut lancer une application Spring Boot en indiquant le nom du fichier de configuration :

```bash
java -jar monProjetSpringBoot.jar --spring.config.name=monFichierDeconf
```

Ou en indiquant directement l'emplacement des fichiers de configuration :

```bash
java -jar monProjetSpringBoot.jar \
  --spring.config.location=classpath:/x/y/z/default.properties  classpath:/x/y/oubien.properties
```

##  Fichier de configuration 
```properties
# Exemple de fichier application.properties
spring.main.banner-mode=off

# Vous pouvez y faire usage de variables
ma.variable=Bonjour
utilisation.de.ma.variable=${ma.variable} tout le monde


#Vous pouvez y faire usage de tableaux
ma.variable[0]=premiere valeur
ma.variable[1]=seconde valeur


#Vous pouvez y faire usage de valeurs aléatoires (via le SpEL)
#random long with max
app.refresh-rate-milli=${random.long(1000000)}
#random long range
app.initial-delaymilli=${
random.long[100,90000000000000000]}
#random 32 bytes
app.user-password=${random.value}
#random uuid. Uses java.util.UUID.randomUUID()
app.instance-id=${random.uuid}
```



**`DevTools`** : regroupe des outils pour simplifier le développement (rechargement à chaud du code ou des ressources en cas de modification).