FROM ubuntu:20.04

RUN apt-get update && \
    apt-get install -y openssh-server

RUN useradd remote_user && \
    echo "remote_user:remote_user" | chpasswd && \
    mkdir -p /home/remote_user/.ssh && \
    chmod 700 /home/remote_user/.ssh && \
    mkdir -p -m 755 /var/run/sshd

COPY ./remote-key.pub /home/remote_user/.ssh/authorized_keys

RUN chown remote_user:remote_user -R /home/remote_user

RUN apt-get install -y python3-pip

RUN pip3 install --upgrade pip

RUN pip3 install awscli

RUN mkdir -m 755 /root/.ssh

COPY ./remote-key.pub /root/.ssh/authorized_keys

RUN chmod 400 /root/.ssh/authorized_keys

CMD /usr/sbin/sshd -D