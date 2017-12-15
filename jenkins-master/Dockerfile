
FROM jenkins:2.60.2-alpine
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

HEALTHCHECK --timeout=5s --interval=5s --start-period=1m --retries=3 CMD curl -k http://localhost:8080 || exit 1
