---
title: "<div>How-to: GitLab Single Sign-on with SAML, SCIM, and Azure’s Entra ID</div>"
date: 2025-01-23
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

As organizations increase in size, it becomes increasingly difficult and critical to ensure that the right team members have access to the right groups and projects within their development platform. GitLab offers some powerful methods to manage user access, especially now with custom roles, but performing this at scale through a point-and-click user interface can be frustrating. However, all is not lost. You can use Security Assertion Markup Language (SAML) and System for Cross-domain Identity Management (SCIM) as a solution. (There are moments where I’m grateful for acronyms.)

I was researching this topic for a particular customer, and walking through the GitLab documentation on the capabilities, but I never felt like I truly understood the integration. As is often the case, especially when dealing with integrating components, the knowledge from experience far outweighs that gained from reading or watching. In that light, I wanted to share my steps along this path and invite you all to join me. All you need is a free trial of Microsoft Azure Entra ID and GitLab Premium with a top-level group on GitLab.com.

**Note:** This exercise produces a working integration, however, for production environments there may be necessary deviations. For example, the user account email for the identity provider (Entra ID in this case) will likely not match your GitLab account email.

## Creating the application in Entra ID

First, go to the Entra ID admin center. Within the **Applications** area, select **Enterprise Applications**. We’re going to create a new application, and then create our own application.

