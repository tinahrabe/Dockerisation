# RABEMANANJARA Handinirina Tinah Gillio 
#L1 B N° 210
# Dockerisation

## Installation de Docker (Ubuntu / Debian)

```bash
# Mettre à jour les paquets
sudo apt update

# Installer les dépendances nécessaires
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# Ajouter la clé GPG officielle de Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Ajouter le dépôt Docker à APT
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Mettre à jour le cache des paquets
sudo apt update

# Installer Docker
sudo apt install docker-ce

# Vérifier l'installation
docker --version
```

## Installation de Docker Desktop sur Windows

1.  Télécharger Docker Desktop :
    [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

2.  Lancer le fichier `.exe` téléchargé et suivre les étapes d'installation.

3.  Activer la **virtualisation** dans le BIOS si ce n'est pas déjà fait (nécessaire pour WSL2).

4.  Vérifier que **WSL2** est installé (Docker Desktop l'utilise par défaut) :

```powershell
wsl --install
```

5.  Redémarrer votre machine si demandé.

6.  Une fois Docker Desktop installé et lancé, ouvrez un terminal (PowerShell ou CMD) et tapez :

```powershell
docker --version
```

Tu devrais voir une sortie comme :

```bash
Docker version 24.x.x, build xxxxxxx
```


## Commandes Docker pour gérer les images

### Télécharger (pull) une image Docker depuis Docker Hub

```bash
docker pull nginx
```

### Lister les images Docker présentes localement

```bash
docker images
```

### Construire une image à partir d'un Dockerfile

```bash
docker build -t mon-image .
```


### Lancer un conteneur à partir de l’image

```bash
docker run -d -p 8080:80 mon-image
```

### Supprimer une image Docker

```bash
docker rmi mon-image
```

### Supprimer toutes les images inutilisées

```bash
docker image prune
```

##  Commandes Docker : Conteneurs

###  Lancer un conteneur

```bash
docker run -d --name mon-conteneur -p 8080:80 nginx
```

- `-d` : détaché (en arrière-plan)
- `--name` : nom du conteneur
- `-p 8080:80` : associer le port 80 du conteneur au port 8080 de la machine hôte

---

### Lister les conteneurs

- Conteneurs actifs :

```bash
docker ps
```

- Tous les conteneurs (actifs et stoppés) :

```bash
docker ps -a
```

---

### Arrêter un conteneur

```bash
docker stop mon-conteneur
```

---

### Redémarrer un conteneur

```bash
docker start mon-conteneur
```

---

### Recréer un conteneur (si supprimé)

```bash
docker run -d --name mon-conteneur nginx
```

---

### Supprimer un conteneur

```bash
docker rm mon-conteneur
```

---

### Supprimer tous les conteneurs arrêtés

```bash
docker container prune
```
## 🛠️ Exemple de Dockerfile (Node.js App)

Voici un exemple de `Dockerfile` minimal pour une application Node.js :

```Dockerfile
# Utilise une image officielle Node.js comme base
FROM node:18

# Crée un répertoire de travail dans le conteneur
WORKDIR /app

# Copie les fichiers package.json et package-lock.json
COPY package*.json ./

# Installe les dépendances de l'application
RUN npm install

# Copie tout le reste des fichiers dans le conteneur
COPY . .

# Expose le port sur lequel l'app va tourner
EXPOSE 3000

# Commande pour démarrer l'application
CMD ["node", "index.js"]
```

---

### Construire l’image Docker

```bash
docker build -t mon-app-node .
```

### Lancer un conteneur basé sur cette image

```bash
docker run -d -p 3000:3000 --name conteneur-node mon-app-node
```
## Exemple de `docker-compose.yml`

Voici un exemple de configuration Docker Compose pour une application **Node.js + MongoDB** :

```yaml
version: '3.8'

services:
  app:
    build: .
    container_name: mon-app
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    depends_on:
      - mongo

  mongo:
    image: mongo:6
    container_name: mongo-db
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

---

## Commandes utiles Docker Compose

### Lancer les services (build + up)

```bash
docker-compose up --build -d
```

### Arrêter les services

```bash
docker-compose down
```

### Rebuild uniquement (sans démarrer)

```bash
docker-compose build
```

### Voir les logs

```bash
docker-compose logs -f
```

---

## Structure de projet recommandée

```
mon-projet/
├── Dockerfile
├── docker-compose.yml
├── package.json
├── index.js
└── ...
```

---



## Docker Volume : Stockage persistant

### Créer un volume Docker

```bash
docker volume create mon_volume
```

---

### Lister tous les volumes

```bash
docker volume ls
```

---

### Supprimer un volume

```bash
docker volume rm mon_volume
```



---

### Utiliser un volume avec un conteneur

```bash
docker run -d \
  --name conteneur-avec-volume \
  -v mon_volume:/app/data \
  nginx
```

Cela monte le volume `mon_volume` dans le dossier `/app/data` du conteneur.

---

### Exemple avec Docker Compose

```yaml
version: '3.8'

services:
  web:
    image: nginx
    volumes:
      - mon_volume:/usr/share/nginx/html

volumes:
  mon_volume:
```

### 🧹 Supprimer tous les volumes non utilisés

```bash
docker volume prune
```

---



## Docker Network : Communication entre conteneurs

### Créer un réseau personnalisé

```bash
docker network create mon_reseau
```



### Lister tous les réseaux

```bash
docker network ls
```



### Lancer deux conteneurs sur le même réseau

```bash
docker run -d --name web --network mon_reseau nginx
docker run -d --name app --network mon_reseau node
```



---

### Se connecter manuellement à un réseau existant

```bash
docker network connect mon_reseau conteneur_existant
```

---

### Se déconnecter d’un réseau

```bash
docker network disconnect mon_reseau conteneur_existant
```



### Supprimer un réseau

```bash
docker network rm mon_reseau
```



## Exemple avec Docker Compose

```yaml
version: '3.8'

services:
  web:
    image: nginx
    networks:
      - mon_reseau

  app:
    image: node
    networks:
      - mon_reseau

networks:
  mon_reseau:
```















