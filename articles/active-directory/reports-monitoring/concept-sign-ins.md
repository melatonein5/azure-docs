---
title: Sign-in logs in Azure Active Directory | Microsoft Docs
description: Overview of the sign-in logs in Azure Active Directory.  
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: karenhoran
editor: ''

ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/02/2022
ms.author: markvi
ms.reviewer: besiler

ms.collection: M365-identity-device-management
---
# Sign-in logs in Azure Active Directory

As an IT administrator, you want to know how your IT environment is doing. The information about your system’s health enables you to assess whether and how you need to respond to potential issues. 

To support you with this goal, the Azure Active Directory portal gives you access to three activity logs:

- **[Sign-ins](concept-sign-ins.md)** – Information about sign-ins and how your resources are used by your users.
- **[Audit](concept-audit-logs.md)** – Information about changes applied to your tenant such as users and group management or updates applied to your tenant’s resources.
- **[Provisioning](concept-provisioning-logs.md)** – Activities performed by the provisioning service, such as the creation of a group in ServiceNow or a user imported from Workday.

This article gives you an overview of the sign-ins report.


## What can you do with it?

You can use the sign-ins log to find answers to questions like:

- What is the sign-in pattern of a user?

- How many users have signed in over a week?

- What’s the status of these sign-ins?


## Who can access it?

