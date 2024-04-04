FROM nginx:stable-alpine3.17

RUN apk update \
  && apk upgrade --no-cache \
  && deluser nginx \
  && addgroup -S devops \
  && adduser -S devops -G devops

ENV TZ=Asia/Seoul

RUN apk --no-cache add tzdata \
  && cp /usr/share/zoneinfo/$TZ /etc/localtime \
  && apk del tzdata

RUN chown -R devops:devops /var/cache/nginx \
  && chown -R devops:devops /var/log/nginx \
  && touch /run/nginx.pid \
  && chown -R devops:devops /run/nginx.pid \
  && chown -R devops:devops /etc/nginx

RUN sed -i 's/user  nginx;/#user  nginx;/g' /etc/nginx/nginx.conf

USER devops

RUN rm /etc/nginx/conf.d/default.conf

COPY nginx/nginx.conf /etc/nginx/conf.d

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]