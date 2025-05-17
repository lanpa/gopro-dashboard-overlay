FROM ubuntu:24.04

MAINTAINER huang.dexter@gmail.com
ENV PATH=$PATH:/usr/bin

WORKDIR /

# Install all dependencies in a single layer to reduce image size
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        gnupg \
        unzip \
        fonts-noto \
        fonts-roboto \
        python3.12 \
        python3.12-venv && \
    python3.12 -mvenv venv && \
    /venv/bin/python3.12 -m pip install --upgrade pip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install application
COPY bin /work/bin
COPY gopro_overlay /work/gopro_overlay
COPY setup.py README-PYPI.md requirements.txt /work
COPY docker/profile /profile
COPY docker/ffmpeg docker/ffprobe /bin
COPY docker/entrypoint.sh /
# Set up volume and working directory
VOLUME /work
WORKDIR /work

RUN /venv/bin/pip install setuptools
RUN /venv/bin/pip install -r requirements.txt
RUN /venv/bin/python3 setup.py install


ENTRYPOINT [ "/entrypoint.sh" ]

