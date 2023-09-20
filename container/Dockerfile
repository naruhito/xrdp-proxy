# Builder IMAGE
# ==================
FROM ubuntu:22.04 as builder

RUN apt update && DEBIAN_FRONTEND=noninteractive apt -y install \
  autoconf \
  build-essential \
  cmake \
  git \
  libasound2-dev \
  libavcodec-dev \
  libpam0g-dev \
  libssl-dev \
  libtool \
  libx11-dev \
  libxcursor-dev \
  libxkbfile-dev \
  libxrandr-dev \
  libxv-dev \
  nasm \
  pkg-config \
  zlib1g-dev

# NeutrinoRDP
RUN git clone https://github.com/neutrinolabs/NeutrinoRDP.git /tmp/NeutrinoRDP \
  && cd /tmp/NeutrinoRDP \
  && git checkout 5021122 \
  && cmake -DCMAKE_BUILD_TYPE=Debug -DWITH_SSE2=ON \
  && make \
  && make install

# xrdp
RUN git clone https://github.com/naruhito/xrdp.git /tmp/xrdp \
  && cd /tmp/xrdp \
  && git checkout e3bcff9 \
  && ./bootstrap \
  && ./configure --enable-neutrinordp \
  && make \
  && make install

# packaging
RUN mkdir /opt/xrdp \
  && cd /tmp/NeutrinoRDP && make DESTDIR=/opt/xrdp install \
  && cd /tmp/xrdp && make DESTDIR=/opt/xrdp install


# Runtime IMAGE
# ==================
FROM ubuntu:22.04

RUN apt update && DEBIAN_FRONTEND=noninteractive apt -y install \
  curl \
  less \
  nano \
  net-tools \
  supervisor\ 
  vim \
  && apt clean && rm -rf /var/lib/apt/lists/*

COPY --from=builder /opt/xrdp/ /
RUN ldconfig

COPY bin/* /usr/local/bin/
RUN chmod +x /usr/local/bin/*
COPY supervisor/* /etc/supervisor/conf.d/
ENTRYPOINT ["entrypoint.bash"]
