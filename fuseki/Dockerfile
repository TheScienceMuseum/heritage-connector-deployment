# Fuseki Dockerfile.
# Pulls triples from the HTTP URL specified by TRIPLES_URL and starts a Fuseki database using TDB (on-disk storage).

# java
FROM java:8-jdk

#ARG VERSION=3.16.0
ARG VERSION=4.3.0

# data
ARG TRIPLES_URL
ENV DATA_FILE=/mnt/heritageconnector.nt
RUN curl -L $TRIPLES_URL > $DATA_FILE

# jena - build TDB database
ENV TDB_LOC=/mnt/tdb_db
ENV JENA_URL=https://repository.apache.org/content/repositories/releases/org/apache/jena/apache-jena/${VERSION}/apache-jena-${VERSION}.tar.gz
RUN curl -L $JENA_URL > apache-jena.tar.gz
RUN tar -xvzf apache-jena.tar.gz
RUN apache-jena-${VERSION}/bin/tdbloader2 --loc $TDB_LOC $DATA_FILE

# fuseki
ENV URL=https://repository.apache.org/content/repositories/releases/org/apache/jena/jena-fuseki-server/${VERSION}/jena-fuseki-server-${VERSION}.jar
ENV BASE=/mnt/apache-fuseki

RUN mkdir -p $BASE

WORKDIR $BASE

RUN curl --silent --show-error --output fuseki-server.jar $URL

EXPOSE 3030

ENTRYPOINT [ "/usr/bin/java", "-Xms4g" ,"-Xmx8g", "-Dlog4j2.formatMsgNoLookups=true", "-XX:-UseGCOverheadLimit", "-XX:+UseParallelGC", "-jar", "fuseki-server.jar", "--loc=/mnt/tdb_db", "/heritage-connector"]
