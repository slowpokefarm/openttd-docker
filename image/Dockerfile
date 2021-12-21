FROM ubuntu:20.04
MAINTAINER Stanislav Slow <stanislav.rimsha@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
ARG VERSION
ARG OPENGFX_VERSION
ENV VERSION=${VERSION}
ENV OPENGFX_VERSION=${OPENGFX_VERSION}

RUN sed -i -e 's/http:\/\/archive/mirror:\/\/mirrors/' -e 's/\/ubuntu\//\/mirrors.txt/' /etc/apt/sources.list && \
    sed -i -e 's/http:\/\/security/mirror:\/\/mirrors/' -e 's/\/ubuntu\//\/mirrors.txt/' /etc/apt/sources.list

RUN apt-get update  && \
    apt-get install -y \
    git \
    mc \
    curl \
    cron \
    wget \
    nano \
    supervisor \
    htop \
    unzip \
    ca-certificates \
    libfontconfig1 \
    libfreetype6 \
    libfluidsynth2 \
    libicu-dev \
    libpng16-16 \
    liblzma-dev \
    liblzo2-2 \
    libsdl1.2debian \
    libsdl2-2.0-0 \
    language-pack-ru && \
    apt-get autoremove -y && \
    apt-get clean && \
    apt-get autoclean

RUN chown -R root:root /etc/cron.d && \
    chmod -R 0644 /etc/cron.d

# Supervisor conf
RUN echo "[supervisord]" >> /etc/supervisor/supervisord.conf && \
    echo "nodaemon = true" >> /etc/supervisor/supervisord.conf && \
    echo "user = root" >> /etc/supervisor/supervisord.conf

# Install Composer
RUN curl -sLO https://cdn.openttd.org/openttd-releases/${VERSION}/openttd-${VERSION}-linux-ubuntu-focal-amd64.deb && \
    dpkg -i openttd-${VERSION}-linux-ubuntu-focal-amd64.deb && \
    rm openttd-${VERSION}-linux-ubuntu-focal-amd64.deb

    ## Download GFX and install
RUN mkdir -p /usr/share/games/openttd/baseset/ && \
	cd /usr/share/games/openttd/baseset/ && \
	wget -q -O opengfx-${OPENGFX_VERSION}.zip https://cdn.openttd.org/opengfx-releases/${OPENGFX_VERSION}/opengfx-${OPENGFX_VERSION}-all.zip

RUN	cd /usr/share/games/openttd/baseset/ && \
	unzip opengfx-${OPENGFX_VERSION}.zip && \
	tar -xf opengfx-${OPENGFX_VERSION}.tar && \
	rm -rf opengfx-*.tar opengfx-*.zip


COPY service.sh /
RUN chmod 700 /service.sh
CMD ["./service.sh"]
