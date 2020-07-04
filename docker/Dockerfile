ARG BASE_IMAGE=nvidia/cuda:10.1-base
FROM ${BASE_IMAGE}

ARG PYTHON_VERSION=3.6
ARG PYTORCH_VERSION=1.4

ARG BRANCH=master
ARG FORK=pytorch

ENV TZ=America/New_York
ENV DEBIAN_FRONTEND="noninteractive"

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
         build-essential \
         cmake \
         git \
         curl \
         vim \
         ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# install python and pytorch
RUN curl -o ~/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    chmod +x ~/miniconda.sh && \
    ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh && \
    /opt/conda/bin/conda install -y -c pytorch \
        python=$PYTHON_VERSION \
        pytorch=$PYTORCH_VERSION \
        torchvision && \
    /opt/conda/bin/conda clean -ya

ENV PATH /opt/conda/bin:$PATH

RUN echo $FORK

# install ignite from source
RUN git clone https://github.com/$FORK/ignite && \
    cd ignite && \
    git checkout $BRANCH && \
    pip install -r requirements-dev.txt && \
    python setup.py install

WORKDIR /ignite

ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

CMD ["/bin/bash"]

