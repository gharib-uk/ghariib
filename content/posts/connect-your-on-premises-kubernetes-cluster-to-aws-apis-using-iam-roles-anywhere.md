---
title: "Connect your on-premises Kubernetes cluster to AWS APIs using IAM Roles Anywhere"
date: 2025-03-19
---

> **February 26, 2025**: We’ve updated this post to fix a typo in the code in **Step 5 – Deploy your workload**.

* * *

Many customers want to seamlessly integrate their on-premises Kubernetes workloads with AWS services, implement hybrid workloads, or migrate to AWS. Previously, a common approach involved creating long-term access keys, which posed security risks and is no longer recommended. While solutions such as Kubernetes secrets vault and third-party options exist, they fail to address the underlying issue effectively.

One option to connect your on-premises Kubernetes workloads to AWS APIs is to use the service account issuer discovery feature. This allows the Kubernetes API server to act as an OpenID Connect (OIDC) identity provider and be federated with AWS Identity and Access Management (IAM). However, this approach requires public internet access to the Kubernetes API server, which might not be desirable for some customers.

To help eliminate the need for long-term access keys or exposing the Kubernetes API server to the public internet, AWS has introduced AWS IAM Roles Anywhere. This feature enables secure, seamless integration of on-premises Kubernetes workloads with AWS services, promoting robust security practices and minimizing potential risks associated with long-term credentials or public exposure.

IAM Roles Anywhere enables workloads outside of AWS to access AWS resources by exchanging X.509 bound identities for temporary AWS credentials. With IAM Roles Anywhere, you can use the same IAM roles and policies as your AWS workloads to access AWS resources, promoting consistency.

IAM Roles Anywhere can be combined with a standard public key infrastructure solution. In this blog post, we use AWS Private Certificate Authority, which has several advantages over using a self-signed certificate authority (CA). First, it reduces operational and management overhead, because AWS manages the CA for you. Second, the cryptographic key material can be stored in hardware security modules or at least vaulted, which helps you protect your private CA against key compromises. Additionally, certificates can be short-lived, which aligns with dynamic Kubernetes environments where pod lifetimes are typically shorter than traditional servers.

We also demonstrate how to integrate IAM Roles Anywhere without modifying your existing workload Docker files, and how to automate the X.509 certificate lifecycle with cert-manager and an AWS Private CA backend in short-lived certificate mode. By using these capabilities, you can seamlessly integrate your on-premises Kubernetes workloads with AWS services, promoting robust security practices, minimizing risks associated with long-term credentials, and helping to ensure a streamlined, consistent access management experience.

This post is for customers who run their own Kubernetes cluster outside of AWS without using Amazon EKS Anywhere. If you’re using Amazon Elastic Kubernetes Service (Amazon EKS), use IAM roles for service accounts or Amazon EKS Pod Identity instead.

## Background

“Why should I prefer X.509 certificates over IAM access keys?” Access keys are long-term credentials that must be rotated regularly to minimize the risk of unauthorized access. They need to be securely deployed onto servers hosting applications that use them, requiring procedures for secure transfer and deletion of transient copies. As the number of applications and access keys grows, tracking and managing them becomes operationally challenging.

In contrast, X.509 certificates use public key infrastructure (PKI). The private key is generated directly on the application server and doesn’t leave it. Only a certificate signing request, which doesn’t contain secrets, is sent to the CA for signing and returning the certificate. This alleviates the need for securely transmitting secret keys.

However, you can argue that X.509 certificates are also long-lived credentials. This concern is valid, but not necessarily true. As demonstrated by projects such as Let’s Encrypt, it’s possible to reduce certificate lifetimes from years to months by implementing automation for certificate renewal. After such a mechanism is in place, certificate lifetimes can be further limited to days or even hours.

In this post, we introduce mutually authenticated Transport Layer Security (mTLS), which uses certificates for high-assurance bidirectional authentication. Certificates are used to establish trust between the client and server, making sure that both parties are authenticated and authorized to communicate securely. By implementing mTLS, you can achieve a higher level of security and trust in your communication channels, mitigating potential risks associated with unauthorized access or man-in-the-middle attacks. Here, we implement ephemeral certificates that are tied to the lifecycle of pods. When a pod is started, a certificate is automatically created, and it expires after a short period of time unless it’s actively in use by the pod, in which case it’s automatically renewed by the cert-manager. This approach verifies that certificates are only valid for the duration of the pod’s lifetime, minimizing the potential risk associated with long-lived credentials. Additionally, IAM Roles Anywhere supports certificate revocation list (CRL) checks, allowing you to perform explicit revocation of certificates if required. This feature provides an additional layer of security, enabling you to revoke access promptly in case of compromised credentials or other security concerns.

Throughout this post, we assume that you have a basic understanding of IAM Roles Anywhere. For more information you can see this blog post. Furthermore, we assume that you are familiar with Kubernetes, kubectl, Helm, and cert-manager.

## Solution overview

This solution assumes that you have an existing Kubernetes cluster running outside of AWS.

