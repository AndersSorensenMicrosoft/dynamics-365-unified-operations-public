---
title: Export control licenses
description:
author: t-benebo
ms.author: benebotg
ms.reviewer: kamaybac
ms.search.form:
ms.topic: overview
ms.date: 07/31/2023
audience: Application User
ms.search.region: Global
ms.custom: bap-template
---

<!-- KFM: Is this information really intended for users? Seems like internal developer documentation. What can a user do with this info? -->

# Export control licenses

License information is stored in the `msdyn_exportcontrollicense` table. Tracking of licenses is provided only as it pertains to export control, and this should not be considered a general purpose license management subsystem.

## License evaluation

All license checks occur at the document line level. A single document may contain lines which reference multiple licenses. When evaluating a license, the same evaluation is performed as with restriction and exception rules. Any fields for which a value is provided in the license definition are considered, while fields without a value are ignored. As with rules, the value must match the line being processed for the license to be considered valid.

The document date is compared against the licenses valid from and valid to dates, if provided. In order to perform the check based on the current date instead of the document date, either use a PowerFx rule, or pass the current date in the document being analyzed. The **expected export date** field of the license has no impact and is provided for reference. However, it can be referenced from PowerFx rules.

A license may be provided either at the document level or at the line level. It is assumed that a document-level license is defaulted to all lines, and is overridden if a specific line specifies a license.

## License lines and consumption

The ECCN from the license line must match the ECCN of the document line referencing the license. If no line with a matching ECCN is found in the license, then the check will fail and the **license issues** collection will contain a reference to the missing license.

There are three properties on the export control check call which control how license consumption will be handled.

- "msdyn_ExportControlSourceApplication": "Supply Chain"
- "msdyn_ExportControlSourceDocument": "SO1347134"
- "msdyn_DecrementLicenseQuantity": true

Consumption is tracked in the `msdyn_exportcontrollicenselineconsumption` table. The **SourceApplication** and **SourceDocument** fields are used to reference which incoming document from an external system has consumed the quantity or value. Specifying true for **msdyn_DecrementLicenseQuantity** will cause rows to be written in the `msdyn_exportcontrollicenselineconsumption` table and will block future license checks if no quantity or value remains for that line. If the same `SourceApplication` and `SourceDocument` are passed in again with `DecrementLicenseQuantity`, all existing decrements for that document are deleted, and then re-created with the updated quantities and values. This means if a sales order is updated and a different quantity needs to be reserved (higher or lower), the sales order can just be sent in a second time and it will update all the existing values.

The consumed quantity and value are calculated as rollup fields on the license line. If no total quantity or value is provided, it won't block export control checks for that license line, but will continue to accrue total consumed quantity and total consumed value. The line value is a standard Dataverse currency field. See [Using currency columns](/power-apps/maker/data-platform/types-of-fields#using-currency-columns) for more details on currency fields and exchange rates.

License consumption is automatically computed and updated based on checks with when Decrement License Quantity is set to true. However, users with the appropriate permissions can also create, edit, or delete consumption lines directly from the `msdyn_exportcontrollicenselineconsumption` table if required. Access to viewing and editing this and all other tables follow the standard [Dataverse role-based security](/power-platform/admin/wp-security-cds).
