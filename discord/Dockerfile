#
# Based on:     davidk/docker-discord
# 
#   x11docker --clipboard --home="" --share="/etc/localtime:ro" --share="/usr/share/fonts:ro" \
#             --hostdbus --name="discord" --hostipc -- \
#             --privileged -v /var/run/user/$(id -u)/pulse/native:/run/pulse/native --rm -m 512m --  ijo42/discord
#

FROM debian:buster AS runtime

RUN apt-get update && apt-get install -qqy \
  libc++1 \
  libappindicator1 \
  gconf2 \
  gconf-service \
  gvfs-bin \
  libasound2 \
  libcap2 \
  libgconf-2-4 \
  libgtk-3-0 \
  libcanberra-gtk3-module \
  libpulse0 \
  libgbm1 \
  libnotify4 \
  libnss3 \
  libxkbfile1 \
  libxss1 \
  libxtst6 \
  libx11-xcb1 \
  libxv1 \
  va-driver-all \
  xdg-utils \
  libatomic1 \
  libgl1 \
  --no-install-recommends \
  && rm -rf /var/lib/apt/lists/* \
  && apt-get autoremove -y \
  && apt-get autoclean

# DOWNLOAD_LINK
# Canary URL: https://discord.com/api/download/ptb?platform=linux&format=deb
# Stable URL: https://discord.com/api/download?platform=linux&format=deb
#
# Deb URL: curl -sSI 'https://discord.com/api/download/ptb?platform=linux&format=deb' | grep -F 'location: ' | awk '{print $2}' | tr -d '[:space:]'
# Version: curl -sSI 'https://discord.com/api/download?platform=linux&format=deb' | grep -i 'location: ' | awk '{print $2}' | tr -d '[:space:]' | sed -e 's/.*discord-//g' | sed -e 's/.deb//g'

ENV PULSE_SERVER="unix:/run/pulse/native"
ARG DOWNLOAD_LINK="https://discord.com/api/download/ptb?platform=linux&format=deb"

RUN apt-get update && apt-get install -qqy \
  curl \
  ca-certificates \
  --no-install-recommends \
  && curl -o discord.deb -sSL "${DOWNLOAD_LINK}" \ 
  && dpkg -i discord.deb \
  && rm -rf /var/lib/apt/lists/* discord.deb \
  && apt-get purge -y --auto-remove curl \
  && apt-get autoclean

ENTRYPOINT [ "discord-ptb" ]