Figure 1 shows the high-level architecture of our solution. An on-premises Kubernetes cluster accessing AWS APIs using IAM Roles Anywhere with X.509 certificates issued by AWS Private CA in short-lived-certificate mode.

![Figure 1: High level architecture of on-premises Kubernetes accessing AWS APIs](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/30/img1-2.png)

Figure 1: High level architecture of on-premises Kubernetes accessing AWS APIs

Here’s how the solution works, as shown in Figure 1:

1. An AWS Private CA in short-lived certificate mode issues X.509 certificates for your pods.
2. When you set up your AWS Private CA as a trusted source and establish a specific profile, IAM Roles Anywhere will validate and accept authentication requests that use certificates issued by your AWS Private CA.
3. cert-manager, deployed into your Kubernetes cluster, orchestrates the issuance of AWS Private CA certificates to authorized pods.
4. Each pod uses IAM Roles Anywhere to create an AWS session using its private key and X.509 certificate obtained from cert-manager.

Let’s explore the different parts of the architecture in more detail.

### AWS Private CA short lived credentials

AWS Private CA offers a short-lived certificate, where the validity period is limited to 7 days or fewer. You can see this AWS Blog to learn how to use AWS Private CA short-lived certificates. This new mode can be used to issue certificates for your Kubernetes pods and benefit from lower costs of operations. By synchronizing the certificate lifecycle with the lifecycle of the pod, you can minimize the operational overhead for this solution. To help meet requirements for auditability and transparency, you can use the audit report feature to list the issued certificates in a machine readable format.

### IAM Roles Anywhere

Figure 2 shows a detailed overview of the components involved in authentication with IAM Roles Anywhere.

![Figure 2: Components of IAM Roles Anywhere](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/30/img2-3.png)

Figure 2: Components of IAM Roles Anywhere

IAM Roles Anywhere allows you to obtain temporary security credentials for workloads that run outside of AWS. Your workloads must use a certificate issued by a trusted PKI CA to authenticate with IAM Roles Anywhere. You establish trust between IAM Roles Anywhere and your CA by creating a trust anchor that points to the root of the CA.

## cert-manager

Figure 3 shows a detailed overview of the cert-manager setup used in this post, including the `aws-privateca-issuer` add-on for the integration of AWS Private CA.

![Figure 3: Detailed overview of cert-manager setup](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/30/img3-3.png)

Figure 3: Detailed overview of cert-manager setup

cert-manager is a tool for managing X.509 certificates in Kubernetes. As shown in Figure 3, cert-manager will make sure that certificates are valid and up-to-date and attempt to renew them before they expire. By using add-ons, you can configure different backends for issuing X.509 certificates. In this post, we explore how to integrate cert-manager with AWS Private CA using the `aws-privateca-issuer` add-on. The `aws-privateca-issuer` add-on defines two custom resources, `AWSPCAIssuer` and `AWSPCAClusterIssuer`, which are used to configure the link to AWS Private CA. They are similar to the `Issuer` and `ClusterIssuer` resources that come with cert-manager, but specific to `aws-privateca-issuer`.

After the `AWSPCAIssuer` or `AWSPCAClusterIssuer` is available, `aws-privateca-issuer` authenticates towards AWS APIs using temporary security credentials obtained from IAM Roles Anywhere. cert-manager watches for the certificate resource, which references to an `AWSPCAIssuer`, which in turn references to AWS Private CA. `aws-privatca-issuer` requests a certificate from AWS Private CA. The auto-generated private key and the signed certificate are stored in Kubernetes secrets.

### Using certificates and secrets

cert-manager supports multiple ways of integrating into your Kubernetes workloads. You can use certificate resources, which represent a human-readable definition of a certificate signing request (CSR) and contain information on certificate lifespan and renewal time. When using a certificate, the auto-generated private key and the signed certificate are stored in Kubernetes secrets.

With this option, an X.509 certificate is issued manually and saved as a secret. After a PKI is configured as an issuer, a certificate resource is created to automate the renewal of the certificate. With the certificate resource, the lifecycle of certificates is decoupled from the lifecycle of the pods that use them. This allows you to bootstrap the X.509 certificate even before the trusted PKI is deployed.

### Using the CSI driver

Another way of integrating cert-manager is by using a CSI driver. In this case, the certificate lifecycle is bound to the lifecycle of the pod. An X.509 certificate and private key are mounted into a predefined folder where your workloads can read them. On pod creation, cert-manager automatically creates a private key and requests a certificate for the configured trusted PKI. When the pod is deleted, the private key and certificate are also deleted and become invalid because they aren’t renewed by cert-manager.

In this post, we use the CSI driver approach for workloads to create ephemeral certificates for IAM Roles Anywhere.

## Workload configuration

Figure 4 shows a detailed view of how pods can be configured to use IAM Roles Anywhere without needing to change the underlying Docker images by using a sidecar that provides an IMDSv2 endpoint that mimics the behavior in the Amazon Elastic Compute Cloud (Amazon EC2) instance metadata endpoint.

