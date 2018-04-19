FROM python:2.7-slim as builder

RUN apt-get update \
      && apt-get install -y --no-install-recommends \
      build-essential \
      cmake \
      curl \
      libgtk2.0-dev \
      pkg-config \
      libavcodec-dev \
      libavformat-dev \
      libswscale-dev \
      && rm -rf /var/lib/apt/lists/* \
      && pip install numpy

WORKDIR /usr/local/src

RUN curl -L https://github.com/opencv/opencv/archive/3.4.1.tar.gz | tar zx \
      && mv opencv-3.4.1 opencv

WORKDIR /usr/local/src/opencv/release

RUN cmake -DCMAKE_INSTALL_PREFIX=/usr/local/share/opencv  \
      -DCMAKE_BUILD_TYPE=Release       \
      -DENABLE_CXX11=ON                \
      -DBUILD_PERF_TESTS=OFF           \
      -DWITH_XINE=ON                   \
      -DBUILD_TESTS=OFF                \
      -DENABLE_PRECOMPILED_HEADERS=OFF \
      -DCMAKE_SKIP_RPATH=ON            \
      -DBUILD_WITH_DEBUG_INFO=OFF      \
      -Wno-dev  ..

RUN make && make install

FROM node:8-slim
# RUN apt-get update \
#       && apt-get install -y --no-install-recommends \
#       libgtk2.0-dev \
#       libavcodec-dev \
#       libavformat-dev \
#       libswscale-dev \
#       && rm -rf /var/lib/apt/lists/*
COPY --from=builder /usr/local/share/opencv/ /usr/local/
RUN npm install -g --no-optional pm2 nodemon
WORKDIR /app 