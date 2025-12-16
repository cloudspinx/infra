FROM --platform=linux/amd64 ghcr.io/runatlantis/atlantis:v0.38.0

USER root

# Tool Versions
# https://github.com/opentofu/opentofu/releases
ARG OPENTOFU_VERSION=1.11.1
# https://github.com/gruntwork-io/terragrunt/releases
ARG TERRAGRUNT_VERSION=0.96.0
# https://github.com/hashicorp/vault/releases
ARG VAULT_VERSION=1.21.1

# Install dependencies
RUN apk add --no-cache \
  curl \
  git \
  bash \
  jq \
  openssl \
  unzip \
  ca-certificates \
  zip \
  vim \
  libc6-compat

# Install OpenTofu
RUN curl -Lo /tmp/tofu.zip https://github.com/opentofu/opentofu/releases/download/v${OPENTOFU_VERSION}/tofu_${OPENTOFU_VERSION}_linux_amd64.zip \
  && unzip /tmp/tofu.zip -d /usr/local/bin/ \
  && rm /tmp/tofu.zip \
  && chmod +x /usr/local/bin/tofu

# Install Terragrunt
RUN curl -Lo /usr/local/bin/terragrunt https://github.com/gruntwork-io/terragrunt/releases/download/v${TERRAGRUNT_VERSION}/terragrunt_linux_amd64 \
  && chmod +x /usr/local/bin/terragrunt

# Install Vault
RUN curl -Lo /tmp/vault.zip https://releases.hashicorp.com/vault/${VAULT_VERSION}/vault_${VAULT_VERSION}_linux_amd64.zip \
  && unzip /tmp/vault.zip -d /usr/local/bin/ \
  && rm /tmp/vault.zip \
  && chmod +x /usr/local/bin/vault

USER atlantis
