---
title: Azure with Microsoft Teams
description: Monitor your applications and infrastructure on Azure from Microsoft Teams
ms.date: 02/14/2020
---

# Azure with Microsoft Teams (Private preview)
[Azure](https://azure.microsoft.com/) is a cloud computing service offering by Microsoft. It has over 100 services that help to build, deploy and manage applications across cloud, on-premise and at the edge. Millions of users use Azure services daily.

ChatOps is a team and collaboration centric way of working where in people, conversations, tools, and files are ensembled in one place i.e. workplace messaging apps. Modern day developers spend considerable amount of time on [Microsoft Teams](https://products.office.com/microsoft-teams/group-chat-software) trying to build world class products and services. 


Today, considerable amount of time is spent to monitor applications, infrastructure, and to debug issues by developers and IT operations team. This necessitates constant switching of context between Azure (get alerts, diagnose & take remedial actions) and Microsoft Teams (collaborate). Azure app for Microsoft Teams brings best of both the worlds by integrating Azure with Microsoft Teams. Users can get all the alerts from Azure in their Teams channel by linking their channel to an action group on Azure. 

## Prerequisites 
Authentication to Azure happens via Azure DevOps. To use the app, users need to have an identity in Azure DevOps. In case you do not have an Azure DevOps identity, you can create one during the signin process.

## Get Started - Add the Azure app to your Team
Download the [manifest](https://github.com/microsoft/AzureAppForMicrosoftTeams/blob/master/manifest.zip) and upload it as a custom app and install it in the team of your choice. 

 ![Add as custom app](./teams/add-as-custom-app.png)

Upon installing, a welcome message is displayed as shown in the following image. Use the ``@azure`` handle to start interacting with the app.

 ![welcome message](./teams/welcome-message.png)
 

**_Note: Azure app uses webhooks which are not bound by [rate limit rules](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-rate-limiting). Hence, it is recommended that you create a new channel for getting the alerts from the app. It will also be helpful to have specific channels for specific types of alerts so that right folks can action it. Depending on how the alerts are configured, the channel can get noisy (especially for activity alerts)._**

## Sign in to your app

Once the app is installed in your team, authenticate yourself to Azure app using the ``@azure signin`` command.

 ![sigin button](./teams/signin-button.png)
 
 ![sigin consent](./teams/signin-consent.png)
 
 ![sigin success](./teams/signin-success.png)


## Link your channel to action groups 
Azure uses **action groups** to send notifications about applications and infrastructure to users. Action groups help users to configure the medium (SMS, Email, Voice, Mobile app, Webhooks, etc) through which they want to get notified on. Every alert on Azure is mapped to one or more action groups. Azure app for Microsoft Teams allows users to link to action groups of their choice and get notified on the alerts.

1. To view, link and unlink actions groups for a channel, use the following command:

  ```
   @azure actionGroups
  ```
  The `actionGroups` command lists all the action groups linked to a channel. 

 ![action groups command](./teams/action-groups-command.png)

2. Click on 'Link an action group' button. Select a subscription and the action group that you want to link to the channel.

 ![link action group](./teams/link-action-group.png)

  To link an action group to a channel, one must be part of [Azure Monitor Contributor](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/roles-permissions-security#monitoring-contributor) group. When an action group is linked to a channel, a webhook action will be created with the name MicrosoftTeams_AzureApp_<Time_stamp> in the linked action group. 

## Unlink an action group from a channel
To unlink an action group, run `@azure actionGroups` command. Click on 'View all action groups' button and select the action group that you want to unlink.

 ![view-all-action-groups](./teams/view-all-action-groups.png)

To unlink an action group, one must be part of [Azure Monitor Contributor](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/roles-permissions-security#monitoring-contributor) group. 

## Receiving notifications
Once an action group is linked to a channel, all alerts sent to the action group will be directed to the channel in the form of notifications.

 ![metric notification](./teams/metric-notification.png)

For metric alerts, if the user who linked the action group has access to the resource group for which the alert was sent, a time series graph would be additionally rendered.

## Change alert state
The app also provides the ability to acknowledge or close an alert from the channel reducing the hop to Azure portal and ensure others in your team are up-to-date. 
 
![changeAlert](https://github.com/microsoft/AzureAppForMicrosoftTeams/blob/master/teams/changealert.png)

## View what's changed
To help debugging the alert and taking appropriate action, the app provides you the capability to trace and find the last deployments on your virtual machines. For VM resources you will see **what changed** button and on click of that it shows the last deployments done on the VM. You can browse to the pipeline as well as the specfic instance to find details of the artifact, commits and workitems deployed as well as who made the changes.  
 
![changeAlert](https://github.com/microsoft/AzureAppForMicrosoftTeams/blob/master/teams/whatchanged.png)

## Command reference

The following table lists all the commands you can use in your Microsoft Teams channel.

|Command	| Functionality |
| -------------------- |----------------|
| @azure actionGroups	| View,  link or unlink action groups for a channel |
| @azure signin	| Sign in to your Azure account |
| @azure signout	| Sign out from your Azure account |
| @azure feedback	| Report a problem or suggest a feature |

## Limitations
Being a private preview, Azure app has certain limitations as detailed below. We will continue to invest in the app to remove some of these constraints.

  *  Azure app supports [Common alert schema](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-common-schema)  notifications only.
  * Alerts with multiple conditions or a single metric alert with multiple dimensions are not supported. The notification will have data only for the first dimension. 
  * To report a problem or suggest a feature, users need to have a GitHub account.
  * The app needs to be sideloaded and is not avaialble in Teams app store as we are in private preview. 

## Future work
We’re constantly at work to improve the app, and soon you’ll see new features stated below

  * Ability to get pipeline deployment data (for virtual machines only)
  * Threading of notifications

## Troubleshooting

|Issue	| Possible cause |
| -------------------- |----------------|
| For metric alerts, the cards are not enriched with time charts	|The user who linked the action group does not have access to the resource for which the alert was fired |

## Best practices and recommendation from Azure Monitor 
* Configure only actionable alerts and don’t spam!
* Setup Alerts on all SLIs with thresholds configured based on your SLAs
* Prefer Metric Alerts for better performance/latency; Use Log Alerts for powerful query-based triggers or lack of metrics
* Properly define severity/descriptions and avoid sending blanket notifications to people who cannot take any actions
* Setup Multi-resource Metric Alerts that can grow flexibly with your resources
* Connect Alerts with **different action groups and teams channels** for effective and efficient management

## Private preview terms and condition
PREVIEWS ARE PROVIDED "AS-IS," "WITH ALL FAULTS," AND "AS AVAILABLE," AND ARE EXCLUDED FROM THE SERVICE LEVEL AGREEMENTS AND LIMITED WARRANTY. Previews may not be covered by customer support.We may change or discontinue Previews at any time without notice. We also may choose not to release a Preview into "General Availability". Refer [this](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/) for more details.