![Figure 4: Pod configuration using a sidecar](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/30/img4-3.png)

Figure 4: Pod configuration using a sidecar

As shown in Figure 4, when using a certificate resource, the auto-generated private key and the signed certificate are stored in Kubernetes secrets and mounted into the pod. When using the CSI driver, a private key is generated locally (for the pod), a certificate is requested from cert-manager based on the given attributes and is issued by `AWSPCAIssuer`, and the certificates are mounted directly into the pod with no intermediate secret being created.

IAM Roles Anywhere uses the CreateSession API to authenticate requests with a SigV4a signature using the private key and its associated X.509 certificate. This exchange provides a IAM role session credential, as if you had assumed the IAM role. The aws\_signing\_helper binary is provided to call the `CreateSession` API from the command line. In this post, a sidecar container that provides an IMDSv2 endpoint to the workload container is used. This container uses the `aws_signing_helper` binary and uses its serve command.

This way, applications using AWS SDKs can use the `AWS_EC2_METADATA_SERVICE_ENDPOINT` environment variable to set the instance metadata endpoint to the correct port on the localhost interface. The X.509 certificate and private key are provided as files to the sidecar container.

## Solution deployment

In this section, we show the steps needed to deploy the solution in your AWS account.

### Prerequisites

To deploy the solution in this post, make sure that you have the following in place:

- AWS Command Line Interface (AWS CLI) v2
- An AWS account and IAM permissions for IAM, IAM Roles Anywhere, and AWS Private CA
- Latest stable Kubernetes
- kubectl (matching your Kubernetes version)
- Helm 3
- jq

> **Note**: As an alternative to using the AWS CLI, you can use the AWS Controllers for Kubernetes (ACK) service controller for AWS Private CA for creating and managing `CertificateAuthority`, `Certificate`, and `CertificateAuthorityActivation` resources directly within your Kubernetes cluster. After establishing your CA hierarchy using the ACK controller, you can proceed with the subsequent steps involving IAM Roles Anywhere integration, `aws-privateca-issuer`, and cert-manager as described in this post.

### Step 1 – AWS Private CA

1. Set up a root CA in AWS Private CA, which will issue short lived certificates for your pods. In this example you use only one CA; for production environments, you should check the considerations for designing CA hierarchies. Start by using the AWS CLI to create a configuration.
    
    ```
    cat <<EOF > ca-config.json
    {
       "KeyAlgorithm":"RSA_2048",
       "SigningAlgorithm":"SHA256WITHRSA",
       "Subject":{
          "Country":"DE",
          "Organization":"Example Corp",
          "OrganizationalUnit":"SREs",
          "State":"HE",
          "Locality":"FRANKFURT",
          "CommonName":"Blogpost CA"
       }
    }
    EOF
    ```
    
2. Create the CA in AWS Private CA with short-lived certificates mode.
    
    ```
    aws acm-pca create-certificate-authority 
      --certificate-authority-configuration file://ca-config.json 
      --certificate-authority-type "ROOT" 
      --usage-mode SHORT_LIVED_CERTIFICATE
    ```
    
3. The command will return a `CertificateAuthorityArn`, which you will need for further commands, so export it for later use. Replace `<region>` with your AWS Region.
    
    ```
    export PCA_ARN=arn:aws:acm-pca:<region>:012345678912:certificate-authority/8213159d-cad0-481c-bf14-a0ced4d6d479
    ```
    
4. After creating the root CA, the CA is in a pending state. You need to create a CSR.
    
    ```
    aws acm-pca get-certificate-authority-csr 
         --certificate-authority-arn ${PCA_ARN} 
         --output text > ca.csr
    ```
    
5. Now, the CSR needs to be signed by the root CA.
    
    ```
    aws acm-pca issue-certificate 
         --certificate-authority-arn ${PCA_ARN} 
         --csr fileb://ca.csr 
         --signing-algorithm SHA256WITHRSA 
         --template-arn arn:aws:acm-pca:::template/RootCACertificate/V1 
         --validity Value=365,Type=DAYS
    ```
    
6. This command returns a `CertificateArn` which you will need later. Export it.
    
    ```
    export ROOT_CA_CERTIFICATE_ARN=arn:aws:acm-pca:<region>:012345678912:certificate-authority/8213159d-cad0-481c-bf14-a0ced4d6d479/certificate/5830e475088eee553bd409b7f4964613
    ```
    
7. Download the root CA certificate and upload it to your AWS Private CA.
    
    ```
    aws acm-pca get-certificate 
        --certificate-authority-arn ${PCA_ARN} 
        --certificate-arn ${ROOT_CA_CERTIFICATE_ARN} 
        --output text > cert.pem
    
    aws acm-pca import-certificate-authority-certificate 
         --certificate-authority-arn ${PCA_ARN} 
         --certificate fileb://cert.pem
    ```
    
8. Verify the status of the PCA, it should be `ACTIVE`.
    
    ```
    aws acm-pca describe-certificate-authority 
        --certificate-authority-arn ${PCA_ARN} 
        --output json
    ```
    

