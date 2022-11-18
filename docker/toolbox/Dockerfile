FROM camunda:7.9.0-7-ubi

SHELL ["/bin/bash", "-c"]
USER root
RUN rm -rf /camunda/webapps/examples \
           /camunda/lib/groovy-all-2.4.13.jar \
           /camunda/lib/mail-1.4.1.jar

USER jboss

RUN sed -i 's/<!-- <filter>/<filter>/' /camunda/webapps/engine-rest/WEB-INF/web.xml && \
    sed -i 's/<\/filter-mapping> -->/<\/filter-mapping>/' /camunda/webapps/engine-rest/WEB-INF/web.xml && \
    sed -i 's/connectionTimeout="20000"/connectionTimeout="600000"/g' /camunda/conf/server.xml

COPY --chown=jboss:jboss ./camunda.sh /camunda/
COPY ./context.xml /camunda/conf/

COPY ./target/dependencies/*.jar /camunda/lib/
COPY ./seed/target/*.war /camunda/webapps/
COPY ./target/camunda-ext-*.jar /camunda/lib/camunda-ext.jar

ENV DB_DRIVER= \
    DB_HOST= \
    DB_PORT= \
    DB_NAME= \
    DB_USERNAME= \
    DB_PASSWORD= \
    DB_URL=

LABEL maintainer="Hydra Billing <info@hydra-billing.com>" \
  org.opencontainers.image.authors="Hydra Billing <info@hydra-billing.com>" \
  org.opencontainers.image.title="camunda-ext" \
  org.opencontainers.image.description="Camunda with camunda-ext library inside" \
  org.opencontainers.image.vendor="Hydra Billing Solutions LLC" \
  org.opencontainers.image.licenses="Apache License 2.0" \
  org.opencontainers.image.url="https://hub.docker.com/r/latera/camunda" \
  org.openbuildservice.disturl="https://github.com/latera/camunda-ext/releases" \
  org.opencontainers.image.source="https://github.com/latera/camunda-ext.git" \
  org.opencontainers.image.documentation="https://latera.github.io/camunda-ext"

HEALTHCHECK --start-period=60s \
  CMD wget -q --spider http://127.0.0.1:8080/camunda/app/welcome