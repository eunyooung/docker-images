FROM alpine:latest

ARG VERSION=

LABEL name="jdk${VERSION}-alpine3.19"
LABEL version="0.1"

RUN apk update \
  && apk upgrade --no-cache \
  && apk add --no-cache openjdk${VERSION} \
  && addgroup -S devops \
  && adduser -S devops -G devops \
  && mkdir -p /service

ENV TZ=Asia/Seoul

RUN apk --no-cache add tzdata \
  && cp /usr/share/zoneinfo/${TZ} /etc/localtime \
  && apk del tzdata

RUN chown -R devops:devops /service \
  && sed -i "s/\/home\/devops:\/sbin\/nologin/\/home\/devops:\/bin\/ash/g" /etc/passwd

WORKDIR /service

USER devops

ENV JAVA_HOME=/usr/lib/jvm/java-${VERSION}-openjdk
ENV PATH=$PATH:$JAVA_HOME/bin

COPY ./build/libs/demo-0.0.1-SNAPSHOT.jar /service/service.jar

RUN echo "#!/bin/sh" >> /service/start.sh \
  && echo "java -jar -Dspring.profiles.active=\${PROFILE} /service/service.jar" >> /service/start.sh

RUN chmod +x /service/start.sh

EXPOSE 8080

ENTRYPOINT ["/service/start.sh"]
