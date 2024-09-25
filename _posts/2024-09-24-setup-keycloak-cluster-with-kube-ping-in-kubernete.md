---
title: Keycloak Cluster Setup With kube_ping
date: 2021-03-02 10:45:37 -05:00
layout: post
permalink: blog/setup-keycloak-cluster-with-kube-ping-in-kubernete/
categories:  Code Snippet
author: Arvind Rajpurohit
coverImage: https://dz2cdn1.dzone.com/storage/temp/16193208-1662525593978.png
---

__Take a look at how to auto-discover using kube_ping and Keycloak.__

Keycloak is an open source software which provides single sign-on with Identity Management and access management. Keycoak uses different types of pings to discover other members of cluster. We are going to use kube_ping as discovery **(JGROUPS_DISCOVERY_PROTOCOL).**

## How kube_ping Works

Let's assume we launch a cluster of 3 pods in Kubernetes in the default Namespace. When discovery starts, kube_ping asks for a list of the IP addresses of all pods from Kubernetes.

Kubernetes returns the list of IP addresses. kube_ping sends discovery requests to members using the IP address and port range.

**Prerequisites:**

- 1. Working Kubernetes cluster
- 2. A running MySQL Instance.


## Creating Deployment Object in K8s Cluster

```yml
apiVersion: v1
items:
- apiVersion: extensions/v1beta1
  kind: Deployment
  spec:
    replicas: 3
    template:
       metadata:
         labels:
           app: keycloak
           name: keycloak
       spec:
        containers:
        - env:
          - name: KEYCLOAK_HOSTNAME
            value: {{keycloak host name}}
          - name: KEYCLOAK_LOGLEVEL
            value: DEBUG
          - name: ROOT_LOGLEVEL
            value: DEBUG
          - name: KEYCLOAK_USER
            value: {{keyclock admin user}}
          - name: DB_VENDOR
            value: mysql
          - name: DB_ADDR
            value: {{mysql host}}
          - name: DB_USER
            value: {{mysql user}}
          - name: DB_PASSWORD
            value: {{mysql password}}
          - name: JGROUPS_DISCOVERY_PROTOCOL
            value: kubernetes.KUBE_PING
          - name: JGROUPS_DISCOVERY_PROPERTIES
            value: port_range=0,dump_requests=true
          - name: connectTimeout
            value: "600000"
          - name: KEYCLOAK_PASSWORD
            value: {{keyclock admin password}}
          - name: remoteTimeout
            value: "600000"
          image: jboss/keycloak:4.5.0
          imagePullPolicy: Never
          name: keycloak
          ports:
          - containerPort: 8080
            name: http
            protocol: TCP
          - containerPort: 8443
            name: https
            protocol: TCP
```

## Creating Service Object in K8s Cluster

```yml
apiVersion: v1
kind: Service
metadata:
  name: keycloak
  labels:
    app: keycloak
    name: keycloak
spec:
  type: NodePort
  ports:
    - name: http
      protocol: TCP
      port: 8080
      nodePort: 30000
  selector:
    app: keycloak
    name: keycloak
```
This creates the endpoint of Keycloak. Now the Keycloak admin consol will be available at http://{{hostip}}:30000


