FROM centos:7
MAINTAINER Mateusz Jagiela

RUN yum install -y git

RUN useradd yaka
USER yaka
WORKDIR /home/yaka

ADD jdk-7u79-linux-x64.tar.gz .
ENV JAVA_HOME=/home/yaka/jdk1.7.0_79
ENV PATH=$PATH:$JAVA_HOME/bin
RUN java -version

ADD https://archive.apache.org/dist/maven/maven-2/2.2.1/binaries/apache-maven-2.2.1-bin.tar.gz .
USER root
RUN chown yaka apache-maven-2.2.1-bin.tar.gz
USER yaka
RUN tar xvf apache-maven-2.2.1-bin.tar.gz
ENV PATH=$PATH:/home/yaka/apache-maven-2.2.1/bin
RUN mvn -version

ADD http://ftp.ps.pl/pub/apache/tomcat/tomcat-7/v7.0.72/bin/apache-tomcat-7.0.72.tar.gz .
USER root
RUN chown yaka apache-tomcat-7.0.72.tar.gz
USER yaka
RUN tar xvf apache-tomcat-7.0.72.tar.gz
RUN rm -rv apache-tomcat-7.0.72/webapps/*

RUN rm *.tar.gz

RUN git clone https://github.com/jagm/yaka.git
RUN sed -i -e 's/^OPENSHIFT_MYSQL_DB_HOST=.*$/OPENSHIFT_MYSQL_DB_HOST=db-host/g' yaka/src/main/resources/application.properties
RUN cd yaka && mvn clean install 
RUN cp yaka/target/kanban-1.0-SNAPSHOT.war apache-tomcat-7.0.72/webapps/kanban.war

EXPOSE 8080
CMD ["apache-tomcat-7.0.72/bin/catalina.sh", "run"]
