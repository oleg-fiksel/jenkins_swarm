FROM jenkins:alpine

ENV JENKINS_VERSION 2.89.3

# jenkins.war checksum, download will be validated using it
ARG JENKINS_SHA=79f176e9388cdeb0deb0e6af49ff9d1ff4b7a3c47102af11e3e9461bf268983a
ARG JENKINS_URL=https://repo.jenkins-ci.org/public/org/jenkins-ci/main/jenkins-war/${JENKINS_VERSION}/jenkins-war-${JENKINS_VERSION}.war

USER root

RUN curl -fsSL ${JENKINS_URL} -o /usr/share/jenkins/jenkins.war \
  && echo "${JENKINS_SHA}  /usr/share/jenkins/jenkins.war" | sha256sum -c -
RUN chown jenkins /usr/share/jenkins/jenkins.war

USER jenkins

COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

HEALTHCHECK --timeout=5s --interval=5s --start-period=1m --retries=3 CMD curl -k http://localhost:8080 || exit 1
