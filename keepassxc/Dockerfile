#
#	x11docker --network=none --clipboard --home= --hostdisplay --share /usr/share/X11/xkb/:ro \
#				--share /etc/machine-id:ro --name=keepassxc ijo42/keepassxc
#
FROM alpine:3.13

LABEL maintainer "ijo42 <admin@ijo42.ru>"

ARG KEEPASSXC_VERSION "master"

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
	' \
	set -x \
	&& apk add --no-cache --virtual .build-deps \
		$buildDeps \
	&& git clone --depth 1 --branch ${KEEPASSXC_VERSION} https://github.com/keepassxreboot/keepassxc.git /usr/src/keepassxc \
	&& cd /usr/src/keepassxc \
	&& mkdir build \
	&& cd build \
	&& cmake -DWITH_TESTS=ON -DWITH_XC_AUTOTYPE=ON -DWITH_XC_HTTP=ON .. \
	&& make \
	&& make install \
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
		mesa-dri-intel \
		qt5-qtbase \
		qt5-qtbase-x11 \
		qt5-qtsvg \
		qt5-qttools \
		ttf-dejavu

ENTRYPOINT [ "/usr/local/bin/keepassxc" ]
