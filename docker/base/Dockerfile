FROM java:8-jdk-alpine

ENV DAEMON_RUN=true
ENV SPARK_VERSION=2.4.4
ENV HADOOP_VERSION=2.7
ENV SCALA_VERSION=2.13.1
ENV SCALA_HOME=/usr/share/scala
ENV SBT_VERSION=1.3.2


RUN apk add --no-cache --virtual=.build-dependencies wget ca-certificates && \
	apk add --no-cache bash curl jq && \
	cd "/tmp" && \
	wget -qO- --no-verbose "https://downloads.lightbend.com/scala/${SCALA_VERSION}/scala-${SCALA_VERSION}.tgz"  | tar xzf - && \
	rm "/tmp/scala-${SCALA_VERSION}/bin/"*.bat && \
	mkdir ${SCALA_HOME} && \
	mv "/tmp/scala-${SCALA_VERSION}/bin/" "/tmp/scala-${SCALA_VERSION}/lib/" "${SCALA_HOME}" && \
	ln -s "${SCALA_HOME}/bin/"* "/usr/bin" && \
	apk del .build-dependencies && \
	rm -rf "/tmp/"*I

# Scala installation
RUN export PATH="/usr/local/sbt/bin:$PATH" && \
	apk update && apk add ca-certificates wget tar && \
	mkdir -p "/usr/local/sbt" && \
	wget -qO - --no-check-certificate "https://piccolo.link/sbt-${SBT_VERSION}.tgz" | tar xz -C /usr/local/sbt --strip-components=1 && sbt sbtVersion

RUN apk add --no-cache python3


# getting spark
RUN wget -qO - --no-verbose "http://mirror.ox.ac.uk/sites/rsync.apache.org/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz"| tar -xvzf - && \
	mv spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} spark
