---
title: "Automatic certificate issuing with IdM and cert-manager operator for OpenShift"
date: 2025-01-02
categories: 
  - "general"
---

This article will show you how to automatically issue certificates using Identity Management in Red Hat Enterprise Linux (IdM) and cert-manager operator for OpenShift.

Securing communications is a must, not only to protect the data but to provide greater data integrity as well. Applications achieve greater communications security using cryptographic certificates, but these certificates have their own lifecycle. They need to be renewed when expired and the renewal process, when done manually, is prone to errors.

One of the challenges that modern organizations face is managing a certificate's lifecycle automatically. Application certificates must remain valid to grant not only more secure communications but also to keep application clients' trust.

Automating a certificate's lifecycle means that the platform running the application must interact with a Certification Authority (CA) to request a new certificate. Both parts need to authenticate and exchange information during the process. Deploying a Public Key Infrastructure (PKI) is not an easy task. However, the Internet Security Research Group (ISRG) designed the Automatic Certificate Management Environment (ACME) protocol for its nonprofit CA "Let’s Encrypt", becoming the world’s largest CA. ACME has become a standard for certificate management being implemented by many PKI’s around the world.

Red Hat OpenShift is one of the leaders in container management. Managing a certificate's lifecycle is important, you can take advantage of this to help manage certificate lifecycles via the cert-manager operator for Red Hat OpenShift, which supports the ACME protocol.

**NOTE:** IdM ACME capabilities are Technology Preview (TP) in RHEL 9, so this feature is not ready for production yet.

## IdM as a private ACME server

Traditionally, issuing certificates has been a manual process, but that process does not scale well when the environment is dynamic or the number of certificates is large.

To address this challenge, OpenShift relies on a wildcard certificate to expose applications with greater security to the external world under the `*.apps` subdomain. A wildcard certificate is a convenient approach because it only needs to be configured in one place, but in some organizations wildcard certificates are not allowed. 

Not long ago, the only alternative was to issue certificates manually for every OpenShift application. This is when the ACME protocol came into play, allowing automated interactions between CAs and clients. You can implement your own ACME CA using the IdM CA capabilities. 

IdM will be acting as the private ACME server and the cert-manager operator for OpenShift as the ACME client (see Figure 1). 

<figure>

<figure>

![Diagram of IdM acting as ACME server and cert-manager as ACME client.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/acme-client-server_0.png?itok=BCuxuEe3)

<figcaption>

Figure 1. IdM and cert-manager as ACME server and client.

</figcaption>

License under Apache 2.0. Jorge Tudela González de Riancho,

</figure>

<figcaption>

Figure 1: IdM and cert-manager as ACME server and client.

</figcaption>

</figure>

We will depict two scenarios where a certificate will be issued automatically by IdM:

- Using ACME HTTP-01 challenge with a Kubernetes Ingress resource.
- Using a cert-manager Certificate resource.

Before delving into each scenario, we need to install and configure the cert-manager operator for OpenShift and then create a cert-manager ACME Issuer to define IdM as the ACME server.

## Install and configure the cert-manager operator for OpenShift

Let's install and configure the cert-manager operator. First we need to create the Namespace, OperatorGroup, and Subscription resources: 

```input
---
apiVersion: v1
kind: Namespace
metadata:
  name: cert-manager-operator
---
apiVersion: operators.coreos.com/v1
kind: OperatorGroup
metadata:
  name: cert-manager-operator
  namespace: cert-manager-operator
spec:
  targetNamespaces:
  - cert-manager-operator
  upgradeStrategy: Default
---
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: openshift-cert-manager-operator
  namespace: cert-manager-operator
spec:
  channel: stable-v1
  installPlanApproval: Automatic
  name: openshift-cert-manager-operator
  source: redhat-operators
  sourceNamespace: openshift-marketplace
  startingCSV: cert-manager-operator.v1.14.1
```

**NOTE:** You can install the cert-manager operator for OpenShift using the OpenShift web console as well.

The cert-manager operator will create the cert-manager Namespace and deploy the cert-manager Pods. See below:

