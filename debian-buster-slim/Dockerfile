FROM debian:buster-slim

ENV USER steam
ENV HOME_DIR /home/${USER}

# Add user
RUN useradd -m ${USER}

# Install ca-certificates to fix "src/common/opensslconnection.cpp (1529) : Assertion Failed: unable to load trusted SSL root certificates"
ARG DEBIAN_FRONTEND=noninteractive
RUN dpkg --add-architecture i386 \
    && apt-get update -y \
    && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends ca-certificates lib32gcc1 curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Get steam files
WORKDIR ${HOME_DIR}/.steam
RUN curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -

# Create symlink for steamcmd
RUN ln -s ${HOME_DIR}/.steam/linux32/steamcmd /usr/bin/steamcmd

# Fix permissions
RUN chown -R ${USER}: ${HOME_DIR}/.steam

# Update steamcmd to latest version
USER ${USER}
RUN steamcmd +quit

ENTRYPOINT ["steamcmd"]
CMD ["+help", "+quit"]