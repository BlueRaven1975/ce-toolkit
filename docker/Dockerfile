FROM ubuntu:jammy

LABEL maintainer="Romano Romano <romano.romano@gmail.com>"
LABEL version="1.0.0"

ARG \
  ANSIBLE_VERSION= \
  TERRAFORM_VERSION= \
  USER=

ENV \
  ANSIBLE_DEPRECATION_WARNINGS=false \
  ANSIBLE_HOST_KEY_CHECKING=false \
  ANSIBLE_PIPELINING=1 \
  ANSIBLE_RETRY_FILES_ENABLED=false \
  ANSIBLE_SSH_PIPELINING=1 \
  BOTO_USE_ENDPOINT_HEURISTICS=true \
  DEBIAN_FRONTEND=noninteractive \
  GIT_AUTHOR_EMAIL="" \
  GIT_AUTHOR_NAME="" \
  GIT_COMMITTER_EMAIL="" \
  GIT_COMMITTER_NAME="" \
  LANG=en_US.UTF-8 \
  LANGUAGE=en_US.UTF-8 \
  LC_ALL=en_US.UTF-8 \
  PYTHONWARNINGS="ignore::UserWarning" \
  TERM=xterm-256color

RUN apt-get update && apt-get install --no-install-recommends -y \
  ca-certificates \
  curl \
  gnupg \
  && rm -rf /var/lib/apt/lists/* \
  && curl -fsSL https://packages.microsoft.com/keys/microsoft.asc \
  | gpg --dearmor \
  | tee /usr/share/keyrings/microsoft.gpg > /dev/null \
  && curl -fsSLo /usr/share/keyrings/cloud.google.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg \
  && curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

COPY files/etc/apt/sources.list.d/* /etc/apt/sources.list.d

RUN apt-get update && apt-get install --no-install-recommends -y \
  awscli \
  azure-cli \
  bash-completion \
  bind9-dnsutils \
  git \
  google-cloud-cli \
  iputils-ping \
  iputils-tracepath \
  jq \
  kubectl \
  less \
  locales \
  openssh-client \
  openssl \
  python3-boto3 \
  python3-pip \
  rsync \
  tmux \
  unzip \
  vim \
  wget \
  whois \
  && rm -rf /var/lib/apt/lists/* \
  && curl -fsSLo /usr/local/bin/git-prompt.sh https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh \
  && curl -fsSL https://get.helm.sh/helm-v3.9.4-linux-amd64.tar.gz | tar -xz -C /usr/local/bin --no-same-owner --strip-components=1 linux-amd64/helm \
  && curl -fsSL https://github.com/derailed/k9s/releases/download/v0.26.3/k9s_Linux_x86_64.tar.gz | tar -xz -C /usr/local/bin --no-same-owner k9s \
  && locale-gen en_US.UTF-8 \
  && adduser ${USER}

USER ${USER}

WORKDIR /home/${USER}

RUN set -e; \
  if [ -n "${ANSIBLE_VERSION}" ]; \
  then ANSIBLE="ansible==${ANSIBLE_VERSION}"; \
  else ANSIBLE="ansible"; \
  fi \
  && pip3 install --no-cache-dir --no-warn-script-location --user \
  ${ANSIBLE} \
  ansible-tower-cli \
  && ~/.local/bin/ansible-galaxy collection install amazon.aws \
  && ~/.local/bin/ansible-galaxy collection install azure.azcollection \
  && ~/.local/bin/ansible-galaxy collection install google.cloud \
  && ~/.local/bin/ansible-galaxy collection install kubernetes.core

RUN set -e; \
  if [ -n "$TERRAFORM_VERSION" ]; \
  then TERRAFORM="${TERRAFORM_VERSION}"; \
  else TERRAFORM="latest"; \
  fi \
  && git clone https://github.com/tfutils/tfenv.git ~/.tfenv \
  && ~/.tfenv/bin/tfenv install ${TERRAFORM} \
  && ~/.tfenv/bin/tfenv use ${TERRAFORM}

COPY --chown=${USER} files/.bashrc .bashrc
COPY --chown=${USER} files/.gitconfig .gitconfig
COPY --chown=${USER} files/.vimrc .vimrc

RUN mkdir /home/${USER}/data

VOLUME [ "/home/${USER}/data" ]

ENTRYPOINT [ "/bin/bash" ]
