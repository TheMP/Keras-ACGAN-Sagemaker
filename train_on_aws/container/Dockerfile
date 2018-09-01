# Build an image that can do training  in SageMaker
# This image contains CUDA 9.0 (CUDA libs are backward-compatible), cuddn version 7 and 64bit ubuntu
FROM nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04

# FROM ubuntu:16.04 - if we do not want cuda support
MAINTAINER Amazon AI <sage-learner@amazon.com>

RUN apt-get -y update && apt-get install -y --no-install-recommends \
         wget \
         python \
         nginx \
         ca-certificates \
         python-dev \
         python-tk \
         gcc \
         g++ \
         libopenblas-dev \
    && rm -rf /var/lib/apt/lists/*

# Here we get all python packages.
RUN wget https://bootstrap.pypa.io/get-pip.py && python get-pip.py && \
    pip install numpy scikit-learn pandas flask gevent gunicorn matplotlib tensorflow-gpu keras Pillow six

# Set some environment variables. PYTHONUNBUFFERED keeps Python from buffering our standard
# output stream, which means that logs can be delivered to the user quickly. PYTHONDONTWRITEBYTECODE
# keeps Python from writing the .pyc files which are unnecessary in this case. We also update
# PATH so that the train and serve programs are found when the container is invoked.

ENV PYTHONUNBUFFERED=TRUE
ENV PYTHONDONTWRITEBYTECODE=TRUE
ENV PATH="/opt/program:${PATH}"

# Set up the program in the image
COPY keras-nn /opt/program
WORKDIR /opt/program
