FROM ubuntu:18.04

# Install pacakges
RUN apt update && apt install default-jdk -y && apt install maven -y && apt install git -y

# Install ssh
RUN apt-get -y install openssh-client

# Confugure ssh client
COPY pk /root/.ssh/pk
RUN mv /root/.ssh/pk /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa

# Install docker 18.03
RUN apt-get -y install apt-transport-https ca-certificates curl gnupg2 software-properties-common
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
RUN apt-get update
RUN apt-get -y install docker-ce docker-ce-cli containerd.io

# Clear cache
RUN apt-get clean