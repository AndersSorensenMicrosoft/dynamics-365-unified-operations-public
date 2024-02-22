---
title: Configure system parameters to report Sales tax books
description: This article explains how to configure system parameters to report Sales tax books for legal entities in Italy.
author: liza-golub
ms.date: 06/20/2017
ms.topic: article
ms.prod: 
ms.technology: 
audience: Application User
ms.reviewer: kfend
ms.search.region: Italy
ms.author: egolub
ms.search.validFrom: 2016-11-30
ms.dyn365.ops.version: Version 1611
ms.custom: 269664
ms.assetid: af07d122-5694-4de6-96bf-7bf5478b0175
ms.search.form: TaxYearlyCom_IT, TaxAuthority, TaxPeriod
---

# Configure system parameters to report Sales tax books for Italy

[!include [banner](../../includes/banner.md)]

This article explains how to configure system parameters to report Sales tax books for legal entities in Italy.

As of version 10.0.39 of Finance you can generate Italian Sales tax books in legal entities with multiple VAT registration numbers and primary address outside of Italy. For more information, see [Multiple VAT registration numbers](../global/emea-multiple-vat-registration-numbers.md).

## Set up a company, vendors and customers fiscal code and tax registration number

> [!NOTE]
> When [Tax Calculation service](../global/global-tax-calcuation-service-overview.md) is used and [Multiple VAT registration numbers](../global/emea-multiple-vat-registration-numbers.md) functionality is enabled, you must set up [**Registration IDs**](../europe/emea-registration-ids.md) to define **Tax exempt number** (Tax registration number) of your legal entity, customers and vendors as it is described in the [Set up a VAT ID for a legal entity, customers, and vendors](../global/emea-multiple-vat-registration-numbers.md#set-up-a-vat-id-for-a-legal-entity-customers-and-vendors) section of [Multiple VAT registration numbers](../global/emea-multiple-vat-registration-numbers.md) guidance. 

### Company fiscal code and tax registration number

| **Field**        | **Description**                                                    | **Page > FastTab** |
|------------------|--------------------------------------------------------------------|--------------------|
| **Tax registration number** | Enter the tax registration number for the legal entity in Italy |  Legal entities > Tax registration <br> See *Note* above in this section for reference on setup in **Multiple VAT registration numbers** scenario. |
| **Fiscal Code**  | Enter the fiscal code from the legal entity registration in Italy. | Legal entities > Registration numbers |

### Customers fiscal code and tax registration number

| **Field**             | **Description**                                        | **Page > FastTab** |
|-----------------------|--------------------------------------------------------|--------------------|
| **Tax exempt number** | Enter the tax exempt number for VAT declaration.       | Account receivable > Customers > All customers > Invoice and delivery <br> See *Note* above in this section for reference on setup in **Multiple VAT registration numbers** scenario. |
| **Fiscal code**       | Enter the fiscal code for this customer.               | Account receivable > Customers > All customers > Invoice and delivery |

### Vendors fiscal code and tax registration number

| **Field**             | **Description**                                      | **Page > FastTab** |
|-----------------------|------------------------------------------------------|--------------------|
| **Tax exempt number** | Enter the tax exempt number for VAT declaration.     | Account payable > Vendors > All vendors > Invoice and delivery <br> See *Note* above in this section for reference on setup in **Multiple VAT registration numbers** scenario. |
| **Fiscal code**       | Enter the fiscal code for this vendor.               | Account payable > Vendors > All vendors > Invoice and delivery |

### Set up parameters for Italian sales tax book

On the **Sales tax** page, create sales tax books to be reported to the Italian government after the settlement period is past.

| **Field**                     | **Description**                                 |
|-------------------------------|-------------------------------------------------|
| **Sales tax book type**       | Enter the movement direction. (Sales/Purchase)  |
| **Sales tax book**            | Enter the identifier of the sales tax book.     |
| **Name**                      | Enter the name of the sales tax book.           |
| **Sales tax book type**       | Enter the movement direction. (Sales/Purchase)  |
| **Settlement period**         | The period for this tax book.                   |
| **Closed to**                 | Select the transaction date.                    |
| **EU sales**                  | The amount sold to the European Union.          |
| **ATECOFIN**                  | The tax code for sales tax reporting.           |
| **Print summary and payment** | Select to print the summary and payment report. |

## Yearly tax communication report
After the year end, generate the report for each settlement period. The transmission is mandatory as required by the Italian fiscal authority via a website. On the **Yearly tax communication** page, it is possible to create or view a report.

### Overview

| **Field**                | **Description**                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------|
| **Tax communication ID** | Enter the identification number of the Yearly tax communication report.                       |
| **Years**                | Enter the year of the tax communication.                                                      |
| **ATECOFIN Code**        | Enter the tax code that is associated with the classification of possible company activities. |
| **Exported**             | Indicates that the .ivc file is exported.                                                     |

### General

| **Field**                | **Description**                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------|
| **Tax communication ID** | Enter the identification number of the Yearly tax communication report.                       |
| **Years**                | Enter the year of the tax communication.                                                      |
| **ATECOFIN Code**        | Enter the tax code that is associated with the classification of possible company activities. |
| **Exported**             | Indicates that the file was exported.                                                         |
| **Date of export**       | The date when the .ivc file was exported.                                                     |
| **Export file name**     | The name of the .ivc file.                                                                    |

Find more details in [Yearly tax communication](emea-ita-yearly-tax-communication.md).

## VAT summary report
For VAT reporting, Italy has specific information to be reported and formatted. In the **Sales tax authorities** page, on **Tax setup**, configure the Italian authority.

| **Field**                                 | **Description**                                                                                |
|-------------------------------------------|------------------------------------------------------------------------------------------------|
| **Report layout**                         | Select the Italian report to enable Italian fields.                                            |
| **Print blank page with no transactions** | Select to print blank pages when no transactions are present.                                  |
| **Separate summary register**             | Select this check box to use a separate value added tax (VAT) book to number summary sections. |
| **Account gain**                          | Gain due to rounding.                                                                          |
| **Account loss**                          | Loss due to rounding.                                                                          |

## Sales tax settlement periods
Per Italian legislation, rules apply to settlement periods. For example, after the settlement period is closed, no change is allowed. You are required to report if the settlement refers to the last period on the fiscal year. View the settlement period status and report if it refers to a year closing. On the **Sales tax settlement period** page &gt; **Period Intervals** tab, select the following fields.


|    <strong>Field</strong>    |                                   <strong>Description</strong>                                    |
|------------------------------|---------------------------------------------------------------------------------------------------|
|   <strong>Closed</strong>    | Indicates if the Italian sales tax book for the period has been updated and automatically closed. |
| <strong>Last period</strong> |             Select this option if the period is the last period in a sales tax year.              |



[!INCLUDE[footer-include](../../../includes/footer-banner.md)]
