# Official Microsoft devcontainer image — multi-arch (amd64 + arm64), so
# this builds natively on ARM64 laptops (e.g. Snapdragon Windows devices)
# under Docker Desktop's WSL2 backend, no x64 emulation.
FROM mcr.microsoft.com/devcontainers/python:3.11-bookworm

# Beyond git/python (from the base image) and gh (from the github-cli
# feature in devcontainer.json), the day-to-day loop on this project needs:
#   curl, ca-certificates - installers below, and general HA REST calls
#   mosquitto-clients     - mosquitto_pub/mosquitto_sub, to watch or poke
#                            FPP's and Home Assistant's MQTT traffic directly
#   dnsutils               - dig, for checking DNS/reservations
#   iputils-ping            - ping, for basic reachability checks
#   openssh-client          - ssh fpp@<pi-ip> (usually already present, kept explicit)
RUN apt-get update && apt-get install -y --no-install-recommends \
      curl \
      ca-certificates \
      mosquitto-clients \
      dnsutils \
      iputils-ping \
      openssh-client \
    && rm -rf /var/lib/apt/lists/*

# Claude Code, from Anthropic's native installer (not npm) - no Node.js
# runtime dependency, auto-updates in the background. Installed as the
# non-root "vscode" user (the base image's default user, also set as
# remoteUser in devcontainer.json) so it lands in ~/.local and doesn't
# need root to update itself later.
USER vscode
RUN curl -fsSL https://claude.ai/install.sh | bash
ENV PATH="/home/vscode/.local/bin:${PATH}"