```output
$ oc get pods -n cert-manager
NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager-7665468798-4w75r              1/1     Running   0          5h12m
cert-manager-cainjector-7c47d699c5-scx24   1/1     Running   0          5h12m
cert-manager-webhook-7f764d5568-ssbwc      1/1     Running   0          5h12m
```

The cert-manager operator needs to trust the IdM server TLS certificate. Assuming that we have already updated the OpenShift cluster CA bundle to trust IdM, we need to inject the CA into the cert-manager operator.

First we create the ConfigMap with the right label for the cluster CA to be injected and then patch the cert-manager operator Subscription:

```output
$ oc create configmap trusted-ca -n cert-manager
$ oc label cm trusted-ca config.openshift.io/inject-trusted-cabundle=true -n cert-manager
$ oc -n cert-manager-operator patch subscription openshift-cert-manager-operator --type='merge' -p '{"spec":{"config":{"env":[{"name":"TRUSTED_CA_CONFIGMAP_NAME","value":"trusted-ca"}]}}}' 
```

Roll out the cert-manager operator and cert-manager Deployments:

```output
$ oc rollout status deployment/cert-manager-operator-controller-manager -n cert-manager-operator && 
oc rollout status deployment/cert-manager -n cert-manager && 
oc rollout status deployment/cert-manager-webhook -n cert-manager && 
oc rollout status deployment/cert-manager-cainjector -n cert-manager
```

Verify the CA has been properly mounted in the cert-manager Pods:

```output
$ oc get deployment cert-manager -n cert-manager -o=jsonpath={.spec.template.spec.volumes[0]}
{"configMap":{"defaultMode":420,"name":"trusted-ca"},"name":"trusted-ca"}
$ oc get deployment cert-manager -n cert-manager -o=jsonpath={.spec.template.spec.'containers[0].volumeMounts[0]'}
{"mountPath":"/etc/pki/tls/certs/cert-manager-tls-ca-bundle.crt","name":"trusted-ca","subPath":"ca-bundle.crt"}
```

### Create cert-manager ACME HTTP-01 issuer

We are using the ACME HTTP-01 challenge, which is fully supported by the cert-manager operator and TP for IdM in RHEL 9.

Because of how the ACME HTTP-01 challenge works, bidirectional communication is needed between the ACME server (IdM) and the ACME client (cert-manager operator). The ACME HTTP-01 challenge can only be done on port 80. IdM is installed with ACME enabled at `https://idm.melmac.univ`. 

Let's create the cert-manager ACME HTTP-01 ClusterIssuer to use our IdM server as the private ACME server:

```input
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: idm-acme
spec:
  acme:
    preferredChain: ""
    privateKeySecretRef:
      name: idm-acme-issuer-private-key
    server: https://idm.melmac.univ/acme/directory
    solvers:
    - http01:
        ingress:
          ingressClassName: openshift-default
```

The difference between a cert-manager Issuer and a ClusterIssuer is that Issuer resources are namespace scoped and can only be referenced by Certificates in the same Namespace, while ClusterIssuers are cluster scoped resources and can be referenced by Certificates in any Namespace.

## Use ACME HTTP-01 challenge with a Kubernetes Ingress resource

TLS certificates will be requested by cert-manager securing your Ingress resources by simply adding annotations to the Ingress resources, then cert-manager will create the Certificate resource for you.

We will deploy a dummy application, hello-openshift, and use an Ingress resource to expose it to the external world. We will protect the application using a TLS certificate automatically issued by IdM.

First we deploy the `hello-openshift` application:

```input
---
apiVersion: v1
kind: Namespace
metadata:
  name: hello-openshift
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: hello-openshift
    app.kubernetes.io/component: hello-openshift
    app.kubernetes.io/instance: hello-openshift
  name: hello-openshift
  namespace: hello-openshift
spec:
  selector:
    matchLabels:
      deployment: hello-openshift
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        deployment: hello-openshift
    spec:
      containers:
      - image: quay.io/openshifttest/hello-openshift:1.2.0
        imagePullPolicy: IfNotPresent
        name: hello-openshift
        ports:
        - containerPort: 8080
          protocol: TCP
        - containerPort: 8888
          protocol: TCP
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: hello-openshift
    app.kubernetes.io/component: hello-openshift
    app.kubernetes.io/instance: hello-openshift
  name: hello-openshift
  namespace: hello-openshift
spec:
  ports:
  - name: 8080-tcp
    port: 8080
    protocol: TCP
    targetPort: 8080
  - name: 8888-tcp
    port: 8888
    protocol: TCP
    targetPort: 8888
  selector:
    deployment: hello-openshift
  sessionAffinity: None
  type: ClusterIP
```

