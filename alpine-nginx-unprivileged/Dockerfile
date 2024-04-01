FROM nginx:stable-alpine

RUN apk update \
  && apk upgrade --no-cache \
  && deluser nginx \
  && addgroup -S devops \
  && adduser -S devops -G devops

RUN chown -R devops:devops /var/cache/nginx \
  && chown -R devops:devops /var/log/nginx \
  && touch /run/nginx.pid \
  && chown -R devops:devops /run/nginx.pid \
  && chown -R devops:devops /etc/nginx

RUN sed -i 's/user  nginx;/#user  nginx;/g' /etc/nginx/nginx.conf

USER devops

COPY nginx/nginx.conf /etc/nginx/conf.d

CMD ["nginx", "-g", "daemon off;"]