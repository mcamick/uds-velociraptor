FROM ubuntu:22.04
LABEL version="Velociraptor v0.73.4"
LABEL description="Velociraptor server in a Docker container"
COPY ./entrypoint .
RUN chmod +x entrypoint && \
    apt-get update && \
    apt-get install -y curl wget jq rsync && \
    # Create dirs for Velo binaries
    mkdir -p /opt/velociraptor && \
    for i in linux mac windows; do mkdir -p /opt/velociraptor/$i; done && \
    # Detect architecture and get appropriate Velox binaries
    ARCH=$(uname -m) && \
    if [ "$ARCH" = "x86_64" ]; then \
        LINUX_ARCH="amd64"; \
        MAC_ARCH="amd64"; \
    elif [ "$ARCH" = "aarch64" ]; then \
        LINUX_ARCH="arm64"; \
        MAC_ARCH="arm64"; \
    else \
        LINUX_ARCH="amd64"; \
        MAC_ARCH="amd64"; \
    fi && \
    WINDOWS_EXE=$(curl -s https://api.github.com/repos/velocidex/velociraptor/releases/latest | jq -r '[.assets | sort_by(.created_at) | reverse | .[] | .browser_download_url | select(test("windows-amd64.exe$"))][0]') && \
    WINDOWS_MSI=$(curl -s https://api.github.com/repos/velocidex/velociraptor/releases/latest | jq -r '[.assets | sort_by(.created_at) | reverse | .[] | .browser_download_url | select(test("windows-amd64.msi$"))][0]') && \
    LINUX_BIN=$(curl -s https://api.github.com/repos/velocidex/velociraptor/releases/latest | jq -r "[.assets | sort_by(.created_at) | reverse | .[] | .browser_download_url | select(test(\"linux-${LINUX_ARCH}$\")) | select(test(\".sig$\") | not)][0]") && \
    MAC_BIN=$(curl -s https://api.github.com/repos/velocidex/velociraptor/releases/latest | jq -r "[.assets | sort_by(.created_at) | reverse | .[] | .browser_download_url | select(test(\"darwin-${MAC_ARCH}$\"))][0]") && \
    wget -O /opt/velociraptor/linux/velociraptor "$LINUX_BIN" && \
    chmod +x /opt/velociraptor/linux/velociraptor && \
    wget -O /opt/velociraptor/mac/velociraptor_client "$MAC_BIN" && \
    chmod +x /opt/velociraptor/mac/velociraptor_client && \
    wget -O /opt/velociraptor/windows/velociraptor_client.exe "$WINDOWS_EXE" && \
    wget -O /opt/velociraptor/windows/velociraptor_client.msi "$WINDOWS_MSI" && \
    # Clean up
    apt-get remove -y --purge wget && \
    apt-get clean

# Create non-root user for security
RUN groupadd -g 1000 velociraptor && \
    useradd -u 1000 -g 1000 -s /bin/bash -m velociraptor && \
    mkdir -p /velociraptor && \
    chown -R velociraptor:velociraptor /velociraptor /opt/velociraptor

USER velociraptor
WORKDIR /velociraptor
CMD ["/entrypoint"]
