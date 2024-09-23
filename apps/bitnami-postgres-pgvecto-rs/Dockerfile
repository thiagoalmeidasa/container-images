ARG PGVECTORS_TAG=pg16-v0.3.0-${TARGETARCH}

ARG BITNAMI_TAG=16.4.0-debian-12-r11


FROM scratch as nothing

FROM tensorchord/pgvecto-rs-binary:${PGVECTORS_TAG} as binary

FROM docker.io/bitnami/postgresql:${BITNAMI_TAG}

COPY --from=binary /pgvecto-rs-binary-release.deb /tmp/vectors.deb

USER root

RUN apt-get install --no-install-recommends -y /tmp/vectors.deb && rm -f /tmp/vectors.deb && \
      mv /usr/lib/postgresql/*/lib/vectors.so /opt/bitnami/postgresql/lib/ && \
      mv usr/share/postgresql/*/extension/vectors* opt/bitnami/postgresql/share/extension/
USER 1001

ENV POSTGRESQL_EXTRA_FLAGS="-c shared_preload_libraries=vectors.so"

LABEL org.opencontainers.image.source="https://github.com/bitnami/containers/tree/main/bitnami/postgresql"
