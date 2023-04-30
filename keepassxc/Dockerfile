#
#	x11docker --network=none --clipboard --home= --hostdisplay --share /usr/share/X11/xkb/:ro \
#				--share /etc/machine-id:ro --name=keepassxc ijo42/keepassxc
#
FROM alpine:3.17 AS runtime

LABEL maintainer="ijo42 <admin@ijo42.ru>"

ARG KEEPASSXC_VERSION=develop
ARG KEEPASSXC_NETWORKING=ON
ARG KEEPASSXC_TESTS=ON
ARG KEEPASSXC_AUTOTYPE=ON

RUN buildDeps=' \
		automake \
		argon2-dev \
		bash \
		asciidoctor \
		cmake \
		curl-dev \
		expat \
		g++ \
		gcc \
		git \
		libgcrypt-dev \
		libmicrohttpd-dev \
		libqrencode-dev	\
		libsodium-dev \
		make \
		qt5-qtbase-dev \
		qt5-qtsvg-dev \
		qt5-qttools-dev \
		qt5-qtx11extras-dev \
		botan-dev \
		alpine-conf \
        libx11 \
        libxtst-dev \
        libxi-dev \
	' \
	set -x \
	&& apk add --no-cache --virtual .build-deps \
		$buildDeps \
	&& git clone --depth 1 --branch ${KEEPASSXC_VERSION} https://github.com/keepassxreboot/keepassxc.git /usr/src/keepassxc \
	&& cd /usr/src/keepassxc \
	&& mkdir build \
	&& cd build \
	&& cmake -DWITH_TESTS=${KEEPASSXC_TESTS} -DWITH_XC_AUTOTYPE=${KEEPASSXC_AUTOTYPE} -DWITH_XC_NETWORKING=${KEEPASSXC_NETWORKING} .. \
	&& make \
	&& make install \
	&& setup-timezone -z UTC \
	&& apk del .build-deps \
	&& rm -rf /usr/src/keepassxc \
	&& echo "Build complete."

RUN	apk --no-cache add \
		argon2-libs \
		libcurl \
		libmicrohttpd \
		libgcrypt \
		libqrencode \
		libsodium \
		mesa \
		qt5-qtbase \
		qt5-qtbase-x11 \
		qt5-qtsvg \
		qt5-qttools \
		qt5-qtx11extras \
		botan \
		ttf-dejavu

ENTRYPOINT [ "keepassxc" ]
