# Filtering in ASP.NET MVC Pivot Table

## Table of Contents
- [Overview](#overview)
- [Member Filtering](#member-filtering)
- [Label Filtering](#label-filtering)
- [Value Filtering](#value-filtering)
- [Typical Use Cases](#typical-use-cases)
- [Best Practices](#best-practices)

## Overview

The Pivot Table supports three types of filtering to help you focus on specific data:

1. **Member Filtering** - Show/hide specific dimension members (e.g., USA, UK, Canada)
2. **Label Filtering** - Filter members based on text patterns in their names
3. **Value Filtering** - Filter by aggregated values meeting certain conditions

By default, `AllowMemberFilter` is **true**, allowing users to toggle members on/off. To enable label and value filtering, you must explicitly set `AllowLabelFilter` and `AllowValueFilter` to **true**.

## Member Filtering

Member filtering allows you to include or exclude specific dimension members from the Pivot Table. This is the most common filtering approach.

**Include Only Specific Members:**

```html
@using Syncfusion.EJ2.PivotView

@Html.EJS().PivotView("pivotview").DataSourceSettings(ds => ds
        .DataSource((IEnumerable<object>)ViewBag.DataSource)
        .Rows(rows => {
            rows.Name("Country").Add();
        })
        .Columns(columns => {
            columns.Name("Year").Add();
        })
        .Values(values => {
            values.Name("Sales").Type(Syncfusion.EJ2.PivotView.SummaryTypes.Sum).Add();
        })
        .FilterSettings(filters => {
            // Include only USA and UK
            filters.Name("Country").Type(Syncfusion.EJ2.PivotView.FilterType.Include).Items(new[] { "USA", "UK" }).Add();
        })).Height("450").Width("100%").Render()
```

**Exclude Specific Members:**

```html
@Html.EJS().PivotView("pivotview").DataSourceSettings(ds => ds
        .DataSource((IEnumerable<object>)ViewBag.DataSource)
        .Rows(rows => {
            rows.Name("Country").Add();
        })
        .Columns(columns => {
            columns.Name("Year").Add();
        })
        .Values(values => {
            values.Name("Sales").Type(Syncfusion.EJ2.PivotView.SummaryTypes.Sum).Add();
        })
        .FilterSettings(filters => {
            // Exclude Canada and Mexico
            filters.Name("Country").Type(Syncfusion.EJ2.PivotView.FilterType.Exclude).Items(new[] { "Canada", "Mexico" }).Add();
        })).Height("450").Width("100%").Render()
```

**Enable Member Filter UI:**

```html
@Html.EJS().PivotView("pivotview").DataSourceSettings(ds => ds
        .DataSource((IEnumerable<object>)ViewBag.DataSource)
        .AllowMemberFilter(true)
        .Rows(rows => {
            rows.Name("Country").Add();
        })
        .Columns(columns => {
            columns.Name("Year").Add();
        })
        .Values(values => {
            values.Name("Sales").Type(Syncfusion.EJ2.PivotView.SummaryTypes.Sum).Add();
        })).ShowFieldList(true).Height("450").Width("100%").Render()
```

With the Field List displayed, users can click the filter icon next to any field to open the member editor and select/deselect members interactively.

## Label Filtering

Label filtering allows you to display only data with specific text patterns in the member names. This works with string, number, and date data types.

**Important:** To enable label filtering, set `AllowLabelFilter(true)` in the DataSourceSettings.

### String Data Type

**Contains Pattern:**

```html
@Html.EJS().PivotView("pivotview").DataSourceSettings(ds => ds
        .DataSource((IEnumerable<object>)ViewBag.DataSource)
        .AllowLabelFilter(true)
        .Rows(rows => {
            rows.Name("Product").Add();
        })
        .Columns(columns => {
            columns.Name("Year").Add();
        })
        .Values(values => {
            values.Name("Sales").Type(Syncfusion.EJ2.PivotView.SummaryTypes.Sum).Add();
        })
        .FilterSettings(filters => {
            // Show only products containing "Bike"
            filters.Name("Product")
                .Type(Syncfusion.EJ2.PivotView.FilterType.Label)
                .Condition(Syncfusion.EJ2.PivotView.Operators.Contains)
                .Value1("Bike")
                .Add();
        })).Height("450").Width("100%").Render()
```

**Begins With Pattern:**

```html
.FilterSettings(filters => {
    // Show only products beginning with "M"
    filters.Name("Product")
        .Type(Syncfusion.EJ2.PivotView.FilterType.Label)
        .Condition(Syncfusion.EJ2.PivotView.Operators.BeginWith)
        .Value1("M")
        .Add();
})
```

**Ends With Pattern:**

```html
.FilterSettings(filters => {
    // Show only products ending with "Wheel"
    filters.Name("Product")
        .Type(Syncfusion.EJ2.PivotView.FilterType.Label)
        .Condition(Syncfusion.EJ2.PivotView.Operators.EndsWith)
        .Value1("Wheel")
        .Add();
})
```

**Other String Operators:**

```html
// Equals (exact match)
.Condition(Syncfusion.EJ2.PivotView.Operators.Equals).Value1("Exact Product Name")

// DoesNotEquals
.Condition(Syncfusion.EJ2.PivotView.Operators.DoesNotEquals).Value1("Unwanted Product")

// DoesNotContains
.Condition(Syncfusion.EJ2.PivotView.Operators.DoesNotContains).Value1("Old")

// Between (alphabetically)
.Condition(Syncfusion.EJ2.PivotView.Operators.Between).Value1("A").Value2("M")

// GreaterThan (alphabetically)
.Condition(Syncfusion.EJ2.PivotView.Operators.GreaterThan).Value1("P")
```

### Number Data Type

For numeric text values in your labels, use `FilterType.Number`:

```html
@Html.EJS().PivotView("pivotview").DataSourceSettings(ds => ds
        .DataSource((IEnumerable<object>)ViewBag.DataSource)
        .AllowLabelFilter(true)
        .Rows(rows => {
            rows.Name("ProductCode").Add();
        })
        .Columns(columns => {
            columns.Name("Year").Add();
        })
        .Values(values => {
            values.Name("Sales").Type(Syncfusion.EJ2.PivotView.SummaryTypes.Sum).Add();
        })
        .FilterSettings(filters => {
            // Show only products with codes less than 250
            filters.Name("ProductCode")
                .Type(Syncfusion.EJ2.PivotView.FilterType.Number)
                .Condition(Syncfusion.EJ2.PivotView.Operators.LessThan)
                .Value1("250")
                .Add();
        })).Height("450").Width("100%").Render()
```

**Supported Number Operators:**
- `Syncfusion.EJ2.PivotView.Operators.Equals`, `Syncfusion.EJ2.PivotView.Operators.DoesNotEquals`
- `Syncfusion.EJ2.PivotView.Operators.GreaterThan`, `Syncfusion.EJ2.PivotView.Operators.GreaterThanOrEqualTo`
- `Syncfusion.EJ2.PivotView.Operators.LessThan`, `Syncfusion.EJ2.PivotView.Operators.LessThanOrEqualTo`
- `Syncfusion.EJ2.PivotView.Operators.Between`, `Syncfusion.EJ2.PivotView.Operators.NotBetween`

### Date Data Type

For date values in your labels, use `FilterType.Date`:

```html
@Html.EJS().PivotView("pivotview").DataSourceSettings(ds => ds
        .DataSource((IEnumerable<object>)ViewBag.DataSource)
        .AllowLabelFilter(true)
        .Rows(rows => {
            rows.Name("OrderDate").Add();
        })
        .Columns(columns => {
            columns.Name("Year").Add();
        })
        .Values(values => {
            values.Name("Sales").Type(Syncfusion.EJ2.PivotView.SummaryTypes.Sum).Add();
        })
        .FilterSettings(filters => {
            // Show only orders before a specific date
            filters.Name("OrderDate")
                .Type(Syncfusion.EJ2.PivotView.FilterType.Date)
                .Condition(Syncfusion.EJ2.PivotView.Operators.Before)
                .Value1("2019-01-07")
                .Add();
        })).Height("450").Width("100%").Render()
```

**Supported Date Operators:**
- `Syncfusion.EJ2.PivotView.Operators.Equals`, `Syncfusion.EJ2.PivotView.Operators.DoesNotEquals`
- `Syncfusion.EJ2.PivotView.Operators.Before`, `Syncfusion.EJ2.PivotView.Operators.BeforeOrEqualTo`
- `Syncfusion.EJ2.PivotView.Operators.After`, `Syncfusion.EJ2.PivotView.Operators.AfterOrEqualTo`
- `Syncfusion.EJ2.PivotView.Operators.Between`, `Syncfusion.EJ2.PivotView.Operators.NotBetween`

## Value Filtering

Value filtering displays only members with aggregated values meeting specific conditions. This is useful for focusing on high-performing or low-performing items.

**Important:** To enable value filtering, set `AllowValueFilter(true)` in the DataSourceSettings.

**Greater Than Threshold:**

```html
@Html.EJS().PivotView("pivotview").DataSourceSettings(ds => ds
        .DataSource((IEnumerable<object>)ViewBag.DataSource)
        .AllowValueFilter(true)
        .Rows(rows => {
            rows.Name("Country").Add();
        })
        .Columns(columns => {
            columns.Name("Year").Add();
        })
        .Values(values => {
            values.Name("Sales").Type(Syncfusion.EJ2.PivotView.SummaryTypes.Sum).Add();
        })
        .FilterSettings(filters => {
            // Show only countries with sales > 5000
            filters.Name("Country")
                .Type(Syncfusion.EJ2.PivotView.FilterType.Value)
                .Measure("Sales")
                .Condition(Syncfusion.EJ2.PivotView.Operators.GreaterThan)
                .Value1("5000")
                .Add();
        })).Height("450").Width("100%").Render()
```

**Between Range:**

```html
.FilterSettings(filters => {
    // Show only sales between 1000 and 10000
    filters.Name("Country")
        .Type(Syncfusion.EJ2.PivotView.FilterType.Value)
        .Measure("Sales")
        .Condition(Syncfusion.EJ2.PivotView.Operators.Between)
        .Value1(1000)
        .Value2(10000)
        .Add();
})
```

**Less Than or Equal:**

```html
.FilterSettings(filters => {
    // Show only sales <= 5000
    filters.Name("Country")
        .Type(Syncfusion.EJ2.PivotView.FilterType.Value)
        .Measure("Sales")
        .Condition(Syncfusion.EJ2.PivotView.Operators.LessThanOrEqualTo)
        .Value1(5000)
        .Add();
})
```

## Typical Use Cases

**Multiple Filters on Different Fields:**

```html
@Html.EJS().PivotView("pivotview").DataSourceSettings(ds => ds
        .DataSource((IEnumerable<object>)ViewBag.DataSource)
        .AllowMemberFilter(true)
        .AllowLabelFilter(true)
        .AllowValueFilter(true)
        .Rows(rows => {
            rows.Name("Country").Add();
            rows.Name("Product").Add();
        })
        .Columns(columns => {
            columns.Name("Year").Add();
        })
        .Values(values => {
            values.Name("Sales").Type(Syncfusion.EJ2.PivotView.SummaryTypes.Sum).Add();
        })
        .FilterSettings(filters => {
            // Member filter: Include only USA and UK
            filters.Name("Country")
                .Type(Syncfusion.EJ2.PivotView.FilterType.Include)
                .Items(new[] { "USA", "UK" })
                .Add();
            
            // Label filter: Products containing "Bike"
            filters.Name("Product")
                .Type(Syncfusion.EJ2.PivotView.FilterType.Label)
                .Condition(Syncfusion.EJ2.PivotView.Operators.Contains)
                .Value1("Bike")
                .Add();
            
            // Value filter: Sales > 2000
            filters.Name("Sales")
                .Type(Syncfusion.EJ2.PivotView.FilterType.Value)
                .Measure("Sales")
                .Condition(Syncfusion.EJ2.PivotView.Operators.GreaterThan)
                .Value1("2000")
                .Add();
        })).Height("450").Width("100%").Render()
```

## Best Practices

- **Enable Filters Explicitly:** Always set `AllowLabelFilter(true)` and `AllowValueFilter(true)` when needed
- **Use Include for Specific Items:** More efficient than Exclude when filtering to few items
- **Use Exclude for General Filters:** Better for "hide unwanted" scenarios
- **Label Filters for Text Patterns:** Use Contains/BeginsWith for product names, descriptions
- **Value Filters for Aggregates:** Use for sales thresholds, quantity ranges, performance metrics
- **Performance Consideration:** Member filters are fastest; value filters on large datasets may be slower
- **Case-Sensitivity:** Field names are case-sensitive and must match exactly
- **Field Names:** Use the actual data field name (as defined in DataSource), not the caption
- **Combine Strategically:** Member + Label + Value filters work together for complex analysis
- **Validate Conditions:** Use appropriate `Operators` enum values for each data type (string, number, date)
- **Always Set AllowMemberFilter:** Explicitly set if you want users to toggle members via UI
