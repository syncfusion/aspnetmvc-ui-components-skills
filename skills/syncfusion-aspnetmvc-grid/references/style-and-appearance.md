# Style and Appearance in ASP.NET MVC Grid

Customize the visual appearance of the grid using themes, CSS, and built-in styling properties.

## When to Use This

Use this reference when you need to:
- Apply and switch between themes
- Add custom CSS classes to grids
- Implement conditional row or cell styling
- Customize header and row styling
- Override built-in CSS classes
- Add tooltips to grid elements

## Table of Contents
- [Themes](#themes)
- [Apply Custom CSS Class to Grid](#apply-custom-css-class-to-grid)
- [Conditional Row Styling via RowDataBound](#conditional-row-styling-via-rowdatabound)
- [Conditional Cell Styling via QueryCellInfo](#conditional-cell-styling-via-querycellinfo)
- [CustomAttributes on Columns](#customattributes-on-columns)
- [Alternate Row Styling](#alternate-row-styling)
- [Built-in CSS Classes](#built-in-css-classes)
- [Grid Border Customization](#grid-border-customization)
- [Row Height](#row-height)
- [Header Styling](#header-styling)
- [Tooltip on Hover](#tooltip-on-hover)

## Themes

Include a Syncfusion theme CDN in `_Layout.cshtml`. Replace `fluent2.css` with your desired theme:

```cshtml
<head>
    <!-- Theme: fluent2, bootstrap5, tailwind, material3, fabric, highcontrast -->
    <link rel="stylesheet" href="https://cdn.syncfusion.com/ej2/29.1.33/fluent2.css" />
    <script src="https://cdn.syncfusion.com/ej2/29.1.33/dist/ej2.min.js"></script>
</head>
```

| Theme CSS File | Description |
|----------------|-------------|
| `fluent2.css` | Microsoft Fluent 2 design (default/recommended) |
| `bootstrap5.css` | Bootstrap 5 design system |
| `tailwind.css` | Tailwind CSS design system |
| `material3.css` | Google Material Design 3 |
| `fabric.css` | Microsoft Office Fabric UI |
| `highcontrast.css` | High contrast for accessibility |
| `fluent2-highcontrast.css` | Fluent 2 high contrast variant |

## Apply Custom CSS Class to Grid

Add a CSS class to the grid container for scoped styling:

```cshtml
@Html.EJS().Grid("Grid").DataSource((IEnumerable<object>)ViewBag.DataSource)
    .CssClass("custom-grid")
    .Columns(col => {
        col.Field("OrderID").HeaderText("Order ID").Width("120").Add();
        col.Field("CustomerID").HeaderText("Customer").Width("150").Add();
    })
    .Render()
```

```css
.custom-grid .e-gridheader {
    background-color: #1565c0;
    color: white;
}
.custom-grid .e-headercell {
    font-size: 14px;
    font-weight: 600;
}
```

## Conditional Row Styling via RowDataBound

Apply styles to rows based on data values:

```cshtml
@Html.EJS().Grid("Grid").RowDataBound("rowDataBound")
    .Columns(col => {
        col.Field("OrderID").HeaderText("Order ID").Width("120").Add();
        col.Field("Freight").HeaderText("Freight").Format("C2").Width("120").Add();
    })
    .Render()

<script>
function rowDataBound(args) {
    if (args.data['Freight'] > 100) {
        args.row.classList.add('high-freight');
    } else if (args.data['Freight'] > 50) {
        args.row.classList.add('medium-freight');
    }
}
</script>

<style>
.high-freight { background-color: #fde8e8 !important; }
.medium-freight { background-color: #fff3cd !important; }
</style>
```

## Conditional Cell Styling via QueryCellInfo

Apply styles to individual cells:

```cshtml
@Html.EJS().Grid("Grid").QueryCellInfo("queryCellInfo")
    .Columns(col => {
        col.Field("Freight").HeaderText("Freight").Format("C2").Width("120").Add();
    })
    .Render()

<script>
function queryCellInfo(args) {
    if (args.column.field === 'Freight') {
        if (args.data['Freight'] < 30) {
            args.cell.style.color = 'green';
        } else if (args.data['Freight'] > 100) {
            args.cell.style.color = 'red';
            args.cell.style.fontWeight = 'bold';
        }
    }
}
</script>
```

## CustomAttributes on Columns

Apply CSS class to all cells in a column:

```cshtml
col.Field("ShipCity").HeaderText("Ship City").Width("150")
    .CustomAttributes(new { @class = "highlighted-city" }).Add();
```

```css
.highlighted-city {
    background-color: #e3f2fd;
    font-style: italic;
}
```

## Alternate Row Styling

Enable/disable alternating row colors (enabled by default):

```cshtml
@Html.EJS().Grid("Grid").EnableAltRow(true)  // default: true
    .Columns(col => { /* ... */ })
    .Render()
```

Override alternate row color via CSS:

```css
.e-grid .e-altrow {
    background-color: #f5f5f5;
}
```

## Built-in CSS Classes

| Class | Description |
|-------|-------------|
| `.e-grid` | Root grid element |
| `.e-gridheader` | Header container |
| `.e-gridcontent` | Content container |
| `.e-gridpager` | Pager container |
| `.e-headercell` | Header cell |
| `.e-rowcell` | Content cell |
| `.e-altrow` | Alternate row |
| `.e-selectionbackground` | Selected row background |
| `.e-cellselectionbackground` | Selected cell background |
| `.e-row` | Data row |
| `.e-groupcaptionrow` | Group caption row |

## Grid Border Customization

```css
/* Remove all grid borders */
.e-grid .e-gridheader, .e-grid .e-gridcontent {
    border: none;
}

/* Custom border color */
.e-grid {
    border-color: #1565c0;
}
```

## Row Height

Set uniform row height for the grid:

```cshtml
@Html.EJS().Grid("Grid").RowHeight(45)
    .Columns(col => { /* ... */ })
    .Render()
```

## Header Styling

Customize the grid header via CSS:

```css
/* Header background and text */
.e-grid .e-headercell {
    background-color: #283593;
    color: white;
    font-size: 13px;
}

/* Sorted header column highlight */
.e-grid .e-columnheader .e-headercell.e-focus {
    background-color: #3949ab;
}
```

## Tooltip on Hover

Wrap the grid in a Syncfusion Tooltip targeting `.e-rowcell`:

```cshtml
@Html.EJS().Tooltip("tooltip").Target(".e-rowcell").Content(
    "<script>document.write(args.target.innerText);<\/script>"
).ChildContent(content => {
    @Html.EJS().Grid("Grid").DataSource((IEnumerable<object>)ViewBag.DataSource)
        .Columns(col => {
            col.Field("OrderID").HeaderText("Order ID").Width("100").Add();
        })
        .Render();
}).Render()
```
