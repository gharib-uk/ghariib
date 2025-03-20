---
title: "Preserving content provenance by integrating Content Credentials into Cloudflare Images"
date: 2025-02-06
---

Today, we are thrilled to announce the integration of the Coalition for Content Provenance and Authenticity (C2PA) provenance standard into Cloudflare Images. Content creators and publishers can seamlessly preserve the entire provenance chain — from how an image was created and by whom, to every subsequent edit — across the Cloudflare network.

### What is the C2PA and the Content Authenticity Initiative?

When you hear the word provenance, you might have flashbacks to your high school Art History class. In that context, it means that the artwork you see at the Met in New York really came from the artist in question and isn’t a fake. Its provenance is how that piece of physical art changed possession over time, from the original artist all the way to the museum. 

Digital content provenance builds upon this concept. It helps you understand how a piece of digital media — images, videos, PDFs, and more — was created and subsequently edited. The provenance of a photo I posted on Instagram might look like this: I took the picture with my iPhone, performed an auto-magic edit using Apple Photos’ editing tools, uploaded it to Instagram, cropped it using Instagram’s editing tools, and then posted it. 

Why does digital content provenance matter? At a fundamental level, it’s an important way to give content creators credit for their work. Many photographers have had the experience of seeing their photograph or video go viral online, but with their name and attribution stripped away. In that scenario, the opportunities that might have accrued to the creator once the world saw their work don’t materialize. If you help ensure an artist or content creator gets credit for their work, that exposure could result in more career opportunities. 

Digital content provenance can also be an important tool in understanding the world around us. If you see a video or a photo of a newsworthy event, you’d like to know if that photo was really taken at that particular location, or if it was from years prior at a different location. If you see a grainy picture of a UFO flying over New Jersey, knowing when and where that photo was taken is helpful information in understanding what is actually happening. 

The C2PA is a project of the non-profit Joint Development Foundation and has developed technical specifications for attaching digital content provenance to a piece of media. The standards also specify how to cryptographically sign that manifest, thereby allowing anyone to verify that the manifest hasn't been tampered with. The JSON manifests and the associated signatures are together referred to as **Content Credentials**.

The Adobe-led Content Authenticity Initiative, which has thousands of members across a variety of industries, aims to drive global adoption of Content Credentials. 

### Why integrate Content Credentials into Cloudflare Images?

Cloudflare Images allows you to build an effortlessly scalable and cost-effective image pipeline. With our new Content Credentials integration, you can now preserve existing Content Credentials, ensuring they remain intact from creation all the way to end-user delivery.

Many media organizations across the globe, such as the BBC, the New York Times, and Dow Jones, are members of the Content Authenticity Initiative. Imagine one of these news organizations wanted to include the Content Credentials of their photojournalist’s photos and allow anyone to verify the provenance of that image. Before now, even if the news organization was using a C2PA-compliant camera and editing flow, these credentials would frequently be stripped if the image was transformed by their CDN.

If you use Cloudflare, that is now a solved problem. In Cloudflare Images, you can now preserve Content Credentials when transforming images from remote sources. Enabling this integration will retain any existing Content Credentials that are embedded in the image.

When you use Images to resize or change the file format to your images, these transformations will be cryptographically signed by Cloudflare. This ensures, for example, that the end-user who sees the photograph on your website can use an open-source verification service such as contentcredentials.org/verify  to verify the full provenance chain. 

### How it works

Imagine you are a photojournalist using a Nikon camera that has C2PA-compliant signing. That photojournalist  could opt to attach Content Credentials to their photo, identifying the key elements of the photograph such as the camera model, the original image size, and aperture settings. 

Below is a simplified example of what a C2PA-compliant Content Credential for a photograph taken with that Nikon camera could look like. 

Content Credentials are stored using JUMBF (JPEG Universal Metadata Box Format), which serves as a standardized container format for embedding metadata within files. You can think of it as an envelope system that packages together both the data about where a piece of digital content came from and how it changed, as well as the cryptographic signatures that can be used to verify that data.

