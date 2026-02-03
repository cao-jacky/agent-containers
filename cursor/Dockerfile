FROM agent-base AS builder

USER root
RUN apt-get update && \
    apt-get install -y --no-install-recommends ca-certificates curl bash && \
    rm -rf /var/lib/apt/lists/*

# Install as root instead of node
ENV HOME=/root
ENV PATH="/root/.local/bin:${PATH}"

# Install Cursor Agent using the official script
RUN curl -fsS https://cursor.com/install | bash

# Ensure bin dir exists (some installers may not create it)
RUN mkdir -p /root/.local/bin


FROM agent-base

USER root
RUN apt-get update && \
    apt-get install -y --no-install-recommends ca-certificates bash && \
    rm -rf /var/lib/apt/lists/*

# Copy the full Cursor agent installation from builder
COPY --from=builder /root/.local/share/cursor-agent /root/.local/share/cursor-agent

# Create bin dir + recreate symlinks to the installed cursor-agent binary (dynamic version)
RUN mkdir -p /root/.local/bin && \
    AGENT_BIN="$(echo /root/.local/share/cursor-agent/versions/*/cursor-agent)" && \
    ln -sf "$AGENT_BIN" /root/.local/bin/agent && \
    ln -sf "$AGENT_BIN" /root/.local/bin/cursor-agent && \
    chmod -R a+rx /root/.local/share/cursor-agent/versions

ENV HOME=/root
ENV PATH="/root/.local/bin:${PATH}"

# Display version information
RUN echo "" && \
  echo "\e[34m=================================================================\e[0m" && \
  echo "\e[34m                   CURSOR AGENT VERSION                          \e[0m" && \
  echo "\e[34m=================================================================\e[0m" && \
  echo "\e[34m" && agent --version && \
  echo "\e[0m" && \
  echo ""

CMD ["agent"]