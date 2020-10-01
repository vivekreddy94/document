# ELK stack on kubernetes cluster
Elasticsearch, Logstash, Kibana and filebeat installation on kubernetes cluster using Jenkins CICD pipeline.

## Prerequisites
* kubernetes installed
* Ansible >= 2.9 version (If Jenkins installation is also needed)

## Jenkins installation
Jenkins installation is done via ansible-playbook with a custom docker image and making use of jenkins configuration as code plugin.
### Build Jenkins image
Dockerfile includes installation of
* Jenkins suggested plugins
* Docker installation to enable docker commands inside Jenkins
* Ansible package
* kubectl binary
![name-of-you-image](https://github.com/vivekreddy94/document/blob/main/elk_architecture.png)
