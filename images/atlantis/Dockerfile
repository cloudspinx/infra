FROM ghcr.io/opentofu/opentofu:1.11.1-minimal as binary
FROM hashicorp/vault:1.21 as vault-cli
FROM ghcr.io/runatlantis/atlantis:v0.38.0

USER root

COPY --from=binary /usr/local/bin/tofu /usr/local/bin/tofu
COPY --from=vault-cli /bin/vault /usr/local/bin/vault

RUN apk add --no-cache jq bash postgresql-client uv nodejs npm yarn python3 curl google-cloud-sdk

ENV PATH $PATH:/usr/local/google-cloud-sdk/bin

USER atlantis
