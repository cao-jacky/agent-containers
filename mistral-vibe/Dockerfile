FROM agent-base AS builder

USER root
RUN apt-get update && \
  apt-get install -y --no-install-recommends python3 python3-pip python3-venv curl bash && \
  rm -rf /var/lib/apt/lists/*

USER node
ENV HOME=/home/node
ENV PATH="/home/node/.local/bin:$PATH"

RUN curl -LsSf https://mistral.ai/vibe/install.sh | bash

# Runtime stage
FROM agent-base

# Install runtime Python dependencies
USER root
RUN apt-get update && \
  apt-get install -y --no-install-recommends python3 python3-pip python3-venv && \
  rm -rf /var/lib/apt/lists/*

# Copy uv and mistral-vibe installation from builder
COPY --from=builder --chown=node:node /home/node/.local /home/node/.local

# Ensure executables have correct permissions
RUN chmod -R +x /home/node/.local/bin

# Switch back to node user
USER node

# Add uv and mistral-vibe to PATH
ENV PATH="/home/node/.local/bin:$PATH"

# Display version information
RUN echo "" && \
  echo "\e[34m=================================================================\e[0m" && \
  echo "\e[34m                   MISTRAL VIBE VERSION                          \e[0m" && \
  echo "\e[34m=================================================================\e[0m" && \
  echo "\e[34m" && vibe --version && \
  echo "\e[0m" && \
  echo ""

CMD ["vibe"]
