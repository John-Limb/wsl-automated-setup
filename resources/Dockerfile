#The base image
FROM ubuntu:22.10
#Set Timezone
ENV TZ=Europe/London
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# Add user
ARG USER

RUN mkdir -p /scripts
COPY ./resources/bootstrap.sh /scripts
WORKDIR /scripts
RUN chmod +x bootstrap.sh
RUN ./bootstrap.sh

#Add current user to environment 
RUN /bin/bash -c 'useradd -m -s /bin/bash -g root ${USER}; echo "${USER}:PassW0rd" | chpasswd'

#Add wsl settings to change mount point and default user
COPY ./resources/wsl.conf /etc/wsl.conf
RUN /bin/bash -c 'echo "default=${USER}" >> /etc/wsl.conf; \
    echo "${USER} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers;'
RUN systemctl enable --now code-server@$USER
#Set the default user
USER $USER

#Add default .bashrc for new user
COPY --chown=$USER:root ./resources/bashrc /home/${USER}/.bashrc 


