FROM pivotaldata/centos-gpdb-dev:5

RUN yum install -q -y ccache
# backport the no-tty fix from RHEL7 to RHEL5
RUN sed -i -e "s/Defaults    requiretty.*/ #Defaults    requiretty/g" /etc/sudoers

# generate server keys
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N ''

RUN useradd --password '$5$uitFy1zdHuG$f7NutSS0aAyl7EqqPBhKZOk4iKJzxFkGGYd8s2hERM8' --group adm gpadmin \
	&& echo '%adm ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

RUN chmod g+w -R /opt \
	&& chgrp -R adm /opt

WORKDIR /build
RUN sudo chown -R gpadmin:gpadmin /build

USER gpadmin

RUN ssh-keygen -f ~/.ssh/id_rsa -N '' \
	&& cp ~/.ssh/{id_rsa.pub,authorized_keys}

CMD sudo /usr/sbin/sshd -D
LABEL io.github.d.uber-script="4.3"
ENV JAVA_HOME=/usr/lib/jvm/java-openjdk
