
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

- Les conteneurs tournent dans des **réseaux isolés**
- On peut en connecter plusieurs au **même réseau personnalisé**

```bash
docker network ls                             # lister les réseaux
docker network create mon_reseau              # créer un réseau
docker run --net mon_reseau postgres          # rattacher un conteneur à ce réseau
```




# make Nework command  here   



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
