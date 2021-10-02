# 
#   x11docker --clipboard --home="" --share="/etc/localtime:ro" \
#             --hostdbus --name="spotify" -- -m 512m \
#             -v /var/run/user/$(id -u)/pulse/native:/run/pulse/native --rm -- ijo42/spotify
#

FROM debian:buster AS runtime
LABEL maintainer="ijo42 <admin@ijo42.ru>"
ENV PULSE_SERVER="unix:/run/pulse/native"

RUN	apt-get update && apt-get install -y \
	ca-certificates \
	curl \
	dirmngr \
	gnupg \
	--no-install-recommends \
	&& curl -sS https://download.spotify.com/debian/pubkey_0D811D58.gpg | apt-key add -  \
	&& echo "deb http://repository.spotify.com stable non-free" >> /etc/apt/sources.list.d/spotify.list \
	&& apt-get update && apt-get install -y \
	alsa-utils \
	libpulse0 \
	libsm6 \
	libx11-xcb1 \
	spotify-client \
	xdg-utils \
	--no-install-recommends \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get autoremove -y \
    && apt-get autoclean

# make search bar text better
RUN echo "QLineEdit { color: #000 }" > /opt/spotify-override.css

ENTRYPOINT	[ "spotify" ]
CMD [ "-stylesheet=/opt/spotify-override.css" ]