Once the application is deployed, we create an Ingress resource annotated with `cert-manager.io/cluster-issuer: idm-acme`, where `idm-acme` is the name of cert-manager ClusterIssuer we created before. See below:

```input
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    cert-manager.io/cluster-issuer: idm-acme
  name: hello-openshift
  namespace: hello-openshift
spec:
  ingressClassName: openshift-default
  rules:
  - host: hello-openshift.apps.qisdetdd.eastus.aroapp.io
    http:
      paths:
      - backend:
          service:
            name: hello-openshift
            port:
              number: 8080
        path: /
        pathType: Prefix
  tls:
  - hosts:
    - hello-openshift.apps.qisdetdd.eastus.aroapp.io
    secretName: ingress-hello-openshift
```

A few seconds after creating the Ingress resource we will see a temporary Ingress resource `cm-acme-http-solver-XXXX` created by the cert-manager operator as the ACME HTTP01 challenge solver. We can also verify this temporary solver Ingress resource is only published on port 80 as mentioned before:

```output
$ oc -n hello-openshift get ingress
NAME        CLASS      HOSTS        ADDRESS      PORTS        AGE
cm-acme-http-solver-x9v7d   openshift-default   hello-openshift.apps.qisdetdd.eastus.aroapp.io    router-default.apps.qisdetdd.eastus.aroapp.io   80        13s
hello-openshift             openshift-default   hello-openshift.apps.qisdetdd.eastus.aroapp.io   router-default.apps.qisdetdd.eastus.aroapp.io   80, 443   98s
```

The Certificate resource has been created in the `hello-openshift` Namespace and there is a new Secret as well. The Certificate resource will take a few seconds to change to `Ready=True` status. See below: 

```output
$ oc -n hello-openshift get certificate
NAME                      READY   SECRET                    AGE
ingress-hello-openshift   True    ingress-hello-openshift   89m
$ oc -n hello-openshift get secret ingress-hello-openshift
NAME                      TYPE                DATA   AGE
ingress-hello-openshift   kubernetes.io/tls   3      90m
```

 If we browse the application defined in the Ingress resource `https://hello-openshift.apps.qisdetdd.eastus.aroapp.io`, we can inspect the TLS certificate and see that it has been issued by the IdM CA (see Figure 2). 

<figure>

<figure>

![Mozilla Firefox displaying the TLS certificate of the hello-openshift app.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/hello-ocp-cert-firefox.png?itok=Eq9gz-1C)

<figcaption>

Figure 2. hello-openshift app TLS certificate in the web browser.

</figcaption>

</figure>

<figcaption>

Figure 2: hello-openshift app TLS certificate in the web browser.

</figcaption>

</figure>

 Figure 3 shows that a new certificate has been created in IdM.

<figure>

<figure>

![hello-openshift app certificated shown in IdM with a default duration of 90 days.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/hello-ocp-cert-idm_0.png?itok=NT6lt1lW)

<figcaption>

Figure 3. hello-openshift app TLS certificate shown in IdM

</figcaption>

</figure>

<figcaption>

Figure 3: hello-openshift app TLS certificate shown in IdM.

</figcaption>

</figure>

## Use a cert-manager Certificate resource

In this scenario we will generate a TLS certificate stored in a Kubernetes Secret, with no application involved. There are situations where a TLS certificate is needed for infrastructure purposes rather than application purposes. 

We need to create a cert-manager Certificate resource:

