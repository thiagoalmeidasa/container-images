ARG VERSION

FROM DOCKER_IMAGE=manjarolinux/base:${VERSION}

ARG BUILD_DATE
ARG VCS_REF

LABEL maintainer="Thiago Almeida <thiagoalmeidasa@gmail.com>"
LABEL author="Thiago Almeida <thiagoalmeidasa@gmail.com>"
LABEL description="A distccd image"

ENV LANG=en_US.utf8

RUN pacman-key --init && pacman -Sy archlinux-keyring --noconfirm && pacman -Syu --noconfirm && \
  pacman -S --noconfirm \
  clang \
  ccache \
  base-devel \
  bash \
  distcc \
  && pacman -Scc --noconfirm

ENTRYPOINT [ "distccd", \
  "--daemon", "--no-detach", \
  "--user", "distcc", \
  "--port", "3632", \
  "--stats", \
  "--stats-port", "3633", \
  "--log-level", "info", \
  "--log-stderr", \
  "--listen", "0.0.0.0", \
  "--allow", "0.0.0.0/0", \
  "--nice", "4" \
]

EXPOSE 3632/tcp 3633/tcp

HEALTHCHECK --interval=5m --timeout=3s \
  CMD curl -f http://0.0.0.0:3633/ || exit 1
