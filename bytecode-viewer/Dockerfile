#
#   x11docker --clipboard --home="" --name="bytecode-viewer" --  --  ijo42/bytecode-viewer
#
FROM ubuntu:20.04 AS runtimebase

RUN set -xe \
	&& apt-get update \
	&& apt-get install -y openjdk-11-jre python2 python3 unzip astyle --no-install-recommends \
 	&& rm -rf /var/lib/apt/lists/* \
  	&& apt-get autoremove -y \
  	&& apt-get autoclean

FROM ubuntu:20.04 AS dependencies

ARG BCV_VERSION=2.11.2
ARG BCV_URL=https://github.com/Konloch/bytecode-viewer/releases/download/v${BCV_VERSION}/Bytecode-Viewer-${BCV_VERSION}.jar

WORKDIR /dependencies

ENV DEBIAN_FRONTEND=noninteractive

RUN set -xe \
	&& apt-get update \
	&& apt-get install -y curl

RUN set -xe \
	&& curl -qsSL --create-dirs -o ./out/bcv.jar $BCV_URL

FROM runtimebase AS runtime

LABEL maintainer="ijo42 <admin@ijo42.ru>"

COPY --from=dependencies /dependencies/out /opt

ENTRYPOINT [ "java", "-jar", "/opt/bcv.jar" ]
