# 🧩 Design Patterns
Les Design Patterns (ou patrons de conception) sont des manières éprouvées de concevoir du code pour résoudre des problèmes récurrents en programmation orientée objet.
Ils permettent de rendre le code :
- ✅ Plus lisible
- 🔧 Plus facilement maintenable
- ♻️ Plus réutilisable

💡 Un design pattern n’est pas un morceau de code à copier-coller, mais plutôt une solution type à un problème type, que l’on peut adapter à son contexte.

🌐 Les `design patterns` ne sont pas liés à un langage de programmation spécifique.
Ils s’appliquent à tout langage supportant les concepts de la programmation orientée objet (comme Java, C#, Python, etc.).

## 🧩 Design Pattern : Proxy
Le pattern Proxy est un design pattern structurel qui consiste à insérer un intermédiaire entre un client et un objet cible. L'objectif est de contrôler l'accès à cet objet tout en isolant certains comportements (comme la sécurité, la journalisation, la mise en cache, etc.).

__🎯 Objectif__ : 
Ce pattern permet d'intercepter les appels à un objet et d’ajouter des traitements avant ou après l’appel réel à l’objet cible, sans que le client ne s’en rende compte.

- __🔧 Exemple sans Proxy__
    ``` java 
    public class Controller {
        IService service;

        public void setService(IService service) {
            this.service = service;
        }
    }

    public interface IService {
        int compute();
    }

    public class Service implements IService {
        public int compute() {
            System.out.println("Traitement principal");
            return 42;
        }
    }

    // Dans le contexte de l'application
    Controller controller = new Controller();
    controller.setService(new Service());  // Injection directe du service
    ```
    Dans cet exemple, le contrôleur appelle directement le service. Aucun traitement supplémentaire ne peut être ajouté sans modifier la classe Service

- __🛡️ Exemple avec le pattern Proxy__

    Grâce au Proxy, nous pouvons ajouter des comportements autour de l’appel au service sans modifier ce dernier.


    ``` java 
    public class Controller {
        IService service;

        public void setService(IService service) {
            this.service = service;
        }
    }

    public interface IService {
        int compute();
    }

    public class Service implements IService {
        public int compute() {
            System.out.println("Traitement principal");
            return 42;
        }
    }

    // Le Proxy implémente la même interface
    public class ServiceProxy implements IService {
        private final Service service = new Service();

        public int compute() {
            System.out.println("Pré-traitement (ex: vérification de sécurité)");

            int result = service.compute();

            System.out.println("Post-traitement (ex: journalisation)");
            return result;
        }
    }

    // Utilisation dans l'application
    Controller controller = new Controller();
    controller.setService(new ServiceProxy());  // Injection du proxy à la place du service

    ```
Dans cet exemple, le contrôleur utilise toujours l’interface IService, mais sans savoir qu’il parle à un proxy. Le proxy, lui, délègue les appels au Service réel tout en ajoutant des traitements avant ou après. Ce mécanisme rend le code plus flexible, modulaire et maintenable.
 