### Step 2 – IAM Roles Anywhere

At this point your root CA is set up and ready to use. The next step is to configure IAM Roles Anywhere.

1. Start by defining a trust anchor that will refer to your newly created AWS Private CA and export the `trustAnchorArn`. Replace `<value-of-trustAnchorArn>` with the Amazon Resource Name (ARN) value of your IAM Roles Anywhere trust anchor.
    
    ```
    aws rolesanywhere create-trust-anchor 
    --name onprem-k8s-issuer 
    --enabled 
    --source sourceType=AWS_ACM_PCA,sourceData={acmPcaArn=${PCA_ARN}}
    
    export TRUST_ANCHOR_ARN=<value-of-trustAnchorArn>
    ```
    
2. Create an IAM role to be used by the `aws-privateca-issuer` cert-manager plugin. This role needs to include the actions `sts:AssumeRole`, `sts:SetSourceIdentity` and `sts:TagSession`, which are required by IAMRA. Replace `<TA_ID>` with your trust anchor.  
    
    > **Note**: You should specify a PrincipalTag with the CN. Furthermore, it should be scoped to the IAMRA service principal. This further restricts authorization based on attributes that are extracted from the X.509 certificate and provides an additional layer of security by helping to ensure that even if an unauthorized party gains access to a valid certificate, they cannot assume the role unless the certificate’s CN matches the specified value.
    
    ```
    cat <<EOF > trust-policy.json
    {
        "Version": "2012-10-17",
        "Statement": [{
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": {
                "Service": "rolesanywhere.amazonaws.com"
            },
            "Action": [
                "sts:AssumeRole",
                "sts:SetSourceIdentity",
                "sts:TagSession"
            ],
            "Condition": {
                "StringEquals": {
                    "aws:PrincipalTag/x509Subject/CN": "iamra-issuer"
                },
                "ArnEquals": {
                    "aws:SourceArn": [
                        "arn:aws:rolesanywhere:<region>:012345678912:trust-anchor/<TA_ID>"
                    ]
                }
    
            }
        }]
    }
    EOF
    ```
    
    - Use the following to create the `iamra-issuer` role:
        
        ```
        aws iam create-role --role-name iamra-issuer 
          --assume-role-policy-document file://trust-policy.json
        ```
        
3. The command will return a JSON document containing information about the newly created role. Export the ARN for later use.
    
    ```
    export IAMRA_ISSUER_ROLE=arn:aws:iam::012345678912:role/iamra-issuer
    ```
    
4. Attach an inline policy that allows the role request certificates from your PCA and retrieve these. Note that there is a condition limiting the AWS Private CA templates to only allow `EndEntityCertificate`.
    
    ```
    cat <<EOF > inline-policy.json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "awspcaissuerread",
          "Action": [
            "acm-pca:DescribeCertificateAuthority",
            "acm-pca:GetCertificate"
          ],
          "Effect": "Allow",
          "Resource": "$PCA_ARN"
        },
        {
          "Sid": "awspcaissuerwrite",
          "Action": [
            "acm-pca:IssueCertificate"
          ],
          "Effect": "Allow",
          "Resource": "$PCA_ARN",
          "Condition":{
            "StringEquals":{
              "acm-pca:TemplateArn":"arn:aws:acm-pca:::template/EndEntityCertificate/V1"
            }
          }
        }
      ]
    }
    EOF
    ```
    
    - Use the following to associate the inline policy (created in the preceding step) with the `iamra-issuer` role.
        
        ```
        aws iam put-role-policy --role-name iamra-issuer 
          --policy-name iamra-issuer 
          --policy-document file://inline-policy.json
        ```
        
5. To finish, create a profile that defines which IAM roles can be assumed and then export the returned ARN.
    
    ```
    aws rolesanywhere create-profile --name iamra-issuer 
      --role-arns ${IAMRA_ISSUER_ROLE} 
      --enabled
    ```
    
    - Export the returned ARN:
        
        ```
        export IAMRA_PROFILE_ARN=arn:aws:rolesanywhere:<region>:012345678912:profile/<Profile_ID>
        ```
        

The created role `iamra-issuer` will only be used by the `aws-privateca-issuer` to integrate with AWS Private CA. You should repeat the process of creating IAM roles and IAMRA profiles for your workloads. it’s recommended to create a separate IAM role for each workload and limit its use with condition statements in the trust policy, checking for the workload identity and trust anchor (for example, matching the common name). Furthermore, it’s important that you add IAMRA to the trust policy and allow the aforementioned actions. Best practice with IAM roles is to apply least-privilege permissions.

### Step 3 – Create the init container

To integrate IAM Roles Anywhere within your Kubernetes environment, you need to provide an IMDSv2 endpoint to your application containers by running the `aws_signing_helper` binary as a sidecar. You also need to configure your applications using an environment variable to use the new instance metadata endpoint. To do so, build a Docker image that works as a sidecar.

In this step, create a basic image that fulfills the preceding requirements. In your environment, you might want to adapt this example to use your own base image and implement your image hardening processes.

