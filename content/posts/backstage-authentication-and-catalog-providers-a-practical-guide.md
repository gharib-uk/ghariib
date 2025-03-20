---
title: "Backstage authentication and catalog providers: A practical guide"
date: 2025-01-07
---

A previous article demonstrated how you can get an internal developer portal up and running with Backstage on Kubernetes in a few minutes using Red Hat Developer Hub on Red Hat OpenShift. That article configured Red Hat Developer Hub with a guest authentication provider–an acceptable option for experimentation, but unsuitable for production environments where developers are actively using the internal developer portal.

This article guides you through the process of configuring a production-ready authentication provider and a catalog provider to import catalog entities. Specifically, you’ll learn how to use GitHub as an authentication provider, and a GitHub organization as a source of truth for organization data to create user and group entities. 

While you might plan to use one of Red Hat Developer Hub’s other supported authentication providers in production, the concepts introduced in this article will remain relevant in your internal developer portal journey.

## Prerequisites

Before diving into the specific instructions to configure an authentication provider, make sure that you have access to:

- An instance of Red Hat Developer Hub and Red Hat OpenShift (get started for free).
- A GitHub organization (create one for free).

## Backstage authentication providers

Authentication is a broad term, so let’s narrow it down to refer specifically to user authentication for the purposes of this article. In this case, the users are your developers accessing your internal developer portal.

When a developer attempts to log in to Red Hat Developer Hub, they’ll generally be redirected to an OpenID Connect-compatible provider or software-as-a-service (SaaS) such as GitHub. Figure 1 illustrates this process, although it omits some details for the sake of simplicity.

<figure>

