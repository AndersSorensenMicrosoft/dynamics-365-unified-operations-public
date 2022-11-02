---
title: Message processor messages
description: This article provides information about the message processor.
author: perlynne
ms.author: perlynne
ms.reviewer: kamaybac
ms.search.form: SysMessageProcessorMessage, BusinessEventsWorkspace   
ms.topic: conceptual
ms.date: 11/02/2022
ms.custom: bap-template
---

# Message processor messages

[!include [banner](../includes/banner.md)]

The message processor is a framework for processing messages representing events. The processing has the following properties

- Processes messages in the correct order (dependent messages are processed in sequence)
- Scalable (independent messages can processed in parallel)
- Uses system resources needed
- Avoids exhaustion of system resources if a spike in messages occurs.
- Reliable
- Traceable

<!-- KFM: Provide a bit more background and a couple of usage examples. -->

## The message processor messages list

### Open the message processor messages list

You can view the messages that have been processed by the message processor by going to **System administration \> Message processor \> Message processor messages**.

### Message processor messages grid columns and filters

You can use the fields at the top of the **Message processor messages** page to help find any particular messages you are looking for. Most of these filters match the column headings in the message grid. The following filters and column headings are available:

- **Message type** – Specifies the type of message.
- **Message queue** – Specifies the name of the queue in which the message is to be processed. The following queues are provided:
- **Message state** – Specifies the state of the message. The following states exists:
  - *Queued* – The message is ready to be processed by the message processor.
  - *Processed* – The message was successfully processed by the message processor.
  - *Canceled* – The message was processed, but processing failed.
- **Message content** – This filter does a full-text search of message content. (Message content is not shown in the grid.) The filter treats most special symbols (such as "-") as spaces, and treats all space characters as Boolean OR operators. For example, this means if you search for a specific `journalid` value equal "USMF-123456", the system will find all messages that contain "USMF" or "123456", which is likely to be a long list. Therefore, it would be better to enter just "123456" alone because that will return more specific results.

### View the message log, message content, and details

You can find detailed information about a message by selecting it in the grid and then opening the **Log** or **Message content** tabs under the message grid, where each processing event is shown.

The **Message content** depends on the **Message type** and will therefore have different text length. A typical message content text will start with a **{** and end with a **}** and in between have field names such as `journalId` followed by a **:** and a value such as *USMF-123456*.

The toolbar on the **Log** tab includes the following buttons:

- **Log** – Displays the processing results. This is especially helpful for understanding the reasons for a processing failure for messages having a **Processing result** of *Failed*.
- **Bundle** – Multiple message processing operations can run as part of the same batch process. Select this button to view this detailed data. For example, you can see whether dependencies exist that require the system to process certain messages in a specific sequence.

### Manually process or cancel a message

If needed, you can manually process or cancel a message, depending on its current state. To do so, select the message in the grid and then select **Process** or **Cancel** on the Action Pane.

<!-- KFM: What about the **Queue** button. What does that do and how do we use it? -->

## Message processor batch job

The *Message processor* batch job will automatically be evoked when a new message is created for processing, so you should not need to schedule this job manually.

If necessary, you can access the batch job by going to **System administration \> Message  processor \> Message processor**.

<!-- KFM: Give full description of this dialog, especially how to use the **Message queue** drop-down. -->

## Message processor queue setup

<!-- KFM: Describe what this page is for and how to use it. -->

## Set up business events to deliver alerts for failed processing results

In addition to filtering on the **Message state** value *Canceled* to inquire for failed messages, you can set up [Business events](../../fin-ops-core/dev-itpro/business-events/home-page.md) to alert you to failed processing results. To do so, activate the business event named *Message processor message processed*  on the **Business events catalog** page (**System administration \> Setup \> Business events \> Business events catalog**).

As part of the activation process, you will be guided to specify whether the event is specific to one or all legal entities and provide an **Endpoint name**, which must be defined first.