Copy the following script and save it as `init.sh`.

```
#!/bin/sh

if [[ -z "$TRUST_ANCHOR_ARN" ]]; then
  echo "Must provide TRUST_ANCHOR_ARN environment variable." 1>&2
  exit 1
fi

if [[ -z "$PROFILE_ARN" ]]; then
  echo "Must provide PROFILE_ARN environment variable." 1>&2
  exit 1
fi

if [[ -z "$ROLE_ARN" ]]; then
  echo "Must provide ROLE_ARN environment variable." 1>&2
  exit 1
fi

echo "starting IMDSv2 endpoint with aws_signing_helper ..."
/aws_signing_helper serve 
  --certificate /iamra/tls.crt         
  --private-key /iamra/tls.key         
  --trust-anchor-arn $TRUST_ANCHOR_ARN 
  --profile-arn $PROFILE_ARN           
  --role-arn $ROLE_ARN
```

This script is the entry point of the sidecar container. It expects the environment variables `TRUST_ANCHOR_ARN`, `PROFILE_ARN`, and `ROLE_ARN`, which are required by `aws_signing_helper`. It also expects an X.509 certificate and its private key in the folder `/iamra`, which will be mounted in a later stage during pod initialization. Finally, it invokes the `aws_signing_helper` with the `serve` directive which creates an IMDSv2 endpoint listening on 9911 by default. This can be customized using the `--port` parameter.

Now let’s inspect the Docker file.

> **Note**: At the time of writing, we used the alpine3.17.0 image. Use a hardened base image that’s designed to be secure and aligns with the requirements of your environment.

```
FROM alpine:3.17.0

COPY init.sh .
RUN apk add --no-cache libc6-compat libgcc wget
RUN wget https://rolesanywhere.amazonaws.com/releases/1.3.0/X86_64/Linux/aws_signing_helper
RUN chmod +x /aws_signing_helper /init.sh 
RUN ln -s /lib/libc.musl-x86_64.so.1 /lib/libresolv.so.2
ENTRYPOINT ["/bin/sh", "-c", "/init.sh"]
```

This Docker file copies the `init.sh` and downloads the `aws_signing_helper` binary. The `init.sh` script is defined as an entry point to the container. Dynamic libraries required by `aws_signing_helper` are installed using Alpine Linux package manager (Apk).

Now build the docker image, sign in to it, and push it for later use. For the following commands replace `<my-docker-registry>` with the hostname of your local registry or use an ECR Repository.

```
docker build . -t <my-docker-registry>/iamra-sidecar
docker login <my-docker-registry>
docker push <my-docker-registry>/iamra-sidecar
```

### Step 4 – Install cert-manager

In this step, install cert-manager into your cluster and configure `aws-privateca-issuer` using a manually bootstrapped certificate. `cert-manager-approver-policy` is used to control which certificates can be requested by the workloads. Then, set up the cert-manager CSI driver to automatically provision X.509 certificates for your workload pods.

Start with the cert-manager setup:

1. Add the cert-manager repository to Helm and install the chart.  
    
    > **Note**: At the time of writing, we used cert-manager version 1.16.2. Check for the latest stable version.
    
    ```
    helm repo add jetstack https://charts.jetstack.io
    helm repo update
    helm install 
      cert-manager jetstack/cert-manager 
      --namespace cert-manager 
      --create-namespace 
      --version v1.16.2 
      --set installCRDs=true 
      --set extraArgs={--controllers='*,-certificaterequests-approver'}
      
    helm install 
      cert-manager-approver-policy jetstack/cert-manager-approver-policy 
      --namespace cert-manager 
      --wait 
        --set app.approveSignerNames="{
    issuers.cert-manager.io/*,clusterissuers.cert-manager.io/*,
    awspcaclusterissuers.awspca.cert-manager.io/*,awspcaissuers.awspca.cert-manager.io/*
    }"
    
    
    #make modifications in cert-manager-approver-policy and add below permissions
    
    kubectl edit  Clusterrole cert-manager-approver-policy -n cert-manager -o yaml
    
    - apiGroups:
      - awspca.cert-manager.io
      resources:
      - awspcaissuers
      - awspcaclusterissuers
      verbs:
      - get
      - list
      - watch
    - apiGroups:
      - cert-manager.io
      - awspca.cert-manager.io
      resources:
      - signers
      verbs:
      - approve
    ```
    
    Now, install the cert-manager `aws-privateca-issuer` plugin. This integration connects cert-manager with AWS Private CA and lets you issue short-lived certificates automatically. Currently, `aws-privateca-issuer` Helm chart doesn’t support IAMRA natively. So, you’re going to use the same `init-container` to set up IAMRA as for the workload pods.
    
    You need to issue the first X.509 certificate for `aws-privateca-issuer` IAMRA manually. Later, cert-manager will renew it automatically.
    
