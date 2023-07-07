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

RUN mkdir -p /opt/xrdp

RUN git clone https://github.com/neutrinolabs/NeutrinoRDP.git /tmp/NeutrinoRDP \
  && cd /tmp/NeutrinoRDP \
  && git checkout 5021122 \
  && cmake -DCMAKE_BUILD_TYPE=Debug -DWITH_SSE2=ON \
  && make \
  && make DESTDIR=/opt/xrdp install

RUN ln -s /opt/xrdp/usr/local/include/freerdp /usr/local/include/ \
  && echo /opt/xrdp/usr/local/lib >> /etc/ld.so.conf.d/neutrinordp.conf \
  && echo /opt/xrdp/usr/local/lib/freerdp >> /etc/ld.so.conf.d/neutrinordp.conf \
  && ldconfig

RUN git clone https://github.com/neutrinolabs/xrdp.git /tmp/xrdp \
  && cd /tmp/xrdp \
  && git checkout b26ca93 \
  && ./bootstrap \
  && PKG_CONFIG_PATH=/opt/xrdp/usr/local/lib/pkgconfig ./configure --enable-neutrinordp \
  && make \
  && make DESTDIR=/opt/xrdp install

RUN echo /opt/xrdp/usr/local/lib/xrdp >> /etc/ld.so.conf.d/neutrinordp.conf \
  && ldconfig


FROM ubuntu:22.04
COPY --from=builder /opt/xrdp /opt/xrdp
ENTRYPOINT ["tail", "-f", "/dev/null"]