>[!NOTE]
> If **When a Business Event occurs** is set to *Microsoft Power Automate* (rather than *HTTPS*, for example), the **Endpoint name** will automatically be created in Supply Chain Management based on the *Microsoft Power Automate* setup.

## Implementation examples

<!-- KFM: Add general intro -->

### Implement a new queue

<!-- KFM: Introduce what this example is showing. Give a bit more context at the start (e.g., where are we?) -->


1. Create an extension for the enum `SysMessageQueue`.
1. Add a new enum value for the new queue.

    ![Add a new enum value for the new queue.](media/message-processor-implement-queue-enum.png "Add a new enum value for the new queue")

1. Set a feature class on the enum value.

    ![Set a feature class on the enum value.](media/message-processor-implement-queue-class.png "Set a feature class on the enum value")

1. Create a class that extends `SysMessageQueueMetadata` like below:

    ```xpp
    [SysMessageQueueFactory(SysMessageQueue::MyQueue)]
    public final class MyQueue extends SysMessageQueueMetadata
    {
    
        /// <summary>
        /// Gets the default number of message processor tasks.
        /// </summary>
        /// <returns>The default number of message processor tasks.</returns>
        public SysMessageProcessorsTasks getDefaultMessageProcessorsTasks()
        {
            return 1;
        }
    
        /// <summary>
        /// Get the maximun number of messages in a bundle.
        /// </summary>
        /// <returns>The maximun number of messages in a bundle.</returns>
        public SysMessageProcessorMaxBundleSize getMaxBundleSize()
        {
            return 10;
        }
    
        /// <summary>
        /// Determines if messages should be queued when sending or processing messages.
        /// </summary>
        /// <returns>true if messages should be queued when sending a message; otherwise false.</returns>
        public boolean queueMessageOnSend()
        {
            return true;
        }
    
        /// <summary>
        /// Determines if dependent messages should be committed per created transaction ID.
        /// </summary>
        /// <returns>true if dependent messages should be committed per created transaction ID.; otherwise false.</returns>
        public boolean commitPerCreatedTransactionID()
        {
            return false;
        }
    
    }
    ```

1. Create an extension for the class `SysMessageProcessorToggle` to enable message processor in the UI.

    ```xpp
    [ExtensionOf(classStr(SysMessageProcessorToggle))]
    final class MySysMessageProcessorToggle_Extension
    {
        /// <summary>
        /// Determines if the toogle is enabled.
        /// </summary>
        /// <returns>
        /// true if the toogle is enabled; otherwise, false.
        /// </returns>
        internal boolean isEnabled()
        {
            return next isEnabled() || MyFeature::instance().isEnabled();
        }
    
    }
    ```

### Implement a new message type

<!-- KFM: Introduce what this example is showing. Give a bit more context at the start (e.g., where are we?) -->

1. Create an extension for the enum `SysMessageType`.
1. Add a new enum value for the new message type.

    ![Add a new enum value for the new message type.](media/message-processor-implement-message-type-enum.png "Add a new enum value for the new message type")

1. Set a feature class on the enum value.

    ![Set a feature class on the enum value.](media/message-processor-implement-message-type-class.png "Set a feature class on the enum value")

