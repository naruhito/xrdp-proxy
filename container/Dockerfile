FROM ubuntu:22.04 as builder

RUN apt update && DEBIAN_FRONTEND=noninteractive apt -y install \
  build-essential \
  cmake \
  git \
  libasound2-dev \
  libavcodec-dev \
  libssl-dev \
  libx11-dev \
  libxcursor-dev \
  libxkbfile-dev \
  libxrandr-dev \
  libxv-dev \
