---
title: Rule evaluation
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

# Rule evaluation

Rules are defined primarily as data in the `msdyn_exportcontroljurisdiction`, `msdyn_exportcontrolrule`, and `msdyn_exportcontrollicense` tables.

## High level flow

Evaluations are performed per document line. The result is a set of matching restrictions, exceptions, and applicable licenses for each line. If no restrictions were found, then then set of exceptions and licenses will be empty.

No document-level checks are performed, rather the document is considered blocked if there are any restrictions that do not have a matching exception.  The following pseudo-code illustrates how the restrictions, exceptions and licenses are considered.

```
foreach (Line in Request)
{
    Line.MatchingRestrictions = FindAnyMatchingRestrictions(Line);

    foreach (MatchingRestriction in Line.MatchingRestrictions)
    {
        Line.MatchingExceptions = FindAnyMatchingExceptions(Line);

        foreach (MatchingException in Line.MatchingExceptions)
        {
            if (MatchingException.RequireLicense)
            {
                Line.ApplicableLincenses = FindApplicableLicenses(Line);
            }
        }
    }
}
```

## Evaluation of an individual rule

The msdyn_exportcontrolrule table has a variety of fields such as sell to and ship to country, de minimis threshold, and transaction purpose. If a field is not set, then it has no impact on rule evaluation. The values from the line must match *all* rules that have a value set in order for the rule to apply.

Consider a line with the following values:

- Sell To Country = France
- Ship To Country = Mexico
- Transaction Purpose = Return
- De Minimis = 28%

Rule Example 1:

- Sell To Country = (empty)
- Ship To Country = Mexico, Canada
- Transaction Purpose = (emtpy)
- De Minimis Threshold = 25%
- This rule evaluates to *true* because only the ship to country and de minimis are evaluated. Since 28% is greater than the 25% threshold and the ship to country is Mexico, then the rule evaluates to true.

Rule Example 2:

- Sell To Country = Italy
- Ship To Country = Mexico, Canada
- Transaction Purpose = (emtpy)
- De Minimis Threshold = (empty)
- This rule evaluates to *false* because the sell to country of France is not in the list of values provided by the rule.

A rule only applies if the ECCN for the line is one of the codes listed for the rule explicitly, or if the ECCN is part of one of the categories for the rule. A rule marked as *Apply to All Codes* will automatically evaluate to true regardless of what ECCN is included on the line. 

If an exception rule is marked as *Require License*, then it evaluates to true only if one or more licenses are found which match the line.

License evaluation works in the same way. Fields that are left empty are ignored in validation. If a field is not empty, then the item being tested must match one of the values in the field. All fields must evaluate to true in order for the overall evaluation to be true.

## PowerFx Formulas

[PowerFx](/power-platform/power-fx/overview) formulas allow complex rules to be expressed for scenarios that are not simple references to countries or purposes. Similar to the other fields of rules, if a PowerFx formula is not provided then it has no impact on the rule evaluation. The document object being evaluated is passed to the rule, and can be used in formulas for comparison.

Following are various examples of PowerFx formulas for Export Control. For more details, see the [Formula reference](/power-platform/power-fx/formula-reference)

- Documents being shipped to one of three countries
    ```CountRows(Filter(["NOR","SWE","FIN"], Value=Document.SellToCountryRegion)) > 0```
- Documents being shipped to one of three countries containing at least one 6A994 product in the EAR jurisdiction
    ```And(CountRows(Filter(["NOR","SWE","FIN"], Value=Document.SellToCountryRegion)) > 0, CountIf(Document.Lines, CountIf(Codes, And(Jurisdiction="EAR",Code="6A994")) > 0) > 0)```
- Documents after a specific date containing no more than 5 items on any line
    ```And(Document.DocumentDate > Date(2023, 12, 1), CountIf(Document.Lines, Quantity > 5) = 0)```

## Overrides

Export control checks may be overridden at a line code level. This allows overriding a failing export control check for one jurisdiction on one line of a document, while still enforcing checks on other lines. To override the export control check, pass **"msdyn_overridden": true** as a property of one of the line codes in the **msdyn_CheckExportControlLineCodes** collection. The export checks will still be evaluated, and any info or warning messages will still be shown. However, any error messages will be downgraded to warning messages and will not block the activity.