You can always access your own sign-ins history using this link: [https://mysignins.microsoft.com](https://mysignins.microsoft.com)

To access the sign-ins log, you need to be:

- A global administrator

- A user in one of the following roles:
    - Security administrator

    - Security reader

    - Global reader

    - Reports reader



## What Azure AD license do you need?

The sign-in activity report is available in [all editions of Azure AD](reference-reports-data-retention.md#how-long-does-azure-ad-store-the-data). If you have an Azure Active Directory P1 or P2 license, you also can access the sign-in activity report through the Microsoft Graph API.


## Where can you find it in the Azure portal?

The Azure portal provides you with several options to access the log. For example, on the Azure Active Directory menu, you can open the log in the **Monitoring** section.  

![Open sign-in logs](./media/concept-sign-ins/sign-ins-logs-menu.png)

Additionally, you can get directly get to the sign-in logs using this link: [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)


## What is the default view?

A sign-ins log has a default list view that shows:

- The sign-in date
- The related user
- The application the user has signed in to
- The sign-in status
- The status of the risk detection
- The status of the multi-factor authentication (MFA) requirement

![Screenshot shows the Office 365 SharePoint Online Sign-ins.](./media/concept-sign-ins/sign-in-activity.png "Sign-in activity")

You can customize the list view by clicking **Columns** in the toolbar.

![Screenshot shows the Columns option in the Sign-ins page.](./media/concept-sign-ins/19.png "Sign-in activity")

The **Columns** dialog gives you access to the selectable attributes. In a sign-in report, you can't have fields
that have more than one value for a given sign-in request as column. This is, for example, true for authentication details, conditional access data and network location.   

![Screenshot shows the Columns dialog box where you can select attributes.](./media/concept-sign-ins/columns.png "Sign-in activity")




## Sign-in error code

If a sign-in failed, you can get more information about the reason in the **Basic info** section of the related log item. 

![sign-in error code](./media/concept-all-sign-ins/error-code.png)
 
While the log item provides you with a failure reason, there are cases where you might get more information using the [sign-in error lookup tool](https://login.microsoftonline.com/error). For example, if available, this tool provides you with remediation steps.  

![Error code lookup tool](./media/concept-all-sign-ins/error-code-lookup-tool.png)



## Filter sign-in activities


You can filter the data in a log to narrow it down to a level that works for you:

![Screenshot shows the Add filters option.](./media/concept-sign-ins/04.png "Sign-in activity")

**Request ID** - The ID of the request you care about.

**User** - The name or the user principal name (UPN) of the user you care about.

**Application** - The name of the target application.
 
**Status** - The sign-in status you care about:

- Success

- Failure

- Interrupted


**IP address** - The IP address of the device used to connect to your tenant.

The **Location** - The location the connection was initiated from:

- City

- State / Province

- Country/Region


**Resource** - The name of the service used for the sign-in.


**Resource ID** - The ID of the service used for the sign-in.


**Client app** - The type of the client app used to connect to your tenant:

![Client app filter](./media/concept-sign-ins/client-app-filter.png)


> [!NOTE]
> Due to privacy commitments, Azure AD does not populate this field to the home tenant in the case of a cross-tenant scenario.


|Name|Modern authentication|Description|
|---|:-:|---|
|Authenticated SMTP| |Used by POP and IMAP client's to send email messages.|
|Autodiscover| |Used by Outlook and EAS clients to find and connect to mailboxes in Exchange Online.|
|Exchange ActiveSync| |This filter shows all sign-in attempts where the EAS protocol has been attempted.|
|Browser|![Blue checkmark.](./media/concept-sign-ins/check.png)|Shows all sign-in attempts from users using web browsers|
|Exchange ActiveSync| | Shows all sign-in attempts from users with client apps using Exchange ActiveSync to connect to Exchange Online|
|Exchange Online PowerShell| |Used to connect to Exchange Online with remote PowerShell. If you block basic authentication for Exchange Online PowerShell, you need to use the Exchange Online PowerShell module to connect. For instructions, see [Connect to Exchange Online PowerShell using multi-factor authentication](/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell).|
|Exchange Web Services| |A programming interface that's used by Outlook, Outlook for Mac, and third-party apps.|
|IMAP4| |A legacy mail client using IMAP to retrieve email.|
|MAPI over HTTP| |Used by Outlook 2010 and later.|
|Mobile apps and desktop clients|![Blue checkmark.](./media/concept-sign-ins/check.png)|Shows all sign-in attempts from users using mobile apps and desktop clients.|
|Offline Address Book| |A copy of address list collections that are downloaded and used by Outlook.|
|Outlook Anywhere (RPC over HTTP)| |Used by Outlook 2016 and earlier.|
|Outlook Service| |Used by the Mail and Calendar app for Windows 10.|
|POP3| |A legacy mail client using POP3 to retrieve email.|
|Reporting Web Services| |Used to retrieve report data in Exchange Online.|
|Other clients| |Shows all sign-in attempts from users where the client app is not included or unknown.|







**Operating system** - The operating system running on the device used sign-on to your tenant. 


**Device browser** - If the connection was initiated from a browser, this field enables you to filter by browser name.


**Correlation ID** - The correlation ID of the activity.




**Conditional access** - The status of the applied conditional access rules

- **Not applied**: No policy applied to the user and application during sign-in.

- **Success**: One or more conditional access policies applied to the user and application (but not necessarily the other conditions) during sign-in. 

- **Failure**: The sign-in satisfied the user and application condition of at least one Conditional Access policy and grant controls are either not satisfied or set to block access.



## Sign-ins data shortcuts

Azure AD and the Azure portal both provide you with additional entry points to sign-ins data:

- The Identity security protection overview
- Users
- Groups
- Enterprise applications

### Users sign-ins data in Identity security protection

The user sign-in graph in the **Identity security protection** overview page shows weekly aggregations of sign-ins. The default for the time period is 30 days.

![Screenshot shows a graph of Sign-ins over a month.](./media/concept-sign-ins/06.png "Sign-in activity")

When you click on a day in the sign-in graph, you get an overview of the sign-in activities for this day.

Each row in the sign-in activities list shows:

* Who has signed in?
* What application was the target of the sign-in?
* What is the status of the sign-in?
* What is the MFA status of the sign-in?

By clicking an item, you get more details about the sign-in operation:

- User ID
- User
- Username
- Application ID
- Application
- Client
- Location
- IP address
- Date
- MFA Required
- Sign-in status

> [!NOTE]
> IP addresses are issued in such a way that there is no definitive connection between an IP address and where the computer with that address is physically located. Mapping IP addresses is complicated by the fact that mobile providers and VPNs issue IP addresses from central pools that are often very far from where the client device is actually used. 
> Currently, converting IP address to a physical location is a best effort based on traces, registry data, reverse lookups and other information.

On the **Users** page, you get a complete overview of all user sign-ins by clicking **Sign-ins** in the **Activity** section.

![Screenshot shows the Activity section where you can select Sign-ins.](./media/concept-sign-ins/08.png "Sign-in activity")

## Authentication details

The **Authentication Details** tab located within the sign-ins report provides the following information, for each authentication attempt:

- A list of authentication policies applied (such as Conditional Access, per-user MFA, Security Defaults)
- A list of session lifetime policies applied (such as Sign-in frequency, Remember MFA, Configurable Token lifetime)
- The sequence of authentication methods used to sign-in
- Whether or not the authentication attempt was successful
- Detail about why the authentication attempt succeeded or failed

This information allows admins to troubleshoot each step in a user’s sign-in, and track:

- Volume of sign-ins protected by multi-factor authentication 
- Reason for authentication prompt based on the session lifetime policies
- Usage and success rates for each authentication method 
- Usage of passwordless authentication methods (such as Passwordless Phone Sign-in, FIDO2, and Windows Hello for Business) 
- How frequently authentication requirements are satisfied by token claims (where users are not interactively prompted to enter a password, enter an SMS OTP, and so on)

While viewing the Sign-ins report, select the **Authentication Details** tab: 

![Screenshot of the Authentication Details tab](media/concept-sign-ins/auth-details-tab.png)

>[!NOTE]
>**OATH verification code** is logged as the authentication method for both OATH hardware and software tokens (such as the Microsoft Authenticator app).

>[!IMPORTANT]
>The **Authentication details** tab can initially show incomplete or inaccurate data, until log information is fully aggregated. Known examples include: 
>- A **satisfied by claim in the token** message is incorrectly displayed when sign-in events are initially logged. 
>- The **Primary authentication** row is not initially logged. 


## Usage of managed applications

With an application-centric view of your sign-in data, you can answer questions such as:

* Who is using my applications?
* What are the top three applications in your organization?
* How is my newest application doing?

The entry point to this data is the top three applications in your organization. The data is contained within the last 30 days report in the **Overview** section under **Enterprise applications**.

![Screenshot shows where you can select Overview.](./media/concept-sign-ins/10.png "Sign-in activity")

The app-usage graphs weekly aggregations of sign-ins for your top three applications in a given time period. The default for the time period is 30 days.

![Screenshot shows the App usage for a one month period.](./media/concept-sign-ins/graph-chart.png "Sign-in activity")

If you want to, you can set the focus on a specific application.

![Reporting](./media/concept-sign-ins/single-app-usage-graph.png "Reporting")

When you click on a day in the app usage graph, you get a detailed list of the sign-in activities.

The **Sign-ins** option gives you a complete overview of all sign-in events to your applications.

## Microsoft 365 activity logs

You can view Microsoft 365 activity logs from the [Microsoft 365 admin center](/office365/admin/admin-overview/about-the-admin-center). Consider the point that, Microsoft 365 activity and Azure AD activity logs share a significant number of the directory resources. Only the Microsoft 365 admin center provides a full view of the Microsoft 365 activity logs. 

You can also access the Microsoft 365 activity logs programmatically by using the [Office 365 Management APIs](/office/office-365-management-api/office-365-management-apis-overview).

## Next steps

- [Basic info in the Azure AD sign-in logs](reference-basic-info-sign-in-logs.md)

- [How to download logs in Azure Active Directory](howto-download-logs.md)

- [How to access activity logs in Azure AD](howto-access-activity-logs.md)