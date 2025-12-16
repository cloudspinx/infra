FROM ghcr.io/actions/actions-runner:latest

USER root

# Install essential packages
RUN apt-get update && apt-get install -y \
  wget \
  gnupg \
  unzip \
  make \
  curl \
  binutils \
  tar \
  gettext \
  xz-utils \
  apt-transport-https \
  ca-certificates \
  libasound2 \
  libatk1.0-0 \
  libcairo2 \
  libcups2 \
  libdrm2 \
  libxcomposite1 \
  libxdamage1 \
  libxrandr2 \
  libgbm1 \
  libgtk-3-0 \
  libnspr4 \
  libnss3 \
  zip \
  build-essential \
  gcc \
  xdg-utils \
  fonts-liberation \
  --no-install-recommends && \
  rm -rf /var/lib/apt/lists/*

# Install Docker Compose based on the architecture
RUN mkdir -p /usr/local/lib/docker/cli-plugins && \
  dpkgArch="$(dpkg --print-architecture)" && \
  case "${dpkgArch##*-}" in \
  amd64) compose_arch="x86_64";; \
  arm64) compose_arch="aarch64";; \
  armhf) compose_arch="armhf";; \
  *) echo "Unsupported architecture"; exit 1 ;; \
  esac && \
  curl -SL "https://github.com/docker/compose/releases/download/v2.24.6/docker-compose-linux-$compose_arch" -o /usr/local/lib/docker/cli-plugins/docker-compose && \
  chmod +x /usr/local/lib/docker/cli-plugins/docker-compose

# Add the official GitHub CLI repository to the list of sources
RUN curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | \
  dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg && \
  chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg && \
  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | \
  tee /etc/apt/sources.list.d/github-cli.list > /dev/null && \
  apt-get update && apt-get install -y gh && \
  apt-get clean && rm -rf /var/lib/apt/lists/*

# Detect architecture and download/install AWS CLI v2
RUN dpkgArch="$(dpkg --print-architecture)" \
  && case "${dpkgArch##*-}" in \
  amd64) aws_arch="x86_64";; \
  arm64) aws_arch="aarch64";; \
  armhf) aws_arch="arm";; \
  *) echo "Unsupported architecture"; exit 1 ;; \
  esac \
  && echo "Detected architecture: $aws_arch" \
  && wget "https://awscli.amazonaws.com/awscli-exe-linux-$aws_arch.zip" -O "awscliv2.zip" \
  && unzip awscliv2.zip \
  && ./aws/install \
  && rm -rf awscliv2.zip ./aws

# Install Google Cloud CLI
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && \
  curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg && \
  apt-get update && apt-get install -y google-cloud-cli && \
  rm -rf /var/lib/apt/lists/*

# Install Skaffold
RUN dpkgArch="$(dpkg --print-architecture)" && \
  case "${dpkgArch##*-}" in \
  amd64) skaffold_arch="amd64";; \
  arm64) skaffold_arch="arm64";; \
  *) echo "Unsupported architecture"; exit 1 ;; \
  esac && \
  curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-$skaffold_arch && \
  install skaffold /usr/local/bin/ && \
  rm skaffold

USER runner
