#
#   x11docker --clipboard --home="" --name="recaf" --  --  ijo42/recaf
#
FROM debian:buster AS runtimebase

RUN apt-get update && \
	apt-get install -y curl fontconfig locales libxext6 libgle3 libxrender1 libxtst6 && \
	rm -rf /var/lib/apt/lists/* && \
	echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && \
	locale-gen

ENV  LANG=en_US.UTF-8 \
     LANGUAGE=en_US:en
#	 LC_ALL=en_US.UTF-8

ARG LIBERICA_VERSION=8u332
ARG LIBERICA_BUILD=9
ARG LIBERICA_ROOT=/usr/lib/jvm/jdk-${LIBERICA_VERSION}-bellsoft
ARG LIBERICA_VARIANT=jdk
ARG LIBERICA_RELEASE_TAG=8u332
ARG LIBERICA_USE_LITE=1
ARG RSUFFIX=-full

RUN LIBERICA_ARCH='' && LIBERICA_ARCH_TAG='' && \
  case `uname -m` in \
        x86_64) \
            LIBERICA_ARCH="amd64" \
            ;; \
        i686) \
            LIBERICA_ARCH="i586" \
            ;; \
        aarch64) \
            LIBERICA_ARCH="aarch64" \
            ;; \
        armv[67]l) \
            LIBERICA_ARCH="arm32-vfp-hflt" \
            ;; \
        *) \
            LIBERICA_ARCH=`uname -m` \
            ;; \
  esac  && \
  RTAG="$LIBERICA_RELEASE_TAG" && if [ "x${RTAG}" = "x" ]; then RTAG="$LIBERICA_VERSION"; fi && \
  LITE_VERSION_EXCLUDES="" && if [ "$LIBERICA_USE_LITE" = "1" ]; then LITE_VERSION_EXCLUDES="--exclude=demo --exclude=sample --exclude=src.zip "; fi && \
  mkdir -p $LIBERICA_ROOT && \
  mkdir -p /tmp/java && \
  LIBERICA_BUILD_STR=${LIBERICA_BUILD:+"+${LIBERICA_BUILD}"} && \
  PKG=`echo "bellsoft-${LIBERICA_VARIANT}${LIBERICA_VERSION}${LIBERICA_BUILD_STR}-linux-${LIBERICA_ARCH}${RSUFFIX}.tar.gz"` && \
  curl -SL "https://download.bell-sw.com/java/${LIBERICA_VERSION}${LIBERICA_BUILD_STR}/${PKG}" -o /tmp/java/jdk.tar.gz && \
  SHA1=`curl -fSL "https://download.bell-sw.com/sha1sum/java/${LIBERICA_VERSION}${LIBERICA_BUILD_STR}" | grep ${PKG} | cut -f1 -d' '` && \
  echo "${SHA1} */tmp/java/jdk.tar.gz" | sha1sum -c - && \
  tar xzf /tmp/java/jdk.tar.gz -C /tmp/java $LITE_VERSION_EXCLUDES && \
  find "/tmp/java/${LIBERICA_VARIANT}${LIBERICA_VERSION}${RSUFFIX}" -maxdepth 1 -mindepth 1 -exec mv "{}" "${LIBERICA_ROOT}/" \; && \
  ln -s "${LIBERICA_ROOT}" /usr/lib/jvm/jdk && \
  rm -rf /tmp/java

ENV JAVA_HOME=${LIBERICA_ROOT} \
	PATH=${LIBERICA_ROOT}/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

FROM debian:buster AS dependencies

ARG RECAF_VERSION=2.21.13
ARG RECAF_URL=https://github.com/Col-E/Recaf/releases/download/${RECAF_VERSION}/recaf-${RECAF_VERSION}-J8-jar-with-dependencies.jar

WORKDIR /dependencies

ENV DEBIAN_FRONTEND=noninteractive

RUN set -xe \
    && apt-get update \
    && apt-get install -y curl

RUN set -xe \
    && curl -qsSL --create-dirs -o ./out/recaf.jar $RECAF_URL


FROM runtimebase AS runtime

LABEL maintainer="ijo42 <admin@ijo42.ru>"
ENV DEBIAN_FRONTEND=noninteractive

RUN set -xe \
    && apt-get update \
    && apt-get install -qyy libgtk2.0-0

COPY --from=dependencies /dependencies/out /opt

ENTRYPOINT [ "java", "-jar", "/opt/recaf.jar" ]
