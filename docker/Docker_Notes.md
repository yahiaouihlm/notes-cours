
# 📘 Notes techniques : Docker

## 🐳 Dockerfile

```Dockerfile
FROM <define work environment>
LABEL version="1.1.0" # metadata visible via Docker inspect
ENV FLASK_APP=app.py  # defines environment variable
```

```bash
docker build -t my_app <docker_image_folder>  # build and tag image
                                          # -t for image name 


```
## DockerFile commands 
```Dockerfile
- ADD         : Copy files from the host into the container’s filesystem at the specified destination.
- CMD         : Execute a specific command within the container.
- ENTRYPOINT  : Set a default application to run every time a container is created from the image 
                (automatically executed when the container starts).
- ENV         : Set environment variables.
- EXPOSE      : Expose a specific port to enable networking between the container and the outside world.
- FROM        : Define the base image used to start the build process.
- MAINTAINER  : Specify the full name and email address of the image creator. *(Note: deprecated in favor of LABEL)*
- RUN         : Execute commands in a new layer on top of the current image and commit the results.
- USER        : Set the user (by username or UID) to run the container.
- VOLUME      : Enable access from the container to a directory on the host machine.
- WORKDIR     : Set the working directory where the command defined with CMD or RUN will be executed.

```

- Exemple
   ```Docker 
      FROM  nginx
      COPY index.html usr/share/ngix/html
      Expose 80  # nginx ecoute sur un port interne 80 (intérieur du container)
   
   ```

---

## 🌐 Docker Network
Un network Docker (réseau Docker) est une couche d’abstraction que Docker utilise pour permettre à des conteneurs de communiquer entre eux et avec l’extérieur.

- Les conteneurs tournent dans des **réseaux isolés**
- On peut en connecter plusieurs au **même réseau personnalisé**


## types de Networking In Docker

# 🌐 Réseaux Docker

### 🧱 Bridge (mode par défaut)
- Mode de réseau **par défaut** utilisé lorsqu’un conteneur est lancé sans configuration spécifique.
- Docker crée un **réseau privé interne** sur le système hôte.
- Chaque conteneur reçoit une adresse IP (souvent dans la plage `172.17.0.x`).
- Les conteneurs peuvent **communiquer entre eux** via leurs adresses IP internes.
- Pour accéder à un conteneur depuis l’extérieur (hors du host), il faut **mapper ses ports** sur ceux du système hôte (`-p 8080:80`).

---

### 🌐 Host
- Le conteneur utilise **la pile réseau du système hôte**.
- Il **ne possède pas d’interface réseau propre**.
- Améliore les performances, mais réduit l'isolation.
- Ex : un service exposé sur le port `80` dans le conteneur est accessible directement via `localhost:80`.


### 🚫 None
- Le conteneur n’a **aucun accès réseau**.
- Aucune communication possible avec d'autres conteneurs ou l’extérieur.
- Utilisé pour des cas d'**isolement complet**, comme des tests de sécurité.



### 🔄 Overlay
- Permet aux conteneurs situés sur **plusieurs hôtes Docker** (dans un cluster) de communiquer.
- Crée un réseau virtuel **au-dessus des réseaux physiques**.
- Utilisé principalement avec **Docker Swarm** ou des orchestrateurs.



### 🛠️ Macvlan
- Le conteneur obtient une **adresse IP du réseau local physique** (comme une machine physique).
- Utile lorsque le conteneur doit être **directement visible** sur le réseau local.
- Exemple d’usage : serveur DHCP, NAS virtuel, etc.


### Creation 
```bash 
docker network create  --driver bridge  --subnet 182.18.0.0/16  mon_resea #create network
```
- `--driver brige` : indique que le réseau est de type bridge (par défaut).
- `--subnet` : (optionnel) permet de spécifier une plage d'adresses IP personnalisée.
Si elle est omise, Docker attribue une plage automatiquement.