The assertions, or facts about the content provenance, are typically written in JSON for a better developer experience. Note that this example deliberately simplifies the JUMBF box nesting and adds comments to make it easier to follow.

```
{
  "jumbf": {
    "c2pa.manifest": {
      "claim_generator": "Nikon Z9 Firmware v1.2",
      "assertions": [
        {
          "label": "c2pa.actions",
          "data": {
            "actions": [
              {
                "action": "c2pa.captured",
                "when": "2025-01-10T12:00:00Z",
                "softwareAgent": "Nikon Z9",
                "parameters": {
                  "captureDevice": "NIKON Z9",
                  "serialNumber": "7DX12345",
                  "exposure": "1/250",
                  "aperture": "f/2.8",
                  "iso": 100,
                  "focalLength": "70mm"
                }
              }
            ]
          }
        }
      ],
      "signature_info": {
        "issuer": "Nikon",
        "time": "2025-01-10T12:00:00Z",
        "cert_fingerprint": "01234567890abcdef"
      },
      "claim_metadata": {
        "claim_id": "nikon_z9_123"
      }
    }
  }
}
```

Now imagine that you want to use this photograph on your website. 

If you’ve enabled the Preserve Content Credentials setting in Cloudflare, then that metadata is now preserved in Cloudflare Images. 

If you use Cloudflare Images to dynamically resize or transform this image, then Cloudflare automatically appends and cryptographically signs any additional actions in that same manifest. Below we show what the new Content Credentials could look like. 

```
{
  "jumbf": {
    // Original Nikon manifest
    "c2pa.manifest.nikon": {
  /*unchanged*/      
},

    // New Cloudflare manifest
    "c2pa.manifest.cloudflare": {
      "claim_generator": "Cloudflare Images",
      "assertions": [
        {
          "label": "c2pa.actions",
          "data": {
            "actions": [
              {
                "action": "c2pa.resized",
                "when": "2025-01-10T12:05:00Z",
                "softwareAgent": "Cloudflare Images",
                "parameters": {
                  "originalDimensions": {
                    "width": 8256,
                    "height": 5504
                  },
                  "newDimensions": {
                    "width": 800,
                    "height": 533
                  }
                }
              }
            ]
          }
        }
      ],
      "signature_info": {
        "issuer": "Cloudflare, Inc",
        "time": "2025-01-10T12:05:00Z",
        "cert_fingerprint": "fedcba9876543210"
      },
      "claim_metadata": {
        "claim_id": "cf_resize_123",
        "parent_claim_id": "nikon_z9_123"
      }
    }
  }
}
```

In this example, the `c2pa.action.resized` entry describes a non-destructive transformation from one set of dimensions to another. This is included as a separate, independent assertion about this particular photograph. 

Notice how there are two cryptographic signatures in this manifest, each referenced by `signature_info`. Since there were two entities involved in this example image — Nikon for the image’s creation, then Cloudflare for resizing it — both Nikon and Cloudflare independently signed their respective assertions about the content provenance.  

In this example, the signature reference looks like this: 

```
"signature_info": {
        "issuer": "Cloudflare, Inc",
        "time": "2025-01-10T12:05:00Z",
        "cert_fingerprint": "fedcba9876543210"
```

During the creation, editing, and resizing process of a piece of digital content, a unique hash of metadata is created for each action and then signed using a private key. The signature, along with the signer’s public certificate or reference to it, are contained in the JUMBF container as referenced by this JSON. 

These hashes and signatures allow any open source verification tool to recalculate the hash, validate it against the signature, and check the certificate chain to ensure trustworthiness for each action taken on the image. This is what is meant by Content Credentials being tamper-evident: if any of these hashes and signatures fail to validate, it means that the metadata has been tampered with. 

Each cryptographic signature is part of a Trust List, allowing anyone to verify the provenance chain across various entities, such as from a camera manufacturer to photo editing software to distribution across Cloudflare. More from the Content Authenticity Initiative: 

