# <h1 align="center"> Hibernate Mapping  </h1>


## Principale annotations de hibernet 
- __`@Enity`__ : indique que la classe Java correspond à une entité JPA, c’est-à-dire qu’elle sera mappée à une table dans une base de données relationnelle.

- __`@Table(name="[table_name]")`__ : précise explicitement le nom de la table dans la base de données à laquelle l'entité doit être liée. Si cette annotation est omise, JPA utilisera par défaut le nom de la classe.

- __`@Id`__: Indique que l’attribut est la clé primaire de l’entité. Obligatoire pour toute entité JPA.

- __`@GeneratedValue(strategy = GenerationType.IDENTITY) [(strategy = GenerationType.UUID)]`__ : Spécifie que la clé primaire est générée automatiquement par la base de données (ex : auto-incrément en PostgreSQL/MySQL) depuis hirebenet 6 `UUID` aussi .


- __`@Column(name = "[nom_colonne]", unique=true,legth...)`__
Permet de définir le nom exact de la colonne en base, ainsi que d'autres contraintes comme nullable, length, unique, etc.


- __`@Temporal(TemporalType.TIMESTAMP)`__
Spécifie comment une java.util.Date ou java.util.Calendar doit être convertie en base (DATE, TIME, TIMESTAMP).

- __`@manyToOne`__ Définit une relation plusieurs-à-un entre cette entité et une autre (ex : plusieurs patients peuvent avoir le même médecin).

-  __`@JoinColumn(name = "[nom_colonne]")`__: Utilisé avec une relation `(@ManyToOne, etc.)`, pour indiquer la colonne de jointure dans la table.

exemple : __`@manyToOne`__ et __`OneToMany`__
```sql
        -- Table des médecins
                CREATE TABLE IF NOT EXISTS medecin (
                    id SERIAL PRIMARY KEY,
                    firstname VARCHAR(100) NOT NULL,
                );

                -- Table des patients
                CREATE TABLE IF NOT EXISTS patient (
                    id SERIAL PRIMARY KEY,
                    firstname VARCHAR(100) NOT NULL,
                    lastname VARCHAR(100) NOT NULL,
                    email VARCHAR(300),
                    UNIQUE(email)
                );    

                -- Table de consultation (table de jointure)
                CREATE TABLE IF NOT EXISTS consultation (
                    id SERIAL PRIMARY KEY,
                    date_consultation TIMESTAMP NOT NULL,

                    medecin_id INT NOT NULL,
                    patient_id INT NOT NULL,
                    rendez_vous_id INT NOT NULL,

                    FOREIGN KEY (medecin_id) REFERENCES medecin(id),
                    FOREIGN KEY (patient_id) REFERENCES patient(id),
                    FOREIGN KEY (rendez_vous_id) REFERENCES rendez_vous(id)
                );
        
```
 Dans cette  exemple chaque `medecin` ou `patient` peuvent  avoir  __plusireus__ `consultations` mais  une `consultation` il devrait etre attribue à  un seule `medecin`  et `patient` donc 

- __class Medecin ou Patient__
    ```java
                public  class Medecin{
                    @OneToMany(mappedby="patient") // ça se lit One Medcin To Many Consultation
                    private List<Consultation>
                }
    ```
    - `mappedby="patient"`:  indique l’attribut propriétaire de la relation dans l’autre entité.

- __class Consultation__        
        
    ```java
            public  class Consultation{
                @ManyToOne // ça se lit many consultation to One Medcin
                private  Medecin medecin; 

                // ça se lit many consultation to One patient
                @JoinColumn(name = "medecin_id") : //définit la colonne de jointure (clé étrangère) dans la table
                private Patient patient;
            }
    ```   
   - `@JoinColumn(name = "medecin_id")` : définit la colonne de jointure (clé étrangère) dans la table consultation.      

