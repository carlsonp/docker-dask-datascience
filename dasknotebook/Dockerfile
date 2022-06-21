FROM ghcr.io/dask/dask-notebook:latest

USER root

# for the local apt-cacher-ng proxy
RUN echo 'Acquire::HTTP::Proxy "http://192.168.1.226:3142";' >> /etc/apt/apt.conf.d/01proxy && \
    echo 'Acquire::HTTPS::Proxy "false";' >> /etc/apt/apt.conf.d/01proxy

RUN apt-get update && \
    DEBIAN_FRONTEND="noninteractive" apt-get -y install tzdata && \
    apt-get install -y --no-install-recommends \
    nano curl procps && \
    apt-get -y upgrade && \
    rm -rf /var/lib/apt/lists/*

USER jovyan

# disable a required password to login
RUN mkdir -p ~/.jupyter/ && printf 'c.NotebookApp.token = "" ' >> ~/.jupyter/jupyter_notebook_config.py

RUN conda install python-graphviz matplotlib

RUN pip3 install --no-cache-dir tpot dask-ml

