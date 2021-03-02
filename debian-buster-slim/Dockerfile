FROM debian:buster-slim

ENV USER steam

# Add user
RUN useradd -m ${USER}

# Install dependencies
ARG DEBIAN_FRONTEND=noninteractive
RUN dpkg --add-architecture i386 \
    && apt-get update -y \
    && apt-get install -y --no-install-recommends ca-certificates lib32gcc1 curl \
    && rm -rf /var/lib/apt/lists/*

ENV STEAMCMD_DIR /home/${USER}/.steam

# Install steamcmd manually
WORKDIR ${STEAMCMD_DIR}
RUN curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -

# Fix user permissions
RUN chown -R ${USER}: ${STEAMCMD_DIR}

# Add script to run steamcmd 
COPY exec-steamcmd.sh /usr/local/bin/steamcmd

# Update steamcmd to latest version
USER ${USER}
RUN steamcmd +quit

ENTRYPOINT ["steamcmd"]
CMD ["+help", "+quit"]