1. Create a class that extends SysMessageTypeMetadata like below:

    ```xpp
    [SysMessageTypeFactory(SysMessageType::MyMessage)]
    public final class MyMessage extends SysMessageTypeMetadata
    {
        /// <summary>
        /// Validates if it is allowed to send a message on a given queue.
        /// </summary>
        /// <param name = "_messageQueue">A queue to send message to.</param>
        /// <param name = "_messageTarget">A message target.</param>
        /// <param name = "_sourceMessageTarget">A message source.</param>
        /// <returns>true if it is allowed to send a message; false, otherwise.</returns>
        public boolean canSendMessage(SysMessageQueue _messageQueue, SysMessageTargetRecId _messageTarget, SysMessageTargetRecId _sourceMessageTarget)
        {
            if (_messageQueue == SysMessageQueue::MyQueue)
            {
                return true;
            }
    
            return false;
        }
    
        /// <summary>
        /// Returns a list of allowed message state transitions.
        /// </summary>
        /// <returns>A list of allowed message state transitions.</returns>
        public Enumerator getAllowedMessageStateTransitions()
        {
            // Allow the following state transitions:
    
            // SysMessageState::None -> SysMessageState::Queued
            // SysMessageState::None -> SysMessageState::Processed
    
            // SysMessageState::Queued -> SysMessageState::Processed
    
            // SysMessageState::Failed -> SysMessageState::Queued
            // SysMessageState::Failed -> SysMessageState::Cancelled
            // SysMessageState::Failed -> SysMessageState::Processed
    
            return SysMessageStateTransition::getProcessedCancelledStateTransitions();
        }
    
        /// <summary>
        /// Returns dependencies related to a message.
        /// </summary>
        /// <param name = "_message">The message to add dependencies to.</param>
        /// <returns>A list of depdendies related to a message.</returns>
        public List getDependencies(SysMessage _message)
        {
            List dependencies = new List(Types::Record);
    
            // Add a dependency to the message if you need to process messages in order. 
            // For example, process a start message before an end message. 
            // The message will not be processed before the dependent message is processed or cancelled.
    
            // The default scheduler, SysMessageKeyDateTimeSequenceProcessorScheduler only sypports one dependency per message.
            // Messages with different keys may be processed in parallel.
    
            SysMessageDependencyKey dependencyKey = strFmt('Mykey:%1', this.getContract(_message).parmMyKey());
            dependencies.addEnd(SysMessageKeyDateTimeSequenceDependency::create(dependencyKey));
    
            return dependencies;
        }
    
        /// <summary>
        /// Execute the logic associated with the given message state transition.
        /// </summary>
        /// <param name = "_message">The message.</param>
        /// <param name = "_messageStateTransition">The state transition to execute.</param>
        public void processMessageStateTransition(SysMessage _message, SysMessageStateTransition _messageStateTransition)
        {
            if (_messageStateTransition.isEqual(SysMessageStateTransition::newFromStates(SysMessageState::Queued, SysMessageState::Processed)))
            {
                this.processMessage(_message);
            }
            else if (_messageStateTransition.isEqual(SysMessageStateTransition::newFromStates(SysMessageState::Failed, SysMessageState::Processed)))
            {
                this.cancelMessage(_message);
            }
        }
    
        private void processMessage(SysMessage _message)
        {
            // Add logic to process the message.
        }
    
        private void cancelMessage(SysMessage _message)
        {
            // Add logic to cancel the message if needed.
        }
    
        private ClassId contractClass()
        {
            return classNum(MyContract);
        }
    
        protected MyContract getContract(SysMessage _message, ClassId _objectTypeId = this.contractClass())
        {
            var contract = FormJsonSerializer::deserializeObject(_objectTypeId, _message.Content) as MyContract;
    
            return contract;
        }
    
    }
    ```

## Microsoft Power Automate example

In this example, use **When a Business Event occurs** with *Microsoft Power Automate* sends emails containing InfoLog messages and hyperlinks to open the **Message processor messages** page for a specific failed message. If needed, you can add extra logic to send the notifications in parallel using different channels and control the recipients based on the event data.

