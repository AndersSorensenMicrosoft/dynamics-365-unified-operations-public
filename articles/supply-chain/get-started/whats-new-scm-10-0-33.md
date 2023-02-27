---
title: Preview of Dynamics 365 Supply Chain Management 10.0.33 (April 2023)
description: This article describes features that are either new or changed in Microsoft Dynamics 365 Supply Chain Management 10.0.33. 
author: kamaybac
ms.author: kamaybac
ms.reviewer: kamaybac
ms.search.form:
ms.topic: conceptual
ms.date: 03/03/2023
audience: Application User
ms.search.region: Global
ms.custom: bap-template
---

# Preview of Dynamics 365 Supply Chain Management 10.0.33 (April 2023)

[!include [banner](../includes/banner.md)]
[!include [preview banner](../includes/preview-banner.md)]

This article lists features that are either new or changed in Microsoft Dynamics 365 Supply Chain Management preview version 10.0.33. This version has a build number of 10.0. <!-- KFM: Update build number -->  and is available on the following schedule:

- **Preview of release:** March 2023
- **General availability of release (self-update):** April 2023
- **General availability of release (auto-update):** May 2023

## Features included in this release

The following table lists the features that are included in this release. We might update this article to include features that were added to the build after this article was originally published.

| Feature area | Feature | More information | Enabled by |
|---|---|---|---|
| Inventory and logistics | [Assign shipments to related route segments](/dynamics365/release-plan/2023wave1/finance-operations/dynamics365-supply-chain-management/assign-shipments-related-route-segments) | This feature enables the system to apportion shipment freight costs more accurately, including for loads with multiple shipments delivered to various segment destinations along a single route. It assigns each shipment to the most suitable route segment based on the destination addresses of the shipment and segment. The feature then calculates each shipment's freight cost as a proportion of the load's total freight cost, based on the shipment's relative weight, volume, quantity and distance traveled. This feature only applies to shipments managed using the Transportation management (TMS) module. | Feature management:<br>*(Preview)Assign shipments to related route segments* <!-- KFM: Is this the right FM feature for this RP feature? The FM feature description is more clear than that on the RP, we should consider replacing the RP text. We first announced this feature as an "enhancement" for 10.0.30, so where does this belong? --> |
| Inventory and logistics | [Create and update containers in batch processing mode](/dynamics365/release-plan/2023wave1/finance-operations/dynamics365-supply-chain-management/create-update-containers-batch-processing-mode) | [Manage shipping containers](../landed-cost/manage-shipping-containers.md) | Feature management:<br>*(Preview) Enable shipping container creation and update in batch mode* |
| Inventory and logistics | [Split cost type codes for multiple voyages in the vendor invoice journal](/dynamics365/release-plan/2023wave1/finance-operations/dynamics365-supply-chain-management/split-cost-type-codes-multiple-voyages-vendor-invoice-journal) | This feature for the Landed cost module enables business to better allocate transportation costs associated with multiple voyages. With this feature, when a user is creating a vendor invoice journal for multiple voyages, each cost type code will have its own journal line that includes the voyage name in its description. This allows for easier reconciliation. Previously, the system merged costs from all voyages that have the same cost type code into the same line in the vendor invoice journal. But now, vendor invoice journals will be split based on the cost type code. | Feature management:<br>*(Preview) Enable split vendor invoice journal line per cost type code and voyage id from multiple voyages* <!-- KFM: Is this the right FM feature for this RP feature? Note that the FM description contains an incomplete sentence and other grammar errors, and is very hard to understand. Is any documentation expected here? --> |
| Inventory and logistics | Inventory Visibility integration with soft reservation on sales order lines | [Inventory Visibility reservations](../inventory/inventory-visibility-reservations.md) | Feature management:<br>*Inventory Visibility integration with soft reservation on sales order lines* |
| Inventory and logistics | [Manage attribute-based omnichannel sales pricing](/dynamics365/release-plan/2023wave1/finance-operations/dynamics365-supply-chain-management/manage-attribute-based-omnichannel-sales-pricing) | *Coming soon* | Feature management:<br>*(Preview) Pricing management*<!-- KFM: Release plan says April, yet feature is visible now. What's the deal? --> |
| Manufacturing and asset management | [Empower maintenance workers with new mobile experience](/dynamics365/release-plan/2023wave1/finance-operations/dynamics365-supply-chain-management/empower-maintenance-workers-new-mobile-experience) | *Coming soon* | Enabled by default. Mobile app download required.<!-- KFM: Hide for now? --> |
| Warehouse management | Create and print custom label layouts | [Custom label layouts and printing](../warehousing/custom-label-layouts-and-printing.md) | Enabled by default |

