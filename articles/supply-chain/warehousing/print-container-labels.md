---
title: Print container labels
description: This article describes how to set up and print container labels.
author: perlynne
ms.author: perlynne
ms.reviewer: kamaybac
ms.search.form: WHSContainerLabelRouting, WHSLabelLayout, WHSLabelLayoutDataSource, SysCorpNetPrinterList, WHSDocumentRouting, WHSPackProfile, WHSContainerTable, WHSRFMenuItem
ms.topic: how-to
ms.date: 10/14/2022
ms.custom: bap-template
---

# Print container labels

[!include [banner](../includes/banner.md)]
[!INCLUDE [preview-banner](../includes/preview-banner.md)]
<!-- KFM: Preview until 10.0.31 GA -->

Container labels provide information about a container and the related shipment data. A typical scenario that might involve this type of label is one where a worker is [creating and packing containers by using the Warehouse Management mobile app](warehouse-app-packing-containers.md). The worker can print a container label that includes a bar code of the container ID and apply it to the physical container.

As for [license plate labels](document-routing-layout-for-license-plates.md), the Zebra Programming Language (ZPL) is used to create label layouts for container labels.

## Turn on the container label printing functionality

Container label functionality is part of the *Pack containers using the Warehouse Management mobile app* feature. (For more information, see [Packing containers with the Warehouse Management mobile app](warehouse-app-packing-containers.md).) To use this functionality, the feature must be turned on in your system. Admins can use the [feature management](../../fin-ops-core/fin-ops/get-started/feature-management/feature-management-overview.md) settings to check the status of the feature and turn it on. In the **Feature management** workspace, the feature is listed in the following way:

- **Module:** *Warehouse management*
- **Feature name:** *Pack containers using the Warehouse Management mobile app*

## Example scenario: Print container labels when containers are created by using the Warehouse Management mobile app

This example scenario shows how you can set up your system to print container labels when a worker creates a container by using the Warehouse Management mobile app. This scenario builds on the information that is provided in [Packing containers with the Warehouse Management mobile app](warehouse-app-packing-containers.md). That article provides more details about the full process of packing containers by using the Warehouse Management mobile app.

### Make sample data available

To work through this scenario by using the sample records and values that are specified here, you must be on a system where the standard [demo data](../../fin-ops-core/fin-ops/get-started/demo-data.md) is installed. Additionally, you must select the *USMF* legal entity before you begin.

You can also use this scenario as guidance for using the feature on a production system. However, in that case, you must substitute your own values for each setting that is described here.

### Create a container label layout

The label layout controls what information is printed on the label and how it's laid out. Here, you enter the ZPL code that is sent to the printer. Typically, you will copy this code from a label designer program.

As the system generates a label, it can replace field and method names that are used in the label layout with actual values. You can easily find text that will be replaced by looking for dollar signs (`$`) in the code.

Follow these steps to create a container label layout.

