---
title: Extending Copilot in finance and operations apps
description: This article provide guidance
author: jaredha
ms.author: jaredha
ms.reviewer: kamaybac
ms.search.form:
ms.topic: how-to
ms.date: 12/11/2023
audience: Application User
ms.search.region: Global
ms.custom: bap-template
---

# Extending Copilot capabilities in finance and operations apps (preview)

[!include [banner](../includes/banner.md)]



## Copilot architecture
Copilot in finance and operations apps builds on [Microsoft Copilot Studio](https://learn.microsoft.com/microsoft-copilot-studio/fundamentals-what-is-copilot-studio), which provides the central AI orchestration of the Copilot capabilities. This framework enables you to extend the capabilities, creating powerful AI-powered experiences in finance and operations apps. 

### Copilot interface
The Copilot interface for the chat experience in finance and operations apps uses the same [AI Copilot](https://learn.microsoft.com/power-apps/maker/canvas-apps/ai-overview) control that is used in [canvas apps](https://learn.microsoft.com/power-apps/maker/canvas-apps/add-ai-copilot) and [model-driven apps](https://learn.microsoft.com/power-apps/maker/model-driven-apps/add-ai-copilot), including other Dynamics 365 applications. The control is embedded in the finance and operations client, hosted in the `SysCopilotChatPanel` form. It manages the communication between the finance and operations client and Microsoft Copilot Studio, and acts as the executor for client actions.

### Orchestration with Microsoft Copilot Studio
[Microsoft Copilot Studio](https://learn.microsoft.com/microsoft-copilot-studio/fundamentals-what-is-copilot-studio) (MCS) provides the central AI orchestration of the capabilities in finance and operations apps. When a prompt is entered in the Copilot chat panel, it is sent to Copilot Studio, which then determines the intent of the prompt and which topics or plugins should be invoked to provide a response. Copilot Studio executes plugins, gets the required data, and provides an output in natural language that is returned back to the user in the Copilot interface.

Copilot in finance and operations apps is bound to a single chatbot in Copilot Studio, called **Copilot in Finance and Operation**. The chatbot is deployed as part of the Copilot in Finance and Operations (msdyn_FnoCopilot) solution. See [Step 4: Install the Copilot application in your finance and operations apps environment](https://learn.microsoft.com/dynamics365/fin-ops-core/dev-itpro/copilot/enable-copilot#step-4-install-the-copilot-application-in-your-finance-and-operations-apps-environment) for guidance on installing the solution and chatbot in your envioronment.

## Plugins
Plugins are used to enable Copilot capabilities. From a technical perspective, a **plugin** is a package that includes a description of an API and instructions on how to use it. Conceptually, a plugin is something Copilot knows how to do. For example, if you want Copilot to be able to get customer balances, you create a plugin with a manifest that describes the capability, and knows which API to call to get the information to be returned to the user.

When designing your Copilot experiences, consider the types of questions or actions that the application user would expect to ask/prompt Copilot to answer or perform. Plugins themselves should not be thought of as an end-to-end scenario. A plugin should be thought of as an individual skill that the user can prompt in Copilot in various scenarios and sequences as part of the natural language conversation. The plugins can be chained together by the MCS orchestration to create an end-to-end conversational experience, but won't necessarily be the same sequence each time. The conversation of chained-together plugins becomes the end-to-end business scenario.

### Types of plugins
A consideration when developing your plugins is understanding what type of plugin to develop for your scenario. There are two primary types of plugins to consider: headless plugins and client plugins (contextual runtime plugins).

**Headless plugins:** Headless plugins are ideal for scenarios whre there is no specific user infarface for the scenario. If the plugin is querying general information that is applicable in any UI, then it should be engineers as a headless plugin. For example, you might want to check a customer balance or on-hand inventory while in a Teams converation. This information doesn't necessarily require application context to understand and receive value from the data. In these cases, using a headless plugin enables making the plugin available to any user interface connecte to the Dataverse plugin registry.

If a headless plugin is the right approach for your business scenario, you can find more detail on the concepts associated with headless plugins [INSERT LINK HERE TO PLUGIN REGISTRY DOCUMENTATION]. This documentatino focuses on the concepts behind making AI skills discoverable in Dataverse and available across any Copilot interface connected to the discovery service, such as [Copilot for Microsoft 365](https://www.microsoft.com/microsoft-365/copilot-for-work) and Copilot in finance and operations apps.

**Client plugins:** Client plugins are idfeal for use cases that are applicable only in the currently connected/running conversation and application context. If it's a skill that only makes sense within the flow of an in-app business process, or requires client action, then you would likely want to develop this as a client plugin. For example, a plugin that uses AI to calculate alternate production plans for a production schedule may make sense when in the application context of the production schedule, but wouldn't be helpful in other interfaces outside finance and operations apps.
