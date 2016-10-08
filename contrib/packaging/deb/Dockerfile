FROM ubuntu:16.04

MAINTAINER "Andre Martins <andre@cilium.io>"

RUN apt-get update && \
apt-get install -y --no-install-recommends curl gcc make libc6-dev.i386
RUN cd /tmp && \
curl -Sslk -o go1.7.1.linux-amd64.tar.gz \
https://storage.googleapis.com/golang/go1.7.1.linux-amd64.tar.gz && \
tar -C /usr/local -xzf go1.7.1.linux-amd64.tar.gz

ADD . /tmp/cilium-net-build/src/github.com/cilium/cilium

WORKDIR /tmp/cilium-net-build/src/github.com/cilium/cilium

RUN export GOROOT=/usr/local/go && \
export GOPATH=/tmp/cilium-net-build && \
export PATH="$GOROOT/bin:$PATH" && \
export VERSION_PATH="cilium_$(cat VERSION)" && \
export PKG_BUILD=1 && \
make && \
mkdir -p "/tmp/$VERSION_PATH/etc/init" && \
make DESTDIR="/tmp/$VERSION_PATH" install && \
cp contrib/upstart/* "/tmp/$VERSION_PATH/etc/init" && \
chmod 644 /tmp/$VERSION_PATH/etc/init/*

# Debian specific
RUN apt-get install -y gettext-base
RUN cd /tmp/cilium-net-build/src/github.com/cilium/cilium && \
export VERSION_PATH="cilium_$(cat VERSION)" && \
export VERSION="$(cat VERSION)" && \
mkdir -p "/tmp/$VERSION_PATH/DEBIAN" && \
envsubst \\\$VERSION < control > "/tmp/$VERSION_PATH/DEBIAN/control" && \
cd /tmp && \
dpkg-deb --build "$VERSION_PATH"
