FROM ubuntu:22.04

ENV DEBIAN_FRONTEND="noninteractive"
ENV LC_ALL="C"

# Update APT
RUN apt-get update

# Install Ninja
RUN apt-get install -y ninja-build

# Install the correct version of CMake (the version from apt-get installs GCC, the presence of which sometimes confuses LLVM's build)
ADD https://github.com/Kitware/CMake/releases/download/v3.23.2/cmake-3.23.2-linux-x86_64.sh /root/install-cmake.sh
RUN chmod +x /root/install-cmake.sh && \
      /root/install-cmake.sh --prefix=/usr/ --exclude-subdir --skip-license && \
      rm /root/install-cmake.sh

# Install clang 15
RUN apt-get install -y software-properties-common gnupg
ADD https://apt.llvm.org/llvm-snapshot.gpg.key /root/llvm-snapshot.gpg.key
RUN apt-key add /root/llvm-snapshot.gpg.key && \
      apt-add-repository 'deb http://apt.llvm.org/jammy/ llvm-toolchain-jammy-15 main' && \
      apt update && \
      apt install -y \
        clang-15 \
        clangd-15

# Install correct version of Swift
# To update, select a new package from https://www.swift.org/download/
ADD https://download.swift.org/development/ubuntu2204/swift-DEVELOPMENT-SNAPSHOT-2022-10-03-a/swift-DEVELOPMENT-SNAPSHOT-2022-10-03-a-ubuntu22.04.tar.gz /root/swift-toolchain.tar.gz
RUN tar -xf /root/swift-toolchain.tar.gz --directory / --strip-components=1 && \
    rm /root/swift-toolchain.tar.gz

# Use Clang with CMake
COPY cmake-tools-kits.json /root/.local/share/CMakeTools/cmake-tools-kits.json
