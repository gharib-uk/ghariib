---
title: "Announcing AWS KMS Elliptic Curve Diffie-Hellman (ECDH) support"
date: 2025-01-05
categories: 
  - "cryptography"
  - "encryption"
  - "security"
tags: 
  - "awskeymanagementservice"
  - "awskms"
  - "cryptographiclibrary"
  - "securityidentitycompliance"
  - "securityblog"
---

When using cryptography to protect data, protocol designers often prefer symmetric keys and algorithms for their speed and efficiency. However, when data is exchanged across an untrusted network such as the internet, it becomes difficult to ensure that only the exchanging parties can know the same key. Asymmetric key pairs and algorithms help to solve \[…\]

When using cryptography to protect data, protocol designers often prefer symmetric keys and algorithms for their speed and efficiency. However, when data is exchanged across an untrusted network such as the internet, it becomes difficult to ensure that _only_ the exchanging parties can know the same key. Asymmetric key pairs and algorithms help to solve this problem by allowing a public key to be shared over an untrusted network. And by using a key agreement scheme, two parties can use each other’s public key in combination with their own private key to each derive the same shared secret.

We’re excited to announce that AWS Key Management Service (AWS KMS) now supports Elliptic Curve Diffie-Hellman (ECDH) key agreement on elliptic curve (ECC) KMS keys. You can use the new `DeriveSharedSecret` API action to enable two parties to establish a secure communication channel by using a derived shared secret.

In this blog post we provide an overview of the new API action and explain how it can help you establish secure communications by exchanging only public keys to obtain a derived shared secret. We then show example commands to demonstrate how AWS KMS and OpenSSL can be used by two parties to derive a shared secret.

With this new `DeriveSharedSecret` API action, customers can take an external party’s public key and, in combination with a private key that resides within AWS KMS, derive a shared secret which can be used to derive a symmetric encryption key with a key derivation function (KDF). Customers can then use this symmetric encryption key to encrypt data locally within their application.

The same external party can combine their own related private key with the customer’s corresponding public key from AWS KMS to derive the same shared secret.

Now that both parties have the same shared secret, they can generate a symmetric encryption key that can be used to encrypt and decrypt the data they exchange.

`DeriveSharedSecret` offers a simple and secure way for customers to use their private key from within their application, enabling new asymmetric cryptography use cases for keys protected by AWS KMS, such as elliptic curve integrated encryption scheme (ECIES) or end-to-end encryption (E2EE) schemes.

## AWS KMS DeriveSharedSecret overview

The AWS KMS API Reference documentation covers the `DeriveSharedSecret` API action in more detail than we include in this post. We broadly describe how to interact with the API action, using the following steps:

1. Create an elliptic curve (ECC) KMS key, selecting that the key be used for `KEY_AGREEMENT` and choosing one of the supported key specs. You will not be able to modify existing ECC keys to be used for key agreement.
2. Have another party create an elliptic curve key that matches the key spec you defined for your KMS key.
3. Retrieve the public key associated with your KMS key by using the existing `GetPublicKey` API action.
4. Exchange public keys through a trusted means of exchange with the other party. Note that `DeriveSharedSecret` expects a base64-encoded DER-formatted public key.
5. Use the other party’s public key as an input, along with your specified `KEY_AGREEMENT` key. The only key agreement algorithm supported by AWS KMS at launch is ECDH.
6. The other party should use the public key retrieved from AWS KMS and the private key associated with their generated ECC key pair to derive a shared secret.

The result of the preceding steps is that both parties have the same output without exchanging secret information. Only public keys were exchanged between the two parties. The output of `DeriveSharedSecret` is the raw shared secret. This shared secret is the multiplication of points on the elliptic curves and can result in many more bytes than are needed for an encryption key. We recommend that customers use a KDF, following the National Institute of Standards and Technology (NIST) SP800-56A Rev. 3 section 5.8 guidance, to derive encryption keys from this shared secret.

For the purposes of this post, we will demonstrate the steps by using the AWS CLI and OpenSSL command line. AWS has incorporated best practices for customers within the AWS Encryption SDK. You can find more details at AWS KMS ECDH keyrings.

## Example use case

An example use case where you might wish to use ECDH key agreement is for end-to-end encryption. Although protocols exist that provide a secure framework for secure communications (for example, within AWS Wickr), we will highlight the simplified high-level steps behind some of these protocols. In our example use case, Alice and Bob are both part of a messaging network. This network is managed by a centralized service, and this service must not be able to access Alice or Bob’s unencrypted messages.

