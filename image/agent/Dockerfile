#!/bin/bash
FROM --platform=amd64 openjdk:8-jdk-slim
	
RUN apt-get -y update && apt-get upgrade -y
RUN apt-get -y install \
    net-tools \
    curl

#--- Environment ---#
ENV PATH /root/bin:$PATH
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
ENV CATALINA_HOME=/opt/tomcat
ENV CATALINA_PID=/opt/tomcat/tomcat9.pid
ENV SCOUTER_HOME=/opt/scouter
ENV TOMCAT_HOME=CATALINA_HOME

#RUN mkdir /opt/scouter
RUN mkdir -p /opt/tomcat
RUN mkdir -p /opt/scouter/agent.java
RUN mkdir -p /opt/scouter/agent.host

#RUN cd /tmp
##RUN curl -L -s https://github.com/scouter-project/scouter/releases/download/v2.17.1/scouter-all-2.17.1.tar.gz | tar xvz; \
#RUN curl -L -O https://github.com/scouter-project/scouter/releases/download/v2.17.1/scouter-all-2.17.1.tar.gz
##RUN tar -zxvf scouter-all-2.17.1.tar.gz
##RUN cp -Rv ./scouter/agent.host /opt/scouter/agent.host/
##RUN cp -Rv ./scouter/agent.java /opt/scouter/agent.java/

##RUN curl -L -s https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.68/bin/apache-tomcat-9.0.68.tar.gz | tar xvz; \
RUN curl -L -O https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.68/bin/apache-tomcat-9.0.68.tar.gz 
RUN tar -zxvf apache-tomcat-9.0.68.tar.gz 
RUN cp -Rv ./apache-tomcat-9.0.68/* /opt/tomcat/

#######################
# Copy configuration files 
# the destination must ends with / 
# ie: /usr/local/tomcat/webapps/ is correct, not /usr/local/tomcat/webapps
#######################
COPY ./agent.host/ /opt/scouter/agent.host/ 
COPY ./agent.java/ /opt/scouter/agent.java/
COPY ./catalina.sh /opt/tomcat/bin/catalina.sh
COPY ./scouter-host.conf /opt/scouter/agent.host/conf/scouter.conf
COPY ./scouter-java.conf /opt/scouter/agent.java/conf/scouter.conf

#RUN cp -r agent.host /opt/scouter/agent.host/
#RUN cp -r agent.java /opt/scouter/agent.java/

#RUN cp ./scouter-host.conf /opt/scouter/agent.host/conf/scouter.conf
#RUN cp ./scouter-java.conf /opt/scouter/agent.java/conf/scouter.conf

#######################
# EXPOSE 
#######################
EXPOSE 6180/tcp
EXPOSE 6188/tcp
EXPOSE 8080/tcp
EXPOSE 6100/tcp
EXPOSE 6100/udp

# Define default command.
#SHELL ["/bin/bash", "-c"] 
##WORKDIR /opt/scouter/agent.host
RUN cd /opt/scouter/agent.host

#--- agent.java ---#
CMD ["java", "-classpath", "/opt/scouter/agent.host/scouter.host.jar", "scouter.boot.Boot", "/opt/scouter/agent.host/lib"]
#CMD ["java", "-classpath", "/opt/scouter/agent.host/scouter.host.jar", "scouter.boot.Boot", "./lib"]
RUN nohup java  -classpath ./scouter.host.jar scouter.boot.Boot ./lib > nohup.out &

WORKDIR /opt/tomcat/bin
RUN nohup /opt/tomcat/bin/catalina.sh run > nohup.out &
#CMD ["/opt/tomcat/bin/catalina.sh", "run"]