![An image showing the authentication provider redirecting a user to an identity provider, and catalog provider synchronizing user and group data from the identity provider.](https://developers.redhat.com/sites/default/files/arch_10.png)

<figcaption>

Figure 1: An authentication provider redirecting a user to an identity provider, and a catalog provider synchronizing user and group data from the identity provider.

</figcaption>

</figure>

Synchronizing user and group entities into the Backstage Catalog is a crucial aspect of authentication in Backstage. By default, Backstage will prevent users without a corresponding user entity in the Catalog from logging in. This means that even if a user can successfully authenticate against your identity provider, they might be unable to access Backstage. 

A Catalog provider can be used to synchronize user and group information from an identity provider or other source, as seen in Figure 1. Your users will be able to log in once a user entity can be mapped to the authenticating user’s identity using a resolver.

**Note**: Red Hat Developer Hub supports a dangerouslyAllowSignInWithoutUserInCatalog configuration. Setting this to true removes the requirement to have corresponding user entities in the Catalog for any user that attempts to sign in. This should only be used for development and testing purposes.

## Configure a Backstage authentication provider

The following guide assumes that you’ve followed the prior article in this series, and have an instance of Red Hat Developer Hub running on OpenShift.

The high-level steps involved in configuring GitHub as an authentication and catalog provider are as follows:

1. Create a GitHub organization.
2. Create a GitHub Application within the GitHub organization.
3. Configure the Callback URL for the GitHub Application.
4. Create a Secret in OpenShift to store the GitHub Application details.
5. Update the Red Hat Developer Hub configuration to use the GitHub integration.

### Create a GitHub Application 

Remember the GitHub organization that was mentioned in the prerequisites? You’ll be using that as a location to repositories, and also create a Github Application that your Red Hat Developer Hub instance will use to communicate with GitHub APIs. 

The `create-github-app` command from the Backstage CLI is the easiest way to create a GitHub Application with the correct settings. To use this, find the name of your GitHub organization from your browser’s address bar, for example, `rhdh-demo-gh`, as shown in Figure 2.

<figure>

![An image showing a GitHub Organization's name in a browsers address bar.](https://developers.redhat.com/sites/default/files/gh-org-url-name.png)

<figcaption>

Figure 2: The GitHub organization's name in a browsers address bar.

</figcaption>

</figure>

Once you have the name of your organization, pass it to the following command, replacing the `<replace-me>` placeholder. Enable all of the permissions when requested by the CLI and press **Enter**. See below:

```plaintext
npx @backstage/cli create-github-app <replace-me>
```

**Note**: The `npx` command is bundled with recent versions of Node.js. Node.js v18.x was used when writing this article.

A browser window will appear. Sign in to GitHub if prompted, then enter a name for the new GitHub Application, e.g., `RHDH Blog`, and click **Create GitHub App**. See Figure 3.

<figure>

![The GitHub Create App UI that has a form for the user to enter the application name.](https://developers.redhat.com/sites/default/files/creating-gh-app.png)

<figcaption>

Figure 3: Creating a GitHub App in a GitHub organization.

</figcaption>

</figure>

You’ll be prompted to install this new application in your organization (Figure 4). Go ahead and click **Install**, granting access to the organization’s repositories.

<figure>

![A GitHub Organization administrator installing the previously created application and granting it access to all repositories in the Organization.](https://developers.redhat.com/sites/default/files/installing-gh-app.png)

<figcaption>

Figure 4: Installing the previously created application and granting it access to all repositories in the organization.

</figcaption>

</figure>

One last point to note is that a file named `github-app-rhdh-blog-credentials.yaml` was created in the directory where you executed the `create-github-app` command. You’ll need that file in a moment, so don’t lose it.

### Configure the GitHub Application callback URL

You’ll need to update your new GitHub Application with a callback URL that references your instance of Red Hat Developer Hub, otherwise the authentication redirect will fail due to GitHub not recognizing the host. You can do this by visiting the **GitHub Apps** section in the **Settings** > **Developer Settings** screen of your organization and clicking **Edit** on your application, as shown in Figure 5.

<figure>

![The GitHub App listing in the Developer Settings screen of the GitHub Organization.](https://developers.redhat.com/sites/default/files/edit-gh-app.png)

<figcaption>

Figure 5: The GitHub App listing in the Developer Settings screen of the GitHub organization.

</figcaption>

</figure>

The callback URL should be the URL to your instance of Red Hat Developer Hub, with the path `api/auth/github/handler/frame` appended to it. Scroll down and click **Save changes** once you’ve entered the callback URL. See Figure 6.

<figure>

![Setting the callback URL for the GitHub Application in the GitHub Application's settings screen.](https://developers.redhat.com/sites/default/files/set-callback-url.png)

<figcaption>

Figure 6: Setting the callback URL for the GitHub Application in the GitHub Application's settings screen.

</figcaption>

</figure>

Official documentation of this process is provided in the authentication section of the Red Hat Developer Hub documentation.

### Optional: Create a team in the GitHub organization

Groups are a critical part of assigning ownership to assets in the Backstage Software Catalog. The GitHub integration will automatically create groups that correspond to teams in your GitHub organization. If you’d like to see this in action, visit the **Teams** section of your GitHub organization and create a team, for example a team named “Platform Engineering” as shown in Figure 7.

<figure>

![Creating a Team within the GitHub Organization.](https://developers.redhat.com/sites/default/files/create-team-gh.png)

<figcaption>

Figure 7: Creating a Team within the GitHub organization.

</figcaption>

</figure>

Add members to the team after it has been created, as shown in Figure 8. This basic setup will be enough to see the integration work its magic.

<figure>

![A Team named Platform Engineering in a GitHub Organization. It has two members: Evan Shortiss and Ben Wilcock.](https://developers.redhat.com/sites/default/files/team-details-gh.png)

<figcaption>

Figure 8: A team named Platform Engineering with two members: Evan Shortiss and Ben Wilcock.

</figcaption>

</figure>

### Create a Secret for the GitHub app credentials

The GitHub application ID, client ID, client secret, and private key should be stored in a Secret. This Secret will be loaded into the running instance of Red Hat Developer Hub shortly.

Create the Secret named `github-secrets` using the **Secrets** section of the OpenShift Web Console, as shown in Figure 9. Add the following keys (listed below), and the corresponding values from the `github-app-rhdh-blog-credentials.yaml` file that was generated when you created the GitHub Application:

- `AUTH_GITHUB_APP_ID`: `appId`
- `AUTH_GITHUB_CLIENT_ID`: `clientId`
- `AUTH_GITHUB_CLIENT_SECRET`: `clientSecret`
- `GITHUB_PRIVATE_KEY`: `privateKey` (make sure to remove the indentation/tabs)
- `GITHUB_ORGANIZATION`: This is the URL-safe name of your organization you used with the `create-github-app` command

<figure>

![The OpenShift Web Console showing an administrator creating a secret named "github-secrets".](https://developers.redhat.com/sites/default/files/gh-secrets-ocp.png)

<figcaption>

Figure 9: Creating a secret named "github-secrets" in the OpenShift Web Console.

</figcaption>

</figure>

### Update the Red Hat Developer Hub configuration

In the **Helm** section from the Developer perspective in OpenShift, choose the **Upgrade** option for the **redhat-developer-hub** chart. See Figure 10.

<figure>

![An administrator using the OpenShift Console to upgrade the Red Hat Developer Hub Helm Chart with new parameters.](https://developers.redhat.com/sites/default/files/upgrade-helm-chart.png)

<figcaption>

Figure 10: Using the OpenShift Console to upgrade the Red Hat Developer Hub Helm Chart with new parameters.

</figcaption>

</figure>

**Note**: Modifying the YAML configuration can be tedious. Use this GitHub gist as a reference when crafting your own configuration to ensure it matches the YAML structures outlined below.

Use the **YAML** **View** to update the `global.dynamic` section with the required GitHub plug-ins. These plug-ins communicate with the GitHub API to enable authentication and synchronize data from your organization and repositories to the Red Hat Developer Hub Software Catalog:

```plaintext
dynamic:
  includes:
    - dynamic-plugins.default.yaml
  plugins:
    - disabled: false
      package: ./dynamic-plugins/dist/backstage-plugin-catalog-backend-module-github-dynamic
    - disabled: false
      package: ./dynamic-plugins/dist/backstage-plugin-catalog-backend-module-github-org-dynamic
```

Add a new `upstream.backstage.extraEnvVarsSecrets` block that references the Secret named `github-secrets` that you created earlier. This will load your GitHub credentials into the Red Hat Developer Hub container as environment variables:

```plaintext
extraEnvVarsSecrets:
  - github-secrets
```

Replace the `appConfig.auth` section and `signInPage` to match the following sample. This enables the GitHub authentication provider, configures it with environment variables loaded from your `github-secrets`, and adds a GitHub prompt to the Red Hat Developer Hub sign in page:

```plaintext
signInPage: github
auth:
  environment: production
  providers:
    github:
      production:
        clientId: '${AUTH_GITHUB_CLIENT_ID}'
        clientSecret: '${AUTH_GITHUB_CLIENT_SECRET}'
```

Add the GitHub integration to `appConfig.integrations`. The GitHub plug-ins you added leverage this configuration to communicate with the GitHub API:

```plaintext
integrations:
  github:
    - host: github.com
      apps:
        - appId: '${AUTH_GITHUB_APP_ID}'
          clientId: '${AUTH_GITHUB_CLIENT_ID}'
          clientSecret: '${AUTH_GITHUB_CLIENT_SECRET}'
          privateKey: |
            ${GITHUB_PRIVATE_KEY}
          webhookSecret: none
```

Finally, add two new `appConfig.catalog.providers` to populate the Software Catalog with entities based on your GitHub organization repositories (`providers.github`), and members and teams (`providers.githubOrg`): 

```plaintext
catalog:
  providers:
    github:
      providerId:
        catalogPath: /catalog-info.yaml
        organization: ${GITHUB_ORGANIZATION}
        schedule:
          frequency: { minutes: 5 }
          initialDelay: { seconds: 15 }
          timeout: { minutes: 3 }
    githubOrg:
      githubUrl: 'https://github.com'
      id: production
      orgs:
        - ${GITHUB_ORGANIZATION}
      schedule:
        frequency: { minutes: 5 }
        initialDelay: { seconds: 15 }
        timeout: { minutes: 3 }
```

These providers are a critical part of the configuration since they are responsible for synchronizing your organization’s teams into Red Hat Developer Hub as group entities, and synchronizing your organization’s members into Red Hat Developer Hub as user entities. Failing to do so will result in login failures.

Click the **Upgrade** button to redeploy Red Hat Developer Hub with your updated configuration, as seen in Figure 11. A new Red Hat Developer Hub Pod will be created and should be ready to receive HTTP traffic after a minute or two.

<figure>

![An administrator using the OpenShift Web Console to edit Helm Chart parameters using the YAML editor view.](https://developers.redhat.com/sites/default/files/upgrade-helm-chart-yaml.png)

<figcaption>

Figure 11: Editing the Helm Chart parameters using the YAML editor view, prior to the upgrade.

</figcaption>

</figure>

## Verify the GitHub integration

Once the new Pod has started, navigate to Red Hat Developer Hub Pod in a new browser window. You should be prompted to log in using GitHub as shown in Figure 12. Clicking the **Sign In** button will cause a pop-up window to appear. Enter your credentials and you'll be redirected to Red Hat Developer Hub if you successfully authenticated against GitHub.

<figure>

![A user logging into Red Hat Developer Hub using the GitHub provider.](https://developers.redhat.com/sites/default/files/rhdh-gh-login.png)

<figcaption>

Figure 12: Logging in to Red Hat Developer Hub using the GitHub provider.

</figcaption>

</figure>

Visit the **Settings** screen after logging in and you’ll see that your user's details have been imported from GitHub, including the Group you’re part of if you set up a team and added yourself to it in the GitHub organization (Figure 13).

<figure>

![The user details being shown on the Red Hat Developer Hub settings screen, including the GitHub Team that the user is a member of.](https://developers.redhat.com/sites/default/files/rhdh-user-details.png)

<figcaption>

Figure 13: User details being shown on the Red Hat Developer Hub settings screen, including the GitHub team(s) that the user has membership in.

</figcaption>

</figure>

Try creating a repository in your organization, adding a file named `catalog-info.yaml` that resembles some of the sample entities in the Backstage documentation, and waiting a few minutes–it should appear in the Software Catalog.

## Conclusion

Nice work! You’ve configured an authentication provider and integration to automatically create entities in the Software Catalog. What’s next? Start onboarding your developers and use Red Hat Developer Hub’s RBAC features to control who can perform specific actions in your internal developer portal.

The post Backstage authentication and catalog providers: A practical guide appeared first on Red Hat Developer.  
  

Go to Source
