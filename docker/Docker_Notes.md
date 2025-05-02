
# 📘 Notes techniques : Docker, Réseaux, Volumes & Wireshark

---

## 📡 Wireshark

- Permet de **surveiller toutes les requêtes sortantes** du système.

---

## 🔄 Système d'exploitation

- Prévoir un **changement ou une réinstallation** selon les besoins techniques.

---

## 📅 Tâches à faire

- Contacter **Ayoub** pour avoir **l'accès au JIRA de la Tech Société**  
  ⏳ Prochaine invitation : **13 mai**

---

## 🐳 Dockerfile

```Dockerfile
FROM <define work environment>
LABEL version="1.1.0" # metadata visible via Docker inspect
ENV FLASK_APP=app.py  # defines environment variable
```

```bash
docker build -t my_app <docker_image_folder>  # build and tag image
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

---

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

---

## 🧠 À retenir sur les Bind Mounts

- `--mount type=bind,source=...,destination=...` permet de **partager un dossier local**
- Le dossier **du conteneur est masqué** pendant le run
- Tout ce qu’il y a dans le `source=` est visible dans `destination=`
