# The base-deps Docker image installs main libraries needed to run Ray

# The GPU option is nvidia/cuda:11.0-cudnn8-runtime-ubuntu18.04
ARG BASE_IMAGE="ubuntu:focal"
FROM ${BASE_IMAGE}
ENV TZ=America/Los_Angeles
# TODO(ilr) $HOME seems to point to result in "" instead of "/root"
ENV PATH "/root/anaconda3/bin:$PATH"
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update -y && apt-get upgrade -y \
    && apt-get install -y \
        git \
        wget \
        cmake \
        tmux \
        screen \
        rsync \
        openssh-client \
    && wget \
        --quiet "https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh" \
        -O /tmp/miniconda.sh \
    && /bin/bash /tmp/miniconda.sh -b -u -p $HOME/anaconda3 \
    && $HOME/anaconda3/bin/conda init \ 
    && echo 'export PATH=$HOME/anaconda3/bin:$PATH' > /etc/profile.d/conda.sh \
    && rm /tmp/miniconda.sh \
    && $HOME/anaconda3/bin/conda install -y \
        libgcc python=3.7.7 \ 
    && $HOME/anaconda3/bin/conda clean -y --all \
    && $HOME/anaconda3/bin/pip install --no-cache-dir \
        flatbuffers \
        cython==0.29.0 \
        numpy==1.15.4 \
        psutil \
    # To avoid the following error on Jenkins:
    # AttributeError: 'numpy.ufunc' object has no attribute '__module__'
    && $HOME/anaconda3/bin/pip uninstall -y dask \ 
    # We install cmake temporarily to get psutil
    && apt-get autoremove cmake -y \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean 
