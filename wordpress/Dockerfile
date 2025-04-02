FROM wordpress:latest

# Installation de paquets supplémentaires si nécessaire
RUN apt-get update && apt-get install -y \
    vim \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Définition du répertoire de travail
WORKDIR /var/www/html

# Exposer le port utilisé par WordPress
EXPOSE 80