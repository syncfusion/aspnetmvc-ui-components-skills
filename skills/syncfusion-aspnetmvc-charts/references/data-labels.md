# Data Labels

## Table of Contents
- [Overview](#overview)
- [Enabling Data Labels](#enabling-data-labels)
- [Label Positioning](#label-positioning)
  - [Position by Chart Type](#position-by-chart-type)
- [Label Formatting](#label-formatting)
  - [Basic Format](#basic-format)
  - [Number Formatting](#number-formatting)
  - [Date Formatting](#date-formatting)
- [Label Templates](#label-templates)
  - [Complex Templates](#complex-templates)
  - [Template with Icons](#template-with-icons)
- [Smart Labels](#smart-labels)
  - [Trim](#trim)
  - [Rotation](#rotation)
  - [Auto Arrange](#auto-arrange)
- [Label Customization](#label-customization)
  - [Font and Color](#font-and-color)
  - [Background and Border](#background-and-border)
  - [Margin and Padding](#margin-and-padding)
  - [Alignment](#alignment)
- [Connector Lines](#connector-lines)
- [Advanced Label Features](#advanced-label-features)
  - [Conditional Label Visibility](#conditional-label-visibility)
  - [Dynamic Label Colors](#dynamic-label-colors)
  - [Label with Custom Calculations](#label-with-custom-calculations)
- [Common Patterns](#common-patterns)
  - [Column Chart with Outside Labels](#column-chart-with-outside-labels)
  - [Line Chart with Formatted Labels](#line-chart-with-formatted-labels)
  - [Stacked Chart with Percentage Labels](#stacked-chart-with-percentage-labels)
- [Performance Considerations](#performance-considerations)
  - [Large Datasets](#large-datasets)
- [Troubleshooting](#troubleshooting)
  - [Labels not showing](#labels-not-showing)
  - [Labels overlapping](#labels-overlapping)
  - [Template not rendering](#template-not-rendering)
  - [Formatting not working](#formatting-not-working)
- [Best Practices](#best-practices)
- [Related Topics](#related-topics)
- [API Reference](#api-reference)

## Overview

Data labels display values at data points, making charts more informative and easier to read. They automatically arrange to avoid overlapping and support extensive customization.

## Enabling Data Labels

Data labels are disabled by default. Enable them within the marker configuration.

```cshtml
@Html.EJS().Chart("labelChart").PrimaryXAxis(px => px.ValueType(Syncfusion.EJ2.Charts.ValueType.Category)).Series(series =>
    {
        series.Type(Syncfusion.EJ2.Charts.ChartSeriesType.Column)
              .XName("Month")
              .YName("Sales")
              .Marker(marker => marker
                  .DataLabel(label => label.Visible(true)))
              .DataSource(ViewBag.Data)
              .Add();
    }).Render()
```

## Label Positioning

Control where labels appear relative to data points.

```cshtml
.Marker(marker => marker
    .DataLabel(label => label
        .Visible(true)
        .Position(Syncfusion.EJ2.Charts.LabelPosition.Top)))
```

**Position options:**
- `Top` - Above data point
- `Bottom` - Below data point
- `Middle` - Center of data point
- `Outer` - Outside column/bar (for column/bar charts)
- `Auto` - Automatic best position (default)

### Position by Chart Type

**Column/Bar charts:**
```cshtml
// Inside columns
.Position(Syncfusion.EJ2.Charts.LabelPosition.Middle)

// On top of columns
.Position(Syncfusion.EJ2.Charts.LabelPosition.Top)

// Outside columns
.Position(Syncfusion.EJ2.Charts.LabelPosition.Outer)
```

**Line/Area charts:**
```cshtml
// Above points
.Position(Syncfusion.EJ2.Charts.LabelPosition.Top)

// Below points
.Position(Syncfusion.EJ2.Charts.LabelPosition.Bottom)
```

## Label Formatting

Format label text using format strings.

### Basic Format

```cshtml
.Marker(marker => marker
    .DataLabel(label => label
        .Visible(true)
        .Format("${point.y}K")))  // $35K
```

**Format placeholders:**
- `{point.x}` - X-axis value
- `{point.y}` - Y-axis value
- `{series.name}` - Series name
- `{point.percentage}` - Percentage (for stacked/accumulation charts)

### Number Formatting

```cshtml
// Currency
.Format("${point.y}")  // $35

// Percentage
.Format("{point.y}%")  // 35%

// Thousands
.Format("{point.y}K")  // 35K

// Millions
.Format("{point.y}M")  // 1.2M

// Decimal places
.Format("n2")  // 35.00

// Custom
.Format("Sales: ${point.y}")  // Sales: $35
```

### Date Formatting

For DateTime axes:

```cshtml
.Marker(marker => marker
    .DataLabel(label => label
        .Visible(true)
        .Format("MMM dd")))  // Mar 17
```

**Date format options:**
- `"dd/MM/yyyy"` - 17/03/2026
- `"MMM dd, yyyy"` - Mar 17, 2026
- `"yyyy-MM-dd"` - 2026-03-17
- `"MMMM yyyy"` - March 2026

## Label Templates

Create custom label HTML using templates.

```cshtml
<div id="ControlRegion">
                  @Html.EJS().Chart("container").Series(series =>
                     {
                     series.Type(Syncfusion.EJ2.Charts.ChartSeriesType.Column).
                     Marker(mr=> mr.Visible(false).DataLabel(dl=>dl.Visible(true).Position(Syncfusion.EJ2.Charts.LabelPosition.Middle).Template("#template"))).
                     XName("x").
                     YName("y").
                     DataSource(ViewBag.dataSource).
                     Name("Gold").
                     Width(2).Add();
                     }).PrimaryXAxis(px => px.Interval(1).ValueType(Syncfusion.EJ2.Charts.ValueType.Category)).Title("Olympic Medal Counts - RIO").Render()
<script id="template">
    <div style="background:#f5f5f5; border: 1px solid black; padding: 3px 3px 3px 3px">
        <div>${point.x}</div>
        <div>${point.y}</div>
    </div>
</script>
```

### Complex Templates

```cshtml

<script id="dataLabelTemplate" type="text/x-template">
    <div style="text-align:center;">
        <div style="font-weight:bold; font-size:14px;">${point.y}</div>
        <div style="font-size:10px; color:#666;">${point.x}</div>
    </div>
</script>

```
```cshtml
.DataLabel(label => label
    .Visible(true)
    .Template("#dataLabelTemplate")
)

```

### Template with Icons

```cshtml
<script id="percentDataLabelTemplate" type="text/x-template">
    <div>
        <img src="/images/trend-up.pngspan>${point.y}%</span>
    </div>
</script>
```

```cshtml
.DataLabel(label => label
    .Visible(true)
    .Template("#percentDataLabelTemplate")
)
```


## Smart Labels

Automatic label arrangement to prevent overlapping.

### Auto Arrange

Let chart automatically arrange labels:

```cshtml
.Marker(marker => marker
    .DataLabel(label => label
        .Visible(true)
        .LabelIntersectAction(Syncfusion.EJ2.Charts.LabelIntersectAction.Hide)))
```

**LabelIntersectAction options:**
- `None` - No action (may overlap)
- `Hide` - Hide overlapping labels
- `Trim` - Trim labels
- `Wrap` - Wrap to multiple lines
- `Rotate` - Auto-rotate labels

## Label Customization

### Font and Color

```cshtml
.Marker(marker => marker
    .DataLabel(label => label
        .Visible(true)
        .Font(font => font
            .FontFamily("Arial")
            .Size("12px")
            .FontWeight("600")
            .Color("#FFFFFF"))))
```

### Background and Border

```cshtml
.Marker(marker => marker
    .DataLabel(label => label
        .Visible(true)
        .Fill("rgba(30, 136, 229, 0.8)")
        .Border(border => border
            .Width(2)
            .Color("#1E88E5"))
        .Rx(5)  // Rounded corners
        .Ry(5)))
```

### Margin and Padding

```cshtml
.Marker(marker => marker
    .DataLabel(label => label
        .Visible(true)
        .Margin(margin => margin
            .Left(5)
            .Right(5)
            .Top(5)
            .Bottom(5))))
```

### Alignment

```cshtml
.Marker(marker => marker
    .DataLabel(label => label
        .Visible(true)
        .Alignment(Syncfusion.EJ2.Charts.Alignment.Near)))  // Near, Center, Far
```

## Advanced Label Features

### Conditional Label Visibility

Show labels only for specific conditions:

```cshtml
<script>
    function onTextRender(args) {
        if (args.point.y < 20) {
            args.cancel = true;  // Hide labels below 20
        }
    }
</script>

@Html.EJS().Chart("conditionalLabels")
    .Series(series =>
    {
        series.DataSource(ViewBag.Data)
              .Marker(marker => marker
                  .DataLabel(label => label.Visible(true)))
              .Add();
    })
    .TextRender("onTextRender")
    .Render()
```

### Dynamic Label Colors

Color labels based on values:

```cshtml
<script>
    function onTextRender(args) {
        if (args.point.y > 50) {
            args.color = '#00FF00';  // Green for high values
        } else if (args.point.y < 30) {
            args.color = '#FF0000';  // Red for low values
        } else {
            args.color = '#FFA500';  // Orange for medium
        }
    }
</script>


@Html.EJS().Chart("conditionalLabels").Height("400px").PrimaryXAxis(px => px
        .ValueType(Syncfusion.EJ2.Charts.ValueType.Category)
        .Interval(1)
        .Title("Month")
    ).PrimaryYAxis(py => py
        .Title("Sales")
    ).Series(series =>
    {
        series.Type(Syncfusion.EJ2.Charts.ChartSeriesType.Column)
              .XName("Month")
              .YName("Sales")
              .Marker(marker => marker
                  .Visible(true)
                  .DataLabel(label => label.Visible(true))
              )
              .DataSource(ViewBag.ChartData)
              .Add();
    }).TextRender("onTextRender").Render()

```

### Label with Custom Calculations

```cshtml
<script>
    function onTextRender(args) {
        var value = args.point.y;
        var change = value > 50 ? "↑" : "↓";
        args.text = change + " " + value;
    }
</script>


@Html.EJS().Chart("calculatedLabels")
    .Height("400px")
    .PrimaryXAxis(px => px
        .ValueType(Syncfusion.EJ2.Charts.ValueType.Category)
        .Title("Month")
    )
    .PrimaryYAxis(py => py
        .Title("Sales")
    )
    .Series(series =>
    {
        series.Type(Syncfusion.EJ2.Charts.ChartSeriesType.Column)
              .XName("Month")
              .YName("Sales")
              .DataSource(ViewBag.ChartData)
              .Marker(marker => marker
                  .Visible(true)
                  .DataLabel(label => label.Visible(true))
              )
              .Add();
    }).DataLabelRender("onDataLabelRender").Render()

```

## Common Patterns

### Column Chart with Outside Labels

```cshtml
@Html.EJS().Chart("columnLabels").PrimaryXAxis(px => px.ValueType(Syncfusion.EJ2.Charts.ValueType.Category)).Series(series =>
    {
        series.Type(Syncfusion.EJ2.Charts.ChartSeriesType.Column)
              .XName("Month")
              .YName("Sales")
              .Marker(marker => marker
                  .DataLabel(label => label
                      .Visible(true)
                      .Position(Syncfusion.EJ2.Charts.LabelPosition.Top)
                      .Font(f => f.FontWeight("600").Size("12px"))))
              .DataSource(ViewBag.Sales)
              .Add();
    }).Render()
```

### Line Chart with Formatted Labels

```cshtml
@Html.EJS().Chart("lineLabels").Series(series =>
    {
        series.Type(Syncfusion.EJ2.Charts.ChartSeriesType.Line)
              .XName("Month")
              .YName("Profit")
              .Marker(marker => marker
                  .Visible(true)
                  .DataLabel(label => label
                      .Visible(true)
                      .Position(Syncfusion.EJ2.Charts.LabelPosition.Top)
                      .Format("${point.y}K")
                      .Fill("rgba(255,255,255,0.9)")
                      .Border(b => b.Width(1).Color("#1E88E5"))
                      .Rx(3).Ry(3)))
              .DataSource(ViewBag.Data)
              .Add();
    }).Render()
```

### Stacked Chart with Percentage Labels

```cshtml

@Html.EJS().Chart("stackedLabels")
    .Height("400px")
    .PrimaryXAxis(px => px
        .ValueType(Syncfusion.EJ2.Charts.ValueType.Category)
        .Title("Month")
    )
    .PrimaryYAxis(py => py
        .Title("Sales (%)")
        .RangePadding(Syncfusion.EJ2.Charts.ChartRangePadding.None)
    )
    .LegendSettings(ls => ls.Visible(true))
    .Series(series =>
    {
        series.Type(Syncfusion.EJ2.Charts.ChartSeriesType.StackingColumn100)
              .XName("Month")
              .YName("Sales")
              .Name("Q1")
              .Marker(marker => marker
                  .DataLabel(label => label
                      .Visible(true)
                      .Position(Syncfusion.EJ2.Charts.LabelPosition.Middle)
                      .Format("{point.percentage}%")
                      .Font(f => f.Color("#FFFFFF").FontWeight("600"))
                  )
              )
              .DataSource(ViewBag.Q1)
              .Add();

        series.Type(Syncfusion.EJ2.Charts.ChartSeriesType.StackingColumn100)
              .XName("Month")
              .YName("Sales")
              .Name("Q2")
              .Marker(marker => marker
                  .DataLabel(label => label
                      .Visible(true)
                      .Position(Syncfusion.EJ2.Charts.LabelPosition.Middle)
                      .Format("{point.percentage}%")
                      .Font(f => f.Color("#FFFFFF").FontWeight("600"))
                  )
              )
              .DataSource(ViewBag.Q2)
              .Add();
    }).Render()
```

## Performance Considerations

### Large Datasets

For charts with many data points:

```cshtml
@Html.EJS().Chart("largeDataChart").Series(series =>
    {
        series
              .Marker(marker => marker
                  .DataLabel(label => label.Visible(false)))
              .DataSource(ViewBag.LargeData).Add();
    }).Render()
```

**Guidelines:**
- Disable labels if > 50 points per series
- Use label templates sparingly
- Consider showing labels only on hover
- Use aggregated data for large sets

## Troubleshooting

### Labels not showing
- Check `Visible(true)` is set
- Verify data exists and is not null
- Check if labels are hidden by LabelIntersectAction
- Ensure position is appropriate for chart type

### Labels overlapping
- Use `LabelIntersectAction.Hide` or `Trim`
- Rotate labels with `Angle` property
- Increase chart size
- Reduce font size
- Consider using fewer data points

### Template not rendering
- Check HTML syntax in template string
- Verify placeholder names (`${point.y}`)
- Escape special characters properly
- Check browser console for errors

### Formatting not working
- Verify format string syntax
- Check data types match format
- Use correct placeholder names
- Test with simple format first

## Best Practices

1. **Visibility**: Show labels only when they add value
2. **Readability**: Ensure adequate font size (min 10px)
3. **Contrast**: Use high contrast between label and background
4. **Positioning**: Choose appropriate position for chart type
5. **Formatting**: Use consistent number formatting
6. **Performance**: Disable for large datasets
7. **Accessibility**: Provide alternative text for screen readers

## API Reference

https://help.syncfusion.com/cr/aspnetmvc-js2/Syncfusion.EJ2.Charts.ChartDataLabel.html