> _Trust lists connect the end-entity certificate that signed a manifest back to the originating root CA. This is accomplished by supplying the subordinate public X.509 certificates forming the trust chain (the public X.509 certificate chain)._

In order for Cloudflare to append the Content Credentials with any transformations, we needed to have a publicly available end-entity certificate and join this Trust List. Here we used DigiCert for our end-entity certificate and reference this certificate in the JSON manifests that we are now creating in production:

```
"signature_info": {
        "alg": "sha256",
        "issuer": "Cloudflare, Inc",
        "cert_serial_number": "073E9F61ADE599BE128B02EDC5BD2BDE",
        "time": "2024-01-06T22:42:36+00:00"
      },
```

The end result is that news organizations, journalists, and content companies can now create an auditable chain of digital provenance whose claims can be verified using public-key cryptography.  

### Let’s see an example

Earlier this year, OpenAI announced their support for including Content Credentials in DALL-E. I recently created an image in DALL-E (with ski season on my mind).

Cloudflare Images allows you to transform any image by URL. You can do so by simply changing the URL structure using this syntax: 

```
https://<ZONE>/cdn-cgi/image/<OPTIONS>/<SOURCE-IMAGE>
```

We can break down each of these parameters:

- ZONE is your particular domain.
    
- cdn-cgi/image is a fixed prefix that identifies that this is a special path handled by a built-in Worker.
    
- The OPTIONS parameter allows you to then transform the image — rotating it, changing the width, compressing it, and more. 
    
- SOURCE-IMAGE is the URL where your image is currently hosted.
    

To tie these together, I then have a new URL structure where I want to change the width and quality of the image I created in DALL-E and display this on my personal website. After uploading the image from DALL-E to one of my R2 buckets, I can create this URL: 

```
https://williamallen.com/cdn-cgi/image/width=1000,quality=75,format=webp/https://pub-3d2658f6f7004dc38a4dd6be147b6a86.r2.dev/dalle.webp
```

Anyone can now verify its provenance using the Content Credentials Verify tool to see the result. The provenance chain is fully intact, even after using the Cloudflare Images transformation shown above to resize the image. 

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/p3qbySgWdTzk3IJTevbiU/bae7969708006369a70f8e6cfd7ae2bf/Screenshot_2025-01-29_at_19.00.56.png)

There are numerous open source command line tools that allow you to explore the full details of the Content Credentials. The C2PA Tool is created and maintained by the Content Authenticity Initiative. You can read more about the tool here and view the source code for it on GitHub.

There are two ways to install the tool: through a pre-built binary executable, or using Cargo Binstall if you have already installed Rust. Once installed, the C2PA Tool uses this syntax in your command line: 

```
c2patool [OPTIONS] <PATH> [COMMAND]
```

If I navigate to the link of the image in my browser and save it to my downloads folder on my Mac, then I simply need to use the command -d (short for -detailed) to see the full details of the JSON manifest. Of course, you should change _yourusername_ to your actual Mac username.

```
c2patool /Users/yourusername/Downloads/dalle.webp -d
```

And if you wanted to output this to a JSON file that you can review in VSCode or Cursor, use this command instead:

```
c2patool /Users/yourusername/Downloads/dalle.webp -d > manifest.json
```

This allows you to not just trust, but verify, the details of the image transformation yourself. 

### How to start using Cloudflare Images with Content Credentials 

It’s straightforward to start preserving Content Credentials. Log in to your Cloudflare dashboard and navigate to Images in the dashboard. From there, choose Transformations and choose a Zone where you want to enable this feature. Then toggle this option to on:

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6R24DfomWKBsb5SKMFmZU7/99049bd789ea89b4869bef9c8f0040f2/image2.png)

If the images you are transforming do not contain any Content Credentials, no action is taken. But if they do, we preserve those Content Credentials and attest to any transformations. 

### Looking ahead

We are excited to continue to partner with Adobe and many other organizations to extend support for preserving Content Credentials across our products and services. If you are interested in learning more, we’d love to hear from you: I’m @williamallen on X or on LinkedIn.

Go to Source
