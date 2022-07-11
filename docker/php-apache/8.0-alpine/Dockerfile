#+++++++++++++++++++++++++++++++++++++++
# Dockerfile for webdevops/php-apache:8.0-alpine
#    -- automatically generated  --
#+++++++++++++++++++++++++++++++++++++++

FROM webdevops/php:8.0-alpine

ENV WEB_DOCUMENT_ROOT=/app \
    WEB_DOCUMENT_INDEX=index.php \
    WEB_ALIAS_DOMAIN=*.vm \
    WEB_PHP_TIMEOUT=600 \
    WEB_PHP_SOCKET=""
ENV WEB_PHP_SOCKET=127.0.0.1:9000

COPY conf/ /opt/docker/

RUN set -x \
    # Install apache
    && apk-install \
        apache2 \
        apache2-ctl \
        apache2-utils \
        apache2-proxy \
        apache2-ssl \
    # Fix issue with module loading order of lbmethod_* (see https://serverfault.com/questions/922573/apache2-fails-to-start-after-recent-update-to-2-4-34-no-clue-why)
    && sed -i '2,5{H;d}; ${p;x;s/^\n//}' /etc/apache2/conf.d/proxy.conf \
    && sed -ri ' \
        s!^(\s*CustomLog)\s+\S+!\1 /proc/self/fd/1!g; \
        s!^(\s*ErrorLog)\s+\S+!\1 /proc/self/fd/2!g; \
        ' /etc/apache2/httpd.conf \
    && docker-run-bootstrap \
    && docker-image-cleanup

## Replace go-binaries with the correct ones for the target architecture
ARG TARGETPLATFORM
RUN if [ "$TARGETPLATFORM" = "linux/arm64" ]; then GO_REPLACE="gr-arm64-linux"; else GO_REPLACE="gr-64-linux"; fi \
    && wget -O "/usr/local/bin/go-replace" "https://github.com/webdevops/goreplace/releases/download/1.1.2/${GO_REPLACE}" \
    && chmod +x "/usr/local/bin/go-replace" \
    # Install gosu
    && if [ "$TARGETPLATFORM" = "linux/arm64" ]; then ARCH="arm64"; else ARCH="amd64"; fi \
    && wget -O "/sbin/gosu" "https://github.com/tianon/gosu/releases/download/1.10/gosu-${ARCH}" \
    && wget -O "/tmp/gosu.asc" "https://github.com/tianon/gosu/releases/download/1.10/gosu-${ARCH}.asc"

EXPOSE 80 443
