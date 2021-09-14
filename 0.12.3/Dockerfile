FROM debian:buster-slim

SHELL ["/bin/bash", "-xo", "pipefail", "-c"]

ENV LANG C.UTF-8

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        git \
        openssh-server \
    && rm -rf /var/lib/apt/lists/*

ENV GOGS_VERSION=0.12.3
ARG GOGS_SHA256=13eefa0a0d6fd7ed0c65b52182b6b6c318d4df71c962c744726a6d56367490ec

RUN curl --output gogs.tar.gz --location https://dl.gogs.io/${GOGS_VERSION}/gogs_${GOGS_VERSION}_linux_amd64.tar.gz --show-error \
    && echo "${GOGS_SHA256} gogs.tar.gz" | sha256sum --check -

RUN tar --extract --file gogs.tar.gz --directory /opt/ \
    && rm gogs.tar.gz

RUN adduser --system --group --disabled-login --disabled-password --shell /bin/false git \
    && chown --recursive --verbose git: /opt/gogs/

USER git
WORKDIR /opt/gogs

EXPOSE 3000 22

ENTRYPOINT [ "./gogs" ]
CMD [ "web" ]
