FROM debian:buster-slim as builder

RUN apt update && apt install -y \
  unzip \
  autoconf \
  bison \
  build-essential \
  curl \
  flex \
  libreadline-dev \
  libncurses5-dev \
  libssh-dev \
  m4 

RUN curl -O -L https://gitlab.labs.nic.cz/labs/bird/-/archive/v2.0.7/bird-v2.0.7.zip
RUN unzip bird-v2.0.7.zip

RUN cd bird-v2.0.7 && \
  autoconf && \
  autoheader && \
  ./configure && \
  make

MAINTAINER Alexandre Corso <alexandre@acorso.fr>

EXPOSE 179

FROM debian:buster-slim

RUN apt update && apt install -y \
  libssh-4 \
  libreadline7 

RUN mkdir /etc/bird && mkdir -p /usr/local/var/run

COPY --from=builder /bird-v2.0.7/bird /usr/local/sbin/bird
COPY --from=builder /bird-v2.0.7/birdc /usr/local/sbin/birdc
COPY --from=builder /bird-v2.0.7/birdcl /usr/local/sbin/birdcl
COPY --from=builder /bird-v2.0.7/doc/bird.conf.example /usr/local/etc/bird.conf

ENTRYPOINT [ "bird" ]
#CMD bird -c /usr/local/etc/bird.conf -d -s /usr/local/var/run/bird
