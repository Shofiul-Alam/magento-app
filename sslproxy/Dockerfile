FROM nginx:1.15.7-alpine

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

COPY sslproxy/conf/default.conf.template /etc/nginx/conf.d/default.conf.template
COPY sslproxy/conf/ssl-dhparams.pem /etc/nginx/ssl/ssl-dhparams.pem

RUN chmod 664 /etc/nginx/conf.d/default.conf.template

CMD envsubst '$$SERVER_NAME $$BACKEND_HOST' < /etc/nginx/conf.d/default.conf.template > /etc/nginx/conf.d/default.conf && exec nginx -g 'daemon off;'