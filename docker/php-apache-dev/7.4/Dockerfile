#+++++++++++++++++++++++++++++++++++++++
# Dockerfile for webdevops/php-apache-dev:7.4
#    -- automatically generated  --
#+++++++++++++++++++++++++++++++++++++++

FROM webdevops/php-apache:7.4

ENV WEB_DOCUMENT_ROOT=/app \
    WEB_DOCUMENT_INDEX=index.php \
    WEB_ALIAS_DOMAIN=*.vm \
    WEB_PHP_TIMEOUT=600 \
    WEB_PHP_SOCKET=""
ENV WEB_PHP_SOCKET=127.0.0.1:9000
ENV WEB_NO_CACHE_PATTERN="\.(css|js|gif|png|jpg|svg|json|xml)$"

COPY conf/ /opt/docker/

RUN set -x \
    # Install development environment
    && wget -q -O - https://packages.blackfire.io/gpg.key | apt-key add - \
    && echo "deb https://packages.blackfire.io/debian any main" | tee /etc/apt/sources.list.d/blackfire.list \
    && apt-install \
        blackfire-php \
        blackfire-agent \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    # Enable php development services
    && docker-service enable syslog \
    && docker-service enable postfix \
    && docker-service enable ssh \
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
