FROM alpine

RUN echo 'syncthing:x:383:381::/var/syncthing:/sbin/nologin' >> /etc/passwd \
    && echo 'syncthing:!::0:::::' >> /etc/shadow \
    && mkdir /var/syncthing \
    && chown syncthing /var/syncthing

ENV release=

RUN apk add --no-cache --virtual .deps \
         curl \
         gnupg \
         jq \
    && apk add --no-cache \
         ca-certificates \
    && gpg --keyserver keyserver.ubuntu.com --recv-key D26E6ED000654A3E \
    && set -x \
    && mkdir /syncthing \
    && cd /syncthing \
    && release=${release:-$(curl -s https://api.github.com/repos/syncthing/syncthing/releases/latest | jq -r .tag_name )} \
    && curl -sLO https://github.com/syncthing/syncthing/releases/download/${release}/syncthing-linux-amd64-${release}.tar.gz \
    && curl -sLO https://github.com/syncthing/syncthing/releases/download/${release}/sha256sum.txt.asc \
    && gpg --verify sha256sum.txt.asc \
    && grep syncthing-linux-amd64 sha256sum.txt.asc | sha256sum \
    && tar -zxf syncthing-linux-amd64-${release}.tar.gz \
    && mv syncthing-linux-amd64-${release}/syncthing . \
    && rm -rf syncthing-linux-amd64-${release} sha256sum.txt.asc syncthing-linux-amd64-${release}.tar.gz \
    && apk del .deps

USER syncthing
ENV STNOUPGRADE=1

HEALTHCHECK --interval=1m --timeout=10s \
  CMD nc -z localhost 22000 || exit 1

ENTRYPOINT ["/syncthing/syncthing", "-home", "/var/syncthing/config", "-gui-address", "0.0.0.0:8384"]
