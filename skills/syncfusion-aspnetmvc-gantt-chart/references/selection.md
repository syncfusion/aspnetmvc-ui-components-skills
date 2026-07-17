# Selection — Syncfusion ASP.NET MVC Gantt Chart

## Table of Contents
- [Overview](#overview)
- [Disable Selection](#disable-selection)
- [Selection Mode](#selection-mode)
- [Selection Type](#selection-type)
- [Toggle Selection](#toggle-selection)
- [Hover Highlighting](#hover-highlighting)
- [Row Selection](#row-selection)
  - [Select a Row on Initial Load](#select-a-row-on-initial-load)
  - [Select a Row Dynamically](#select-a-row-dynamically)
  - [Multiple Row Selection](#multiple-row-selection)
  - [Conditional Row Selection](#conditional-row-selection)
  - [Customize Row Selection Action](#customize-row-selection-action)
- [Cell Selection](#cell-selection)
  - [Multiple Cell Selection](#multiple-cell-selection)
  - [Select a Cell Dynamically](#select-a-cell-dynamically)
  - [Customize Cell Selection Action](#customize-cell-selection-action)
- [Get Selected Row Indexes and Records](#get-selected-row-indexes-and-records)
- [Clear Selection](#clear-selection)
- [Touch Interaction](#touch-interaction)

---

## Overview

Selection provides an option to highlight a row or a cell in the Gantt Chart. It can be triggered by clicking or using arrow keys. Selection is enabled by default and configured via `.SelectionSettings()` on the Gantt builder.

---

## Disable Selection

Set `.AllowSelection(false)` on the Gantt builder to disable all selection behaviour:

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .AllowSelection(false)
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()
```

> `Row` selection is the default mode when `AllowSelection` is `true`.

---

## Selection Mode

Set `Mode` on `.SelectionSettings()` to control what can be selected:

| `Mode` Value | Description |
|---|---|
| `Row` (default) | Selects the entire row |
| `Cell` | Selects individual cells |
| `Both` | Selects both rows and cells simultaneously |

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .SelectionSettings(ss => ss
        .Mode(Syncfusion.EJ2.Grids.SelectionMode.Both))
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()
```

---

## Selection Type

Set `Type` on `.SelectionSettings()` to allow single or multiple selections:

| `Type` Value | Description |
|---|---|
| `Single` (default) | Only one row or cell can be selected at a time |
| `Multiple` | Multiple rows or cells can be selected by holding **Ctrl** while clicking |

---

## Toggle Selection

Set `EnableToggle(true)` on `.SelectionSettings()` to allow deselecting an already-selected row or cell by clicking it again. Default is `false`.

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .AllowSelection(true)
    .SelectionSettings(ss => ss
        .Mode(Syncfusion.EJ2.Grids.SelectionMode.Row)
        .Type(Syncfusion.EJ2.Grids.SelectionType.Multiple)
        .EnableToggle(true))
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()
```

Disable toggle at runtime:

```javascript
var ganttObj = document.getElementById('gantt').ej2_instances[0];
ganttObj.selectionSettings.enableToggle = false;
```

---

## Hover Highlighting

Set `.EnableHover(true)` on the Gantt builder to highlight tree grid rows, chart taskbars, header cells, and timeline cells when the mouse hovers over them:

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .EnableHover(true)
    .AllowSelection(true)
    .SelectionSettings(ss => ss
        .Mode(Syncfusion.EJ2.Grids.SelectionMode.Row)
        .Type(Syncfusion.EJ2.Grids.SelectionType.Multiple))
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()
```

---

## Row Selection

### Select a Row on Initial Load

Use `.SelectedRowIndex(n)` on the Gantt builder to pre-select a row when the component first renders. The index is **0-based**:

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .AllowSelection(true)
    .SelectedRowIndex(3)
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()
```

---

### Select a Row Dynamically

Use `selectionModule.selectRow(index)` to select a single row, or `selectionModule.selectRows(indexes)` to select multiple rows programmatically:

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .AllowSelection(true)
    .SelectionSettings(ss => ss
        .Mode(Syncfusion.EJ2.Grids.SelectionMode.Row)
        .Type(Syncfusion.EJ2.Grids.SelectionType.Multiple))
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()

<script>
function selectSingleRow() {
    var ganttObj = document.getElementById('gantt').ej2_instances[0];
    ganttObj.selectionModule.selectRow(2);        // select row at index 2
}

function selectMultipleRows() {
    var ganttObj = document.getElementById('gantt').ej2_instances[0];
    ganttObj.selectionModule.selectRows([1, 2, 3]); // select rows at indexes 1, 2, 3
}
</script>
```

---

### Multiple Row Selection

Set `Type(Syncfusion.EJ2.Grids.SelectionType.Multiple)` on `.SelectionSettings()` to allow selecting more than one row. Hold **Ctrl** while clicking to add rows to the selection:

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .AllowSelection(true)
    .SelectionSettings(ss => ss
        .Mode(Syncfusion.EJ2.Grids.SelectionMode.Row)
        .Type(Syncfusion.EJ2.Grids.SelectionType.Multiple))
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()
```

> Multiple row selection is also required to enable multi-row drag-and-drop (`AllowRowDragAndDrop`).

---

### Conditional Row Selection

Use `selectionModule.selectRows()` inside the `DataBound` event to select specific rows based on data conditions on initial load:

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .AllowSelection(true)
    .DataBound("dataBound")
    .SelectionSettings(ss => ss
        .Mode(Syncfusion.EJ2.Grids.SelectionMode.Row)
        .Type(Syncfusion.EJ2.Grids.SelectionType.Multiple))
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()

<script>
function dataBound(args) {
    var ganttObj = document.getElementById('gantt').ej2_instances[0];
    var rowIndexes = [];
    ganttObj.treeGrid.grid.dataSource.forEach(function (data, index) {
        if (data.TaskId === 3 || data.TaskId === 4) {
            rowIndexes.push(index);
        }
    });
    ganttObj.selectionModule.selectRows(rowIndexes);
}
</script>
```

---

### Customize Row Selection Action

The `RowSelecting` event fires before a row is selected. Use `args.cancel = true` to prevent selection of a specific row. The `RowSelected` event fires after the selection completes.

When a row is deselected, `RowDeselecting` fires before and `RowDeselected` fires after.

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .AllowSelection(true)
    .RowSelecting("rowSelecting")
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()

<script>
function rowSelecting(args) {
    if (args.rowIndex === 3) {
        args.cancel = true;   // prevent selection of row at index 3
    }
}
</script>
```

| Event | Trigger point | Key `args` properties |
|---|---|---|
| `RowSelecting` | Before row selection completes | `rowIndex`, `data`, `cancel` |
| `RowSelected` | After row selection completes | `rowIndex`, `data` |
| `RowDeselecting` | Before row deselection completes | `rowIndex`, `data`, `cancel` |
| `RowDeselected` | After row deselection completes | `rowIndex`, `data` |

---

## Cell Selection

Set `Mode(Syncfusion.EJ2.Grids.SelectionMode.Cell)` on `.SelectionSettings()` to enable cell-level selection. Use `getSelectedRowCellIndexes()` to retrieve selected cell information:

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .AllowSelection(true)
    .SelectionSettings(ss => ss
        .Mode(Syncfusion.EJ2.Grids.SelectionMode.Cell)
        .Type(Syncfusion.EJ2.Grids.SelectionType.Single))
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()
```

> Cell-based selection is **not supported** when virtualization is enabled.

---

### Multiple Cell Selection

Set `Type(Syncfusion.EJ2.Grids.SelectionType.Multiple)` with `Mode(Cell)` to allow selecting more than one cell. Hold **Ctrl** while clicking to add cells to the selection:

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .AllowSelection(true)
    .SelectionSettings(ss => ss
        .Mode(Syncfusion.EJ2.Grids.SelectionMode.Cell)
        .Type(Syncfusion.EJ2.Grids.SelectionType.Multiple))
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()
```

---

### Select a Cell Dynamically

Use `selectionModule.selectCell(cellIndex)` to select a cell programmatically:

```javascript
var ganttObj = document.getElementById('gantt').ej2_instances[0];
ganttObj.selectionModule.selectCell(2);   // select cell at index 2
```

---

### Customize Cell Selection Action

The `CellSelecting` event fires when a cell selection begins. Use `args.cancel = true` to prevent selection of a specific cell. The `CellSelected` event fires after the selection completes.

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .AllowSelection(true)
    .CellSelecting("cellSelecting")
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()

<script>
function cellSelecting(args) {
    if (args.cellIndex === 3) {
        args.cancel = true;   // prevent selection of cell at index 3
    }
}
</script>
```

| Event | Trigger point | Key `args` properties |
|---|---|---|
| `CellSelecting` | Before cell selection completes | `cellIndex`, `rowIndex`, `cancel` |
| `CellSelected` | After cell selection completes | `cellIndex`, `rowIndex` |

---

## Get Selected Row Indexes and Records

Use `getSelectedRowIndexes()` to retrieve the indexes of all selected rows, and `getSelectedRecords()` to retrieve the full data objects of selected rows:

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.DataSource)
    .Height("450px")
    .AllowSelection(true)
    .RowSelected("rowSelected")
    .SelectionSettings(ss => ss
        .Mode(Syncfusion.EJ2.Grids.SelectionMode.Row)
        .Type(Syncfusion.EJ2.Grids.SelectionType.Multiple))
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate")
        .Duration("Duration").Child("SubTasks"))
    .Render()

<script>
function rowSelected(args) {
    var ganttObj = document.getElementById('gantt').ej2_instances[0];
    var selectedIndexes = ganttObj.selectionModule.getSelectedRowIndexes(); // [0, 2, 3]
    var selectedRecords = ganttObj.selectionModule.getSelectedRecords();    // task data objects
    console.log(selectedIndexes);
    console.log(selectedRecords);
}
</script>
```

---

## Clear Selection

Call `clearSelection()` on the Gantt instance to deselect all currently selected rows or cells:

```javascript
var ganttObj = document.getElementById('gantt').ej2_instances[0];
ganttObj.selectionModule.selectRows([1, 2, 3]);
ganttObj.clearSelection();   // deselect all
```

---

## Touch Interaction

The Gantt Chart supports touch-based selection on mobile and tablet devices:

- **Single row selection:** Tap a row to select it.
- **Multiple row selection:** When you tap a row, a popup appears indicating the multi-row selection option. Tap the popup, then tap additional rows to build a multi-row selection.

Touch interaction follows the same `SelectionSettings.Mode` and `SelectionSettings.Type` configuration as pointer-based selection.
