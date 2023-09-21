---
title: Delete environments when Power Platform Integration is enabled
description: This article explains how to delete environments when finance and operations apps are integrated with Power Platform
author: abunduc-ms
ms.author: abunduc
ms.date: 06/02/2023
ms.topic: how-to
ms.reviewer: johnmichalak
ms.custom: bap-template
---

# Delete environments when Power Platform Integration is enabled

When the Microsoft Power Platform Integration is enabled, the finance and operations apps and the customer engagements apps environments are tightly connected. Administrators should look at these two platforms as a one environment with multiple apps. This article explains how to delete environments.

## Delete an environment from Power Platform admin center

It is not possible to delete a linked environment from Power Platform admin center, the following error is shown:

:::image type="content" source="media/ppi-delete-ppac.png" alt-text="Delete an environment from Power Platform admin center." lightbox="media/ppi-delete-ppac.png":::

## Delete an environment from Lifecycle services

The Lifecycle services (LCS) portal can still be used to delete a finance and operations apps environment, the process is not changed.

## Recommendations

- When the finance and operations apps environment is deleted, the linked Dataverse environment is **not** automatically deleted. However, the link is removed and the environment can be further deleted from [Power Platform admin center](/power-platform/admin/delete-environment).
