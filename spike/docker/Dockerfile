FROM ubuntu:24.04

# UNVERIFIED SCAFFOLDING — this has never been built.
# Node SP-01 is to make it work. Expect to fix it.

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
      build-essential \
      cmake \
      ninja-build \
      git \
      python3 \
      llvm-18-dev \
      libclang-18-dev \
      clang-18 \
      clang-tools-18 \
      clang-format-18 \
      cppcheck \
      bear \
    && rm -rf /var/lib/apt/lists/*

ENV LLVM_DIR=/usr/lib/llvm-18/cmake
ENV Clang_DIR=/usr/lib/cmake/clang-18

WORKDIR /build
COPY src/ /build/src/
COPY CMakeLists.txt /build/

RUN cmake -S /build -B /build/out -G Ninja \
      -DCMAKE_BUILD_TYPE=Release \
 && cmake --build /build/out

WORKDIR /work
ENTRYPOINT ["/build/out/sq-analyze"]
