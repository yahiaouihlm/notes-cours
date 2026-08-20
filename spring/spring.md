# Spring

Spring est un **framework Java** qui permet de développer des applications robustes et maintenables. Il repose principalement sur le principe d'**injection de dépendances** (Dependency Injection), qui consiste à déléguer à Spring la gestion des objets et de leurs relations.

---

## 🔧 L'injection de dépendances avec Spring

L’injection de dépendances permet à un objet de recevoir ses dépendances de l’extérieur, plutôt que de les créer lui-même. Cela favorise la modularité et facilite les tests.

Pour activer l'injection de dépendances avec Spring, trois modules principaux sont nécessaires :

- **Spring Core** : le cœur du framework, qui contient les mécanismes de base de l'inversion de contrôle (IoC).
- **Spring Context** : permet de gérer un **contexte applicatif** contenant les objets à injecter.
- **Spring Beans** : fournit les fonctionnalités nécessaires à la déclaration et la gestion des **beans**.

---

## ⚙️ Deux méthodes de configuration dans Spring

Spring propose deux approches principales pour configurer les objets (beans) à injecter :

### 1. Configuration via XML

Dans cette méthode, on déclare les objets dans un fichier XML.

#### Exemple :
```xml
<!-- applicationContext.xml dans  le resources de votre  projet -->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="engine" class="com.example.Engine" />

      <!-- injection setter-->
    <bean id="car" class="com.example.Car">
        <property name="engine" ref="engine" />
    </bean>
    
    <!-- injection constructor-->
    <bean id="studentBean" class="fr.sqli.halim.services.Student">
     <constructor-arg name="name" value="halim"/>
    <constructor-arg name="fullName" value="monkey"/>
    <constructor-arg name="age" value="15"/>
    </bean>

</beans>
```
**Explication :**
- Le bean car dépend du bean engine.

- Spring injecte l’objet engine dans l’objet car via la propriété engine.


**Lancement :** 
```java
    // initilialisation
    ApplicationContext context = new ClassPathXmlApplicationConext("config.xml")
    
    // pour avoir le  Bean 
    IService service =context.getBean ("service",IService.class) //nom dans le fichier xml

```




### 2. **Configuration via annotations**
C’est la méthode la plus moderne, utilisant les annotations Java pour déclarer et injecter les dépendances.
```java
    // Engine.java
    @Component
    public class Engine {
        public String start() {
            return "Engine started!";
        }
    }

    // Car.java
    @Component // @Service 
    public class Car {
        @Autowired
        private Engine engine;

        public void drive() {
            System.out.println(engine.start());
        }
    }
    
        // AppConfig.java
    @Configuration
    @ComponentScan(basePackages = "com.example")
    public class AppConfig { }
```
Explication :
- **@Component** indique que la classe est un bean géré par Spring.

- **@Autowired** permet l’injection automatique de la dépendance.

- **@ComponentScan** demande à Spring de scanner le package pour détecter les composants.

### configuration du  contexte 
```java
   
   public static void main (String [] args) {
   //passer via  une class  de configuration 
   ApplicationContext context = new AnnotationConfigApplicationContext(App.class);
   
   // passer via  un  fichier  de configuration  XML 
   ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

   // demande a  spring instance qui  implement interface ISserice
   IService service = context.getBean(IService.class); 

   }

```


## 🧙‍♂️ `@Configuration` en Spring
est une annotation Spring utilisée pour indiquer qu'une classe Java sert à définir des beans.
Elle remplace les anciens fichiers XML (applicationContext.xml) en vous permettant de configurer vos beans directement en Java, de manière plus lisible et type-safe.


```java

@Service
public class UserService implements IUserService { ... }

@Service
public class AdminService implements IUserService { ... }


```

Spring ne peut pas injecter automatiquement deux beans qui implémentent la même interface sans distinction,
car il ne sait pas lequel utiliser lors de l'injection (ambiguïté).

Autrement dit, si tu as plusieurs classes qui implémentent la même interface (comme IUserService),
et que tu tries d’injecter cette interface sans précision, Spring lève une exception de type

``Solution``
```java
@Autowired
@Qualifier("userService")
private IUserService userService;
```
### À quoi sert `@Qualifier` ?
Quand tu as plusieurs beans du même type (par exemple plusieurs classes qui implémentent la même interface),
Spring ne sait pas lequel injecter automatiquement car il y a une ambiguïté.

L’annotation `@Qualifier` sert à distinguer précisément quel bean tu veux injecter.

```java
@Service("userService")
public class UserService implements IUserService { ... }

@Service("adminService")
public class AdminService implements IUserService { ... }

//  en suite  on  fait 
@Autowired
private IUserService userService; // spring  ne pas pas savoir quel instance de spring  faut-il injecter 


@Autowired
@Qualifier("userService") // indiquer à spring l'intance exacte à  injecter
private IUserService userService;


```

```java
    @Configuration
    @Profile("prod")
    @PropertySource("classpath:application.properties") // dire à spring  de charger le fichier properties dans la mémoire 

    public class SpringConfiguration {


        @Value("halim.database.password") // charger la variable 
        String passString


@Conditional(Administration Administration.class) // utilisation la methode uniquement sous la consition (implements Condition)
        @Bean(name = "universityBean")
        public University university () {
            return new University(List.of(studentBean()));
        }

        @Bean
          @Scope("prototype") // creer une nouvelle instance à chaque fois 
         @Bean(initMethod = "sayHello") // éxecuter la methode 'sayHello' définie dans Student aprés  initialiser le Bean
         @Bean(destroyMethod = "sayHello") // uniquement apres la fermeture de contexte spring  pas avec arret du programme 
    
        public Student studentBean () {
        var student=  new Student();

        student.setAge(26);
        student.setFullName("mokey D");
        student.setName("Luffy");

        return student; 
        }



// permet d'appler des traitement post - pres initilialisation de chaque bean 
// from interface  BeanPostProcessor
          @Override
    public Object postProcessBeforeInitialization( Object bean,  String beanName) {
        System.out.println("AVANT : " + beanName);
        return bean;
    }

      @Override
    public Object postProcessAfterInitialization( Object bean,  String beanName) {
        System.out.println("APRES : " + beanName);
        return bean;
    }



    
    }


```





---

## Spring Configuration 

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





