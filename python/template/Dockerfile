# OS
FROM python:3.10-slim-bookworm

# SSH
ARG UID=1000
ARG GID=1000
ARG USER="devuser"
ARG PASS="password"

RUN apt-get update && \
    apt-get install -y openssh-server wget sudo git && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    groupadd -g $GID $USER && \
    useradd -g $GID -u $UID -G sudo -m $USER && \
    echo "$USER:$PASS" | chpasswd && \
    echo "root:$PASS" | chpasswd

# Jupyterlab
RUN pip install jupyterlab
EXPOSE 8888

# Project
USER $UID:$GID
RUN mkdir "/home/$USER/project"
WORKDIR "/home/$USER/project"
COPY --chown=$UID:$GID . "/home/$USER/project"
RUN pip install --user --no-cache-dir -e .[dev]

# Start ssh
USER root
EXPOSE 22
CMD service ssh start -D
