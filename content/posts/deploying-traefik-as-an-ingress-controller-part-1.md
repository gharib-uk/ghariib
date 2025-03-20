---
title: "Deploying Traefik as an Ingress Controller. (Part 1)"
date: 2025-01-07
---

In the world of Kubernetes, managing traffic and securing APIs are critical tasks for any DevOps professional. This guide is the first in a three-part series designed to streamline your journey into modern application architecture:

Part 1: Deploying Traefik as an Ingress Controller

Part 2: Configuring KrakenD as an API Gateway.

Part 3: Building services that leverage Traefik and KrakenD for seamless traffic management and API integration.

In this first part, we will focus on deploying Traefik, a powerful Ingress Controller that simplifies external traffic management within Kubernetes environments. Whether you're experimenting with Kubernetes or managing production workloads, this guide will equip you with the knowledge to integrate Traefik effectively.

## Why Traefik and KrakenD?

### Traefik

Traefik is a modern, cloud-native reverse proxy and load balancer. Its dynamic configuration and seamless integration with Kubernetes make it ideal for handling HTTP and HTTPS traffic in microservices environments. Additionally, extensive features like middleware, TLS management, and monitoring reduce operational overhead. This makes Traefik a go-to solution for both small-scale and enterprise deployments.

### KrakenD

KrakenD is an open-source API Gateway designed for high performance and flexibility. It handles complex operations such as request routing, aggregation, and transformation, allowing developers to expose APIs in a structured and efficient manner. With KrakenD, you can aggregate multiple microservices into a single endpoint, reducing latency and simplifying client interactions.

This series will ultimately demonstrate how to combine these tools to create robust, scalable architectures for modern applications.

All files mentioned in this guide are available in the GitHub repository: traefik-krakend-k8s-setup under the folder `1. traefik`.

## Prerequisites

To follow along, ensure you have the following:

1. **A Kubernetes Cluster**: This can be a local setup using Minikube or Kind, or a managed cluster like AWS EKS, Google GKE, or Azure AKS.
2. **`kubectl` CLI**: Installed and configured to interact with your cluster.
3. **`Helm` CLI**: Installed for managing Kubernetes packages.

## Architecture Overview

This guide will cover:

- Deploying Traefik to manage external traffic.

## Step 1: Setting Up Traefik

### Installing Traefik CRDs

To enable advanced features, start by installing Traefik's Custom Resource Definitions (CRDs):  

```
helm repo add traefik https://traefik.github.io/charts
helm repo update
kubectl apply --server-side -k https://github.com/traefik/traefik-helm-chart/traefik/crds/
```

### Creating Namespaces

Namespaces help organize Kubernetes resources. Create namespaces for Traefik, your applications, and KrakenD:

**00-init-namespace.yaml**:  

```
apiVersion: v1
kind: Namespace
metadata:
  name: traefik
---
apiVersion: v1
kind: Namespace
metadata:
  name: my-application
---
apiVersion: v1
kind: Namespace
metadata:
  name: krakend
```

Apply the namespaces:  

```
kubectl apply -f 00-init-namespace.yaml
```

### Configuring TLS

TLS is crucial for securing data, authenticating servers, and establishing trust with users. Certificates are necessary to enable encryption and verify server identity. Here, we use a self-signed certificate for internal or testing purposes.

Enable HTTPS by generating a self-signed certificate:  

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout wildcard.mydns.local.key -out wildcard.mydns.local.crt -subj "/CN=*.mydns.local" -addext "subjectAltName=DNS:*.mydns.local,DNS:mydns.local"
```

Store the certificate in Kubernetes:  

```
kubectl create secret tls traefik-tls-secret --key ./wildcard.mydns.local.key --cert ./wildcard.mydns.local.crt -n traefik
```

#### **Define a default TLS store:**

In Traefik, certificates are grouped together in certificates stores.

Note that, any store definition other than the default one (named default) will be ignored, and there is therefore only one globally available TLS store.

**00-just-tls.yaml**:  

```
apiVersion: traefik.io/v1alpha1
kind: TLSStore
metadata:
  name: default
  namespace: traefik
spec:
  defaultCertificate:
    secretName: traefik-tls-secret
```

Apply the TLS store configuration:  

```
kubectl apply -f 00-just-tls.yaml
```

### Granting Permissions

ClusterRole defines the set of permissions needed for Traefik to manage Kubernetes resources.  
ServiceAccount provides an identity for Traefik within the cluster.  
ClusterRoleBinding then associates this ClusterRole with the ServiceAccount, allowing Traefik to perform actions like reading endpoints or updating ingresses across all namespaces.

Create a ClusterRole for Traefik to interact with Kubernetes resources:

**00-traefik-clusterrole.yaml**:  

```
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: traefik-role
rules:
- apiGroups:
  - ""
  resources:
  - services
  - secrets
  - nodes
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - discovery.k8s.io
  resources:
  - endpointslices
  verbs:
  - list
  - watch
