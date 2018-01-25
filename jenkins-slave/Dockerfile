FROM docker:17.11

ENV SWARM_CLIENT_VERSION="3.6" \
    DOCKER_COMPOSE_VERSION="1.17.1" \
    COMMAND_OPTIONS="" \
    USER_NAME_SECRET="" \
    PASSWORD_SECRET=""

ARG user=jenkins
ARG group=jenkins
ARG uid=1000
# replace this with docker group on the host
# stat /var/run/docker.sock
ARG gid=998

RUN addgroup -g ${gid} ${group} \
    && adduser -u ${uid} -G ${group} -s /bin/sh -D ${user}

RUN apk --update --no-cache add openjdk8-jre python py-pip git openssh ca-certificates openssl && \
    wget -q https://repo.jenkins-ci.org/releases/org/jenkins-ci/plugins/swarm-client/${SWARM_CLIENT_VERSION}/swarm-client-${SWARM_CLIENT_VERSION}.jar -P /home/jenkins/ && \
    pip install docker-compose

COPY run.sh /home/jenkins/run.sh
RUN chmod +x /home/jenkins/run.sh

USER ${user}

CMD ["/home/jenkins/run.sh"]