![Figure 1: High-level architecture for the service described in the example use case](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/08/14/img1_v2.png)

Figure 1: High-level architecture for the service described in the example use case

As shown in Figure 1, Alice and Bob each have an ECC key pair and participate in the secret derivation by using ECDH, through the following steps:

1. Alice registers her public key in the centralized key storage service. A detailed discussion of the key storage service is beyond the scope of this post.
2. Bob, an AWS KMS user, calls the AWS KMS `GetPublicKey` action to obtain the public key for the ECC KMS key pair.
3. Bob registers his public key in the same centralized key storage service.
4. Alice, who wants to exchange encrypted messages with Bob, retrieves Bob’s public key from the centralized key storage service.
5. Bob gets a notification that Alice wants to communicate with him, and he retrieves Alice’s public key from the centralized key storage service.
6. Using Bob’s public key and her private key, Alice derives a shared secret by using her cryptography provider.
7. Using Alice’s public key and his private key, Bob derives a shared secret by using `DeriveSharedSecret`.
8. Alice and Bob now have an identical shared secret. From this shared secret, she can create a symmetric encryption key by using a suitable KDF. The symmetric encryption key can be used to create ciphertext that can be sent to Bob.

## Example use case walkthrough

You can use the following steps to create a KMS key for ECDH use and derive a shared secret by using AWS KMS. For our demonstration purposes, the user Alice (from our example use case) is using OpenSSL as the cryptography tool. We will show how the AWS KMS user Bob and OpenSSL user Alice can derive a shared secret by using each other’s public key.

### General prerequisites

You must have the following prerequisites in place in order to implement the solution:

- AWS CLI — The latest version is recommended. The example here uses aws-cli/2.15.40 and aws-cli/1.32.110.
- OpenSSL — The example here uses OpenSSL 3.3.0.
- Both parties (Alice and Bob, from our example use case) have an ECC key on the same curve. The steps in the next section, **Key creation prerequisite**, explain how these keys can be created.

#### Key creation prerequisite

Alice and Bob must use the same ECC curve during key creation. The `DeriveSharedSecret` API action supports curves ECC\_NIST\_P256, ECC\_NIST\_P384, and ECC\_NIST\_P521, which map to P-256, P-384, and P-521 respectively in OpenSSL. The curves that AWS KMS supports are the curves approved by the U.S. National Institute of Standards and Technology (NIST). Additionally, AWS KMS supports the SM2 key spec only in Amazon Web Services China Regions.

Bob creates an asymmetric KMS key for key agreement purposes

Bob creates a key pair in AWS KMS by using the CreateKey API action. In the following example, Bob creates an ECC key pair with `ECC_NIST_P256` for the `KeySpec` parameter and `KEY_AGREEMENT` for the `KeyUsage` parameter.

```
aws kms create-key 
--key-spec ECC_NIST_P256 
--key-usage KEY_AGREEMENT 
--description "Example ECDH key pair"
```

The response looks something like this:

```
{
    "KeyMetadata": {
        "AWSAccountId": "111122223333",
        "KeyId": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "Arn": "arn:aws:kms:us-east-1:111122223333:key/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
        "CreationDate": "2024-06-25T13:06:24.888000-07:00",
        "Enabled": true,
        "Description": "Example ECDH key pair",
        "KeyUsage": "KEY_AGREEMENT",
        "KeyState": "Enabled",
        "Origin": "AWS_KMS",
        "KeyManager": "CUSTOMER",
        "CustomerMasterKeySpec": "ECC_NIST_P256",
        "KeySpec": "ECC_NIST_P256",
        "KeyAgreementAlgorithms": [
            "ECDH"
        ],
        "MultiRegion": false
    }
}
```

You can follow the Creating asymmetric KMS keys documentation to see how to use the AWS Management Console to create a KMS key pair with the same properties as shown here. This example creates a KMS key with a default KMS key policy. You should review and configure your key policy according to the principle of least privilege, as appropriate for your environment.

> **Note:** When a KMS key is created, it will be logged by AWS CloudTrail, a service that monitors and records activity within your account. API calls to the AWS KMS service are logged in CloudTrail, which you can use to audit access to KMS keys.

