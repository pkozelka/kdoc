# Daniel Malczyk
# ThinkBig Analytics, a Teradata Company

#basic image with CentOS and latest JDK
FROM airhacks/java

MAINTAINER Daniel Malczyk <dmalczyk@gmail.com>

# install dev tools
RUN yum clean all; \
    rpm --rebuilddb; \
    yum install -y curl which tar sudo openssh-server openssh-clients rsync; \
    yum clean all

# passwordless ssh
RUN ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN ssh-keygen -q -N "" -t rsa -f /root/.ssh/id_rsa
RUN ssh-keygen -q -N "" -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key
RUN ssh-keygen -q -N "" -t ed25519 -f /etc/ssh/ssh_host_ed25519_key
RUN cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys

# create nifi user and group
RUN /bin/bash -c 'useradd -r -m -s /bin/bash nifi'

# download and install NiFi with Kylo-provided script
COPY conf/install-nifi.sh .
RUN chmod u+x ./install-nifi.sh && \
    ./install-nifi.sh /opt/nifi nifi users

#VOLUME /opt/nifi/data

COPY scripts/nifi_bootstrap.sh /etc/nifi_bootstrap.sh
RUN chown root.root /etc/nifi_bootstrap.sh && \
    chmod u+x /etc/nifi_bootstrap.sh

ENTRYPOINT ["/etc/nifi_bootstrap.sh"]

# expose NiFi UI
EXPOSE 8079