## Feature enhancements included in this release

The following table lists the feature enhancements that are included in this release. Each of these enhancements provides an incremental improvement to an existing feature. Because they're only enhancements, they aren't listed in the [release plan](/dynamics365-release-plan/2022wave1/finance-operations/dynamics365-supply-chain-management/planned-features). However, to ensure that these enhancements won't conflict with your existing customizations or preferences, each of them is turned off by default (unless otherwise noted).

If you want to turn any of these features on or off, you must do so in [feature management](../../fin-ops-core/fin-ops/get-started/feature-management/feature-management-overview.md).

| Module | Feature name in feature management | More information |
|---|---|---|
| Master planning | Forecast demand plan import service | Allows multiple threads for importing forecast demand plan lines. <!-- KFM: Appears to be preview feature. Should be named with "(Preview)" -->  |
| Master planning | Process manufacturing support for Planning Optimization | Enables the creation of planned orders for formulas, co-products, and by-products when Planning Optimization is used, thereby supporting process manufacturing industries and the sequencing of production jobs. See also [Parameters not used by Planning Optimization](../master-planning/planning-optimization/not-used-parameters.md). |
| Product information management | Setup company specific number sequences | Lets you set up both cross-company and company-specific number sequences on the  **Product information management parameters** page. Company-specific number sequences are on the **Number sequences** tab while cross-company sequences are now available on the **Shared number sequences** tab. |
| Transportation management | (Preview) Performance improvements for post receipt function in Landed Cost | Improves performance of the Landed cost module by reducing the amount of information queried and update when posting product receipts for large purchase orders. |
| Transportation management | (Preview) Post landed cost with the specified posting type and account value on cost type code when credit type is 'Ledger Account'. | Posts landed costs with the specified posting type and account value on cost type code when the credit type is *Ledger Account*. As a result, landed costs won't be posted to clearing account consistently. |

<!-- KFM: We need to clarify which "Shared AP and AR" features apply to SCM (Rebate management) and also add these to the topic about enabling Rebate management features. -->

## Additional resources

### Platform updates for Finance and Operations apps

Microsoft Dynamics 365 Supply Chain Management 10.0.33 includes platform updates. To learn more, see [Platform updates for version 10.0.33 of Finance and Operations apps (April 2023)](../../fin-ops-core/dev-itpro/get-started/whats-new-platform-updates-10-0-33.md)<!--KFM: confirm link -->.

### Bug fixes

For information about the bug fixes included in each of the updates that are part of version 10.0.33, sign in to Microsoft Dynamics Lifecycle Services and view the [KB article](https://fix.lcs.dynamics.com/Issue/Details?bugId=) <!--KFM: Get new KB link -->.

### Dynamics 365 and industry clouds: 2023 release wave 1 plan

Wondering about upcoming and recently released capabilities in any of our business apps or platform?

Check out the [Dynamics 365 and industry clouds: 2023 release wave 1 plan](/dynamics365/release-plan/2023wave1/). We've captured all the details, end to end, top to bottom, in a single document that you can use for planning.

### Removed and deprecated Supply Chain Management features

The [Removed or deprecated features in Dynamics 365 Supply Chain Management](removed-deprecated-features-scm-updates.md) article describes features that have been or are scheduled to be removed or deprecated for Supply Chain Management.

- A *removed* feature is no longer available in the product.
- A *deprecated* feature isn't in active development and may be removed in a future update.

Before any feature is removed from the product, the deprecation notice will be announced in the [Removed or deprecated features in Dynamics 365 Supply Chain Management](removed-deprecated-features-scm-updates.md) article 12 months prior to the removal.

For breaking changes that only affect compilation time, but are binary compatible with sandbox and production environments, the deprecation time will be less than 12 months. Typically, these are functional updates that need to be made to the compiler.

[!INCLUDE[footer-include](../../includes/footer-banner.md)]