![Entra ID application creation flow](https://images.ctfassets.net/r9o86ar0p03f/6ECqgWzOweNnRks9LXGG01/936fb84e86c6e2430551bbd058b00369/image13.png)

<center><i>Figure 1: Entra ID application creation flow</i></center><br>

With our new application created, we can start configuring the single sign-on (SSO) parameters for our application. For this task, you may want to have side-by-side browser windows. One window on your Entra ID application, and another window on the SAML settings for your GitLab group. Those settings are located under **Settings**, then SAML SSO on the left side of your GitLab window, as shown in Figure 2. If you don’t see this option, you aren’t in the top-level group, don’t have permission to configure SAML, or don’t have GitLab Premium enabled for that group.

![GitLab SAML configuration](https://images.ctfassets.net/r9o86ar0p03f/0aOqVkd2HthLzXflRrXM9/4933debfab716bdce26cabe7b461cf5d/image7.png)

<center><i>Figure 2: GitLab SAML configuration</i></center><br>

Within your Entra ID interface, select **Single sign-on** and click the SAML card.

![Entra ID SAML configuration](https://images.ctfassets.net/r9o86ar0p03f/4vHX8CqKeLoEdgSykxpC5C/384540d6032dd716b6380e624c4807d5/image24.png)

<center><i>Figure 3: Entra ID SAML configuration</i></center><br>

With the side-by-side view, the SAML configuration settings are on the left and the GitLab SSO settings on the right.

![Side-by-side view of Entra ID and GitLab](https://images.ctfassets.net/r9o86ar0p03f/2fdWvcyK3f9LpwqhUFsxpc/7c77eed1ff26c9d507f7f703e22c9d33/image16.png)

<center><i>Figure 4: Side-by-side view of Entra ID and GitLab</i></center><br>

Now we can start copying and pasting parameters. Within the Entra ID interface, select **Edit** within the “Basic SAML Configuration” block. The parameter sources and destination are identified in the following table.

| Source (GitLab) | Destination (Entra ID) |
| --- | --- |
| Identifier | Identifier (Entity ID) |
| Assertion consumer service URL | Reply URL (Assertion Consumer Service URL) |
| GitLab single sign-on URL | Sign on URL (Optional) |

<br> Once completed, your side-by-side view should appear similar to the following (noting the URLs are unique to your environment).<br>

![Completed basic SAML SSO configuration](https://images.ctfassets.net/r9o86ar0p03f/64oJYy65an3BM2ia8rS2EG/74ac354002931bba98a56c0668e11bc9/image9.png)

<center><i>Figure 5: Completed basic SAML SSO configuration</i></center><br>

Click **Save** within the Entra ID “Basic SML Configuration” window to save your hard work thus far. Note: You may need to click on the “X” in the upper right of the “Basic SAML Configuration” window if it doesn’t close automatically.

After this window closes, you may get a popup to test single sign-on with your application. Select **No, I’ll test later**, because we still have more work to do (there is always more work to do).

## Configuring attributes and claims

Within the Entra ID user interface, look for the section for “Attributes and Claims,” and click the **Edit** pencil icon. The first thing we want to do is modify the Unique User identifier (Name ID) value, so click on that row and set the Source attribute to **user.objectid**. Additionally, the Name identifier format must be updated, and set to **Persistent**.

![Configuring attributes and claims](https://images.ctfassets.net/r9o86ar0p03f/1zbCkXhdbeGsxvIBvOpIvU/98312a7e3f92a765f44d7743bd5b02df/image14.png)

<center><i>Figure 6: Configuring attributes and claims</i></center><br>

Save that claim configuration. Now we have additional claims to configure, but there are only three that we need here. So, feel free to go wild and delete those default four items under **Additional claims**, or you can edit the existing ones to match the table below. Note that these values (specifically, the Name) are case sensitive. <br>

| Name | Namespace | Source Attribute |
| --- | --- | --- |
| emailaddress | http://schemas.microsoft.com/ws/2008/06/identity/claims | user.otheremail |
| NameID | http://schemas.microsoft.com/ws/2008/06/identity/claims | user.objectid |

<br>

The resulting claims configuration should appear as follows. Note the use of **otheremail** for the “emailaddress” attribute. This was necessary for me as my primary email addresses within Entra ID are not the addresses used on GitLab.com. If you recall, when I set up my “user," I modified the contact information to include my gitlab.com email address as one of my “Other emails.”

![Configuring the claims](https://images.ctfassets.net/r9o86ar0p03f/1JcDjXQJ3CLRYZHNAmp17Q/ee7d433eb6b96d377d37bbf401f985fb/image21.png)

<center><i>Figure 7: Configuring the claims</i></center><br>

With your attributes configured, under the Advance settings, enable **Include attribute name format** setting.

![Advanced claims configuration](https://images.ctfassets.net/r9o86ar0p03f/4IqIBDychLovg2FPH4l94P/94e4b49de2d4644b8e820fe830ec0a37/image8.png)

<center><i>Figure 8: Advanced claims configuration</i></center><br>

Your "Attributes and Claims" window should now look similar to Figure 9 below.

![Configured attributes and claims](https://images.ctfassets.net/r9o86ar0p03f/6KtyKI6nqRXytPvBt7rPYo/e4642a9e912968794548cef4cb1b99d1/image18.png)

<center><i>Figure 9: Configured attributes and claims</i></center><br>

If you’re happy, or at least relatively content, with your configuration, click the “X” in the top right corner of the "Attributes and Claims" window to close it.

## Configuring and assigning users

Now that we have our application configured, we need to ensure that our users have been assigned to that application. I'll assume you’re working with a test instance that does not have the same email address as what is configured within your GitLab.com namespace.

So let’s go to the “Users and groups” within the Entra ID user interface for your configured application.

![Managing application users and groups](https://images.ctfassets.net/r9o86ar0p03f/5rzhVXJhcqcXaugKmC4zvx/833051a022ff4e2227bf56ac48ae8601/image17.png)

<center><i>Figure 10: Managing application users and groups</i></center><br>

Select **Add user/group**, and under the “Users and groups” where it says “None Selected,” click that text. Now you can select the user(s) to add to your application. These are the users that will be permitted to log into GitLab, authenticating themselves through Entra ID.

![User selection](https://images.ctfassets.net/r9o86ar0p03f/6OOlsau20F6cbDIgbfle7G/e34a5027a9529e2e53451b991b9711ea/image23.png)

<center><i>Figure 11: User selection</i></center><br>

Once selected, at the bottom of that page, click **Select**, and at the bottom of the next, select **Assign**. Now you should have a user assigned to your application.

![User assigned to application](https://images.ctfassets.net/r9o86ar0p03f/34yKnMDjV9jMa5zpOotz7S/9c2e73a45f05de47fbf92f07e788fc33/image12.png)

<center><i>Figure 12: User assigned to application</i></center><br>

Next, we need to ensure that the GitLab.com email address for that user is configured correctly. By clicking on the user itself, we can modify or configure some additional information about that user. We can see below, the User principal name, which is based on an “onmicrosoft” domain. This is not the email address I have associated with my GitLab.com account. If you recall that we set the “Email address” attribute to “otheremail,” this is where we now configure that “other” email address.

![User properties](https://images.ctfassets.net/r9o86ar0p03f/47bRS61CjkVDlghomlStDf/208c27b179494638d1ac6f069cb6daf6/image20.png)

<center><i>Figure 13: User properties</i></center><br>

Click the option to **Edit properties** for the user, and click on the **Contact Information** heading. Here we can add other emails – more specifically, the email address utilized for your GitLab.com account.

![Configuration of alternate email address](https://images.ctfassets.net/r9o86ar0p03f/4htJTJkjY20kau7dxsJEig/6bcdada5381fc8a324260cc4cf9bcf5a/image15.png)

<center><i>Figure 14: Configuration of alternate email address</i></center><br>

That should complete the configuration parameters that we need in Entra ID, but wait, there’s more.

Within the GitLab side now, you will need to configure a couple parameters. First, you might as well enable SAML for the group as that’s kind of a key piece here. GitLab offers some additional options to disable password authentication or enforce SSO to reduce the security risks within your application, but we’ll leave those unchecked for now. Similar to the table above, we’ll need a couple things from Entra ID to configure into GitLab. Please refer to the table below. <br>

| Source (Entra ID) | Destination (GitLab) |
| --- | --- |
| Login URL | Identity provider single sign-on URL |
| Thumbprint | Certificate fingerprint |

<br>

![GitLab SAML configuration from Entra ID](https://images.ctfassets.net/r9o86ar0p03f/2efiSMW9nUmdGOcjIWpjjo/bfa9154201303dd7838c840db63b1629/image25.png)

<center><i>Figure 15: GitLab SAML configuration from Entra ID</i></center><br>

Lastly, you want to configure the default membership role for users logging in via SAML. Note that the access that you set for users here will cascade down to other groups and projects within your top-level group. Therefore, I would strongly recommend NOT setting this role to be “Owner.” Either “Guest” or “Minimal Access” would be acceptable options here, depending on the security posture of your organization. For more information about what these roles can and can not do, refer to the GitLab documentation on Roles and Permissions. Now, save your work on the GitLab interface by clicking that beautiful blue **Save changes** button.

With your GitLab settings saved, you can now test your setup. I would encourage you to do this both through the “Verify SAML Configuration” on the GitLab system as well as with the Entra ID SSO "Test" button.

## Troubleshooting SAML

In addition to the troubleshooting steps included within GitLab documentation, I wanted to include a couple other items that I personally experienced.

If you get an error stating that the SAML reference did not contain an email address, check the Claim name for your email within the “Attributes and Claims” section within your Entra ID application. With GitLab 16.7, we added support for the “2008” attribute names, and at least for the email address setting, I found the default “xmlsoap” name for the email address claim to be a disappointing failure.

Another common error is “SAML Name ID and email address do not match your user account.” As you may suspect, this error is caused by a mismatch of the “NameID” and “emailaddress” attributes within the Entra ID application. This could be a misconfiguration of the “Attributes and Claims,” but it could also be that the properties of your test user don’t match your configuration. One helpful method to identify exactly what is coming through the SAML exchange is to use a SAML Tracer or SAML Message Decoder plugin with your web browser.

## SCIM

Now that you have SAML configured to enable users to log in via your Entra ID application, let’s make sure that people are assigned to the proper group(s) upon login. This can be incredibly helpful at scale, where instead of manually identifying which groups the particular users belong to, GitLab can learn this information from your identity application, Entra ID in this case.

Because SCIM utilizes groups to identify group membership, we need to create a group within Entra ID and add the relevant user(s) to the group. For this we’ll need the main administration menu for Entra ID.

![Entra ID Group configuration](https://images.ctfassets.net/r9o86ar0p03f/7E8Wa5MDnHK8TvM3YuPX9J/8973800b4be1eab4325d53dfc7e011c9/image19.png)

<center><i>Figure 16: Entra ID Group configuration</i></center><br>

We’re going to create a new group and assign our user(s) to that group. So click **New group** and configure a new group, which only requires you to configure a “Group name.” I used the default group type of “Security.” Leave the “Membership type” as “Assigned.” From this window, we can also assign the members.

![Creating a New Entra ID Group](https://images.ctfassets.net/r9o86ar0p03f/36ghuZp6lfOIBljKGcsiRZ/129c2bffded8c9e698efdbee8b2ce948/image2.png)

<center><i>Figure 17: Creating a New Entra ID Group</i></center><br>

Once you’ve added the member(s), click **Create** in the bottom of that window. With your group created, and the user(s) assigned to the group, we can configure SCIM.

Immediately below the SAML configuration section within the GitLab UI, you’ll see the “SCIM Token” area. Here you can generate a new token, and copy the endpoint URL, both of which will be useful for the next steps. Note that if you forget or already have a SCIM token, it can be reset.

![SCIM token and endpoint within GitLab](https://images.ctfassets.net/r9o86ar0p03f/1lCOTuUlQsB3CuzTjNfgne/c894c56dba9575be087e0933ef549fe8/image10.png)

<center><i>Figure 18: SCIM token and endpoint within GitLab</i></center><br>

With this information saved, return to your Entra ID application configuration. Within the left side menu, you’ll find the following:

![Provisioning SCIM within Entra ID](https://images.ctfassets.net/r9o86ar0p03f/1cjyv1DI0X5imVH46YgJx1/ef7f7e1fc8b5bdfea1710fa9558365fa/image3.png)

<center><i>Figure 19: Provisioning SCIM within Entra ID</i></center><br>

Within the "Provisioning" section, click on **New Configuration**, which opens a new page where that token and URL from GitLab will be used.

![New provisioning configuration](https://images.ctfassets.net/r9o86ar0p03f/7fm2HcFxb23Ghn5tJAvEj0/e48332ff15fc895190c7e0202afa500e/image22.png)

<center><i>Figure 20: New provisioning configuration</i></center><br>

Feel free to test the connection to ensure that you’ve configured the parameters properly. After testing, click on the **Create** button to establish the configuration and work on our mappings and settings. You may need to click the “X” in the top right corner of the panel to return to the overview configuration.

Expand the “Mappings,” which includes two parameters; “Provision Microsoft Entra ID Groups” and “Provision Microsoft Entra ID Users.” SCIM group provisioning isn’t currently supported in GitLab, and although it doesn’t break the integration, keeping group provisioning enabled may cause negligible error messages. Therefore, we want to disable “Provision Microsoft Entra ID Groups,” so click that entry and set the “Enabled” field to “No.”

![Provisioning attribute mapping](https://images.ctfassets.net/r9o86ar0p03f/1Fy60PYj4ghKhWLIotItfo/34fa4bdf40fcdd5ca1176508a44e7c22/image4.png)

<center><i>Figure 21: Provisioning attribute mapping</i></center><br>

Save that configuration and select “Provision Microsoft Entra ID Users.” Validate that all three "Target Object Actions" are enabled, and then proceed to the “Attribute Mapping” section. Delete all existing mappings available to delete (I find this easier because attributes can’t be assigned twice), and then configure the Attribute Mappings per the following table:

| customappsso Attribute (Destination) | Microsoft Entra ID Attribute (Source) | Matching Precedence | Mapping Type |
| --- | --- | --- | --- |
| externalID | objectId | 1 | Direct |
| active | Switch(\[IsSoftDeleted\], , "False", "True", "True", "False") |  | Expression |
| userName | mailNickname |  | Direct |
| name.formatted | displayName |  | Direct |
| Emails\[type eq “other”\].value | userPrincipalName |  | Direct |

<br>

![Editing attributes](https://images.ctfassets.net/r9o86ar0p03f/6dQwfg6tJ4DOH1MFCyvLvn/3c0ad4618054411364bb3291100e32ac/image26.png)

<center><i>Figure 22: Editing attributes</i></center><br>

After configuring all of the attribute mappings, the result should be similar to that found in Figure 22.

![Completed attribute mapping configuration](https://images.ctfassets.net/r9o86ar0p03f/2l8Dq5suFv8pv6s4xQcwsl/fa44f551b7133ddf8702bb00d631ca84/image5.png)

<center><i>Figure 23: Completed attribute mapping configuration</i></center><br>

Note the use of the “other” email within the **customappssso** attribute. This relates back to the “other” email we configured for the user back in the Entra ID user properties. In a production situation, the emails for the SSO account and the email address for the account within GitLab should match.

With your mapping complete (congratulations, Ptolemy), there are some advanced configuration settings necessary. Underneath the "Attribute Mappings," click the box for “Show advanced options.” Once this box is checked, a link called “Edit attribute list for customappsso” is revealed.

![Advanced attribute configuration](https://images.ctfassets.net/r9o86ar0p03f/65om0difHvM06VWbaFYL6g/7fc436635e776f63dc71b2027e351dc8/image6.png)

<center><i>Figure 24: Advanced attribute configuration</i></center><br>

Click that link, and ensure that the Name “ID” is both “Primary Key” and “Required,” and that “externalID” is also “Required.” These attributes both refer to a unique user ID generated by Entra ID. However, although the “id” itself is required, it is not consistently provided within the API calls. Therefore, GitLab relies on the “externalID” to ensure the proper connection between the Entra ID and GitLab user accounts.

![Required attribute list](https://images.ctfassets.net/r9o86ar0p03f/1ujUonTIvsNUgo1glDg8Sf/baf4d0ebd4ec1dc912e286961d7c0846/image1.png)

<center><i>Figure 25: Required attribute list</i></center><br>

Save these settings, and then close the “Attribute Mapping” page with the “X” in the top right of the window. Return to the "Application Provisioning" section and click **Start provisioning**.

Within GitLab, we need to configure the association between the group we configured within Entra ID and the level of access we want those users to have within the GitLab top-level group. Note that this association can be configured on each sub-group within GitLab for more extensive provisioning, but within GitLab, permissions flow downhill. Whatever permission you set for a user at a top-level group, or sub-group, will cascade down to all projects and groups contained therein.

Within the "Settings" portion of the GitLab menu, select **SAML Group Links**. Here is where you’ll configure the group name and determine what access level, or role, members of the Entra ID Group will have within this particular GitLab Group.

![GitLab SAML Group link](https://images.ctfassets.net/r9o86ar0p03f/6izdNhzMSssEBSGxqDwGEN/326c6d09a2b572f438375464ae903189/image27.png)

<center><i>Figure 26: GitLab SAML Group link</i></center><br>

As shown in Figure 26, I’ve configured my membership to The Academy such that any users within the dev-security group from Entra ID will be granted Developer access. Note that this is a slight variation of what a typical production environment would look like. In most instances, the user account within the identity provider (Entra ID, in this case) would match the user’s corporate account email (and we wouldn’t require “other” emails). When configured properly, if the user does not already have an account on GitLab, one will be created for them tied to their SSO account.

![GitLab SSO tutorial - image11](https://images.ctfassets.net/r9o86ar0p03f/58htcOWFV7kPf18UUhUe2Z/f0d58dd9e23b96dce43e56e14d787c21/image11.png)

<center><i>Figure 27: SAML Group Links configured</i></center><br>

Now that you’ve completed the configuration, give it a try! From another browser, preferably in private mode to ignore any cookies or other yummy artifacts, paste the link for the GitLab SSO URL found in the GitLab SAML configurations. You should be prompted to log in with your Entra ID credentials and gain the proper access to your GitLab group!

Congratulations, you’ve made it! I hope you’ve learned from and appreciate the work here, and we can all rejoice in the fact that the users within the Play-Dough app can now all properly authenticate, with the right permissions, to The Academy!

> Don't have a GitLab account? Sign up for a free, 60-day trial today.

## Read more

- The ultimate guide to enabling SAML and SSO on GitLab.com
- SAML SSO for GitLab.com groups documentation

As organizations increase in size, it becomes increasingly difficult and critical to ensure that the right team members have access to the right groups and projects within their development platform. GitLab offers some powerful methods to manage user access, especially now with custom roles, but performing this at scale through a point-and-click user interface can be frustrating. However, all is not lost. You can use Security Assertion Markup Language (SAML) and System for Cross-domain Identity Management (SCIM) as a solution. (There are moments where I’m grateful for acronyms.)

I was researching this topic for a particular customer, and walking through the GitLab documentation on the capabilities, but I never felt like I truly understood the integration. As is often the case, especially when dealing with integrating components, the knowledge from experience far outweighs that gained from reading or watching. In that light, I wanted to share my steps along this path and invite you all to join me. All you need is a free trial of Microsoft Azure Entra ID and GitLab Premium with a top-level group on GitLab.com.

**Note:** This exercise produces a working integration, however, for production environments there may be necessary deviations. For example, the user account email for the identity provider (Entra ID in this case) will likely not match your GitLab account email.

## Creating the application in Entra ID

First, go to the Entra ID admin center. Within the **Applications** area, select **Enterprise Applications**. We’re going to create a new application, and then create our own application.

![Entra ID application creation flow](https://images.ctfassets.net/r9o86ar0p03f/6ECqgWzOweNnRks9LXGG01/936fb84e86c6e2430551bbd058b00369/image13.png)

<center><i>Figure 1: Entra ID application creation flow</i></center><br>

With our new application created, we can start configuring the single sign-on (SSO) parameters for our application. For this task, you may want to have side-by-side browser windows. One window on your Entra ID application, and another window on the SAML settings for your GitLab group. Those settings are located under **Settings**, then SAML SSO on the left side of your GitLab window, as shown in Figure 2. If you don’t see this option, you aren’t in the top-level group, don’t have permission to configure SAML, or don’t have GitLab Premium enabled for that group.

![GitLab SAML configuration](https://images.ctfassets.net/r9o86ar0p03f/0aOqVkd2HthLzXflRrXM9/4933debfab716bdce26cabe7b461cf5d/image7.png)

<center><i>Figure 2: GitLab SAML configuration</i></center><br>

Within your Entra ID interface, select **Single sign-on** and click the SAML card.

![Entra ID SAML configuration](https://images.ctfassets.net/r9o86ar0p03f/4vHX8CqKeLoEdgSykxpC5C/384540d6032dd716b6380e624c4807d5/image24.png)

<center><i>Figure 3: Entra ID SAML configuration</i></center><br>

With the side-by-side view, the SAML configuration settings are on the left and the GitLab SSO settings on the right.

![Side-by-side view of Entra ID and GitLab](https://images.ctfassets.net/r9o86ar0p03f/2fdWvcyK3f9LpwqhUFsxpc/7c77eed1ff26c9d507f7f703e22c9d33/image16.png)

<center><i>Figure 4: Side-by-side view of Entra ID and GitLab</i></center><br>

Now we can start copying and pasting parameters. Within the Entra ID interface, select **Edit** within the “Basic SAML Configuration” block. The parameter sources and destination are identified in the following table.

| Source (GitLab) | Destination (Entra ID) |
| --- | --- |
| Identifier | Identifier (Entity ID) |
| Assertion consumer service URL | Reply URL (Assertion Consumer Service URL) |
| GitLab single sign-on URL | Sign on URL (Optional) |

<br> Once completed, your side-by-side view should appear similar to the following (noting the URLs are unique to your environment).<br>

![Completed basic SAML SSO configuration](https://images.ctfassets.net/r9o86ar0p03f/64oJYy65an3BM2ia8rS2EG/74ac354002931bba98a56c0668e11bc9/image9.png)

<center><i>Figure 5: Completed basic SAML SSO configuration</i></center><br>

Click **Save** within the Entra ID “Basic SML Configuration” window to save your hard work thus far. Note: You may need to click on the “X” in the upper right of the “Basic SAML Configuration” window if it doesn’t close automatically.

After this window closes, you may get a popup to test single sign-on with your application. Select **No, I’ll test later**, because we still have more work to do (there is always more work to do).

## Configuring attributes and claims

Within the Entra ID user interface, look for the section for “Attributes and Claims,” and click the **Edit** pencil icon. The first thing we want to do is modify the Unique User identifier (Name ID) value, so click on that row and set the Source attribute to **user.objectid**. Additionally, the Name identifier format must be updated, and set to **Persistent**.

![Configuring attributes and claims](https://images.ctfassets.net/r9o86ar0p03f/1zbCkXhdbeGsxvIBvOpIvU/98312a7e3f92a765f44d7743bd5b02df/image14.png)

<center><i>Figure 6: Configuring attributes and claims</i></center><br>

Save that claim configuration. Now we have additional claims to configure, but there are only three that we need here. So, feel free to go wild and delete those default four items under **Additional claims**, or you can edit the existing ones to match the table below. Note that these values (specifically, the Name) are case sensitive. <br>

| Name | Namespace | Source Attribute |
| --- | --- | --- |
| emailaddress | http://schemas.microsoft.com/ws/2008/06/identity/claims | user.otheremail |
| NameID | http://schemas.microsoft.com/ws/2008/06/identity/claims | user.objectid |

<br>

The resulting claims configuration should appear as follows. Note the use of **otheremail** for the “emailaddress” attribute. This was necessary for me as my primary email addresses within Entra ID are not the addresses used on GitLab.com. If you recall, when I set up my “user," I modified the contact information to include my gitlab.com email address as one of my “Other emails.”

![Configuring the claims](https://images.ctfassets.net/r9o86ar0p03f/1JcDjXQJ3CLRYZHNAmp17Q/ee7d433eb6b96d377d37bbf401f985fb/image21.png)

<center><i>Figure 7: Configuring the claims</i></center><br>

With your attributes configured, under the Advance settings, enable **Include attribute name format** setting.

![Advanced claims configuration](https://images.ctfassets.net/r9o86ar0p03f/4IqIBDychLovg2FPH4l94P/94e4b49de2d4644b8e820fe830ec0a37/image8.png)

<center><i>Figure 8: Advanced claims configuration</i></center><br>

Your "Attributes and Claims" window should now look similar to Figure 9 below.

![Configured attributes and claims](https://images.ctfassets.net/r9o86ar0p03f/6KtyKI6nqRXytPvBt7rPYo/e4642a9e912968794548cef4cb1b99d1/image18.png)

<center><i>Figure 9: Configured attributes and claims</i></center><br>

If you’re happy, or at least relatively content, with your configuration, click the “X” in the top right corner of the "Attributes and Claims" window to close it.

## Configuring and assigning users

Now that we have our application configured, we need to ensure that our users have been assigned to that application. I'll assume you’re working with a test instance that does not have the same email address as what is configured within your GitLab.com namespace.

So let’s go to the “Users and groups” within the Entra ID user interface for your configured application.

![Managing application users and groups](https://images.ctfassets.net/r9o86ar0p03f/5rzhVXJhcqcXaugKmC4zvx/833051a022ff4e2227bf56ac48ae8601/image17.png)

<center><i>Figure 10: Managing application users and groups</i></center><br>

Select **Add user/group**, and under the “Users and groups” where it says “None Selected,” click that text. Now you can select the user(s) to add to your application. These are the users that will be permitted to log into GitLab, authenticating themselves through Entra ID.

![User selection](https://images.ctfassets.net/r9o86ar0p03f/6OOlsau20F6cbDIgbfle7G/e34a5027a9529e2e53451b991b9711ea/image23.png)

<center><i>Figure 11: User selection</i></center><br>

Once selected, at the bottom of that page, click **Select**, and at the bottom of the next, select **Assign**. Now you should have a user assigned to your application.

![User assigned to application](https://images.ctfassets.net/r9o86ar0p03f/34yKnMDjV9jMa5zpOotz7S/9c2e73a45f05de47fbf92f07e788fc33/image12.png)

<center><i>Figure 12: User assigned to application</i></center><br>

Next, we need to ensure that the GitLab.com email address for that user is configured correctly. By clicking on the user itself, we can modify or configure some additional information about that user. We can see below, the User principal name, which is based on an “onmicrosoft” domain. This is not the email address I have associated with my GitLab.com account. If you recall that we set the “Email address” attribute to “otheremail,” this is where we now configure that “other” email address.

![User properties](https://images.ctfassets.net/r9o86ar0p03f/47bRS61CjkVDlghomlStDf/208c27b179494638d1ac6f069cb6daf6/image20.png)

<center><i>Figure 13: User properties</i></center><br>

Click the option to **Edit properties** for the user, and click on the **Contact Information** heading. Here we can add other emails – more specifically, the email address utilized for your GitLab.com account.

![Configuration of alternate email address](https://images.ctfassets.net/r9o86ar0p03f/4htJTJkjY20kau7dxsJEig/6bcdada5381fc8a324260cc4cf9bcf5a/image15.png)

<center><i>Figure 14: Configuration of alternate email address</i></center><br>

That should complete the configuration parameters that we need in Entra ID, but wait, there’s more.

Within the GitLab side now, you will need to configure a couple parameters. First, you might as well enable SAML for the group as that’s kind of a key piece here. GitLab offers some additional options to disable password authentication or enforce SSO to reduce the security risks within your application, but we’ll leave those unchecked for now. Similar to the table above, we’ll need a couple things from Entra ID to configure into GitLab. Please refer to the table below. <br>

| Source (Entra ID) | Destination (GitLab) |
| --- | --- |
| Login URL | Identity provider single sign-on URL |
| Thumbprint | Certificate fingerprint |

<br>

![GitLab SAML configuration from Entra ID](https://images.ctfassets.net/r9o86ar0p03f/2efiSMW9nUmdGOcjIWpjjo/bfa9154201303dd7838c840db63b1629/image25.png)

<center><i>Figure 15: GitLab SAML configuration from Entra ID</i></center><br>

Lastly, you want to configure the default membership role for users logging in via SAML. Note that the access that you set for users here will cascade down to other groups and projects within your top-level group. Therefore, I would strongly recommend NOT setting this role to be “Owner.” Either “Guest” or “Minimal Access” would be acceptable options here, depending on the security posture of your organization. For more information about what these roles can and can not do, refer to the GitLab documentation on Roles and Permissions. Now, save your work on the GitLab interface by clicking that beautiful blue **Save changes** button.

With your GitLab settings saved, you can now test your setup. I would encourage you to do this both through the “Verify SAML Configuration” on the GitLab system as well as with the Entra ID SSO "Test" button.

## Troubleshooting SAML

In addition to the troubleshooting steps included within GitLab documentation, I wanted to include a couple other items that I personally experienced.

If you get an error stating that the SAML reference did not contain an email address, check the Claim name for your email within the “Attributes and Claims” section within your Entra ID application. With GitLab 16.7, we added support for the “2008” attribute names, and at least for the email address setting, I found the default “xmlsoap” name for the email address claim to be a disappointing failure.

Another common error is “SAML Name ID and email address do not match your user account.” As you may suspect, this error is caused by a mismatch of the “NameID” and “emailaddress” attributes within the Entra ID application. This could be a misconfiguration of the “Attributes and Claims,” but it could also be that the properties of your test user don’t match your configuration. One helpful method to identify exactly what is coming through the SAML exchange is to use a SAML Tracer or SAML Message Decoder plugin with your web browser.

## SCIM

Now that you have SAML configured to enable users to log in via your Entra ID application, let’s make sure that people are assigned to the proper group(s) upon login. This can be incredibly helpful at scale, where instead of manually identifying which groups the particular users belong to, GitLab can learn this information from your identity application, Entra ID in this case.

Because SCIM utilizes groups to identify group membership, we need to create a group within Entra ID and add the relevant user(s) to the group. For this we’ll need the main administration menu for Entra ID.

![Entra ID Group configuration](https://images.ctfassets.net/r9o86ar0p03f/7E8Wa5MDnHK8TvM3YuPX9J/8973800b4be1eab4325d53dfc7e011c9/image19.png)

<center><i>Figure 16: Entra ID Group configuration</i></center><br>

We’re going to create a new group and assign our user(s) to that group. So click **New group** and configure a new group, which only requires you to configure a “Group name.” I used the default group type of “Security.” Leave the “Membership type” as “Assigned.” From this window, we can also assign the members.

![Creating a New Entra ID Group](https://images.ctfassets.net/r9o86ar0p03f/36ghuZp6lfOIBljKGcsiRZ/129c2bffded8c9e698efdbee8b2ce948/image2.png)

<center><i>Figure 17: Creating a New Entra ID Group</i></center><br>

Once you’ve added the member(s), click **Create** in the bottom of that window. With your group created, and the user(s) assigned to the group, we can configure SCIM.

Immediately below the SAML configuration section within the GitLab UI, you’ll see the “SCIM Token” area. Here you can generate a new token, and copy the endpoint URL, both of which will be useful for the next steps. Note that if you forget or already have a SCIM token, it can be reset.

![SCIM token and endpoint within GitLab](https://images.ctfassets.net/r9o86ar0p03f/1lCOTuUlQsB3CuzTjNfgne/c894c56dba9575be087e0933ef549fe8/image10.png)

<center><i>Figure 18: SCIM token and endpoint within GitLab</i></center><br>

With this information saved, return to your Entra ID application configuration. Within the left side menu, you’ll find the following:

![Provisioning SCIM within Entra ID](https://images.ctfassets.net/r9o86ar0p03f/1cjyv1DI0X5imVH46YgJx1/ef7f7e1fc8b5bdfea1710fa9558365fa/image3.png)

<center><i>Figure 19: Provisioning SCIM within Entra ID</i></center><br>

Within the "Provisioning" section, click on **New Configuration**, which opens a new page where that token and URL from GitLab will be used.

![New provisioning configuration](https://images.ctfassets.net/r9o86ar0p03f/7fm2HcFxb23Ghn5tJAvEj0/e48332ff15fc895190c7e0202afa500e/image22.png)

<center><i>Figure 20: New provisioning configuration</i></center><br>

Feel free to test the connection to ensure that you’ve configured the parameters properly. After testing, click on the **Create** button to establish the configuration and work on our mappings and settings. You may need to click the “X” in the top right corner of the panel to return to the overview configuration.

Expand the “Mappings,” which includes two parameters; “Provision Microsoft Entra ID Groups” and “Provision Microsoft Entra ID Users.” SCIM group provisioning isn’t currently supported in GitLab, and although it doesn’t break the integration, keeping group provisioning enabled may cause negligible error messages. Therefore, we want to disable “Provision Microsoft Entra ID Groups,” so click that entry and set the “Enabled” field to “No.”

![Provisioning attribute mapping](https://images.ctfassets.net/r9o86ar0p03f/1Fy60PYj4ghKhWLIotItfo/34fa4bdf40fcdd5ca1176508a44e7c22/image4.png)

<center><i>Figure 21: Provisioning attribute mapping</i></center><br>

Save that configuration and select “Provision Microsoft Entra ID Users.” Validate that all three "Target Object Actions" are enabled, and then proceed to the “Attribute Mapping” section. Delete all existing mappings available to delete (I find this easier because attributes can’t be assigned twice), and then configure the Attribute Mappings per the following table:

| customappsso Attribute (Destination) | Microsoft Entra ID Attribute (Source) | Matching Precedence | Mapping Type |
| --- | --- | --- | --- |
| externalID | objectId | 1 | Direct |
| active | Switch(\[IsSoftDeleted\], , "False", "True", "True", "False") |  | Expression |
| userName | mailNickname |  | Direct |
| name.formatted | displayName |  | Direct |
| Emails\[type eq “other”\].value | userPrincipalName |  | Direct |

<br>

![Editing attributes](https://images.ctfassets.net/r9o86ar0p03f/6dQwfg6tJ4DOH1MFCyvLvn/3c0ad4618054411364bb3291100e32ac/image26.png)

<center><i>Figure 22: Editing attributes</i></center><br>

After configuring all of the attribute mappings, the result should be similar to that found in Figure 22.

![Completed attribute mapping configuration](https://images.ctfassets.net/r9o86ar0p03f/2l8Dq5suFv8pv6s4xQcwsl/fa44f551b7133ddf8702bb00d631ca84/image5.png)

<center><i>Figure 23: Completed attribute mapping configuration</i></center><br>

Note the use of the “other” email within the **customappssso** attribute. This relates back to the “other” email we configured for the user back in the Entra ID user properties. In a production situation, the emails for the SSO account and the email address for the account within GitLab should match.

With your mapping complete (congratulations, Ptolemy), there are some advanced configuration settings necessary. Underneath the "Attribute Mappings," click the box for “Show advanced options.” Once this box is checked, a link called “Edit attribute list for customappsso” is revealed.

![Advanced attribute configuration](https://images.ctfassets.net/r9o86ar0p03f/65om0difHvM06VWbaFYL6g/7fc436635e776f63dc71b2027e351dc8/image6.png)

<center><i>Figure 24: Advanced attribute configuration</i></center><br>

Click that link, and ensure that the Name “ID” is both “Primary Key” and “Required,” and that “externalID” is also “Required.” These attributes both refer to a unique user ID generated by Entra ID. However, although the “id” itself is required, it is not consistently provided within the API calls. Therefore, GitLab relies on the “externalID” to ensure the proper connection between the Entra ID and GitLab user accounts.

![Required attribute list](https://images.ctfassets.net/r9o86ar0p03f/1ujUonTIvsNUgo1glDg8Sf/baf4d0ebd4ec1dc912e286961d7c0846/image1.png)

<center><i>Figure 25: Required attribute list</i></center><br>

Save these settings, and then close the “Attribute Mapping” page with the “X” in the top right of the window. Return to the "Application Provisioning" section and click **Start provisioning**.

Within GitLab, we need to configure the association between the group we configured within Entra ID and the level of access we want those users to have within the GitLab top-level group. Note that this association can be configured on each sub-group within GitLab for more extensive provisioning, but within GitLab, permissions flow downhill. Whatever permission you set for a user at a top-level group, or sub-group, will cascade down to all projects and groups contained therein.

Within the "Settings" portion of the GitLab menu, select **SAML Group Links**. Here is where you’ll configure the group name and determine what access level, or role, members of the Entra ID Group will have within this particular GitLab Group.

![GitLab SAML Group link](https://images.ctfassets.net/r9o86ar0p03f/6izdNhzMSssEBSGxqDwGEN/326c6d09a2b572f438375464ae903189/image27.png)

<center><i>Figure 26: GitLab SAML Group link</i></center><br>

As shown in Figure 26, I’ve configured my membership to The Academy such that any users within the dev-security group from Entra ID will be granted Developer access. Note that this is a slight variation of what a typical production environment would look like. In most instances, the user account within the identity provider (Entra ID, in this case) would match the user’s corporate account email (and we wouldn’t require “other” emails). When configured properly, if the user does not already have an account on GitLab, one will be created for them tied to their SSO account.

![GitLab SSO tutorial - image11](https://images.ctfassets.net/r9o86ar0p03f/58htcOWFV7kPf18UUhUe2Z/f0d58dd9e23b96dce43e56e14d787c21/image11.png)

<center><i>Figure 27: SAML Group Links configured</i></center><br>

Now that you’ve completed the configuration, give it a try! From another browser, preferably in private mode to ignore any cookies or other yummy artifacts, paste the link for the GitLab SSO URL found in the GitLab SAML configurations. You should be prompted to log in with your Entra ID credentials and gain the proper access to your GitLab group!

Congratulations, you’ve made it! I hope you’ve learned from and appreciate the work here, and we can all rejoice in the fact that the users within the Play-Dough app can now all properly authenticate, with the right permissions, to The Academy!

> Don't have a GitLab account? Sign up for a free, 60-day trial today.

## Read more

- The ultimate guide to enabling SAML and SSO on GitLab.com
- SAML SSO for GitLab.com groups documentation

Go to Source
