ARG REF \
    REPOSITORY="peschi90/darts-caller"

FROM --platform=${BUILDPLATFORM} alpine:latest AS build

ARG REF \
    REPOSITORY \
    TARGETPLATFORM

WORKDIR /
RUN apk update && \
    apk add wget tar && \
    case ${TARGETPLATFORM} in \
        "linux/amd64")  DOWNLOAD_ARCH=""  ;; \
        "linux/arm64") DOWNLOAD_ARCH="-arm64"  ;; \
    esac && \
    ASSETNAME="darts-caller$DOWNLOAD_ARCH" && \
    wget "https://github.com/${REPOSITORY}/releases/download/${REF}/$ASSETNAME" && \
    mv $ASSETNAME darts-caller

FROM debian:bookworm-slim
ARG TARGETPLATFORM
ENV AUTODARTS_EMAIL= \
    AUTODARTS_PASSWORD= \
    AUTODARTS_BOARD_ID= \
    MEDIA_PATH='/usr/share/darts-caller/media' \
    MEDIA_PATH_SHARED='/usr/share/darts-caller/media-shared' \
    CALLER_VOLUME= \
    CALLER= \
    RANDOM_CALLER= \
    RANDOM_CALLER_LANGUAGE= \
    RANDOM_CALLER_GENDER= \
    CALL_CURRENT_PLAYER= \
    CALL_BOT_ACTIONS= \
    CALL_EVERY_DART= \
    CALL_EVERY_DART_TOTAL_SCORE= \
    POSSIBLE_CHECKOUT_CALL= \
    POSSIBLE_CHECKOUT_CALL_YOURSELF_ONLY= \
    AMBIENT_SOUNDS= \
    AMBIENT_SOUNDS_AFTER_CALLS= \
    DOWNLOADS= \
    DOWNLOADS_LANGUAGE= \
    DOWNLOADS_NAME= \
    REMOVE_OLD_VOICE_PACKS= \
    BACKGROUND_AUDIO_VOLUME= \
    LOCAL_PLAYBACK= \
    WEB_CALLER_DISABLE_HTTPS= \
    HOST_PORT=8079\
    DEBUG= \
    CERT_CHECK= \
    MIXER_FREQUENCY= \
    MIXER_SIZE= \
    MIXER_CHANNELS= \
    MIXER_BUFFERSIZE=

WORKDIR /usr/bin/darts-caller
COPY --from=build /darts-caller* ./darts-caller
COPY ./entrypoint.sh ./entrypoint.sh
RUN chmod +x darts-caller && \
    chmod +x entrypoint.sh && \
    apt update && \
    apt install -y libglib2.0-0 alsa-utils && \
    rm -rf /var/lib/apt/lists/*

EXPOSE 8079

CMD ./entrypoint.sh