FROM openjdk:11

####################################### (1) #######################################
# INSTALL SCALA AND SBT - taken from https://github.com/broadinstitute/scala-baseimage/blob/master/Dockerfile
####################################### (1) #######################################

RUN \
  apt-get update -q && \
  apt-get upgrade -qq && \
  apt-get install -y git && \
  rm -rf /var/lib/apt/lists/*

ARG SBT_VERSION=1.6.2
RUN \
  curl -L "https://github.com/sbt/sbt/releases/download/v$SBT_VERSION/sbt-$SBT_VERSION.tgz" | tar zxf - -C /usr/share  && \
  cd /usr/share/sbt/bin && \
  rm sbt.bat sbtn-x86_64-apple-darwin sbtn-x86_64-pc-linux sbtn-x86_64-pc-win32.exe && \
  ln -s /usr/share/sbt/bin/sbt /usr/local/bin/sbt

ARG SCALA_VERSION=2.13.4
RUN \
  mkdir /setup-project && \
  cd /setup-project && \
  echo "scalaVersion := \"${SCALA_VERSION}\"" > build.sbt && \
  echo "case object Temp" > Temp.scala && \
  sbt compile && \
  rm -rf /setup-project

####################################### (2) #######################################
# COPY SOURCE CODE AND BUILD A JAR FILE
####################################### (2) #######################################

COPY akka-http-helloworld /akka-http-helloworld
WORKDIR /akka-http-helloworld
RUN sbt assembly

####################################### (3) #######################################
# CREATE A SEPARATE LIGHTWEIGHT IMAGE WITH THE JAR
####################################### (3) #######################################

FROM openjdk:11-jre

COPY --from=0 /akka-http-helloworld/target/scala-*/akka-http-quickstart-scala-assembly-*.jar ./app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
