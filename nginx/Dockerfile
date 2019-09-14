FROM nginx:1.15.7-alpine as magento2-nginx-base

ARG COMMIT="local-build"
ARG DATE="2019-09-01T00:00:00Z"
ARG URL="https://github.com/Shofiul-Alam/"
ARG VERSION="1.1"

LABEL org.label-schema.schema-version="1.0" \
    org.label-schema.build-date=$DATE \
    org.label-schema.vendor="Shofiul alam" \
    org.label-schema.name="shofiul/magento2" \
    org.label-schema.description="A PHP container with Magento 2" \
    org.label-schema.version="$VERSION" \
    org.label-schema.vcs-url=$URL \
    org.label-schema.vcs-ref=$COMMIT

COPY nginx/conf/default.conf.template /etc/nginx/conf.d/default.conf.template

RUN chmod 664 /etc/nginx/conf.d/default.conf.template

CMD envsubst '$$SERVER_NAME $$BACKEND_HOST' < /etc/nginx/conf.d/default.conf.template > /etc/nginx/conf.d/default.conf && exec nginx -g 'daemon off;'

FROM magento2-nginx-base

COPY --from=magento2-php-prod --chown=1000:nginx /var/www/html/index.php /var/www/html/index.php
COPY --from=magento2-php-prod --chown=1000:nginx /var/www/html/nginx.conf.sample /var/www/html/nginx.conf.sample
COPY --from=magento2-php-prod --chown=1000:nginx /var/www/html/pub/static /var/www/html/pub/static