```input
---
apiVersion: v1
kind: Namespace
metadata:
  name: test-cert-manager
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: test-cert
  namespace: test-cert-manager
spec:
  isCA: false
  secretName: test-cert
  commonName: "test-cert-manager.apps.qisdetdd.eastus.aroapp.io"
  dnsNames:
  - "test-cert-manager.apps.qisdetdd.eastus.aroapp.io"
  issuerRef:
    name: idm-acme
    kind: ClusterIssuer
```

If everything goes well we should see the Certificate resource in `Ready=True` state and a Kubernetes Secret storing the TLS certificate and key issued by the IdM CA. See below:

```output
$ oc -n test-cert-manager get certificate
NAME        READY   SECRET      AGE
test-cert   True    test-cert   45s
$ oc -n test-cert-manager get secret test-cert
NAME        TYPE                DATA   AGE
test-cert   kubernetes.io/tls   3      62s
```

We can inspect the content of the Secret, the `tls.crt` key that holds the TLS certificate, to verify that it has been issued by the IdM CA. We can check that the certificate subject matches the Certificate resource definition:

```output
$ oc -n test-cert-manager extract secret/test-cert --keys=tls.crt --to=- | openssl x509 -noout -issuer -subject
# tls.crt
issuer=O=MELMAC.UNIV, CN=Certificate Authority
subject=CN=test-cert-manager.apps.qisdetdd.eastus.aroapp.io
```

 We can inspect `tls.key` of the Secret, holding the key of the certificate:

```output
$ oc -n test-cert-manager extract secret/test-cert --keys=tls.key --to=- | openssl rsa -text -noout                                                                                                                                           
# tls.key                                                                                                                                                                                                                                     
Private-Key: (2048 bit, 2 primes)                                                                                                                                                                                                             
modulus:
...
...                                 
```

 In IdM the new certificate appears (see Figure 4).

<figure>

<figure>

![New TLS certificate in IdM generated by the cert-manager using a Certificate resource.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/test-cert-idm.png?itok=_As21XEd)

<figcaption>

Figure 4. New TLS certificate in IdM.

</figcaption>

</figure>

<figcaption>

Figure 4: New TLS certificate in IdM.

</figcaption>

</figure>

## Certificates lifecycle

cert-manager will manage the lifecycle of the certificate including the renewal operation. The default validity period is 90 days. Once an X.509 certificate has been issued, cert-manager will calculate the renewal time for the certificate. By default, this will be 2/3 through the X.509 certificate's validity period. It is important to highlight that once a certificate has been issued, the X.509 certificate becomes the source of truth when it comes to calculating and triggering the renewal process.

If we want to modify the default validity period in the IdM server we need to export the profile used to issue ACME certificates:

```output
$ ipa certprofile-show --out acmeIPAServerCert.cfg acmeIPAServerCert
```

To configure a custom validity period we need to change some parameters in the `acmeIPAServerCert.cfg` file:

```plaintext
policyset.serverCertSet.7.constraint.params.range=72
policyset.serverCertSet.7.constraint.params.rangeUnit=hour
policyset.serverCertSet.7.default.params.range=48
policyset.serverCertSet.7.default.params.rangeUnit=hour                                                  
```

The `default` parameter defines the default validity period and the `constraint` defines the maximum validity period for the certificate. By default, the range unit is `day`, but other units can be used such as `minute`, `hour`, `month`, or `year`.

The new configuration has to be applied to the IdM’s ACME profile:

```output
$ ipa certprofile-mod acmeIPAServerCert --file=acmeIPAServerCert.cfg 
```

## Takeaways

IdM’s ACME capabilities will make the life of PKI administrators and OpenShift users easier by integrating it with the cert-manager operator for OpenShift. TLS certificate lifecycle management will never again be a time-consuming maintenance burden. 

Remember that IdM is included in your RHEL subscription, you can set up your own ACME server in your infrastructure at no additional cost!

Reach out to your Red Hat Services representative to get your open source journey started. Red Hat  Services is helping our customers to get the most out of your investment. We can provide you with the resources you need to begin, accelerate, and expand your open source journey as a part of your competitive advantage.

The post Automatic certificate issuing with IdM and cert-manager operator for OpenShift appeared first on Red Hat Developer.  
  

Go to Source
