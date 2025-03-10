---
title: ADDDAYS ER function
description: Learn about how the ADDDAYS Electronic reporting (ER) function is used, including syntax strings, arguments, return values, usage notes, and examples.
author: kfend
ms.author: filatovm
ms.topic: article
ms.date: 12/03/2019
ms.reviewer: kfend
audience: IT Pro
ms.search.region: Global
ms.search.validFrom: 2016-02-28
ms.search.form: ERDataModelDesigner, ERExpressionDesignerFormula, ERMappedFormatDesigner, ERModelMappingDesigner
ms.dyn365.ops.version: AX 7.0.0
ms.assetid: 24223e13-727a-4be6-a22d-4d427f504ac9
---

# ADDDAYS ER function

[!include [banner](../includes/banner.md)]

The `ADDDAYS` function calculates a *DateTime* value that is the specified number of days before or after a specified start date.

## Syntax

```vb
ADDDAYS (datetime, days)
```

## Arguments

`datetime`: *DateTime*

A date/time value that represents the start date.

`days`: *Integer*

The number of days before or after `datetime`.

## Return values

*DateTime*

The resulting date/time value.

## Usage notes

A positive value for `days` yields a future date. A negative value yields a past date.

## Example 1

`ADDDAYS (NOW(), 7)` returns the date and time seven days in the future.

## Example 2

`ADDDAYS (NOW(), -3)` returns the date and time three days in the past.

## Additional resources

[Date and time functions](er-functions-category-datetime.md)


[!INCLUDE[footer-include](../../../includes/footer-banner.md)]
