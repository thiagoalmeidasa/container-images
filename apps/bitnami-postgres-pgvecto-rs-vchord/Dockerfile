ARG PGVECTORS_TAG=pg16-v0.3.0-${TARGETARCH}
ARG VCHORD_TAG=pg16-v0.3.0-${TARGETARCH}

ARG VERSION

FROM tensorchord/pgvecto-rs-binary:${PGVECTORS_TAG} AS pgvecto-rs-binary
FROM tensorchord/vchord-binary:${PGVECTORS_TAG} AS vchord-binary

FROM docker.io/bitnami/postgresql:${VERSION}

COPY --from=pgvecto-rs-binary /pgvecto-rs-binary-release.deb /tmp/pgvecto-rs.deb
COPY --from=vchord-binary /workspace/postgresql-16-vchord*.deb /tmp/vchord.deb

USER root

RUN apt-get install --no-install-recommends -y /tmp/pgvecto-rs.deb && rm -f /tmp/pgvecto-rs.deb && \
      mv /usr/lib/postgresql/*/lib/vectors.so /opt/bitnami/postgresql/lib/ && \
      mv usr/share/postgresql/*/extension/vectors* opt/bitnami/postgresql/share/extension/ && \
      apt-get install --no-install-recommends -y /tmp/vchord.deb && rm -f /tmp/vchord.deb && \
      mv /usr/lib/postgresql/*/lib/vchord.so /opt/bitnami/postgresql/lib/ && \
      mv  usr/share/postgresql/*/extension/vchord* opt/bitnami/postgresql/share/extension/
USER 1001

ENV POSTGRESQL_EXTRA_FLAGS="-c shared_preload_libraries=vectors.so,vchord.so"

LABEL org.opencontainers.image.source="https://github.com/thiagoalmeidasa/container-images/tree/main/apps/bitnami-postgres-pgvecto-rs-vchord"
