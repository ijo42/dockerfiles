FROM alpine:3.14
LABEL maintainer="ijo42 <admin@ijo42.ru>"

ARG GLIBC_REPO=https://github.com/sgerrand/alpine-pkg-glibc
ARG GLIBC_VERSION=2.34-r0
ARG OSIGN_REPO=https://github.com/mtrojnar/osslsigncode
ENV LANG='en_US.UTF-8' \
    LANGUAGE='en_US.UTF-8'

RUN buildDeps=' \
        curl \
		curl-dev \
		unzip \
		automake \
		autoconf \
		libressl-dev \
		autoconf \
		g++ \
		make \
		git \
		libtool \
	' \
	set -xe && \
    apk add --no-cache --virtual .build-deps \
		$buildDeps && apk add --no-cache libstdc++ ca-certificates libcurl && \
    echo "export LANG=C.UTF-8" > /etc/profile.d/locale.sh && \
    cd /tmp && curl -SsLo /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub && \
    for pkg in glibc glibc-bin glibc-i18n ; do \
        curl -SsLo ${pkg}-${GLIBC_VERSION}.apk ${GLIBC_REPO}/releases/download/${GLIBC_VERSION}/${pkg}-${GLIBC_VERSION}.apk && \
        apk add --no-cache ${pkg}-${GLIBC_VERSION}.apk && rm ${pkg}-${GLIBC_VERSION}.apk ; \
    done && \
    /usr/glibc-compat/bin/localedef -i en_US -f UTF-8 en_US.UTF-8 && \
    mkdir /tmp/osslsigncode && cd /tmp/osslsigncode && PATH=/usr/glibc-compat/sbin:/usr/glibc-compat/bin:$PATH && \
    git clone ${OSIGN_REPO} osslsigncode-master && \
    cd osslsigncode-master && ./bootstrap && ./configure && make && make install && make clean && \
    apk del .build-deps && rm -rf /tmp/* /usr/share/doc /usr/share/man

ENTRYPOINT ["osslsigncode"]
CMD ["-v"]
