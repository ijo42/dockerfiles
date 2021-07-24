FROM        --platform=$BUILDPLATFORM adoptopenjdk/openjdk8-openj9:alpine-jre

LABEL       author="ijo42 <admin@ijo42.ru>"

ARG         S6_OVERLAY_RELEASE=https://github.com/just-containers/s6-overlay/releases/latest/download/s6-overlay-amd64.tar.gz
ENV         PS1="$(whoami)@$(hostname):$(pwd)\\$ " \
                HOME="/root" \
                TERM="xterm"

ADD         ${S6_OVERLAY_RELEASE} /tmp/s6overlay.tar.gz

RUN         apk upgrade --update --no-cache && apk add --update --no-cache ca-certificates curl \
                fontconfig git openssl sqlite tar tzdata bash coreutils procps shadow \
            && tar xzf /tmp/s6overlay.tar.gz -C / \
            && rm /tmp/s6overlay.tar.gz \
            && groupmod -g 1000 users \
            && useradd -u 911 -U -d /config -s /bin/false abc \
            && usermod -G users abc \
            && mkdir -p \
                    /app \
                    /config \
                    /defaults \
            && mv /usr/bin/with-contenv /usr/bin/with-contenvb

COPY        root /
WORKDIR     /app/server

EXPOSE      25565
ENTRYPOINT  [ "/init" ]
CMD         ["s6-setuidgid", "abc", "ash", "start.sh"]
