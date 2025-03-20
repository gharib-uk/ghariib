---
title: "Install RHEL packages in container images using Shipwright"
date: 2025-01-17
---

Most container-based development starts with a base image on top of which developers can layer any additional libraries, binaries, and files needed to run an application. The Red Hat Universal Base Image (UBI) is a collection of OCI-compliant, freely redistributable, container-based images. These images are designed to be a foundation for cloud-native and web application use cases, maintained and updated regularly by Red Hat. While most Red Hat Enterprise Linux (RHEL) packages (RPMs) are available as a freely redistributable part of UBI images, there are use cases where developers need to install RPMs that require an active RHEL subscription (entitlement) for building images (i.e., via Dockerfile).

Red Hat OpenShift Container Platform is the trusted, comprehensive, and consistent application platform to develop, modernize, and deploy applications at scale. It includes RHEL entitlements (available on OpenShift clusters) to simplify developer workflows for building images. In this article, we will explain step-by-step how to use Shipwright Builds and RHEL entitlements to build images and install RPMs inside their images through Dockerfiles.

## Sharing Secrets across namespaces

Builds for Red Hat OpenShift provides developers with a consistent and secure way to create container images directly within OpenShift clusters. With its foundation built on Shipwright, an open source CNCF project, builds for Red Hat OpenShift simplifies the process of creating OCI-compliant images using the image build tool of your choice while leveraging Red Hat OpenShift's enterprise-grade capabilities. Builds includes a capability called the Shared Resource CSI Driver which takes advantage of the Kubernetes Container Storage Interface (CSI) to enable sharing Secrets and ConfigMaps across namespaces while controlling granular access through Kubernetes role-based access control (RBAC). This is accomplished through injecting a Secret or ConfigMap that exists in a namespace into the pods in a different namespace through an inline CSI volume. This is particularly useful for building images using the RHEL entitlements included in an OpenShift Container Platform subscription and available on the cluster as a Secret named “etc-pki-entitlement” in the “openshift-config-managed” namespace.

The main advantage of using the Shared Secret CSI Driver is that it removes the operational overhead of the cluster admin replicating a Secret across multiple namespaces, and periodically refreshing it as the entitlement certificates get regularly updated based on the changes in the organization’s Red Hat subscriptions.

Shipwright Builds and the Shared Secret CSI Driver can be installed through the builds for Red Hat OpenShift operator available in the OpenShift OperatorHub.

## Step 1: Create a SharedSecret resource

First, we will build images using the RHEL entitlements from the Red Hat OpenShift cluster by creating a `SharedSecret` resource to allow injecting the content of the entitlements Secret into pods in other namespaces, as follows:

```plaintext
apiVersion: sharedresource.openshift.io/v1alpha1
kind: SharedSecret
metadata:
  name: etc-pki-entitlement
spec:
  secretRef:
    name: etc-pki-entitlement
    namespace: openshift-config-managed
```

The `SharedSecret` resource enables sharing the `etc-pki-entitlement` Secret (which contains the RHEL entitlement certificates) with pods in other namespaces. The `etc-pki-entitlement` Secret is managed by the Red Hat OpenShift control plane and is refreshed periodically to reflect the latest state of the subscriptions in the associated Red Hat account. 

## Step 2: Define permissions for sharing the Secret resource

Sharing Secrets across namespaces is sensitive and requires caution. Therefore, in order to ensure security when sharing Secrets between namespaces, the Shared Resource CSI Driver by design requires granular permissions defined for both sharing and consuming the Secret resource:

- Explicit permissions for Shared Resource CSI Driver service account to be able to access the Secret in the source namespace.
- Explicit permissions for the builds service account in the target namespace to access the Shared Secret.

Next, create a role and a `RoleBinding` that allows the Shared Resource CSI Driver to access the `etc-pki-entitlement` Secret that is referenced in the `SharedSecret` object we just created.

```plaintext
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: share-etc-pki-entitlement
  namespace: openshift-config-managed
rules:
  - apiGroups:
      - ""
    resources:
      - secrets
    resourceNames:
      - etc-pki-entitlement
    verbs:
      - get
      - list
      - watch
```