2. Create the bootstrap certificate. When asked for a common name, enter `iamra-issuer`.
    
    ```
    openssl req -out iamra.csr -new -newkey rsa:2048 
    -nodes -keyout iamra.key
    ```
    
    The previous command will create an RSA private key named `iamra.key` and a certificate signing request name `iamra.csr`. Now you need to call AWS Private CA to issue the bootstrap certificate.
    
3. Set the validity period of the certificate to 1 day so that cert-manager will replace it after it’s set up. The IAM role that’s performing this action must have permissions to AWS Certificate Manager (ACM), IAM, and IAM Roles Anywhere to complete the setup.
    
    ```
    aws acm-pca issue-certificate 
          --certificate-authority-arn ${PCA_ARN} 
          --csr fileb://iamra.csr 
          --signing-algorithm "SHA256WITHRSA" 
          --validity Value=1,Type="DAYS"
    ```
    
4. The command will return a `CertificateArn` for your `iamra-issuer` certificate. Export it and save the certificate to a file.
    
    ```
    export IAMRA_ISSUER_CERT_ARN=arn:aws:acm-pca:<region>:012345678912:certificate-authority/8213159d-cad0-481c-bf14-a0ced4d6d479/certificate/afc47911ed2ded9c2664fa597a33b9fb
    aws acm-pca get-certificate 
          --certificate-authority-arn ${PCA_ARN} 
          --certificate-arn ${IAMRA_ISSUER_CERT_ARN} | 
          jq -r .'Certificate' > iamra-cert.pem
    ```
    
5. Create a Kubernetes secret that contains the certificate and private key.
    
    ```
    kubectl create secret tls -n cert-manager iamra-issuer 
      --cert=iamra-cert.pem 
      --key=iamra.key
    ```
    
    You’re ready to install the `aws-privateca-issuer`. You need to modify the Helm chart because it doesn’t currently support IAMRA. You will render the Helm chart into YAML manifests, which are then adapted for IAMRA.
    
6. Install the Helm repository and render the charts into a file.
    
    ```
    helm repo add awspca https://cert-manager.github.io/aws-privateca-issuer
     helm template --release-name iamra --include-crds awspca/aws-privateca-issuer 
       -n cert-manager > privateca-issuer.yaml
    ```
    
7. Add your previously built image as a sidecar and replace the environment variables with your exported values. Search for the deployment definition and add the following section:
    
    ```
    # Source: aws-privateca-issuer/templates/deployment.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: iamra-aws-privateca-issuer
      namespace: cert-manager
      labels:
        helm.sh/chart: aws-privateca-issuer-v1.4.0
        app.kubernetes.io/name: aws-privateca-issuer
        app.kubernetes.io/instance: iamra
        app.kubernetes.io/version: "v1.4.0"
        app.kubernetes.io/managed-by: Helm
    spec:
      replicas: 1
      revisionHistoryLimit: 10
      selector:
        matchLabels:
          app.kubernetes.io/name: aws-privateca-issuer
          app.kubernetes.io/instance: iamra
      template:
        metadata:
          labels:
            app.kubernetes.io/name: aws-privateca-issuer
            app.kubernetes.io/instance: iamra
        spec:
          serviceAccountName: iamra-aws-privateca-issuer
          securityContext:
            runAsUser: 65532
          volumes:
            - name: "iamra-secret"
              projected:
                sources:
                  - secret:
                      name: iamra-issuer
          containers:
            - name: iamra-sidecar
              image: 012345678912.dkr.ecr.us-east-2.amazonaws.com/<replace-with-iamra-sidecar-repository>
              imagePullPolicy: Always
              env:
                - name: "TRUST_ANCHOR_ARN"
                  value: "arn:aws:rolesanywhere:us-east-2:012345678912:trust-anchor/05d183f8-a34e-4f0c-ad2a-de6f803"
                - name: "PROFILE_ARN"
                  value: "arn:aws:rolesanywhere:us-east-2:012345678912:profile/7b45f9a9-73fa-47f8-a20f-88aacbf57"
                - name: "ROLE_ARN"
                  value: "arn:aws:iam::012345678912:role/iamra-issuer"
              volumeMounts:
                - name: iamra-secret
                  mountPath: "/iamra"
                  readOnly: true
            - name: aws-privateca-issuer
              securityContext:
                allowPrivilegeEscalation: false
              image: "public.ecr.aws/k1n1h4h4/cert-manager-aws-privateca-issuer:latest"
              env:
               - name: "AWS_EC2_METADATA_SERVICE_ENDPOINT"
                 value: "http://localhost:9911/"
              imagePullPolicy: IfNotPresent
              command:
                - /manager
              args:
                - --leader-elect
              ports:
                - containerPort: 8080
                  name: http
              livenessProbe:
                httpGet:
                  path: /healthz
                  port: 8081
                initialDelaySeconds: 15
                periodSeconds: 20
              readinessProbe:
                httpGet:
                  path: /healthz
                  port: 8081
                initialDelaySeconds: 5
                periodSeconds: 10
          terminationGracePeriodSeconds: 10
    ```
    
