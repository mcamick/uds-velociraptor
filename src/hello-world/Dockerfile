FROM registry1.dso.mil/ironbank/opensource/nginx/nginx:1.27.5

USER root

COPY index.html /etc/nginx/html/index.html

RUN chown -R nginx:nginx /etc/nginx/ && \
    chown -R nginx:nginx /var/cache/nginx/

USER nginx
