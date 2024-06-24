ARG VERSION

FROM python:${VERSION}-slim

ARG VERSION

LABEL name="python${VERSION}-slim-miniconda3"
LABEL version="0.1"

RUN apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y wget bzip2 curl git \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  && useradd -ms /bin/bash devops \
  && mkdir -p /app

ENV TZ=Asia/Seoul

RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime \
  && echo ${TZ} > /etc/timezone

RUN chown -R devops /app \
  && chgrp -R devops /app

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

# Install miniconda3
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-py38_4.12.0-Linux-x86_64.sh -O ~/miniconda.sh \
  && /bin/bash ~/miniconda.sh -b -p /opt/conda \
  && rm ~/miniconda.sh \
  && /opt/conda/bin/conda clean -tipsy \
  && ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh \
  && /bin/bash -c ". /opt/conda/etc/profile.d/conda.sh" \
  && echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc \
  && echo "conda activate app_mini" >> ~/.bashrc
# Install Miniconda End

# 취약점 보완
RUN conda update --all \
  && /bin/bash -c "conda install -c conda-forge dask django twisted"

COPY environment.yml /app/environment_init.yml

USER devops

RUN /bin/bash -c "conda env create -f /app/environment_init.yml"
ENV PATH /opt/conda/envs/app_mini/bin:$PATH

WORKDIR /app

COPY ./src /app

ENTRYPOINT ["conda", "run", "--no-capture-output", "-n", "app_mini", "python", "run.py"]
