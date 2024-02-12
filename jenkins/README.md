# Prerequisites
```
kubectl apply -f kubernetes/bzk-pvc-maven.yaml -n nm-bzk-jenkins;

kubectl apply -f kubernetes/owasp-zap-pvc.yaml -n nm-bzk-jenkins;

```
# Upgrade jenkins

current helm version release:

Chart: 3.2.1
App Version: 2.263.4


## Remove ruby-runtime and gitlab-hook plugin

- Remove internally ruby-runtime and gitlab-hook plugins , deleting corresponding *.jpi at path /var/jenkins_home/plugins.
- Execute from jenkins console:
```
System.setProperty("com.nirima.jenkins.plugins.docker.DockerContainerWatchdog.enabled", "false")
```

## Update version using ArgoCD
Chart: 4.1.17
App Version: 2.346.3

## Remove static plugins from  values yaml

- Remove static plugins from values_am|zw.yaml file. Changes will be applied by ArgoCD

## Review plugins from UI Management
- Update only the security warnings plugins.
- Update role-base authorizity (can be necessary remove *.jpi first)
- Update Build monitor view plugin.
- Update Kubernetes Plugin

## Remove helm management: 
```
kubectl delete secret -l owner=helm -l name=jenkins -n NAMESPACE
```

## EQAP Zwolle
### Jenkins Location
- Jenkins Location - Jenkins URL: https://jenkins-zw.rijksportaal.overheid-i.nl
- System Admin e-mail address: noreply@overheid-i.nl
### Global Properties
- DOCKER_HOST:tcp://172.22.13.10:4243
- DOCKER_HOST_COPY: tcp://172.21.9.10:4243
- GITLAB_EQAP: gitlab-zw.rijksportaal.overheid-i.nl
- GITLAB_EQAP_INSTANCE: gitlab-instance-6b925eb6
- HOST_ALIASES: 172.22.1.14
- JENKINS_EQAP:  jenkins-zw.rijksportaal.overheid-i.nl
- KEYCLOAK_EQAP: monitoring-zw.rijksportaal.overheid-i.nl
- newman: /home/node
- NEXUS_EQAP: nexus.rijksportaal.overheid-i.nl
- SONARQUBE_EQAP: sonarqube-zw.rijksportaal.overheid-i.nl
### Global Pipeline Libraries
- Name: gitlabdevopslibrary
- Default version: develop
### Extended E-mail Notification
- SMTP Server: mail.nm-bzk-mail.svc.cluster.local
- SMTP Port: 587
- TLS must be disabled.
### E-mail Notification
- SMTP Server: mail.nm-bzk-mail.svc.cluster.local
- Default user e-mail suffix: @capgemini.com
### SonarQube servers
- Name: sonarqube
- Server URL: https://sonarqube-zw.rijksportaal.overheid-i.nl
- Server authentication token: sonarqube

## Configure Clouds
### Kubernetes
- Name: Kubernetes
- Kubernetes URL: https://kubernetes.default
- Kubernetes Namespace: nm-bzk-jenkins
- Jenkins URL: http://jenkins.nm-bzk-jenkins.svc.cluster.local:8080
- Jenkins tunnel: jenkins-agent.nm-bzk-jenkins.svc.cluster.local:50000
### Pod Label
- Key: jenkins/jenkins-jenkins-agent
- Value: true
### Gitlab Integration - Jenkins
- Enable Integration: Active
- Trigger: Push
- Jenkins server URL (example: mailservice): http://jenkins.nm-bzk-jenkins.svc.cluster.local:8080/multibranch-webhook-trigger/invoke?token=multibranch-token-mailservice
- Project Name: govportal_mailservice



## Trying piplienes over future environment.
- Mailservice pipline (future).
- Backend pipeline (future).
- Full deployment (future).


