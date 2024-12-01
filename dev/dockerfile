# Utiliser une image de base nginx pour héberger une application statique
FROM nginx:latest
# Détail : `nginx:latest` est une image officielle de NGINX pour servir des fichiers     #  statiques comme HTML, CSS, JS.
# Copier les fichiers de l'application dans le dossier HTML de nginx
COPY index.html /usr/share/nginx/html/
# Détail : Copie `index.html` dans le répertoire par défaut de NGINX pour les             # fichiers web.
COPY style.css /usr/share/nginx/html/
# Détail : Copie `style.css` dans le même répertoire pour inclure le style.
# Exposer le port 80 pour l'application
EXPOSE 80
# Détail : Indique que l’application utilise le port 80 pour la communication HTTP.

# Commande pour lancer le serveur nginx
CMD ["nginx", "-g", "daemon off;"]
# Détail : Définit la commande de démarrage du conteneur. `daemon off;`                  # empêche NGINX de tourner en arrière-plan, ce qui est nécessaire pour Docker.