To allow your KMS key to be identified by a human-readable string rather than by the `KeyId` value, you can create an alias for the KMS key (replace the `target-key-id` value of `a1b2c3d4-5678-90ab-cdef-EXAMPLE11111` with your `KeyId` value). This makes it easier to use and manage your KMS keys.

Bob creates an alias for his KMS key by using the CLI with the following command:

```
aws kms create-alias 
    --alias-name alias/example-ecdh-key 
    --target-key-id a1b2c3d4-5678-90ab-cdef-EXAMPLE11111 
```

Alice creates an ECC key for key agreement purposes by using OpenSSL

Using the ecparam and genkey option of OpenSSL, Alice creates a P-256 ECC key. The P-256 curve is represented by AWS KMS as ECC\_NIST\_P256.

> **Note:** For ECDH to work, the curve of the OpenSSL ECC key must be same as the ECC KMS key created by the other party (Bob, in our example use case).

```
openssl ecparam -name P-256 
        -genkey -out openssl_ecc_private_key.pem
```

### Key exchange and secret derivation process

The following sections outline the steps that Alice and Bob will follow to share their public keys, retrieve one another’s public key, and then derive the same shared secret using AWS KMS and OpenSSL. The shared secrets derived by Alice and Bob respectively are then compared to show that they both derived the same shared secret.

#### Step 1: Alice generates and registers her OpenSSL public key with a central service

AWS KMS expects the public key in DER format. Therefore, in this example Alice creates a DER-format public key by using her ECC private key. Alice runs the following command to produce a DER-format file that contains her public key:

```
openssl ec -in openssl_ecc_private_key.pem 
        -pubout -outform DER 
        > openssl_ecc_public_key.bin.der
```

The file `openssl_ecc_public_key.bin.der` will have the public key in DER format, which Alice can store in the centralized key storage service (or send to anyone she would like to communicate with). Details about the centralized key storage service are beyond the scope of this post.

#### Step 2: Bob obtains the public key for his ECC KMS Key

To retrieve a copy of the public key for his ECC KMS key, Bob uses the GetPublicKey API action. Bob calls this API by using the AWS CLI command get-public-key, as follows:

```
aws kms get-public-key 
    --key-id alias/example-ecdh-key 
    --output text 
    --query PublicKey | base64 --decode > kms_ecdh_public_key.der
```

The returned `PublicKey` value is a DER-encoded X.509 public key. Because the AWS CLI is being used, the public key output is base64-encoded for readability purposes. This base64-encoded value is decoded by using the base64 command, and the decoded value is stored in the output file. The file `kms_ecdh_public_key.der` contains the DER-encoded public key.

> **Note:** If you call this API by using one of the AWS SDKs, such as Boto3, then the returned `PublicKey` value is not base64-encoded.

In our example use case, Alice is using OpenSSL, which expects the public key in PEM format. Bob converts his DER-format public key into PEM format by using the following command:

```
openssl ec -pubin -inform DER -outform PEM 
        -in kms_ecdh_public_key.der 
        -out kms_ecdh_public_key.pem
```

The file `kms_ecdh_public_key.pem` contains the public key in PEM format.

#### Step 3: Bob registers his public key with the centralized key storage service

Bob saves his public key in PEM format, obtained in Step 2, in the centralized key storage service.

#### Step 4: Alice retrieves Bob’s public key to derive a shared secret

To perform ECDH key agreement, the two parties involved (Alice and Bob, in our example use case) need to exchange their public key with each other. Alice, who wants to send encrypted messages to Bob, retrieves Bob’s public key from the centralized key storage service.

Bob’s public key, `kms_ecdh_public_key.pem`, is already in PEM format as expected by OpenSSL.

#### Step 5: Bob retrieves Alice’s public key to derive a shared secret

To perform ECDH key agreement, the two parties involved, Alice and Bob, need to exchange their public key with each other. Bob gets a notification that Alice wants to communicate with him, and he retrieves Alice’s public key from the centralized key storage service.

Alice’s public key, `openssl_ecc_public_key.bin.der`, is already in DER format as expected by AWS KMS.

#### Step 6: Alice uses OpenSSL to derive the shared secret

Alice, using her private key and Bob’s public key, can derive the shared secret by using OpenSSL. Alice derives the shared secret by using the OpenSSL pkeyutl command with the derive option, as follows:

```
openssl pkeyutl -derive 
-inkey openssl_ecc_private_key.pem 
-peerkey kms_ecdh_public_key.pem > openssl.ss
```

The file `openssl.ss` will have the shared secret in binary format.

