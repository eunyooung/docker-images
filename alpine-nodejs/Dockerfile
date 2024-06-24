ARG VERSION

FROM node:${VERSION}-alpine3.19 As base

ARG VERSION

LABEL name="nodejs${VERSION}-alpine3.19"
LABEL version="0.1"

RUN apk update \
  && apk upgrade

ENV TZ=Asia/Seoul

RUN apk --no-cache add tzdata \
  && cp /usr/share/zoneinfo/$TZ /etc/localtime \
  && echo $TZ > /etc/timezone \
  apk del tzdata

FROM base AS builder

WORKDIR /app

COPY package.json ./

CMD ["node", "server.js"]
