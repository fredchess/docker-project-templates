# =========================
# 🌐 Nginx optimisé avec assets statiques
# =========================
# Cette image copie les assets depuis l'image Laravel déjà buildée
# pour éviter le problème de persistence des volumes Docker
FROM ghcr.io/${GITHUB_REPOSITORY:-digipasscmr/digipass-crm}:latest AS assets

# Image finale Nginx ultra-légère
FROM nginx:stable-alpine

# Copier la configuration Nginx personnalisée
COPY ./docker/nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf

# Créer le répertoire pour les fichiers Laravel
RUN mkdir -p /var/www/public

# Copier UNIQUEMENT le dossier public depuis l'image Laravel
# Cela inclut les assets buildés (/var/www/public/build)
COPY --from=assets /var/www/public /var/www/public

# Optimisation: supprimer les fichiers PHP du dossier public de Nginx
# (ils seront servis par php-fpm, pas Nginx)
RUN find /var/www/public -type f -name "*.php" -delete \
    && chown -R nginx:nginx /var/www/public

# Pas d'exposition de port (géré par Traefik dans Dokploy)
CMD ["nginx", "-g", "daemon off;"]