```plaintext
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: share-etc-pki-entitlement
  namespace: openshift-config-managed
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: share-etc-pki-entitlement
subjects:
  - kind: ServiceAccount
    name: csi-driver-shared-resource
    namespace: openshift-builds
```

Finally, we will create a role that allows access to the `etc-pki-entitlement` `SharedSecret` and assign it to the service account that is going to use the referenced Secret content. In the case of Shipwright Builds, by default the pipeline service account in the target namespace runs the builds and would need to access the content of RHEL entitlements Secret, as demonstrated in the following example:

```plaintext
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: use-share-etc-pki-entitlement
rules:
  - apiGroups:
      - sharedresource.openshift.io
    resources:
      - sharedsecrets
    resourceNames:
      - etc-pki-entitlement
    verbs:
      - use
```

```plaintext
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: use-share-etc-pki-entitlement 
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: use-share-etc-pki-entitlement
subjects:
  - kind: ServiceAccount
    name: pipeline
  - kind: ServiceAccount
    name: builder
```

## Step 3: Creating a Shipwright Build

Now that we’ve created the **SharedSecret** and configured explicit permissions, it is time to create a Shipwright Build in the demo namespace to use the shared RHEL entitlements during a Dockerfile build. The following example demonstrates the Buildah build strategy in which Buildah is used to create container images from Dockerfiles and Containerfiles:

```plaintext
apiVersion: shipwright.io/v1beta1
kind: Build
metadata:
  name: buildah-rhel
spec:
  source:
    type: Git
    git:
      url: https://github.com/adambkaplan/builds-openshift-demos
      revision: entitled-builds
    contextDir: demos/6-entitled-builds/build
  strategy:
    name: buildah
    kind: ClusterBuildStrategy
  paramValues:
  - name: dockerfile
    value: Containerfile
  volumes:
  - csi:
      driver: csi.sharedresource.openshift.io
      readOnly: true
      volumeAttributes:
        sharedSecret: etc-pki-entitlement
    name: etc-pki-entitlement
  output:
    image: image-registry.openshift-image-registry.svc:5000/demo/buildah-rhel:latest
```

This build specifies the Git repository for the application source and contains a Containerfile that runs a **dnf install** command for installing RPMs similar to how a Linux admin would install RPMs on a RHEL server. For the RPM installation to succeed, the subscription manager within the container requires the RHEL entitlement certificates to be available at **/etc/pki/entitlement**.

The Buildah build strategy mounts the entitlements Secret referenced by the Shared Secret at that particular path so that **dnf** commands have access to the RHEL entitlements and can successfully connect to the Red Hat network to download and install RHEL packages. 

## Step 4: Install RHEL packages

Push the output image to the Red Hat OpenShift internal registry as follows:

```plaintext
FROM registry.access.redhat.com/ubi9/ubi:latest
RUN dnf --enablerepo=codeready-builder-for-rhel-9-x86_64-rpms install 
    cloud-init  
    nss_wrapper 
    uid_wrapper -y && 
    dnf clean all -y
```

Now it is ready to run a Dockerfile build and build the container image that installs RHEL packages. Once we start the build through the web console or Shipwright CLI, the build logs show the successful installation of the RPM packages (Figure 1).

<figure>

![Shipwright Build running and installing RHEL packages](https://developers.redhat.com/sites/default/files/styles/article_floated/public/image1_93.png?itok=vwWoE254)

<figcaption>

Figure 1: The log shows Shipwright Build running and installing RHEL packages.

</figcaption>

</figure>

## RHEL entitlements simplify developer workflows

Red Hat Enterprise Linux entitlements simplify developer workflows for building images. They are available on Red Hat OpenShift clusters. This article demonstrated four steps for using Shipwright Builds and RHEL entitlements to build images and install RPMs inside their images through Dockerfiles.

To learn more about the examples in this article, visit our GitHub repository.

The post Install RHEL packages in container images using Shipwright appeared first on Red Hat Developer.  
  

Go to Source
