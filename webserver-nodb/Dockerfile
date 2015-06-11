FROM tomcat:7-jre7
MAINTAINER Kelley Spoon <kelley@trueability.com>

# To update, check http://guac-dev.org/releases
ENV GUACAMOLE_VERSION         0.9.4
ENV GUACAMOLE_WAR_SHA1        07d194593db296d20d87b858b02e7816fb4a9315
ENV GUACAMOLE_AUTH_MYSQL_SHA1 a68166aec88784325f3358a16dedc70f2df73342
ENV MYSQL_CONNECTOR_MD5       ad4c875c719247f8e8c41cd7f0609e00


### Guacamole webapp
# Disable Tomcat's manager application.
RUN rm -rf webapps/*

# Fetch and install Guacamole war archive.
RUN echo $GUACAMOLE_WAR_SHA1  ROOT.war > webapps/ROOT.war.sha1 && \
    curl -L -o webapps/ROOT.war http://sourceforge.net/projects/guacamole/files/current/binary/guacamole-${GUACAMOLE_VERSION}.war/download && \
    cd webapps && sha1sum -c --quiet ROOT.war.sha1

# font prep
RUN apt-get update && apt-get install -y apparmor-utils fontconfig fonts-inconsolata && apt-get autoclean

# Expose tomcat runtime options through the RUNTIME_OPTS environment variable.
#   Example to set the JVM's max heap size to 256MB use the flag
#   '-e RUNTIME_OPTS="-Xmx256m"' when starting a container.
RUN echo 'export CATALINA_OPTS="$RUNTIME_OPTS"' > bin/setenv.sh

### Configuration
ENV GUACAMOLE_HOME /guacamole
RUN mkdir -p ${GUACAMOLE_HOME}/classpath
COPY guacamole.properties ${GUACAMOLE_HOME}/
VOLUME ["/etc/guacamole"]
