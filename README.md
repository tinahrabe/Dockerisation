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








