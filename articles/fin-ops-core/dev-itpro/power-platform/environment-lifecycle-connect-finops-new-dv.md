---
# required metadata

title: Connect Finance and Operations apps with a new Microsoft Dataverse instance
description: Connect Finance and Operations apps with a new Microsoft Dataverse instance 
ms.author: laswenka
author: laneswenka
ms.date: 03/16/2023
ms.topic: article
ms.prod:
ms.technology: 

# optional metadata

# ms.search.form:
audience: Developer, IT Pro
# ms.devlang: 
ms.reviewer: sericks
# ms.tgt_pltfrm: 
ms.custom: "intro-internal"
ms.search.region: Global
# ms.search.industry:
ms.search.validFrom: 2021-10-13
ms.dyn365.ops.version: 10.0.0
---
# Connect Finance and Operations apps with a new Microsoft Dataverse instance

[!include[banner](../includes/banner.md)]

Administrators in Lifecycle Services are finding more and more capabilities require connection to Microsoft Dataverse via Power Platform Integration.  Customers who don't already use Microsoft Dataverse for low code or with other Dynamics 365 apps can quickly get one setup and connected in a few button clicks.  This scenario will walk you through connecting your Finance and Operations apps environment with a new Microsoft Dataverse instance to combine them as one logical enviornment.

In this scenario, you will learn how to:

> [!div class="checklist"]
> * Step 1: Click setup from the Power Platform Integration tab
> * Step 2
> * Step 3
> * Step 4

As an example of this scenario, a customer who has Finance and Operations apps environment deployed wishes to connect it to a new Microsoft Dataverse environment.  This will unlock popular features such as Add-ins, Dual-write, Virtual entities, and Business events out of the box so that the rich Finance and Operations apps data can be made available for low code applications and services.

## Preqrequisites
<INFO HERE>

## Step 1: Click setup from the Power Platform Integration tab
In Lifecycle Services, visit your sandbox or Production environment and locate the "Power Platform Integration" tab.  You should see that the **Setup** button is available which means that you can configure your connection to Microsoft Dataverse. 

<img src="media/Scenario1_Step1.png" width="600px" alt="Setup button for enablign Power Platform Integration" />

## Step 2: Configure Microsoft Dataverse using a template
In the slider window, do not select to use an existing Power Platform Environment.  If you already have Microsoft Dataverse deployed with other Dynamics 365 applications and wish to connect to it, see [Connect Finance and Operations apps with an existing Microsoft Dataverse instance](blah).

Select an available template which will create Microsoft Dataverse with several applications pre-installed based on your requirements:

<img src="media/Scenario1_Step2.png" width="600px" alt="Setup button for enablign Power Platform Integration" />

The available templates today include:
* Dynamics 365 standard - this enables Add-ins, Virtual entities, Business events, and Dual-write platform components but does not contain any Dual-write application maps.  This is the most common template.
* Dynamics 365 standard with Dual-write - this enables everything from the standard template as well as installs application maps for Dual-write.
* Project operations - this is a special template for customers with the Dynamics 365 Project Operations license. This pre-installs solutions required for it to run as well on top of the Dynamics 365 standard with Dual-write template.

## Anti-patterns

