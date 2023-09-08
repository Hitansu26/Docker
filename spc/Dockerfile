FROM amazoncorretto:11-alpine3.17
LABEL author="Hitansu"
LABEL organization="learningthoughts"
ARG DOWNLOAD_LOCATION='https://referenceapplicationskhaja.s3.us-west-2.amazonaws.com/spring-petclinic-2.4.2.jar'
ARG USERNAME='petclinic'
ARG HOMEDIR='/petclinic'
RUN adduser -h ${HOMEDIR} -s /bin/sh -D ${USERNAME}
USER ${USERNAME}
WORKDIR ${HOMEDIR}
ADD --chown=${USERNAME}:${USERNAME} ${DOWNLOAD_LOCATION} "${HOMEDIR}/spring-petclinic-2.4.2.jar"
EXPOSE 8080
CMD ["java", "-jar", "spring-petclinic-2.4.2.jar"]



FROM tomcat:9-jdk8
LABEL author="Hitansu"
LABEL organization="learningthoughts"
ADD https://referenceapplicationskhaja.s3.us-west-2.amazonaws.com/gameoflife.war /usr/local/tomcat/webapps
EXPOSE 8080





FROM mcr.microsoft.com/dotnet/runtime:7.0
ADD https://github.com/nopSolutions/nopCommerce/releases/download/release-4.60.4/nopCommerce_4.60.4_NoSource_linux_x64.zip/nopCommerce  /nopCommerce/nopCommerce_4.60.4_NoSource_linux_x64.zip
COPY nopCommerce /nopCommerce 
ENV ASPNETCORE_URLS='https://0.0.0.0:5000'
EXPOSE 5000
WORKDIR /nopCommerce
CMD ["dotnet", "Nop.web.dll", "--urls='http://0.0.0.0:5000'"]
RUN apt update && \
apt install unzip -y && \
mkdir bin logs && \ 
unzip nopCommerce_4.60.4_NoSource_linux_x64.zip
 

FROM alpine:3
ADD https://github.com/nopSolutions/nopCommerce/releases/download/release-4.60.4/nopCommerce_4.60.4_NoSource_linux_x64.zip /nopCommerce/nopCommerce_4.60.4_NoSource_linux_x64.zip





FROM maven:3-amazoncorretto-17 AS builder
COPY . /spring-petclinic
RUN  cd /spring-petclinic && mvn package


FROM amazoncorretto:17-alpine3.17
LABEL author="khaja"
LABEL organization="learningthoughts"
ARG USERNAME='petclinic'
ARG HOMEDIR='/petclinic'
ENV TEST=hello
RUN adduser -h ${HOMEDIR} -s /bin/sh -D ${USERNAME}
USER ${USERNAME}
WORKDIR ${HOMEDIR}
COPY --from=builder --chown=${USERNAME}:${USERNAME} /spring-petclinic/target/spring-petclinic-3.1.0-SNAPSHOT.jar "${HOMEDIR}/spring-petclinic-3.1.0-SNAPSHOT.jar"
EXPOSE 8080
CMD ["java", "-jar", "spring-petclinic-3.1.0-SNAPSHOT.jar"]