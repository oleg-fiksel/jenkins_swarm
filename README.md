# Jenkins on Docker Swarm

# Abbriviations

* Docker-EE - Docker Enterprise Edition
* UCP - Universal Control Plane. Docker-EE Web UI.
* DTR - Docker Trusted Registry

# Dependencies

* Access via ucp-bundle to a running docker-ee swarm with internet access
* docker-compose

# Preparations

## Create secrets in UCP

* In UCP add the following secrets to values you like. Those will be the user jenkins slave will connect to jenkins master to register itself
  * `jenkins_slave_user`
  * `jenkins_slave_password`

## Adjust volumes

Adjust volumes section in `docker-compose.yml` to your needs. Maybe you want to use nfs:

```
volumes:
  jenkins-master-home:
    driver_opts:
      type: "nfs"
      o: "addr=my-nfs-server-hostname,nolock,soft,rw"
      device: ":/mnt/export/jenkins-master-home"
```

# Build images

Better way is to use DTR for the images.

* `docker-compose build`

# Deploy docker stack

* `docker stack deploy -c docker-compose.yml jenkins`
* Wait a couple of minutes until services are started (see in UCP)

# Setup Jenkins

* Get jenkins first time password
   * `docker service logs jenkins_jenkins-master 2>&1 |grep -A 10 'Jenkins initial setup is required'`
* Browse to http://hostname-of-docker-host:8080 and setup jenkins
* Complete the wizard
* create new pipeline job
  * example pipelines are in the folder `example_pipelines`
* create used you set in UCP from the start with the same password

# Scaling Jenkins slaves

* `docker service scale jenkins_jenkins-slave=2`
* second jenkins slave will be automatically registered

# Troubleshooting

## Inspect the logs

* `docker service logs -f jenkins_jenkins-master`
* `docker service logs -f jenkins_jenkins-slave`

# Known issues

## no_proxy env variable is not working

Details: https://issues.jenkins-ci.org/browse/JENKINS-42930
