---
title: How to Publish a NuGet Package | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/05/2017
ms.topic: article
ms.prod: nuget
ms.technology: null
description: Detailed instructions for how to publish a NuGet package to nuget.org or private feeds, and how to manage package ownership on nuget.org.
keywords: NuGet package publishing, publish NuGet package, NuGet package ownership, publish to nuget.org, private NuGet feeds
ms.reviewer:
- anangaur
- karann-msft
- unniravindranathan
---

# Publishing packages

Once you have created a package and have your `.nukpg` file in hand, it's a simple process to make it available to other developers, either publicly or privately:

- Public packages are made available to all developers globally through [nuget.org](https://www.nuget.org/packages/manage/upload) as described in this topic.
- Private packages are available to only a team or organization, by hosting them either a file share, a private NuGet server, [Visual Studio Team Services Package Management](https://www.visualstudio.com/docs/package/nuget/publish), or a third-party repository such as myget, ProGet, Nexus Repository, and Artifactory. For additional details, see [Hosting Packages Overview](../hosting-packages/overview.md).

This topic covers publishing to nuget.org; for publishing to Visual Studio Team Services, see [Package Management](https://www.visualstudio.com/docs/package/nuget/publish).

## Publish to nuget.org

For nuget.org, you must first [register for a free account](https://www.nuget.org/users/account/LogOn?returnUrl=%2F) or sign in if already registered:

![NuGet registration and sign in location](media/publish_NuGetSignIn.png)

Next, you can either upload the package through the nuget.org web portal, push to nuget.org from the command line (requires `nuget.exe` 4.1.0+) , or publish as part of a CI/CD process through Visual Studio Team Services, as described in the following sections.

### Web portal: use the Upload Package tab on nuget.org

![Upload a package with the NuGet Package Manager](media/publish_UploadYourPackage.PNG)

### Command line

> [!Important]
> To push packages to nuget.org you must use [nuget.exe v4.1.0 or above](https://www.nuget.org/downloads), which implements the required [NuGet protocols](../api/nuget-protocols.md).

1. Click on your user name to navigate to your account settings.
1. Under **API Key**, click **copy to clipboard** to retrieve the access key you need in the CLI:

    ![Copying an API Key from account settings](media/publish_APIKey.png)

1. At a command prompt, run the following command:

    ```cli
    nuget setApiKey Your-API-Key
    ```

    This stores your API key on the machine so that you need not do this step again on the same machine.

1. Push your package to NuGet Gallery using the command:

    ```cli
    nuget push YourPackage.nupkg -Source https://api.nuget.org/v3/index.json
    ```

1. Before being made public, all packages uploaded to nuget.org are scanned for viruses and rejected if any viruses are found. All packages listed on nuget.org are also scanned periodically.

1. In your account on nuget.org, click **Manage my packages** to see the one that you just published; you also receive a confirmation email. Note that it might take a while for your package to be indexed and appear in search results where others can find it, during which time you see the following message on your package page:

    ![Message indicating a package is not yet indexed](media/publish_NotYetIndexed.png)

### Package validation and indexing

Packages pushed to nuget.org undergo several validations. When the package has passed all validation checks, it might take a while for it to be indexed and appear in search results. Once indexing is complete, you receive an email confirming that the package was successfully published. If the package fails a validation check, the package details page will update to display the associated error and you also receive an email notifying you about it.

Package validation and indexing usually takes under 15 minutes. If the package publishing is taking longer than expected, visit [status.nuget.org](https://status.nuget.org/) to check if nuget.org is experiencing any interruptions. If all systems are operational and the package hasn't been successfully published within an hour, please login to nuget.org and contact us using the Contact Support link on the package page.

### Visual Studio Team Services (CI/CD)

If you push packages to nuget.org using Visual Studio Team Services as part of your Continuous Integration/Deployment process, you must use `nuget.exe` 4.1 or above in the NuGet tasks. Details can be found on [Using the latest NuGet in your build](https://blogs.msdn.microsoft.com/devops/2017/09/29/using-the-latest-nuget-in-your-build/) (Microsoft DevOps blog).

## Managing package owners on nuget.org

Although each NuGet package's `.nuspec` file defines the package's authors, the nuget.org gallery does not use that metadata to define ownership. Instead, nuget.org assigns initial ownership to the person who publishes the package. This is either the logged-in user who uploaded the package through the nuget.org UI, or the users whose API key was used with `nuget SetApiKey` or `nuget push`.

All package owners have full permissions for the package, including adding and removing other owners, and publishing updates.

To change ownership of a package, do the following:

1. Sign in to nuget.org with the account that is the current owner of the package.
1. Click on your username, then on **Manage my packages**, then on the package you want to manage.
1. Click the **Manage owners** link on the left side.

From here you have several options:

1. To add an owner, enter their NuGet account name and click **Add**. This sends an email to that new co-owner with a confirmation link. Once confirmed, that person has full permissions to add and remove owners. (Until confirmed, the **Manage owners** page indicates "pending approval" for that person).
1. To remove an owner, select their name on the **Manage owners** and click **Remove**.
1. To transfer ownership (as when ownership changes or a package was published under the wrong account), simply add the new owner, and once they've confirmed ownership they can remove you from the list.

To assign ownership to a company or group, create a nuget.org account using an email alias that is forwarded to the appropriate team members. For example, various Microsoft ASP.NET packages are co-owned by the [microsoft](http://nuget.org/profiles/microsoft) and [aspnet](http://nuget.org/profiles/aspnet) accounts, which simply such aliases.

### Recovering package ownership

Occasionally, a package may not have an active owner. For example, the original owner may have left the company that produces the package, nuget.org credentials are lost, or earlier bugs in the gallery left a package ownerless.

If you are the rightful owner of a package and need to regain ownership, use the [contact form](https://www.nuget.org/policies/Contact) on nuget.org to explain your situation to the NuGet team. We then follow a process to verify your ownership of the package, including trying to locate the existing owner through the package's Project URL, Twitter, email, or other means. But if all else fails, we can send you a new invite to become an owner.