1. In [Power Automate](https://preview.flow.microsoft.com), create a new automated cloud flow for the flow trigger **When a Business Event occurs - Fin & Ops App (Dynamics 365)** followed by the **Parse JSON** and **Send an email** steps, as shown in the following illustration.

    :::image type="content" source="media/message-processor-power-automate-example1.png" alt-text="Power Automate automated cloud flow.":::

1. In the **When a Business Event occurs** step, you can look up or enter the **Instance** following the **Category** and then the **Business event** *Message processor message processed*, as shown in the following illustration.

    :::image type="content" source="media/message-processor-power-automate-example2.png" alt-text="Power Automate When a Business Event occurs step.":::

1. For the **Parse JSON** step, enter a **Schema** that defines the extended fields. You can use the *Download schema* option on the **Business events catalog** page in Supply Chain Management or start by pasting in the example schema text. This example text is provided after the following illustration.

    :::image type="content" source="media/message-processor-power-automate-example3.png" alt-text="Power Automate Parse JSON step.":::

    ```json
    {
        "properties": {
            "BusinessEventId": {
                "type": "string"
            },
            "ControlNumber": {
                "type": "integer"
            },
            "EventId": {
                "type": "string"
            },
            "EventTime": {
                "type": "string"
            },
            "MajorVersion": {
                "type": "integer"
            },
            "MessageContent": {
                "type": "string"
            },
            "MessageDestinationCompanyId": {
                "type": "string"
            },
            "MessageDestinationOperationalSiteId": {
                "type": "string"
            },
            "MessageDestinationWarehouseId": {
                "type": "string"
            },
            "MessageDestinationWorkloadName": {
                "type": "string"
            },
            "MessageInfolog": {
                "type": "string"
            },
            "MessageProcessingResult": {
                "type": "string"
            },
            "MessageProcessingResultLabel": {
                "type": "string"
            },
            "MessageProcessorMessagePageUrl": {
                "type": "string"
            },
            "MessageQueue": {
                "type": "string"
            },
            "MessageQueueLabel": {
                "type": "string"
            },
            "MessageSourceCompanyId": {
                "type": "string"
            },
            "MessageSourceOperationalSiteId": {
                "type": "string"
            },
            "MessageSourceWarehouseId": {
                "type": "string"
            },
            "MessageSourceWorkloadName": {
                "type": "string"
            },
            "MessageState": {
                "type": "string"
            },
            "MessageStateLabel": {
                "type": "string"
            },
            "MessageType": {
                "type": "string"
            },
            "MessageTypeLabel": {
                "type": "string"
            },
            "MinorVersion": {
                "type": "integer"
            }
        },
        "type": "object"
    }
    ```

1. In the **Send an email** step, you can select the individual fields or start by pasting the email body example into the **Body** field. This example is provided after the following illustration.

    :::image type="content" source="media/message-processor-power-automate-example4.png" alt-text="Power Automate send an email step.":::

    ```plaintext
    Message queue: @{body('Parse_JSON')?['MessageQueue']}
    Message queue label: @{body('Parse_JSON')?['MessageQueueLabel']}
    Message type: @{body('Parse_JSON')?['MessageQueueType']}
    Message type label: @{body('Parse_JSON')?['MessageQueueTypeLabel']}
    Message content: @{body('Parse_JSON')?['MessageContent']}
    Message state: @{body('Parse_JSON')?['MessageState']}
    Message state label: @{body('Parse_JSON')?['MessageStateLabel']}
    Message processing result: @{body('Parse_JSON')?['MessageProcessingResult']}
    Message processing result label: @{body('Parse_JSON')?['MessageProcessingResultLabel']}
    Message infolog: @{body('Parse_JSON')?['MessageInfolog']}
    Message source company ID: @{body('Parse_JSON')?['MessageSourceCompanyId']}
    Message source workload name: @{body('Parse_JSON')?['MessageSourceWorkloadName']}
    Message source site ID: @{body('Parse_JSON')?['MessageSourceOperationalSiteId']}
    Message source warehouse ID: @{body('Parse_JSON')?['MessageSourceWarehouseId']}
    Message destination company ID: @{body('Parse_JSON')?['MessageDestinationCompanyId']}
    Message destination workload name: @{body('Parse_JSON')?['MessageDestinationWorkloadName']}
    Message destination site ID: @{body('Parse_JSON')?['MessageDestinationOperationalSiteId']}
    Message destination warehouse ID: @{body('Parse_JSON')?['MessageDestinationWarehouseId']}
    Message processor message page URL: @{body('Parse_JSON')?['MessageProcessorMessagePageUrl']}
    ```

1. When you save the business event, it will automatically be activated and ready to use as part of Supply Chain Management.