1. Go to **Warehouse management \> Setup \> Document routing \> Label layout**.
1. At the top of the list pane, set the **Label layout type** field to *Container label*.
1. On the Action Pane, select **New** to create a label.
1. Set the following values for the new label:

    - **Label layout ID:** Enter *Container*.
    - **Description:** Enter *Container ID barcode*.
    - **Label layout data source ID:** Leave this field blank. (Only container data will be used.)
    - **Enable label template support:** Leave this option set to *No* for now. (When it's set to *Yes*, you can add header, row, and footer elements to your layout, as described later in this article.)
    - **Date, time, and number format:** Leave this field blank. (This field is used to set the formats of date, time, and number values that are shown in a label layout, as described later in this article.)

1. Copy the following example of a ZPL container label.

    ``` plaintext
    CT~~CD,~CC^~CT~
    ^XA~TA000~JSN^LT0^MNW^MTT^PON^PMN^LH0,0^JMA^PR8,8~SD15^JUS^LRN^CI0^XZ
    ^XA
    ^MMT
    ^PW812
    ^LL0609
    ^LS0
    ^BY3,3,262^FT658,186^BAI,,Y,N
    ^FD$WHSContainerTable.ContainerId$^FS
    ^FT660,457^A0I,39,38^FH\^FDContainer ID^FS
    ^FT660,515^A0I,39,38^FH\^FDShipment: $WHSContainerTable.ShipmentId$^FS
    ^PQ1,0,1,Y^XZ
    ```

1. On the **Printer text Layout** FastTab, paste the copied text into the large field.
1. Close the page.

> [!NOTE]
> While you're customizing the label code on the **Printer text layout** FastTab, you can add valid field and method names by following these steps.
> 
> 1. In the **Tables** list, select the table.
> 1. Depending on the type of item that you want to add, select either the **Fields** tab or the **Methods** tab, and then select the name of the field or method to add.
> 1. Select **Insert at end of text** to add the field or method to the end of the code.
> 1. As you require, move the new field or method to the place in the code where you want to use it.
>
> In the label layout that is shown in the preceding example, only the container ID (`$WHSContainerTable.ContainerId$`) bar code will be printed. If you want to include additional related information (such as the delivery name that is related to a shipment), and the required layout label data source doesn't already exist, follow these steps to create it.
>
> 1. Go to **Warehouse \> Setup \> Document routing \> Label layout data source**.
> 1. Create a label layout data source that includes a join to the `Shipment` table.
> 1. On the Action Pane, select **New**.
> 1. In the new record, set the **Label layout data source ID**, **Description**, and **Label layout type** fields.
> 1. On the Action Pane, select **Save**.
> 1. On the Action Pane, select **Edit query**.
> 1. A standard query editor dialog box appears. On the **Joins** tab, add a join to the required tables.
> 1. Go back to the **Label layout** page, and then, in the **Label layout data source ID** field, select the new record for a new or existing layout.
> 1. You can now add the new field values to the print layout code. Be sure to reference the correct *table.field-names* values in the ZPL code. The additional tables will include a number (*\_\#*) as a suffix.
>
> On the **Label layout data source** page, be careful about removing a table from the query for an existing record. You might remove field and/or method names that are already used in existing label layouts.

#### Enable label template support

If you must create more advanced label layouts, you can benefit from using some of the widely available label generation tools that are described in [Document routing label layout](document-routing-layout-for-license-plates.md).

To format a label by using header, row, and footer elements, open the **Label layout** page, select or create a layout, and set the **Enable label template support** option to *Yes* for the new or selected layout. Then use the `{{Header ... }}`, `{{Row ... }}`, and `{{Footer ... }}` elements in your code. The following example shows a label that includes all these elements. It prints data about items that are packed in a container.

``` plaintext
{{Header
^FX ... ZPL commands that will be printed on every label ...
CT~~CD,~CC^~CT~
^XA
~TA000
~JSN
^LT0
^MNW
^MTT
^PON
^PMN
^LH0,0
^JMA
^PR6,6
~SD15
^JUS
^LRN
^CI27
^PA0,1,1,0
^XZ
^XA
^MMT
^PW1276
^LL900
^LS0
^FT80,150^A0N,58,58^FH\^CI28^FDShipment: $WHSContainerTable.ShipmentId$^FS^CI27
^FT80,250^A0N,33,33^FH\^CI28^FDItem^FS^CI27
^FT579,250^A0N,33,33^FH\^CI28^FDQuantity^FS^CI27
^FT720,250^A0N,33,33^FH\^CI28^FDUnit^FS^CI27
}}
^FX ... This section goes on the first label only...
^FT85,51^A0N,17,18^FH\^CI28^FDFIRST CONTAINER LABEL^FS^CI27
{{Row Table=WHSContainerLine_1 RowsPerLabel=10 StartY=300 IncY=50
^FX... ZPL commands to format the row using *$position.YPos$* to position the location of the text fields ...
^FT80,$position.YPos$^A0N,33,33^FH\^CI28^FD$WHSContainerLine_1.ItemId$^FS^CI27
^FT579,$position.YPos$^A0N,33,33^FH\^CI28^FD$WHSContainerLine_1.Qty$^FS^CI27
^FT720,$position.YPos$^A0N,33,33^FH\^CI28^FD$WHSContainerLine_1.UnitId$^FS^CI27
}}
^FX ... This part goes on the last label only ... 
^FT100,750^A0N,17,18^FH\^CI28^FDLast container label^FS^CI27
{{Footer
^FX ... ZPL commands that close every label ...
^FT600,750^A0N,58,58^FH\^CI28^FDLabel: $position.labelNumber$/$position.labelCount$^FS^CI27
^XZ
}}
```

> [!NOTE]
> Because of the `RowsPerLabel=10` attribute, this setup will loop over container lines and split out a label for each set of 10 container lines. If you change the attribute to `RowsPerLabel=1`, a label will be generated for each line.
>
> This setup will print one copy of each label. If you require more copies (for example, one copy for each side of the container), set the `n` value for the `\^PQn` section in the footer to the required number of copies. For example, to print two copies of each label, specify `\^PQ2`. <!--KFM: @per Let's add a `\^PQn` section in the sample code so we can see where it goes. -->

#### Specify date, time, and number formats

To set the formats of date, time, and number values that are shown in a label layout, open the **Label layout** page, select or create a layout, and set the **Date, time, and number format** field to the language that uses the formats that you want.

### Set up container label routing

To specify the container label layouts that are used and where they are printed, you must define a **Container label routing** record, as described in the following procedure.

1. Go to **Warehouse management \> Setup \> Document routing \> Container label routing**.
1. On the Action Pane, select **New** to create a routing record.
1. On the header of the new routing record, set the following fields:

    - **Sequence number** – Enter an integer to define the order that the routing record should be evaluated in. Each routing must have a unique sequence number. The system evaluates routings in order of ascending sequence numbers and uses the first routing that criteria are met for. If you're using demo data, enter *1*.
    - **Name** – Enter a name for the routing record. For example, enter *Container packing*.

1. On the **Overview** FastTab, use the following fields to define the criteria that are used to select the label routing:

    - **Warehouse** – Specify the warehouse where the routing should be used. If you're using demo data, enter *62*.
    - **Location** – Specify the location where the routing should be used. If you're using demo data, select *Pack*, based on the assumption that the target printer is physically placed at the packing location.
    - **Worker** – Specify the worker that the routing should be used for. To use the routing for any worker, leave this blank.
    - **Mobile device user ID** – Specify the user ID that the routing should be used for. To use the routing for any worker, leave this blank.
    - **Container type** – Specify the container type that the routing should be used for.
    - **Account number** – Specify the customer account that the routing should be used for. To use the routing for any customer, leave this blank.
    - **Carrier** – Specify the shipping carrier that the routing should be used for. To use the routing for any carrier, leave this blank.
    - **Run query** – To add custom selection criteria to a routing record, set this option to *Yes*, and then select **Edit query** on the Action Pane. A standard query editor dialog box appears, where you can add more selection criteria.

    > [!NOTE]
    > When you print a container label from the Warehouse Management mobile app, the current user's warehouse, location, worker ID, and user ID are passed as possible filter values for selecting the printer and layout. Other values will be found based on the selected shipment.

1. On the **Container label routing printer** FastTab, assign the printer and label layout that should be used when the criteria for the routing record are met. Select **New** on the toolbar to add a line to the grid. Then set the following fields for the new line:

    - **Name** – Select an appropriate ZPL printer. For more information, see [Install the Document Routing Agent to enable network printing](../../fin-ops-core/dev-itpro/analytics/install-document-routing-agent.md).
    - **Label layout ID** – Select the label layout to use. The example label layout ID value that was suggested earlier in this scenario was *Container*.

### Set container labels to be printed automatically when new containers are created

If you want a container label to be printed automatically each time that a new container is created, configure each packing profile as described in the following procedure.

1. Go to **Warehouse management \> Setup \> Packing \> Packing profiles**.
1. On the Action Pane, select **Edit**.
1. Select the profile that container labels should automatically be printed for. If you're working with sample data, select the row where the **Packing profile ID** field is set to *WHS62*.
1. Select the **Print container label at container creation** checkbox for the selected row.
1. Close the page.

> [!NOTE]
> The **Container ID mode** field for packing profile *WH62* is set to *Auto*. Therefore, the [number sequence](../../fin-ops-core/fin-ops/organization-administration/number-sequence-overview.md) that is defined for the *Container ID* reference will be used as part of the container creation process.

### Create a new mobile device menu item for printing container labels

To enable workers to print container labels manually, you must create a new mobile device menu item for the Warehouse Management mobile app.

1. Go to **Warehouse management \> Setup \> Mobile device \> Mobile device menu items**.
1. On the Action Pane, select **New** to add a mobile device menu item.
1. For the new menu item, set the following fields:

    - **Menu item name** – Enter an internal name for the new menu item. For example, enter *Print container label*.
    - **Title** – Enter the item name as it should appear in the Warehouse Management mobile app. For example, enter *Print container label*.
    - **Mode** – Select *Indirect*.
    - **Activity code** – Select *Print container label*.

1. Close the page.

### Add the new mobile device menu item to the menu

Now that you've created the mobile device menu item, you can add it to the mobile device menu. In this example, you will add it to the existing **Outbound** mobile device menu.

1. Go to **Warehouse management \> Setup \> Mobile device \> Mobile device menu**.
1. On the Action Pane, select **Edit**.
1. In the list pane, select the **Outbound** menu.
1. In the **Available menus and menu items** column, select the mobile device menu item that you created (for example, *Print container label*).
1. Select the **Add** button (right arrow) to move the menu item into the **Menu structure** column.
1. Close the page.

### Run a scenario to print container labels

For an example that shows how to print bar codes manually, see [Packing containers with the Warehouse Management mobile app](warehouse-app-packing-containers.md). Follow the instructions there, and confirm that the scenario that is described in this article is also supported.

Here are a few suggestions for ways that you can customize and fine-tune this scenario to help reduce the number of steps that workers must perform when they print container labels.

- You can [query data by using Warehouse Management mobile app detours](warehouse-app-data-inquiry.md) to look up a container ID instead of prompting the worker to enter it manually.
- When a worker selects the **Print container label** mobile device menu item on the **Outbound** menu, the app automatically submits the current **User ID** and **Warehouse** values. If workers want to specify a **Location** value, they can do so in the app.
- If you want the **Location** value to be assigned automatically when a worker selects **Print container label** from the **Pack inventory into containers** menu item, you can set up a [detour]( warehouse-app-detours.md).

## Additional resources

- [Pack containers for shipment](packing-containers.md)

[!INCLUDE[footer-include](../../includes/footer-banner.md)]