- apiGroups:
  - extensions
  - networking.k8s.io
  resources:
  - ingresses
  - ingressclasses
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - extensions
  - networking.k8s.io
  resources:
  - ingresses/status
  verbs:
  - update
- apiGroups:
  - traefik.io
  resources:
  - middlewares
  - middlewaretcps
  - ingressroutes
  - traefikservices
  - ingressroutetcps
  - ingressrouteudps
  - tlsoptions
  - tlsstores
  - serverstransports
  verbs:
  - get
  - list
  - watch
```

> **Note:** This role grants all the permissions Traefik typically needs. If you’re not using certain features (for example, custom Traefik CRDs or managing Ingress status), feel free to remove those permissions. Just ensure you don’t remove anything you rely on, or Traefik may lose necessary functionality.

Apply the ClusterRole:  

```
kubectl apply -f 00-traefik-clusterrole.yaml
```

Create a ServiceAccount and bind it to the ClusterRole:

**01-traefik-serviceaccount.yaml**:  

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: traefik-account
  namespace: traefik
```

**02-traefik-clusterrole-binding.yaml**:  

```
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: traefik-role-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: traefik-role
subjects:
- kind: ServiceAccount
  name: traefik-account
  namespace: traefik
```

Apply these configurations:  

```
kubectl apply -f 01-traefik-serviceaccount.yaml
kubectl apply -f 02-traefik-clusterrole-binding.yaml
```

### Deploying Traefik

To deploy Traefik in Kubernetes, create a Deployment that specifies Traefik's container image, resource requirements, and other necessary configurations. This Deployment will manage the Traefik pods, ensuring they run with the desired settings and scale as needed.

Create a Deployment for Traefik:

**04-traefik-deployment.yaml**:  

```
kind: Deployment
apiVersion: apps/v1
metadata:
  name: traefik-deployment
  namespace: traefik
  labels:
    app: traefik
spec:
  replicas: 1
  selector:
    matchLabels:
      app: traefik
  template:
    metadata:
      labels:
        app: traefik
    spec:
      serviceAccountName: traefik-account
      containers:
      - name: traefik
        image: traefik:v3.2
        args:
        - --accesslog
        - --providers.kubernetescrd
        - --providers.kubernetesingress
        - --log.level=DEBUG
        - --entrypoints.web.address=:80
        - --entrypoints.websecure.address=:443
        - --entrypoints.web.http.redirections.entrypoint.to=websecure
        - --entrypoints.web.http.redirections.entrypoint.scheme=https
        - --entrypoints.websecure.http.tls=true
        ports:
        - name: websecure
          containerPort: 443
        securityContext:
          readOnlyRootFilesystem: true
          runAsNonRoot: true
          runAsUser: 65532
          runAsGroup: 65532
          allowPrivilegeEscalation: false
          capabilities:
            drop:
            - ALL
          seccompProfile:
            type: RuntimeDefault
        resources:
          requests:
            cpu: 200m
            memory: 100Mi
          limits:
            cpu: 200m
            memory: 100Mi
```

> **Note:** The `securityContext` settings follow best practices for container security. Review your organization’s policies and ensure these align with specific requirements.

Apply the Deployment:  

```
kubectl apply -f 04-traefik-deployment.yaml
```

### Exposing Traefik

Create a LoadBalancer service to expose Traefik externally. This service will provide an external IP, making Traefik accessible for incoming traffic from outside the Kubernetes cluster.

**05-traefik-web-service.yaml**:  

```
apiVersion: v1
kind: Service
metadata:
  name: traefik-web-service
  namespace: traefik
spec:
  type: LoadBalancer
  ports:
  - name: websecure
    port: 443
    targetPort: 443
  selector:
    app: traefik
```

Apply the service configuration:  

```
kubectl apply -f 05-traefik-web-service.yaml
```

With this, your Traefik deployment is ready to manage external traffic efficiently.

## What’s Next?

In Part 2, we will deploy KrakenD and configure it as a high-performance API Gateway to manage backend services. This will set the stage for building robust applications in Part 3, where Traefik and KrakenD will work together seamlessly to handle traffic and API integration.

### Share Your Experience

If you found this guide helpful, share your experience or ask questions in the comments. We’d love to hear about your Kubernetes journey and how tools like Traefik and KrakenD fit into your workflow. Stay tuned for more DevOps insights and tutorials!

Go to Source
