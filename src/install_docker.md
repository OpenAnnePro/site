---
title: "Build Using Docker"
layout: page
nav_exclude: true
---
# The following build script is provided by `@zinosat` on the Discord Server

Save the following into a file named `Dockerfile`

```

# docker build -t ap2 .
#
# docker run --privileged -h ap2 --rm -it -v ${PWD}:/host --user $(id -u) -w /home/dev ap2 bash

FROM debian:bullseye
MAINTAINER Davide Viti <zinosat@gmail.com>

RUN sed -i \
    -e "s|deb.debian.org|debian.fastweb.it|g" \
    /etc/apt/sources.list

RUN apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
  build-essential ca-certificates less git sudo \
  pkg-config libusb-1.0-0-dev cargo gcc-arm-none-eabi libstdc++-arm-none-eabi-newlib

RUN adduser --disabled-password --gecos '' dev && \
    adduser dev sudo && \
    echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

RUN cd /home/dev; sudo -H -u dev git clone https://github.com/OpenAnnePro/AnnePro2-Tools.git && \
    cd AnnePro2-Tools && cargo build --release
    
RUN cd /home/dev; sudo -H -u dev git clone https://github.com/OpenAnnePro/qmk_firmware.git annepro-qmk --recursive --depth 1 && \
    cd annepro-qmk && ./util/qmk_install.sh && make annepro2/c18

RUN cd /home/dev; sudo -H -u dev git clone https://github.com/OpenAnnePro/AnnePro2-Shine.git --recursive --depth 1 && \
    cd AnnePro2-Shine && make C18

RUN cp /home/dev/AnnePro2-Tools/target/release/annepro2_tools /home/dev/
RUN cp /home/dev/annepro-qmk/.build/annepro2_c18_default.bin /home/dev/
RUN cp /home/dev/AnnePro2-Shine/build/C18/annepro2-shine-C18.bin /home/dev/

ENV TZ /usr/share/zoneinfo/Europe/Rome

```

and run

```bash
docker build -t ap2 .
docker run --privileged -h ap2 --rm -it -v ${PWD}:/host --user $(id -u) -w /home/dev ap2 bash
```

If you run Linux system it is possible to copy firmware and annepro2_tools binary from the container to the host with this command:

```bash
docker run --rm -it --user $(id -u) -v ${PWD}:/host ap2 bash -c 'cp /home/dev/annepro2* /host'
```

After that you can continue on flashing the firmware from the host system.