8. Apply your modified manifest to install `aws-privateca-issuer` and verify the deployment you have modified. It should show that one pod is ready and available.
    
    ```
    kubectl apply -f privateca-issuer.yaml
    
    kubectl get deployment -n cert-manager -l app.kubernetes.io/name=aws-privateca-issuer
    
    NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
    iamra-aws-privateca-issuer   1/1     1            1           4d10h
    ```
    
9. Define an `AWSPCAIssuer`, which will be used for renewal of the manually bootstrapped certificate for the `aws-privateca-issuer` add-on.  
    
    > **Note**: At the time of writing, we used `awspca cert-manager` API version v1beta1. Check for the latest stable version.
    
    ```
    export AWS_REGION=<region>
    cat <<EOF | kubectl apply -f -
    apiVersion: awspca.cert-manager.io/v1beta1
    kind: AWSPCAIssuer
    metadata:
      name: iamra-cm-issuer
      namespace: cert-manager
    spec:
      arn: ${PCA_ARN}
      region: ${AWS_REGION}
    EOF
    ```
    
10. After at least one `AWSPCAIssuer` or `AWSPCAClusterIssuer` is available, `aws-privateca-issuer` is going to authenticate towards AWS APIs by calling `sts.get-caller-identity` and verify the authentication method. You can verify this using its log files. It should print the assumed role.
    
    ```
    kubectl logs -n cert-manager -l app.kubernetes.io/name=aws-privateca-issuer -c aws-privateca-issuer | grep -i getcalleridentity
    
    Defaulted container "aws-privateca-issuer" out of: aws-privateca-issuer, iamra-init (init)
    {"level":"info","ts":1669240040.2704494,"logger":"controllers.GenericIssuer","msg":"sts.GetCallerIdentity","genericissuer":"cert-manager/iamra-cm-issuer","arn":"arn:aws:sts::012345678912:assumed-role/iamra-issuer/5bafffcfb691969f0616a9b1a68032ec","account":"012345678912","user_id":"AROA2EIPPI5BVJ6SKBYOY:5bafffcfb691969f0616a9b1a68032ec"}
    ```
    
    Now, you can create a cert-manager `Certificate` resource that represents a desired certificate that should be issued by the referenced cert-manager `Issuer`. It combines information of a CSR with details on the validity period and renewal.
    
11. Create the certificate object:
    
    ```
    cat <<EOF | kubectl apply -f - 
      apiVersion: cert-manager.io/v1
      kind: Certificate
      metadata:
        name: iamra-privateca-issuer-cert
        namespace: cert-manager
      spec:
        secretName: iamra-issuer
        duration: 168h # 7d
        renewBefore: 24h # 15d
        subject:
          organizations:
            - "Example Corp."
          organizationalUnits:
            - "Admin"
        commonName: "iamra-issuer"
        isCA: false
        usages:
          - "client auth"
          - "server auth"
        issuerRef:
          group: awspca.cert-manager.io
          kind: AWSPCAIssuer
          name: iamra-cm-issuer
      EOF
      helm upgrade -i -n cert-manager cert-manager-csi-driver jetstack/cert-manager-csi-driver --wait
      -- > install policies:
      policy + role + role binding to allow service account to accept certs.
      cat <<EOF | kubectl apply -f - 
      apiVersion: policy.cert-manager.io/v1alpha1
      kind: CertificateRequestPolicy
      metadata:
        name: iamra-issuer-policy
      spec:
        allowed:
          commonName:
            required: true
            value: "iamra-issuer"
          subject:
            organizations:
              values: ["Example Corp."]
              required: true
            organizationalUnits:
              values: ["Admin"]
              required: true
          usages:
          - "server auth"
          - "client auth"
        selector:
          issuerRef:
            group: awspca.cert-manager.io
            kind: AWSPCAIssuer
            name: iamra-cm-issuer
      ---
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRole
      metadata:
        name: cert-manager-policy:iamra-issuer-policy
      rules:
        - apiGroups: ["policy.cert-manager.io"]
          resources: ["certificaterequestpolicies"]
          verbs: ["use"]
          resourceNames: ["iamra-issuer-policy"]
      ---
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRoleBinding
      metadata:
        name: cert-manager-policy:iamra-issuer-policy
      roleRef:
        apiGroup: rbac.authorization.k8s.io
        kind: ClusterRole
        name: cert-manager-policy:iamra-issuer-policy
      subjects:
      - kind: ServiceAccount
        name: cert-manager
        namespace: cert-manager
      EOF
    ```
    

### Step 5 – Deploy your workload

In Step 4, sub-step 9, you created an `AWSPCAIssuer` named `iamra-cm-issuer`. You then used this `AWSPCAIssuer` to renew the manually bootstrapped certificate for the `aws-privateca-issuer`.

In Step 4, sub-step 11, you created the certificate `iamra-privateca-issuer-cert`, which is used by the `aws-privateca-issuer`.

In this step, you will deploy the sample workload. When deploying the sample workload, make sure to repeat the process of creating IAM roles and IAMRA profiles (from Step 2), the `AWSPCAIssuer` (Step 4, sub-step 9), and the `CertificateRequestPolicy` (Step 4, sub-step 11) for the certificate request.

