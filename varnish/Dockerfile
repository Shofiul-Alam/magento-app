FROM cooptilleuls/varnish:4.1.10-alpine3.8

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

COPY varnish/varnish-template.vcl /usr/local/etc/varnish/
COPY varnish/docker-varnish-entrypoint /usr/local/bin/

RUN chmod 755 /usr/local/bin/docker-varnish-entrypoint

ENV BACKEND_HOST web
ENV BACKEND_PORT 80
ENV VARNISH_HOST web
ENV SSL_OFFLOADED_HEADER X-Forwarded-Proto
ENV GRACE_PERIOD 0
