FROM java:8-jdk

ARG JMETER_VERSION="5.1"
ENV JMETER_HOME /opt/apache-jmeter-${JMETER_VERSION}
ENV	JMETER_BIN	${JMETER_HOME}/bin
ENV	JMETER_DOWNLOAD_URL  https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-${JMETER_VERSION}.tgz

# download and extract JMeter
RUN mkdir /tmp/dependencies
RUN	curl -L --silent ${JMETER_DOWNLOAD_URL} > /tmp/dependencies/apache-jmeter-${JMETER_VERSION}.tgz

RUN tar -xzf /tmp/dependencies/apache-jmeter-${JMETER_VERSION}.tgz -C /opt && \
    rm -rf /tmp/dependencies

# Set global PATH such that "jmeter" command is found
ENV PATH $PATH:$JMETER_BIN

COPY plugins/lib/*.jar ${JMETER_HOME}/lib/
COPY plugins/lib/ext/*.jar ${JMETER_HOME}/lib/ext/
COPY jmeter.sh ${JMETER_HOME}/bin/
#comment the below line if you do not want user.properties file
#COPY user.properties ${JMETER_HOME}/bin/

WORKDIR	/var/lib/jmeter-scenarios

# Ports to be exposed from the container for JMeter Slaves/Server
EXPOSE 1099 50000

# Application to run on starting the container
ENTRYPOINT $JMETER_HOME/bin/jmeter-server \
                        -Dserver.rmi.localport=50000 \
                        -Dserver_port=1099 \
			-Dserver.rmi.ssl.disable=true
