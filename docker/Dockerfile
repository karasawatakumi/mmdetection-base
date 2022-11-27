FROM nvidia/cuda:11.7.1-cudnn8-runtime-ubuntu22.04

ARG TIMEZONE
RUN ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime

RUN apt-get update && apt-get install -y --no-install-recommends \
    sudo git zip curl wget emacs \
    build-essential libssl-dev libbz2-dev libreadline-dev \
    libopencv-dev libffi-dev libsqlite3-dev \
    && rm -rf /var/lib/apt/lists/* /var/cache/apt/archives/*

# user
ARG UID
RUN useradd docker -l -u ${UID} -G sudo -s /bin/bash -m
RUN echo 'Defaults visiblepw' >> /etc/sudoers
RUN echo 'docker ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
USER docker

# python
ARG PYTHON_VERSION
ENV PYENV_ROOT /home/docker/.pyenv
ENV PATH $PYENV_ROOT/shims:$PYENV_ROOT/bin:$PATH
RUN curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
RUN pyenv install ${PYTHON_VERSION} && pyenv global ${PYTHON_VERSION}

# torch, torchvision, mmcv
RUN pip install -U pip setuptools
COPY requirements_core.txt /tmp/requirements_core.txt
RUN pip install -r /tmp/requirements_core.txt

# mmdet
ARG MMDETECTION_VERSION
RUN cd /tmp && \
    git clone https://github.com/open-mmlab/mmdetection.git -b v${MMDETECTION_VERSION} --depth 1 && \
    cd mmdetection && \
    pip install --no-cache-dir -e .

# dev libraries
COPY requirements_dev.txt /tmp/requirements_dev.txt
RUN pip install -r /tmp/requirements_dev.txt


ENV PYTHONPATH $PYTHONPATH:/work
WORKDIR /work
