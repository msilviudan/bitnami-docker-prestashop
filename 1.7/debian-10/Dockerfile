FROM docker.io/bitnami/minideb:buster
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux"

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages acl ca-certificates curl gzip libaudit1 libbsd0 libbz2-1.0 libc6 libcap-ng0 libcom-err2 libcurl4 libexpat1 libffi6 libfftw3-double3 libfontconfig1 libfreetype6 libgcc1 libgcrypt20 libglib2.0-0 libgmp10 libgnutls30 libgomp1 libgpg-error0 libgssapi-krb5-2 libhogweed4 libicu63 libidn2-0 libjemalloc2 libjpeg62-turbo libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 liblcms2-2 libldap-2.4-2 liblqr-1-0 libltdl7 liblzma5 libmagickcore-6.q16-6 libmagickwand-6.q16-6 libmcrypt4 libmemcached11 libmemcachedutil2 libncurses6 libnettle6 libnghttp2-14 libonig5 libp11-kit0 libpam0g libpcre3 libpng16-16 libpq5 libpsl5 libreadline7 librtmp1 libsasl2-2 libsodium23 libsqlite3-0 libssh2-1 libssl1.1 libstdc++6 libsybdb5 libtasn1-6 libtidy5deb1 libtinfo6 libunistring2 libuuid1 libwebp6 libx11-6 libxau6 libxcb1 libxdmcp6 libxext6 libxml2 libxslt1.1 libzip4 procps tar zlib1g
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "php" "7.4.27-11" --checksum 6058fe96e8d9e8c40156a0a054318ec21ed9992bd7865f72e641e18dff5d26ac
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "apache" "2.4.52-4" --checksum 4705387b2f4ef926d0bde06738df29db4abd61583cedc83d238b8b7e10e3f6b8
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "mysql-client" "10.4.24-0" --checksum 5982d7837e92b466961a02300333d746e70136ba54992431ce039a10797d8e8f
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "libphp" "7.4.27-6" --checksum bff2748eb1fb7b9ebcd40a5b660985e17cdccecb0acda003a453d84de99c61bc
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "render-template" "1.0.1-7" --checksum ed811d10882450d4755b893d5e034a0dd912cdb4f261dd332ddf6363411cc0ba
# RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "prestashop" "1.7.8-3-1" --checksum a0fc89bbcf3ae7c385209e174d57738ec95031857a74c55150af67885184a7ed
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.14.0-4" --checksum ec62006ece7e0b49c65926e458ba2b71d6d251552118b1378840b1803dbb3d53
RUN apt-get update && apt-get upgrade -y 
RUN apt-get install -y unzip
RUN rm -r /var/lib/apt/lists /var/cache/apt/archives

RUN mkdir -p /tmpd
COPY archive/* /tmpd/
RUN unzip /tmpd/prestashop_*.zip -d /tmpd
RUN mkdir -p /opt/bitnami/prestashop
RUN unzip /tmpd/prestashop.zip -d /opt/bitnami/prestashop
RUN rm -rf /tmpd

# RUN chmod g+rwX /opt/bitnami

COPY rootfs /
RUN /opt/bitnami/scripts/mysql-client/postunpack.sh
RUN /opt/bitnami/scripts/php/postunpack.sh
RUN /opt/bitnami/scripts/apache/postunpack.sh
RUN /opt/bitnami/scripts/apache-modphp/postunpack.sh
RUN /opt/bitnami/scripts/prestashop/postunpack.sh
ENV APACHE_HTTPS_PORT_NUMBER="" \
    APACHE_HTTP_PORT_NUMBER="" \
    BITNAMI_APP_NAME="prestashop" \
    BITNAMI_IMAGE_VERSION="1.7.8-3-debian-10-r17" \
    PATH="/opt/bitnami/php/bin:/opt/bitnami/php/sbin:/opt/bitnami/apache/bin:/opt/bitnami/mysql/bin:/opt/bitnami/common/bin:/opt/bitnami/prestashop/bin:$PATH"

EXPOSE 8080 8443

RUN chmod g+rwX /opt/bitnami

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/prestashop/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/apache/run.sh" ]
