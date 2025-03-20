---
title: "<div>Per-Bucket Access Keys Now Available for DigitalOcean Spaces</div>"
date: 2025-01-15
---

We’re excited to announce the general availability of Per-Bucket Access Keys for DigitalOcean Spaces Object Storage. This highly requested feature gives you fine-grained control over who can access specific storage buckets with read-only or read/write permissions, making it easier to secure and manage your data.

![spaces per bucket screenshot](https://doimages.nyc3.cdn.digitaloceanspaces.com/002Blog/spaces-per-bucket-keys-screenshot.png)

## What Are Per-Bucket Access Keys?

With Per-Bucket Access Keys, you can assign unique access credentials with read-only or read/write permission for individual buckets. This enables you to grant the precise level of access required for different teams, applications and use cases, without over-permissioning.

### A Real-World Example

Let’s say you’re running a photography business with three key storage buckets:

- Raw Photos: Only accessible by your editing team.
- Final Photos: Your client portal needs read-only access here.
- Marketing Materials: Your social media automation tools need access to this bucket.

Before Per-Bucket Access Keys, managing permissions for these buckets could get tricky. Now, you can:

- Create an access key for your Raw Photos bucket that’s restricted to the editing team.
- Generate a read-only key for the Final Photos bucket that your client portal can use.
- Assign an access key for your social media tools to interact with the Marketing Materials bucket.

## Key Benefits

Per-Bucket Access Keys open up a range of new possibilities for businesses and developers:

- Enhanced Security: Help ensure applications and team members only have access to the data they need.
- Multi-Tenant Environments: Better safeguard customer data by isolating access for each tenant.
- Environment Isolation: Keep development, staging, and production environments separate within the same account.
- Application-Specific Access: Reduce the impact of a compromised access key by limiting its scope to a single bucket.
- Secure File Sharing: Share files without exposing the rest of your bucket’s contents.

### Security Best Practices

This new feature makes it easier to adopt the principle of least privilege, where users and applications are granted only the permissions they require. Here are some recommendations:

- Use separate keys for different applications and team members.
- Opt for read-only keys whenever possible.
- Regularly review and rotate your access keys.
- Combine Per-Bucket Access Keys with presigned URLs to enable user-specific file uploads without granting broad bucket access.

### Future Enhancements

We’re continuously working to improve and expand the capabilities of Per-Bucket Access Keys. Here’s what’s on the horizon:

- API and CLI Support: By mid-2025, you’ll be able to create Per-Bucket Access Keys through the DigitalOcean API and CLI, in addition to the DigitalOcean Control Panel.
- S3-Compatible Bucket Policy Support: Compatibility with S3-compatible bucket policies (PutBucketPolicy) is in progress and expected to be available by mid-2025.

Stay tuned for these updates as we work to enhance functionality and improve your experience.

## Get Started Today

Per-Bucket Access Keys are available now in all DigitalOcean regions at no additional cost. To get started:

1. Visit the Access Keys tab (see image below) on the Spaces Object Storage page in the DigitalOcean Control Panel.
2. Create keys with read-only or full access permissions for specific buckets.
3. Refer to our documentation for detailed guidance.

<figure>

![image alt text](https://doimages.nyc3.cdn.digitaloceanspaces.com/002Blog/Diane-Blog-Images/Spaces-Per-Bucket-Access-Keys/Spaces%20Per-Bucket%20Access%20Keys%20Screenshot.png)

<figcaption>

Visit the Access Keys tab to create a new Access Key

</figcaption>

</figure>

If you haven’t tried Spaces Object Storage yet, now’s the perfect time to explore how seamless and affordable it is for your Kubernetes, App Platform, and Droplets storage needs. Try it today!

Go to Source
