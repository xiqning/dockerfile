FROM 172.31.21.226/ideal/openjdk:8

# 创建者信息
MAINTAINER batizhao <zhaobati@gmail.com>

# Install dependencies
RUN apt-get update && \
    apt-get install -y wget tar && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Define commonly used JAVA_HOME variable
ENV TOMCAT_VERSION 8.5.24

# Get Tomcat
RUN wget --no-cookies http://download.nextag.com/apache/tomcat/tomcat-8/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz -O /tmp/tomcat.tgz && \
    tar xzvf /tmp/tomcat.tgz -C /opt && \
    mv /opt/apache-tomcat-${TOMCAT_VERSION} /opt/tomcat && \
    rm /tmp/tomcat.tgz && \
    rm -rf /opt/tomcat/webapps/examples && \
    rm -rf /opt/tomcat/webapps/docs && \
    rm -rf /opt/tomcat/webapps/ROOT

# Add admin/admin user
ADD tomcat-users.xml /opt/tomcat/conf/

# Tomcat 8.5 要注释 manage 的 ip 限制
RUN sed -i '/Valve/s/^/<!--/g' /opt/tomcat/webapps/manager/META-INF/context.xml
RUN sed -i '/Valve/s/$/-->/g' /opt/tomcat/webapps/manager/META-INF/context.xml
RUN sed -i '/allow/s/^/<!--/g' /opt/tomcat/webapps/manager/META-INF/context.xml
RUN sed -i '/allow/s/$/-->/g' /opt/tomcat/webapps/manager/META-INF/context.xml

ENV CATALINA_HOME /opt/tomcat
ENV PATH $PATH:$CATALINA_HOME/bin

EXPOSE 8080
WORKDIR /opt/tomcat

# Launch Tomcat
CMD ["/opt/tomcat/bin/catalina.sh", "run"]
