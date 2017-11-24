FROM ubuntu:16.04
MAINTAINER c-sayoshni.ghosh@charter.com


ENV KOLLA_BASE=ubuntu \
    KOLLA_TYPE=source \
    KOLLA_TAG=3.0.1 \
    KOLLA_PROJECT=keystone \
    KOLLA_NAMESPACE=kolla \
    KOLLA_VERSION=3.0.1 \
    DOCKER_USER=docker-user \
    DOCKER_PASS=docker-pass \
    DOCKER_REGISTRY=quay.io \
    DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
        git \
        gcc \
        python-virtualenv \
        python-dev \
        python-pip \
        libssl-dev \
        libffi-dev \
        docker.io

RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY scripts/start.sh /usr/local/bin/start.sh
COPY scripts/clean.sh /usr/local/bin/clean.sh
COPY scripts/kolla-push.sh /usr/local/bin/kolla-push.sh

RUN chmod +x /usr/local/bin/start.sh && \
    chmod +x /usr/local/bin/clean.sh && \
    chmod +x /usr/local/bin/kolla-push.sh

WORKDIR /root

RUN git clone http://git.openstack.org/openstack/kolla.git ./kolla-$KOLLA_VERSION && \
    cd ./kolla-$KOLLA_VERSION && \
    git checkout tags/$KOLLA_VERSION

RUN mkdir -p .venv && \
    virtualenv .venv/kolla-builds && \
    . .venv/kolla-builds/bin/activate && \
    cd ./kolla-$KOLLA_VERSION && \
    pip install -e . && \
    mkdir -p /etc/kolla

RUN mkdir -p /root/.kolla-$KOLLA_VERSION/src/$KOLLA_PROJECT && \
    git clone http://git.openstack.org/openstack/$KOLLA_PROJECT.git /tmp/kolla/src/$KOLLA_PROJECT

COPY oslo_config/kolla-build.conf /etc/kolla/kolla-build.conf

WORKDIR /root/.kolla-$KOLLA_VERSION

CMD ["/usr/local/bin/start.sh"]
