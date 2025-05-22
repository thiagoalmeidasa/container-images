ARG PGVECTOR_TAG=0.8.0-pg17
ARG VCHORD_TAG=pg17-v0.3.0-${TARGETARCH}

ARG VERSION

FROM tensorchord/vchord-binary:${VCHORD_TAG} AS vchord-binary
FROM pgvector/pgvector:${PGVECTOR_TAG} AS pgvector

FROM docker.io/bitnami/postgresql:${VERSION}

COPY --from=vchord-binary /workspace/postgresql-17-vchord*.deb /tmp/vchord.deb

COPY --from=pgvector /usr/lib/postgresql/*/lib/vector.so /opt/bitnami/postgresql/lib/
COPY --from=pgvector /usr/share/postgresql/*/extension/vector* /opt/bitnami/postgresql/share/extension/

USER root

RUN apt-get install --no-install-recommends -y /tmp/vchord.deb && rm -f /tmp/vchord.deb && \
      mv /usr/lib/postgresql/*/lib/vchord.so /opt/bitnami/postgresql/lib/ && \
      mv  usr/share/postgresql/*/extension/vchord* opt/bitnami/postgresql/share/extension/
USER 1001

ENV POSTGRESQL_EXTRA_FLAGS="-c shared_preload_libraries=vchord.so"

LABEL org.opencontainers.image.source="https://github.com/thiagoalmeidasa/container-images/tree/main/apps/bitnami-postgres-vchord"
