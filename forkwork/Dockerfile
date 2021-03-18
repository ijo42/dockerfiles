FROM alpine:3.13

LABEL maintainer="ijo42 <admin@ijo42.ru>"

ARG BUILD_REPO=github-tooling/forkwork
ENV PYTHONUNBUFFERED=1 \
    FORKWORK_TOKEN=

RUN apk add --no-cache --virtual .build-deps python3 git gcc python3-dev musl-dev libffi-dev rust cargo openssl-dev \
    && python3 -m ensurepip \
    && pip3 install --no-cache --upgrade pip setuptools \
    && pip install git+https://github.com/${BUILD_REPO} \
    && pip uninstall pip -y \
    && apk del .build-deps && apk add --no-cache python3 openssl \
    && echo "Etc/UTC" >> /etc/timezone

ENTRYPOINT [ "forkwork" ]
