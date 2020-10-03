# ELK stack on kubernetes cluster
Elasticsearch, Logstash, Kibana and filebeat installation on kubernetes cluster using Jenkins CICD pipeline.

## Prerequisites
* kubernetes installed
* Ansible >= 2.9 version

## Jenkins installation
Jenkins installation is done via ansible-playbook with a custom docker image and making use of jenkins configuration as code plugin.
### Build Jenkins image
Dockerfile includes installation of
* Jenkins basic suggested plugins along with plugins like git, kubernetes, job-dsl etc
* Docker installation to enable docker commands inside Jenkins
* Ansible package
* kubectl package
### Before running jenkins playbook
Update jenkins inventory file with
* kubernetes api server address
* Kubeconfig path - Path to your kubeconfig file
* kubeconfig file - Convert file into base64 format before adding to inventory. For example run "cat kubeconfig_file | base64 -w 0", copy ouput to the inventory file

![name-of-you-image](https://github.com/vivekreddy94/document/blob/main/elk_architecture.png)
