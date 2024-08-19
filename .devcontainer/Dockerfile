# syntax = docker/dockerfile:1.0-experimental
FROM registry.access.redhat.com/ubi8/nodejs-12 as podmandevcontainer

USER root

RUN dnf update -y && npm i -g @antora/cli@3.0 @antora/site-generator-default@3.0 \
    && dnf update -y \
    && npm rm --global npx && npm install --global npx && npm install --global gulp \
    && dnf clean all && rm -r /var/cache/dnf

RUN wget https://github.com/mikefarah/yq/releases/download/3.4.0/yq_linux_amd64 -O /usr/local/bin/yq && \
    chmod 755 /usr/local/bin/yq

RUN wget https://github.com/wercker/stern/releases/download/1.11.0/stern_linux_amd64 -O /usr/local/bin/stern && \
    chmod 755 /usr/local/bin/stern

RUN curl -L https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-linux.tar.gz | \
    tar -xvzf - -C /usr/local/bin/ oc && chmod 755 /usr/local/bin/oc && ln -s /usr/local/bin/oc /usr/local/bin/kubectl

# If running (rootless) podman, keep the root user so that outside the container it will match
# the host user.  Otherwise run as default user
FROM podmandevcontainer AS devcontainer
RUN chown -R default $HOME
USER default
