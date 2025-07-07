# Dockerisation

## 🚀 Installation de Docker (Ubuntu / Debian)

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

