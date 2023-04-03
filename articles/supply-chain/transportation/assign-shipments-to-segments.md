---
title: Assign shipments to related route segments
description: This feature brings correct freight cost apportionment for multi-segment scheduled routes with inbound and outbound logistic process.
author: Weijiesa
ms.author: weijiesa
ms.reviewer: kamaybac
ms.search.form:
ms.topic: how-to
ms.date: 04/20/2023
audience: Application User
ms.search.region: Global
ms.custom: bap-template
---

# Assign shipments to related route segments

[!include [banner](../includes/banner.md)]
[!INCLUDE [preview-banner](../includes/preview-banner.md)]

<!-- KFM: Preview until further notice -->

This feature enables the system to apportion shipment freight costs more accurately, including for loads with multiple shipments delivered to various segment destinations along a single route. It assigns each shipment to the most suitable route segment based on the destination addresses of the shipment and segment. The feature then calculates each shipment's freight cost as a proportion of the load's total freight cost, based on the shipment's relative weight, volume, quantity and distance traveled.

This feature brings correct freight cost apportionment for multi-segment scheduled routes with inbound and outbound logistic process. With the correct freight cost apportionment, you can evaluate the true profitability of sales orders by apportioning freight cost at the item line level. This feature provides a fair and trusted foundation for the billing of freight cost and also reflects the cost of inbound freight in the material valuation of stock.

This feature only applies to shipments managed using the Transportation management (TMS) module.

To use this feature, your system must meet the following requirements:

- You must be running Microsoft Dynamics 365 Supply Chain Management version 10.0.33 or later.
- The feature named *(Preview)Assign shipments to related route segments* must be turned on in [feature management](../../fin-ops-core/fin-ops/get-started/feature-management/feature-management-overview.md).