#### Step 7: Bob uses AWS KMS to derive the shared secret

Bob, using his private key (which remains securely within AWS KMS) and Alice’s public key, can derive the shared secret by using AWS KMS. The following example shows how Bob uses the DeriveSharedSecret API action with the AWS CLI command `derive-shared-secret`. At launch, the only supported key agreement algorithm is ECDH. Bob passes Alice’s public key for the `PublicKey` parameter.

```
aws kms derive-shared-secret 
--key-id alias/example-ecdh-key 
--public-key fileb://path/to/openssl_ecc_public_key.bin.der 
--key-agreement-algorithm ECDH 
--output text --query SharedSecret |base64 --decode > kms.ss
```

Because the AWS CLI is being used, the returned `SharedSecret` value is base64-encoded for readability purposes. Using the `base64 --decode` command, the decoded binary format is stored to the file.

> **Note:** If you call this API by using one of the AWS SDKs, such as Boto3, then the returned `SharedSecret` value is not base64-encoded.

The file `kms.ss` will have the shared secret in binary format.

#### Step 8: Using the shared secret and a suitable KDF, Alice derives an encryption key to encrypt her communication to Bob

You can use the following command to compare the two files containing the derived shared secrets that were obtained in Steps 6 and 7 and verify that they are identical:

```
diff -qs openssl.ss kms.ss
```

Because these files are identical, we can see that the same secret was derived using both AWS KMS and OpenSSL.

Using the shared secret, Alice should then derive a symmetric encryption key by using a suitable KDF. She can use this symmetric encryption key to encrypt data and send the ciphertext to Bob.

This blog post does not cover the steps to derive that symmetric encryption key, because that can be a complex topic depending on your use case. However, we note that you should not use the raw shared secret as an encryption key because it is not uniform. In other words, the shared secret has a lot of entropy, but the byte string itself is not random.

NIST recommends that you use a KDF function over the raw shared secret (value Z as described in section 5.8 of NIST SP800-56A Rev. 3). The KDFs that are recommended are described in more detail in NIST SP800-56C Rev. 2. One such example is OpenSSL Single Step KDF (SSKDF) EVP\_KDF-SS, but using this KDF involves choosing the other values, such as `FixedInfo`, carefully.

To help customers make the right choice for the resulting KDF to use on the shared secret, the AWS Encryption SDK now includes AWS KMS ECDH keyrings. The keyring is a construct within the AWS Encryption SDK that you implement within your code. The keyring handles the management of encryption keys while applying best practices to protect your data. You can use the keyring to reference your KMS keys for key agreement, and then call a function to encrypt data. Data will be encrypted by using a derived shared wrapping key following NIST recommendations, and the Encryption SDK applies key commitment to the ciphertext.

## Summary

In this blog post, we highlighted how you can use the recently launched `DeriveSharedSecret` API action to securely derive a shared secret. You’ve seen how ECDH can be used between two parties without having to share secret information across untrusted networks. We explained how you can audit your AWS KMS key usage through AWS CloudTrail logs. We highlighted that you would need to use a KDF to generate a symmetric encryption key from the shared secret. We strongly recommend that you use the AWS Encryption SDK to encrypt your data, which helps make sure that the recommended NIST key derivation functions are used for generating symmetric encryption keys.

   
If you have feedback about this post, submit comments in the **Comments** section below. If you have questions about this post, contact AWS Support.

![Patrick Palmer](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2019/01/21/patrick-palmer-author-bio.jpg)

Patrick Palmer  
Patrick is a Principal Security Specialist Solutions Architect at AWS. He helps customers around the world use AWS services in a secure manner and specializes in cryptography. When not working, he enjoys spending time with his growing family and playing video games.

![Raj Puttaiah](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/08/12/rajherur.jpg)

Raj Puttaiah  
Raj is a Software Development Manager for AWS KMS. Raj leads the development of AWS KMS features, focusing on operational excellence. When not working, Raj spends time with his family hiking the beautiful Washington outdoors, and accompanying his two sons to their activities.

![Michael Miller](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2024/08/14/millerdq.jpg)

Michael Miller  
Michael is a Senior Solutions Architect at AWS, based in Ireland. He helps public sector customers across the UK and Ireland accelerate their cloud adoption journey and specializes in security and networking. In prior roles, Michael has been responsible for designing architectures and supporting implementations across various sectors including service providers, consultancies, and financial services organizations.

Go to Source
