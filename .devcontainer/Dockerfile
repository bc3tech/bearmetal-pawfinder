ARG TARGET_ARCH="linux/amd64"
FROM --platform=$TARGET_ARCH ghcr.io/techiro/devcontainer-template/flutter:latest

# Install Linux development dependencies for Flutter
RUN apt-get update -qq && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    clang \
    cmake \
    ninja-build \
    pkg-config \
    libgtk-3-dev \
    chromium-browser && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Add pub cache bin to PATH
ENV PATH="/root/.pub-cache/bin:$PATH"

# Set Flutter to stable channel
# RUN flutter channel stable && \
#     flutter doctor

# Set working directory
WORKDIR /workspaces/pawfinder
