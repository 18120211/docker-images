FROM ubuntu:18.04

RUN apt-get update && \
    apt-get install -y openssh-server

RUN useradd remote_user && \
    echo "remote_user:remote_user" | chpasswd && \
    mkdir -p /home/remote_user/.ssh && \
    chmod 700 /home/remote_user/.ssh && \
    mkdir -p -m 755 /var/run/sshd

COPY ./remote-key.pub /home/remote_user/.ssh/authorized_keys

RUN chown remote_user:remote_user -R /home/remote_user && \
    chmod 400 /home/remote_user/.ssh/authorized_keys

RUN apt-get install -y mysql-client 

RUN apt-get install -y python3-pip && \
    pip3 install --upgrade pip && \
    pip3 install awscli

# Install Docker
RUN apt-get update && \
    apt-get -y install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
    https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get -y install docker-ce

# Install docker-compose
RUN curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose

RUN apt-get install -y openjdk-11-jre

RUN mkdir -m 755 /root/.ssh

COPY ./remote-key.pub /root/.ssh/authorized_keys

RUN usermod -aG docker remote_user

CMD /usr/sbin/sshd -D