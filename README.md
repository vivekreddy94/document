# ELK stack on kubernetes cluster
Elasticsearch, Logstash, Kibana and filebeat installation on kubernetes cluster using Jenkins CICD pipeline.

## Prerequisites
* kubernetes
* Ansible >= 2.9 version
* Docker

## Jenkins installation
Jenkins installation is done via ansible-playbook with a custom docker image and making use of jenkins configuration as code plugin.
### Jenkins image
Dockerfile includes installation of
* Jenkins basic suggested plugins along with plugins like git, kubernetes, job-dsl etc
* Docker installation to enable docker commands inside Jenkins
* Ansible package
* kubectl package
### Before running jenkins playbook
- Copy kubeconfig file to user home directory ("~/.kube/config") if not already present.
- Update below variables in jenkins inventory file
  * kubernetes api server address
  * kubeconfig_file - Convert file into base64 format before adding to inventory. For example run "cat kubeconfig_file | base64 -w 0", copy ouput to the inventory file.
  * docker_login_username - Docker hub login username
  * docker_login_password  - Docker hub login password
  * github_repo_token - Token to github repository
  * adminuser_password - Password for logging to Jenkins instance as "admin" user.

Note: For encrypting passwords and kubeconfig content before adding to inventory file, follow https://docs.ansible.com/ansible/latest/user_guide/vault.html#creating-encrypted-variables

![name-of-you-image](https://github.com/vivekreddy94/document/blob/main/elk_architecture.png)
