ARG VERSION

FROM python:${VERSION}-slim

ARG VERSION

LABEL name="python${VERSION}-slim"
LABEL version="0.1"

RUN apt-get update \
  && apt-get upgrade -y \
  && apt-get install build-essential -y \
  && useradd -ms /bin/bash devops \
  && mkdir -p /app

ENV TZ=Asia/Seoul

RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime \
  && echo ${TZ} > /etc/timezone

RUN chown -R devops /app \
  && chgrp -R devops /app

USER devops

ENV PATH=/home/devops/.local/bin:$PATH

WORKDIR /app

COPY ./src /app

ENTRYPOINT ["python3", "run.py"]