```bash
docker network ls                             # lister les réseaux
docker run --net mon_reseau postgres          # rattacher un conteneur à ce réseau
docker inspect <container_id> #voir des information sur le  netwrok d'un container
```
### DNS (Docker Name Spaces)
Docker intègre un système de résolution DNS interne, permettant aux conteneurs de communiquer entre eux en utilisant leurs **noms** plutôt que leurs **adresses IP**.
   ### 🔍 Fonctionnement :
   - Docker démarre automatiquement un **serveur DNS interne** (à l'adresse `127.17.0.11`) pour résoudre les noms des conteneurs dans un réseau Docker.
   - Ce serveur DNS permet d’**associer dynamiquement un nom de conteneur à son adresse IP**.
   - Il est **recommandé d’utiliser les noms des conteneurs** plutôt que leurs adresses IP, car ces dernières peuvent changer à chaque redémarrage.
   ## ✅ Avantages :

   - 🔄 Pas besoin de reconfigurer les IP si les conteneurs redémarrent.
   - 🤝 Communication plus simple entre conteneurs dans le même réseau Docker.
   - 🧠 Meilleure lisibilité et maintenance du code ou des fichiers de configuration.

   📦 Exemple :

   | Conteneur     | Adresse IP    |
   |---------------|----------------|
   | `web`         | `172.17.0.2`   |
   | `mysql`       | `172.14.0.4`   |

   Au lieu de se connecter à `172.14.0.4`, le conteneur `web` peut simplement utiliser le nom `mysql` pour accéder à la base de données.

   ```bash
   # Exemple de connexion dans un conteneur web à MySQL
   mysql -h mysql -u root -p
   ```


-------------------------------------










## 💾 Docker Volumes & Bind Mounts

### 🔸 Volumes Docker

- Gérés par Docker
- Données persistées même après redémarrage
- Stockés dans : `/var/lib/docker/volumes/...`
- Plus **sécurisés** mais **isolés**

### 🔸 Bind Mounts

- Lient un dossier **du système hôte** au conteneur
- Modifs visibles **en temps réel**
- Plus **flexibles**, mais **moins sûrs**

### 📌 Résumé

| Type         | Sécurité | Flexibilité | Persistance |
|--------------|----------|-------------|-------------|
| Volume       | ✅ Haute | ❌ Moins    | ✅ Oui      |
| Bind Mount   | ⚠️ Moins | ✅ Haute    | ⚠️ Dépend du host |

---

## 🧪 Commandes sur Volumes

```bash
docker volume ls                             # lister les volumes
docker volume rm mon_volume                  # supprimer un volume
docker volume create my_volume               # créer un volume

docker run -v my_volume:/app my_image        # utiliser un volume Docker
docker run -d -v mydata:/var/lib/postgresql/data postgres
docker run -d --name jenkins_container   -v my_volume:/var/jenkins_home jenkins/jenkins:lts-jdk17
```

### 🧪 Exemple Bind Mount

```bash
docker run --mount type=bind,  source="C:/.../test",  destination=/app/yugi my_image
```

- ⚠️ `/app/yugi` est **masqué** par `/test`
- ✅ Ce qu’il y a dans `/test` devient visible dans le conteneur

---

---

## 🧠 À retenir sur les Bind Mounts

- `--mount type=bind,source=...,destination=...` permet de **partager un dossier local**
- Le dossier **du conteneur est masqué** pendant le run
- Tout ce qu’il y a dans le `source=` est visible dans `destination=`


## 🔌 Port Mapping

```bash
docker run -p 8080:80 -d image_name
# Le port 8080 de l’hôte redirige vers le port 80 du conteneur
```

- `-p host_port:container_port` : mapping des ports
- `-d` : mode détaché (background)

---

## 🧭 Exploration des conteneurs

```bash
docker exec -it nom_container /bin/bash          # entrer dans le conteneur
docker exec -u 0 -it nom_container /bin/bash     # entrer en root
docker logs nom_container                        # voir les logs
docker inspect container_id                      # métadonnées du conteneur
```

---

## 📦 Commandes essentielles

```bash
docker pull image_name         # télécharger une image
docker run image_name          # lancer une image
docker run -d image_name       # détaché
docker start container_id      # démarrer un conteneur
docker stop container_id       # stopper
docker rm container_id         # supprimer
docker rm -f container_id      # forcer la suppression
docker ps                      # conteneurs actifs
docker ps -a                   # tous les conteneurs
docker attach container_id     # attacher à un conteneur
```

###  UserId And volume 

- This command allows you to run the container as a specific user. 

```bash
docker run -d --name my_container -v myvolume:/data -u <user_name> or <user_id> (0 : root) my_image
```


- If you have a volume that was created or written to by user1 (e.g., root), and then you launch a container using that volume with different user (having permission  less that root), and try to create a file inside the volume, you will  get a:Permission denied This is because the current user inside the container does not have the necessary permissions to write into the volume directory that belongs to another user.







### 🧹 Nettoyage

```bash
docker rm $(docker ps -aq --filter status=exited)
```

---

## 🗂️ Image vs Conteneur

| Élément     | Description                          | Commande         |
|-------------|--------------------------------------|------------------|
| Image       | Programme figé                       | `docker images`  |
| Conteneur   | Instance en exécution                | `docker ps -a`   |

---

## 📚 Autres astuces

- `--rm` : supprime automatiquement le conteneur après son exécution
- `-e` : passer une variable d’environnement

```bash
docker run -e VAR=value image_name
```

Exemple :

```bash
docker run -e ABC=123 -e DEF=456 python:3.12   python -c "import os; print(os.environ)"
```

- keep the  container  runing : 
   exmple having just simple program the end !  


```bash
docker run --name my_container -v /data/volume:/app/user my_image  sleep infinity
```
