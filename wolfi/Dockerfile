# First stage: Build
FROM arm64v8/ubuntu:22.04 AS build

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y \
    git \
    cmake \
    make \
    g++ \
    python3 \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Build box64
WORKDIR /box64
RUN git clone https://github.com/ptitSeb/box64 . && \
    mkdir build && \
    cd build && \
    cmake .. -DARM_DYNAREC=ON -DCMAKE_BUILD_TYPE=RelWithDebInfo && \
    make -j$(nproc) && \
    make install DESTDIR=/box

# Second stage: Runtime
FROM cgr.dev/chainguard/wolfi-base

COPY --from=build /box /

RUN apk add --no-cache libcurl4

ENTRYPOINT ["/bin/sh"]
