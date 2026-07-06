# Tamarin devcontainer
# From https://raw.githubusercontent.com/Luchinzzz/tamarin-docker-container/refs/heads/main/Dockerfile
FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG="en_US.UTF-8"
ENV MAUDE_URL=https://github.com/maude-lang/Maude/releases/download/Maude3.5.1/Maude-3.5.1-linux-x86_64.zip
ENV PATH="/root/.local/bin:$PATH"

# Install base dependencies
RUN apt-get update && apt-get install -y \
    nodejs \
    npm \
    build-essential \
    ca-certificates \
    curl \
    git \
    unzip \
    locales \
    libtinfo-dev \
    zlib1g-dev \
    libgmp-dev\
    && update-ca-certificates \
    && rm -rf /var/lib/apt/lists/* 

RUN apt-get update \
    && apt-get upgrade -y

RUN locale-gen "en_US.UTF-8"

# Create user
RUN useradd -m -s /bin/bash user && \
    echo 'user ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers && \
    mkdir -p /home/user/.local/bin && \
    chown -R user:user /home/user/.local/ 
    
ENV PATH="/home/user/.local/bin:$PATH"

# Install Stack
RUN curl -sSL https://get.haskellstack.org/ | sh -s - -d /home/user/.local/bin


# Install Maude via zip
RUN curl -L -o maude.zip $MAUDE_URL \
    && unzip maude.zip -d Maude-3.5.1-linux-x86_64\
    && chmod +x  Maude-3.5.1-linux-x86_64/maude \
    && mv Maude-3.5.1-linux-x86_64/maude /home/user/.local/bin/maude \
    && mv Maude-3.5.1-linux-x86_64/prelude.maude /home/user/.local/bin/prelude.maude \
    && rm -rf maude.zip Maude-3.5.1-linux-x86_64


# Install graphviz
RUN apt-get install -y graphviz

# Clone Tamarin
USER user
WORKDIR /home/user/
RUN git clone https://github.com/tamarin-prover/tamarin-prover.git
WORKDIR /home/user/tamarin-prover

# Build Tamarin
RUN make default

# Test installation
RUN tamarin-prover test
