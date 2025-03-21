<!-- default badges list -->
![](https://img.shields.io/endpoint?url=https://codecentral.devexpress.com/api/v1/VersionRange/202749840/24.2.1%2B)
[![](https://img.shields.io/badge/Open_in_DevExpress_Support_Center-FF7200?style=flat-square&logo=DevExpress&logoColor=white)](https://supportcenter.devexpress.com/ticket/details/T828532)
[![](https://img.shields.io/badge/📖_How_to_use_DevExpress_Examples-e9f6fc?style=flat-square)](https://docs.devexpress.com/GeneralInformation/403183)
[![](https://img.shields.io/badge/💬_Leave_Feedback-feecdd?style=flat-square)](#does-this-example-address-your-development-requirementsobjectives)
<!-- default badges end -->
<!-- default file list -->
*Files to look at*:
* [Form1.cs](./CS/XpoCustomAggregate/Form1.cs) (VB: [Form1.vb](./VB/XpoCustomAggregate/Form1.vb))
* [CustomAggregates](./CS/XpoCustomAggregate/CustomAggregates) (VB: [CustomAggregates](./VB/XpoCustomAggregate/CustomAggregates))
* [DataAccess](./CS/XpoCustomAggregate/DataAccess) (VB: [DataAccess](./VB/XpoCustomAggregate/DataAccess))
<!-- default file list end -->

# XPO - How To Implement Custom Aggregates for Collections of Persistent Objects

In addition to predefined [aggregates](https://docs.devexpress.com/CoreLibraries/DevExpress.Data.Filtering.Aggregate) (Sum, Count, Min, Max, Avg, Single, Exists), XPO users can implement [custom aggregate functions](https://docs.devexpress.com/XPO/401333/concepts/custom-aggregate-functions). You can use them to query data with XPQuery and data sources that support CriteriaOperator (including server mode collections).

This example illustrates how to use the CountDistinct and STDEVP custom aggregates with XPView and XPQuery (research the Form1 code behind and designer files).
``` csharp
// Criteria string for a custom aggregate with a detail collection property or Free Joins.
"YourCollection.YourCustomAggregate(YourNestedProperty)"

// Specific criteria string examples for the Orders collection and the CountDistinct and STDEVP custom aggregates.
"[Orders][].CountDistinct([ProductName])", "[Orders][].STDEVP([Price])"

// Criteria string for a custom aggregate with a top-level collection of persistent objects.
"[].CUSTOM_AGGREGATE('YourCustomAggregate', YourNestedProperty)"

// LINQ to XPO usage of custom aggregates (CountDistinct and STDEVP) with a detail collection property.
new XPQuery<Customer>(theSession)
    .Select(t => new {
        ContactName = t.ContactName,
        DistinctProducts = (int)CountDistinctCustomAggregate.CountDistinct(
            t.Orders, o => o.ProductName
        ),
        QuantityVariance = (double?)STDEVPCustomAggregate.STDEVP(
            t.Orders, o => o.Quantity
        ),
        PriceVariance = (double?)STDEVPCustomAggregate.STDEVP(
            t.Orders, o => o.Price
        ),
     }).OrderBy(t => t.ContactName).ToList();
```

To create a custom aggregate, implement the following interfaces: `ICustomAggregate`, `ICustomAggregateQueryable`, `ICustomAggregateFormattable`.
For more information, research the `CountDistinctCustomAggregate` and `STDEVPCustomAggregate` classes in the *XpoCustomAggregate/CustomAggregates* folder.
To register a custom aggregate, use the `CriteriaOperator.RegisterCustomAggregate` method (see the Form1 constructor).

**See Also**
[How to: Implement and Use Custom Aggregate Functions](https://docs.devexpress.com/XPO/401341/examples/how-to-implement-and-use-custom-aggregate-functions)
<!-- feedback -->
## Does this example address your development requirements/objectives?

[<img src="https://www.devexpress.com/support/examples/i/yes-button.svg"/>](https://www.devexpress.com/support/examples/survey.xml?utm_source=github&utm_campaign=XPO_how-to-implement-custom-aggregates&~~~was_helpful=yes) [<img src="https://www.devexpress.com/support/examples/i/no-button.svg"/>](https://www.devexpress.com/support/examples/survey.xml?utm_source=github&utm_campaign=XPO_how-to-implement-custom-aggregates&~~~was_helpful=no)

(you will be redirected to DevExpress.com to submit your response)
<!-- feedback end -->
