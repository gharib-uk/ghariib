---
title: "Google Authd broker: authenticate to Ubuntu Desktop/Server with your Google account"
date: 2025-03-19
---

Today we are announcing the introduction of Authd support for Google IAM, allowing all Ubuntu users to use their Google account to authenticate to their desktop and servers.

The Google broker snap for Authd is available for free on Ubuntu Desktop and Server 24.04 and it works with both personal and Workspace Google accounts.

## What is Authd?

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_898,h_758/https://ubuntu.com/wp-content/uploads/5c6f/QR-purple-min.png)

In November 2024, we announced the general availability of Authd: a new authentication daemon for Ubuntu that allows direct integration with cloud-based identity providers for both Ubuntu Desktop and Server. At launch, we supported Microsoft Entra ID (formerly Azure Active Directory) and since then we have seen a strong adoption in enterprises that wanted to centralize their identity management controls.

Unlike traditional approaches that require the installation of a standalone infrastructure component (e.g. FreeIPA or Vault), Authd allows Ubuntu endpoints to integrate directly with the cloud, reducing maintenance complexity and centralizing visibility of all authentication events.

Authd features a modular structure that combines a privileged dev, exposing a normalized API over DBUS, and a broker snap to facilitate easy integration with different cloud services. This setup leverages the Oauth 2.0 Device Authorisation Grant (commonly referred to as the Device Flow) to help maintain more robust security and effective user authentication

## Use your Google account to authenticate to Ubuntu

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,c_fill,w_1243,h_767/https://ubuntu.com/wp-content/uploads/ceba/QR-black-min.png)

Google IAM is one of the most used consumer identity providers and we decided to support it as the second identity provider in order to give small businesses using Ubuntu and all our community members  the chance to experience Authd without needing to pay expensive monthly SaaS subscriptions.

With the appropriate configuration, the Google Authd broker is able to support both enterprise Google Workspace accounts and personal Google accounts. You can learn more about how to configure your machine by consulting the official product documentation.

## Device ownership, allowlist, and permission management

In addition to Google support we are also extending Authd functionalities to include the following highly requested from the community:

- **Allowlist** allows you to restrict machine access to a specified list of users who are allowed to log in after a successful authentication with the identity provider
- **Privilege management** administrators can configure custom claims on the identity providers to assign users to a specific linux group (e.g. sudo). This allows permission management on remote machines directly through the identity provider, e.g. by placing users into specific groups
- **Device ownership** introduces rules to define device ownership based on login rules. This feature is designed to simplify corporate laptop provisionings for companies with remote workforces.

All these additional features are supported on both the Entra ID and Google broker snaps and will also be used internally by Canonical on our corporate laptops.

## Get the new broker and additional resources

- Get the new Google Authd broker on Snapcraft
- Read the documentation
- View the project on Github
- Learn how to deploy it at scale with Landscape on Desktop and Server
- Read our Active Directory integration whitepaper

Go to Source
