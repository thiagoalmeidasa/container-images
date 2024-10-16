ARG PGVECTORS_TAG=pg16-v0.3.0-${TARGETARCH}

ARG VERSION=16.4.0-debian-12-r16

FROM tensorchord/pgvecto-rs-binary:${PGVECTORS_TAG} AS binary

FROM docker.io/bitnami/postgresql:${VERSION}

COPY --from=binary /pgvecto-rs-binary-release.deb /tmp/vectors.deb

USER root

RUN apt-get install --no-install-recommends -y /tmp/vectors.deb && rm -f /tmp/vectors.deb && \
      mv /usr/lib/postgresql/*/lib/vectors.so /opt/bitnami/postgresql/lib/ && \
      mv usr/share/postgresql/*/extension/vectors* opt/bitnami/postgresql/share/extension/
USER 1001

ENV POSTGRESQL_EXTRA_FLAGS="-c shared_preload_libraries=vectors.so"

LABEL org.opencontainers.image.source="https://github.com/thiagoalmeidasa/container-images/tree/main/apps/bitnami-postgres-pgvecto-rs"
