#+++++++++++++++++++++++++++++++++++++++
# Dockerfile for webdevops/php-nginx:8.0
#    -- automatically generated  --
#+++++++++++++++++++++++++++++++++++++++

FROM webdevops/php:8.0

ENV WEB_DOCUMENT_ROOT=/app \
    WEB_DOCUMENT_INDEX=index.php \
    WEB_ALIAS_DOMAIN=*.vm \
    WEB_PHP_TIMEOUT=600 \
    WEB_PHP_SOCKET=""
ENV WEB_PHP_SOCKET=127.0.0.1:9000
ENV SERVICE_NGINX_CLIENT_MAX_BODY_SIZE="50m"

COPY conf/ /opt/docker/

RUN set -x \
    # Install nginx
    && printf "deb http://nginx.org/packages/debian/ $(docker-image-info dist-codename)  nginx\n deb-src http://nginx.org/packages/debian/ $(docker-image-info dist-codename) nginx" \
    >> /etc/apt/sources.list \
    && curl -L https://nginx.org/keys/nginx_signing.key | apt-key add - \
    && apt-install \
    nginx \
    && docker-run-bootstrap \
    && docker-image-cleanup
 
## Replace go-binaries with the correct ones for the target architecture
ARG TARGETPLATFORM
RUN if [ "$TARGETPLATFORM" = "linux/arm64" ]; then GO_REPLACE="gr-arm64-linux"; else GO_REPLACE="gr-64-linux"; fi \
    && wget -O "/usr/local/bin/go-replace" "https://github.com/webdevops/goreplace/releases/download/1.1.2/${GO_REPLACE}" \
    && chmod +x "/usr/local/bin/go-replace" \
    && "/usr/local/bin/go-replace" --version \
    # Install gosu
    && if [ "$TARGETPLATFORM" = "linux/arm64" ]; then ARCH="arm64"; else ARCH="amd64"; fi \
    && wget -O "/sbin/gosu" "https://github.com/tianon/gosu/releases/download/1.10/gosu-${ARCH}" \
    && wget -O "/tmp/gosu.asc" "https://github.com/tianon/gosu/releases/download/1.10/gosu-${ARCH}.asc"

EXPOSE 80 443
