FROM frolvlad/alpine-glibc:alpine-3.12

ENV PORT=34197 \
    RCON_PORT=27015 \
    VERSION=1.1.27 \
    SHA1=564df2dfd204679af6afa574d612d16b1b388453 \
    SAVES=/factorio/saves \
    CONFIG=/factorio/config \
    MODS=/factorio/mods \
    SCENARIOS=/factorio/scenarios \
    SCRIPTOUTPUT=/factorio/script-output \
    PUID="$PUID" \
    PGID="$PGID"

RUN archive="/tmp/factorio_headless_x64_$VERSION.tar.xz" \
    && mkdir -p /opt /factorio \
    && apk add --update --no-cache --no-progress bash binutils curl file gettext jq libintl pwgen shadow su-exec \
    && curl -sSL "https://www.factorio.com/get-download/$VERSION/headless/linux64" -o "$archive" \
    && echo "$SHA1  $archive" | sha1sum -c \
    || (sha1sum "$archive" && file "$archive" && exit 1) \
    && tar xf "$archive" --directory /opt \
    && rm "$archive" \
    && ln -s "$SCENARIOS" /opt/factorio/scenarios \
    && ln -s "$SAVES" /opt/factorio/saves \
    && mkdir -p /opt/factorio/config/

COPY files/*.sh /
COPY files/config.ini /opt/factorio/config/config.ini

VOLUME /factorio
EXPOSE $PORT/udp $RCON_PORT/tcp
ENTRYPOINT ["/docker-entrypoint.sh"]
