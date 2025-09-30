# Docker image customizing
# Source: https://github.com/cloudposse/geodesic#customizing-your-docker-image
ARG GEODESIC_REPOSITORY=cloudposse/geodesic
ARG GEODESIC_TAG=4.4.0-debian

# renovate: datasource=github-releases depName=jdx/mise
ARG MISE_VERSION=v2025.9.24

FROM ${GEODESIC_REPOSITORY}:${GEODESIC_TAG}

ENV BANNER="toolbox"

ENV DOCKER_IMAGE="hajlesilesia/toolbox"
ENV DOCKER_TAG="latest"

# SSH server configuration for remote development
RUN adduser \
      --disabled-password \
	  --comment "" \
	  local_dev && \
    echo "local_dev:local_dev" | chpasswd
RUN apt-get update && apt-get install -y \
	  openssh-server \
      socat \
      docker.io && \
    echo "socat TCP-LISTEN:\${GEODESIC_PORT},fork,reuseaddr TCP:localhost:30000 &" >> ~/.bashrc && \
    echo "service ssh start" >> ~/.bashrc

# System-wide git safe directory to avoid ownership issues in the filesystem.
# Reasoning: in the remote (CI/CD workflows) or local usage of this image, the user cloning the repository will be different than the one operating on it.
COPY .gitconfig /etc/gitconfig

# Atmos configuration
# Source: https://atmos.tools/core-concepts/components/terraform/remote-state/#recommended-options
COPY rootfs/ /

# Mise installation
# Source: https://mise.jdx.dev/getting-started.html
ARG MISE_VERSION
ARG MISE_INSTALL_PATH="/usr/local/bin/mise"
ARG MISE_DATA_DIR="/usr/share/xdg_data_home/mise"
RUN curl https://mise.run | \
    MISE_VERSION="${MISE_VERSION}" \
    MISE_INSTALL_PATH="${MISE_INSTALL_PATH}" \
    MISE_DATA_DIR="${MISE_DATA_DIR}" \
    sh
ENV PATH="${MISE_DATA_DIR}/shims:${PATH}"
# Defaults for all users
# Source: https://mise.jdx.dev/configuration.html#system-config-etc-mise-config-toml
COPY .mise.toml /etc/mise/config.toml
# Install tools
# Source: https://mise.jdx.dev/cli/install.html
RUN mise trust && mise install --yes