For more information on certificate request policies, see the cert-manager documentation on approval policies.

Use the following code to deploy the workload.

```
cat <<EOF | kubectl apply -f -
  
apiVersion: v1
kind: Pod
metadata:
   creationTimestamp: null
   labels:
     run: acmpca-csi-test
   name: acmpca-csi-test
spec:
  containers:
      - name: iamra-sidecar
        image: 012345678912.dkr.ecr.us-east-2.amazonaws.com/<replace-with-iamra-sidecar-repository>
        imagePullPolicy: Always
        env:
          - name: "TRUST_ANCHOR_ARN"
            value: "arn:aws:rolesanywhere:us-east-2:012345678912:trust-anchor/05d183f8-a34e-4f0c-ad2a-de6f803ac172"
          - name: "PROFILE_ARN"
            value: "arn:aws:rolesanywhere:us-east-2:012345678912:profile/7b45f9a9-73fa-47f8-a20f-88aacbf579d2"
          - name: "ROLE_ARN"
            value: "arn:aws:iam::012345678912:role/iam-roles-anywhere-s3-full-access"
        volumeMounts:
          - name: "iamra-csi"
            mountPath: "/iamra"
            readOnly: true
      - name: aws-cli
        image: amazon/aws-cli:latest
        env:
        - name: "AWS_EC2_METADATA_SERVICE_ENDPOINT"
          value: "http://127.0.0.1:9911/"
        command:
          - sleep
          - "3600"
  dnsPolicy: ClusterFirst
  restartPolicy: Never
  volumes:
    - name: "iamra-csi"
      csi:
        readOnly: true
        driver: csi.cert-manager.io
        volumeAttributes:
            csi.cert-manager.io/issuer-name: my-pca
            csi.cert-manager.io/issuer-group: awspca.cert-manager.io
            csi.cert-manager.io/issuer-kind: AWSPCAIssuer
            csi.cert-manager.io/common-name: "${SERVICE_ACCOUNT_NAME}.${POD_NAMESPACE}"
            csi.cert-manager.io/duration: 168h
            csi.cert-manager.io/renew-before: 24h
            csi.cert-manager.io/is-ca: "false"
            csi.cert-manager.io/key-usages: "client auth, server auth"
  EOF
```

### Step 6 – Test your deployment

To test the deployment, you can use `kubectl exec` to access the `iamra-sidecar` container. Navigate to the `iamra` directory and check if the certificate and key are mounted.

Command:  
`kubectl exec -it acmpca-csi-test  – sh   ls | grep iamra`

Output: `iamra`

Command:  
`cd iamra   /iamra# ls`

Output: `ca.crt   tls.crt  tls.key`

You can also `exec` into the `aws-cli` container and verify the caller identity and make API calls to Amazon Simple Storage Service (Amazon S3):

Command:  
`kubectl exec -it acmpca-csi-test -c aws-cli  – sh   $aws sts get-caller-identity`

Output: You should see `iam-roles-anywhere-s3-full-access` in `caller-identity`.

Command:  
`$aws s3 ls`

Output: You should be able to list the S3 bucket based on the permissions associated with the assumed role.

## Summary

In this post, you learned about a solution for securely connecting on-premises Kubernetes workloads to AWS services using IAM Roles Anywhere. The approach alleviates the need for long-term access keys or public internet exposure of the Kubernetes API server. By using this solution for containerized and full stack applications, you can benefit from:

- **Enhanced security**: Use short-lived X.509 certificates instead of long-term credentials.
- **Simplified management**: Automate the certificate lifecycle with cert-manager and AWS Private CA.
- **Seamless integration**: No modifications are required to existing workload Docker files.
- **Consistent policies**: Use the same IAM roles and policies across AWS and on premises.

   
If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.  
 

![Varun Sharma](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/02/01/smvarun.jpeg) Varun Sharma  
Varun is a Senior AWS Cloud Security Engineer who wears his security cape proudly. Varun is a go-to subject matter expert for Amazon Cognito and IAM. When he’s not busy securing the cloud, you’ll find him in the world of security penetration testing. Outside of work, Varun switches gears to capture the beauty of nature through the lens of his camera.

![Nishant Mainro](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/02/12/nmainro.jpg) Nishant Mainro  
Nishant is a Senior Security Consultant in the AWS Professional Services team and is based in Atlanta, Georgia. He is a technical and passionate Amazonian with over 16 years of professional experience with a specialization in security, risk, and compliance. His specializes developing and enabling security controls at scale to empower customers to achieve the required security goals for their workloads.

![Roshini Jagarapu](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/30/roshij.jpg) Roshini Jagarapu  
Roshini is an Amazon EKS subject matter expert and an AWS Cloud Support Engineer based in India. She works with services such as Amazon EKS and Amazon ECS, helping customers run at scale. Her day-to-day work involves troubleshooting issues related to container technologies. Roshini conducts learning sessions to educate customers and is passionate about cloud-native solutions.

Go to Source
