---
title: Set up legal entity and tax information for Panama
description: This article explains how to set up legal entity and tax information for a Panama company. 
author: Cpicon85
ms.date: 10/11/2023
ms.topic: Article
ms.reviewer: kfend
ms.author: v-cpicon
ms.custom: bap-template
---
# Set up legal entity and tax information for Panama

This article explains how to set up a legal entity and the tax information for a company in Panama who is using the LATAM features available in Dynamics 365 Finance. A legal entity represents the company and contains the tax and legal attributes required for the rest of the LATAM configuration.
Before you begin, go to the **Feature management** workspace, and verify that the feature, **LATAM globalization expansion – Panama** is enabled. If it's not, enable it. When you have verified that the feature is enabled, complete the following steps.

## Create a legal entity
1.  Go to **Organization administration** > **Organizations** > **Legal entities**, create a new legal entity to use as the company and set the address to Panama. Some functionalities are enabled only for Panama. For example, specific tax reports or electronic invoices.
2.  In the address setup replicate the Panama territorial organization and using State as "Province", County as "District" and City as "Corregimiento".
3.  Go to **Organization administration** > **Setup** > **LATAM** > **LATAM parameters**.
4.  On the **Concept and notes** tab set a concept field from the **Legal entity** section as "Activity" to complete that information in the legal entity tax and legal information.
 

## Set up tax information

1.  [Create a document class letter](../ltm-core-document-class-letter.md) without a prefix. This document class letter will be used in taxpayer and document class configurations.
2.  Go to **Organization administration** > **Setup** > **LATAM** > **Tax ID type**.
3.  Select **New** and in the **Overview** section, in the **Tax ID type** field, enter **RUC** (Unique Taxpayer Registry), which is one of the tax ID types for Panama.
4.  In the **Format** field, enter **XXXXXXXX-XX**. For more information, see [Tax ID types](../ltm-core-tax-id-type.md).
5.  Go to **Organization administration > Global address book > Addresses** select the country where the company is set and select the **LATAM** button to add the tax ID type **RUC**. For more information, see [Address setup for Latin America](https://learn.microsoft.com/en-us/dynamics365/finance/localizations/ltm-core-address-setup).
6.  Go to **Organization administration > Setup > LATAM > Taxpayer type** create a new record that represents organizations "persona juridica" and add the tax ID type created and the document class letter created. For more information, see [Tax payer type](../ltm-core-taxpayer-type.md).
7.  Go to **Organization administration** > **Organizations** > **Legal entities** in the LATAM section configure the entity tax and legal information:
    * Taxpayer type: select "persona juridica" represents an organization.
    * Based in country/region: Panama.
    * Country document type: RUC
    * Complete the country document number with (Tax ID number) of the company.
    * Complete the first field from the "Concept and notes" section with the company activity.
