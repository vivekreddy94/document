# ELK stack on kubernetes cluster
Elasticsearch, Logstash, Kibana and filebeat installation on kubernetes cluster using Jenkins CICD pipeline.

## Prerequisites
* kubernetes
* Ansible >= 2.9 version
* Docker

## Jenkins Installation
Jenkins installation is done via ansible-playbook with a custom docker image and making use of jenkins configuration as code plugin.
### Jenkins image
Dockerfile includes installation of
* Jenkins basic suggested plugins along with plugins like git, kubernetes, job-dsl etc
* Docker installation to enable docker commands inside Jenkins
* Ansible package
* kubectl package
### Jenkins slave image
A custom slave image is built from official jnlp image. This is important as we run all our ELK stack playbooks here. Similar to Jenkins image, Dockerfile consists of below 
* Docker
* Ansible
* kubectl
* kubeval
* polaris
### Jenkins configuration
Configuration is completely managed through Jenkins configuration as code plugin. It includes
* Setting up user access.
* Creates kubernetes cloud and adding pod template with our custom jenkins slave image in it
* Create our first job for ELK CICD pipeline with github hook enabled.
### Before running jenkins playbook
- Copy kubeconfig file to user home directory path("~/.kube/config") if not already present.
- Update below variables in jenkins inventory file
  * kubernetes api server address
  * **kubeconfig_file** - Convert file into base64 format before adding to inventory. 
   ```
   cat ~/.kube/config | base64 -w 0
   ```
  * **docker_login_username** - Docker hub login username
  * **docker_login_password**  - Docker hub login password
  * **github_repo_token** - Token to github repository
  * **adminuser_password** - Password for logging to Jenkins instance as "admin" user.

If needed other variable values can be changed based on requirement.

Note: For encrypting passwords and kubeconfig content before adding to inventory file, follow https://docs.ansible.com/ansible/latest/user_guide/vault.html#creating-encrypted-variables

### Running Jenkins playbook
Execute below ansible command and it should prompt for the vault password which is used previously to encrypt strings.
```
ansible-playbook jenkins.yml -i inventories/stage --ask-vault-pass
```
### Workflow of Jenkins playbook
* Builds core Jenkins image with custom plugins and packages. Pushes it to Dockerhub.
* Installs Jenkins on kubernetes cluster using ansible k8s module. i.e applying role-binding, configmap, service, deployment etc
* Builds Jenkins slave image and push it to Dockerhub

## ELK Stack
Elasticsearch, logstash, kibana and filebeats together are ELK stack. Installation includes three pod elasticsearch cluster, two logstash, one Kibana and one filebeat.
Below is the architecture of ELK setup.

![name-of-you-image](https://github.com/vivekreddy94/document/blob/main/elk_architecture.png)
