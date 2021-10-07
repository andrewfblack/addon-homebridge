ARG BUILD_FROM=hassioaddons/base:2.1.2
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Setup base
RUN apk add --no-cache git python2 python3 make g++ avahi-compat-libdns_sd avahi-dev dbus \
    iputils sudo nano \
  && chmod 4755 /bin/ping \
  && npm set global-style=true \
  && npm set audit=false \ 
  && npm set fund=false

RUN case "$(uname -m)" in \
    x86_64) FFMPEG_ARCH='x86_64';; \
    armv6l) FFMPEG_ARCH='armv6l';; \
    armv7l) FFMPEG_ARCH='armv6l';; \
    aarch64) FFMPEG_ARCH='aarch64';; \
    *) echo "unsupported architecture"; exit 1 ;; \
    esac \
    && set -x \
    && curl -Lfs https://github.com/oznu/ffmpeg-for-homebridge/releases/download/v0.0.9/ffmpeg-alpine-${FFMPEG_ARCH}.tar.gz | tar xzf - -C / --no-same-owner

ENV PATH="${PATH}:/homebridge/node_modules/.bin"

ENV HOMEBRIDGE_VERSION=1.3.4
RUN npm install -g --unsafe-perm homebridge@${HOMEBRIDGE_VERSION}

ENV CONFIG_UI_VERSION=4.41.2 HOMEBRIDGE_CONFIG_UI=1 HOMEBRIDGE_CONFIG_UI_PORT=8581
RUN npm install -g --unsafe-perm homebridge-config-ui-x@${CONFIG_UI_VERSION}

COPY root /

ARG AVAHI
ENV ENABLE_AVAHI="${AVAHI:-0}"

# Labels
LABEL \
    io.hass.name="Homebridge" \
    io.hass.description="Homebridge Addon" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Andrew Black <andrewfblack@addons.community>" \
    org.label-schema.description="Homebridge Addon" \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="Homebridge" \
    org.label-schema.schema-version="1.0" \
    org.label-schema.url="https://community.home-assistant.io/t/community-hass-io-add-on-homebridge/33803?u=frenck" \
    org.label-schema.usage="https://github.com/hassio-addons/addon-homebridge/tree/master/README.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://github.com/andrewfblack/addon-homebridge" \
    org.label-schema.vendor="Community Hass.io Add-ons"
