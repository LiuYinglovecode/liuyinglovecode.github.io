---
title: Deploy kubernetes in Ubuntu 20.04
date: 2020-9-4 20:15:29
tags:
- k8s
- Kubernetes
- Ubuntu
- Linux
categories:
- Kubernetes
---

### Kubernetes dashboard

As we see above the kubernetes-dashboard service in the kube-system namespace has a ClusterIP of 10.152.183.64 and listens on TCP port 443. The ClusterIP is randomly assigned, so if you follow these steps on your host, make sure you check the IP adress you got. Point your browser to https://10.1.29.1:30000 and you will see the kubernetes dashboard UI. To access the dashboard use the default token retrieved with:
``` bash
token=$(microk8s kubectl -n kube-system get secret | grep default-token | cut -d " " -f1)
microk8s kubectl -n kube-system describe secret $token
```
then the terminal will show the token you need to fill in the website:
``` bash
Name:         default-token-dv57v
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: default
              kubernetes.io/service-account.uid: f7ab5770-3e0b-4dba-a431-a12a48fd3d8d

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1103 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IklpT0NEaXZ4YUpsQVBLTlNZUGNjdGE3dWJSN0xNbGE1MFFlUWFUY3NpVFUifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkZWZhdWx0LXRva2VuLWR2NTd2Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImRlZmF1bHQiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJmN2FiNTc3MC0zZTBiLTRkYmEtYTQzMS1hMTJhNDhmZDNkOGQiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06ZGVmYXVsdCJ9.JgECFOXY-hPmhzSCfqKbKxFM7cpngARzwKFqQwZNGZGDN7I3gpZhUcPyHhTJmjMAlAmexzoHHslll75L3mHIwQd-Fnew0m9eSqZuXRa9XGzDkBVqkmhn3FiA32gq7kN1Fwbly-bCC2eaXHyhXRA94dBI9BeNOirVaSbEOs4gQ0EHgBCLYHTtrEOC_SjJeVldCE1fYnWdjZZuKGYDdOEh4y5n6EEdSq2ZdwCUevYQ-ExL7Q4aVvIIY-d4LO-ODXTE_oY28B0QEf_sJZfSJ9BJJZOgHZrShfENFPqflA2UYWMWztwzVfXBGbeJW6OZmiQOrkMybxO9WnvRLlLB9T7XHQ
```
the dashboard will show:
![Alt text](../../images/k8sdashboard.png)