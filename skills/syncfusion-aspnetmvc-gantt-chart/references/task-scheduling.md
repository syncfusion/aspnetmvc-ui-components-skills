# Task Scheduling – Syncfusion ASP.NET MVC Gantt Chart

## Table of Contents
- [Overview](#overview)
- [Auto Scheduling](#auto-scheduling)
- [Manual Scheduling](#manual-scheduling)
- [Validate Manual Tasks on Linking](#validate-manual-tasks-on-linking)
- [Custom Scheduling (Mixed)](#custom-scheduling-mixed)
- [Unscheduled Tasks](#unscheduled-tasks)
- [Milestones](#milestones)
- [Working Time Range](#working-time-range)
- [Week-Specific Working Time](#week-specific-working-time)
- [Non-Working Days](#non-working-days)
- [Duration Units](#duration-units)
- [Baseline](#baseline)
- [Task Constraints](#task-constraints)
- [Task Constraint Conflict Management](#task-constraint-conflict-management)

---

## Overview

Task scheduling in the Gantt Chart is controlled by the `TaskMode` property. It determines whether date calculations are automatic or manual.

| Mode | Behavior |
|---|---|
| `Auto` (default) | Dates automatically validated against working time, holidays, weekends, predecessors |
| `Manual` | Dates used as-is from data source; no automatic adjustment |
| `Custom` | Per-task mode; driven by a boolean field in the data source |

---

## Auto Scheduling

Default mode — Gantt automatically adjusts dates based on dependencies, working hours, and non-working days:

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.GanttData)
    .TaskFields(tf => tf.Id("TaskId").Name("TaskName").StartDate("StartDate").Duration("Duration").Child("SubTasks"))
    .TaskMode(Syncfusion.EJ2.Gantt.ScheduleMode.Auto)
    .Render()
```

---

## Manual Scheduling

Dates are not validated — they are rendered exactly as provided in the data source:

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.GanttData)
    .TaskFields(tf => tf.Id("TaskId").Name("TaskName").StartDate("StartDate").Duration("Duration").Child("SubTasks"))
    .TaskMode(Syncfusion.EJ2.Gantt.ScheduleMode.Manual)
    .Render()
```

In manual mode:
- Task bars can be dragged to any position on the timeline
- Predecessor lines are drawn but do **not** constrain date positions
- Useful for tracking actual vs. planned dates without automatic adjustment

---

## Validate Manual Tasks on Linking

In `Manual` mode, you can still enable predecessor-based date validation by setting `ValidateManualTasksOnLinking(true)`. This causes Gantt to automatically recalculate manual task dates when a dependency link is added or changed.

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.GanttData)
    .TaskFields(tf => tf.Id("TaskId").Name("TaskName").StartDate("StartDate").Duration("Duration").Dependency("Predecessor").Child("SubTasks"))
    .TaskMode(Syncfusion.EJ2.Gantt.ScheduleMode.Manual)
    .ValidateManualTasksOnLinking(true)
    .Render()
```

> `ValidateManualTasksOnLinking` applies predecessor validation only — all other scheduling factors (holidays, weekends, working time) are still ignored in manual mode.

---

## Custom Scheduling (Mixed)

Map a boolean `IsManual` field in the data source. Each task can independently be auto or manual:

**Model:**

```csharp
public class GanttData
{
    public int TaskId { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public int? Duration { get; set; }
    public bool IsManual { get; set; }   // true = manual, false = auto
    public List<GanttData> SubTasks { get; set; }
}
```

**View:**

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.GanttData)
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate").Duration("Duration")
        .Manual("IsManual")       // maps per-task scheduling mode
        .Child("SubTasks")
    )
    .TaskMode(Syncfusion.EJ2.Gantt.ScheduleMode.Custom)
    .Render()
```

---

## Unscheduled Tasks

Tasks without complete date information are called unscheduled tasks. Enable them with `AllowUnscheduledTasks`:

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.GanttData)
    .TaskFields(tf => tf.Id("TaskId").Name("TaskName").StartDate("StartDate").Duration("Duration").Child("SubTasks"))
    .AllowUnscheduledTasks(true)
    .Render()
```

**Rendering behaviour by available data:**

| Available Fields | Auto Mode | Manual Mode |
|---|---|---|
| Start Date only | Half taskbar from start date | Half taskbar from start date |
| End Date only | Half taskbar ending at end date | Half taskbar ending at end date |
| Duration only | Taskbar at project start for given duration | Taskbar at project start for given duration |
| No dates or duration | Milestone at project start | Milestone at project start |
| Start + End | Full taskbar | Full taskbar |

> If `AllowUnscheduledTasks` is `false`, Gantt defaults duration to 1 day from project start date.

---

## Milestones

A milestone is a zero-duration task displayed as a diamond shape. Create one by setting `Duration = 0`:

```csharp
new GanttData { TaskId = 5, TaskName = "Go Live", StartDate = new DateTime(2024, 5, 1), Duration = 0 }
```

Or set `StartDate == EndDate` with `Duration = 0`.

---

## Working Time Range

Define the working hours for all project days using `DayWorkingTime`:

```cshtml
@Html.EJS().Gantt("gantt")
    .TaskFields(tf => tf.Id("TaskId").Name("TaskName").StartDate("StartDate").Duration("Duration").Child("SubTasks"))
    .DayWorkingTime(dwt =>
    {
        dwt.From(8).To(12).Add();   // 8am–12pm
        dwt.From(13).To(17).Add();  // 1pm–5pm
    })
    .Render()
```

Define different working hours per day of week using `WeekWorkingTime`:

```cshtml
.WeekWorkingTime(wwt =>
{
    wwt.DayOfWeek(new List<Syncfusion.EJ2.Gantt.DayOfWeek> { Syncfusion.EJ2.Gantt.DayOfWeek.Monday, Syncfusion.EJ2.Gantt.DayOfWeek.Tuesday })
       .TimeRange(tr => { tr.From(10).To(18).Add(); })
       .Add();
})
```

> `WeekWorkingTime` takes priority over `DayWorkingTime` for the days it covers.

---

## Week-Specific Working Time

Use `WeekWorkingTime` to assign different working hours to specific days of the week, overriding the global `DayWorkingTime` for those days.

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.GanttData)
    .TaskFields(tf => tf.Id("TaskId").Name("TaskName").StartDate("StartDate").Duration("Duration").Dependency("Predecessor").Child("SubTasks"))
    .WeekWorkingTime(wwt =>
    {
        wwt.DayOfWeek(new List<Syncfusion.EJ2.Gantt.DayOfWeek> { Syncfusion.EJ2.Gantt.DayOfWeek.Tuesday })
           .TimeRange(tr => { tr.From(10).To(18).Add(); })
           .Add();
    })
    .TimelineSettings(ts => ts
        .TopTier(tt => tt.Unit(Syncfusion.EJ2.Gantt.TimelineViewMode.Day))
        .BottomTier(bt => bt.Unit(Syncfusion.EJ2.Gantt.TimelineViewMode.Hour))
    )
    .Render()
```

**Priority and fallback rules:**
- `WeekWorkingTime` takes priority over `DayWorkingTime` for specified days.
- Days not listed in `WeekWorkingTime` use the global `DayWorkingTime` values.
- If a day is both a holiday/non-working day and listed in `WeekWorkingTime`, it remains a non-working day.

---

## Non-Working Days

By default, Saturday and Sunday are non-working days. Configure the working week:

```cshtml
@Html.EJS().Gantt("gantt")
    .TaskFields(tf => tf.Id("TaskId").Name("TaskName").StartDate("StartDate").Duration("Duration").Child("SubTasks"))
    .WorkWeek(new List<string> { "Monday", "Tuesday", "Wednesday", "Thursday", "Friday" })
    .Render()
```

Include weekends as working days:

```cshtml
.IncludeWeekend(true)
```

---

## Duration Units

Duration can be measured in Days (default), Hours, or Minutes. Set globally:

```cshtml
@Html.EJS().Gantt("gantt")
    .TaskFields(tf => tf.Id("TaskId").Name("TaskName").StartDate("StartDate").Duration("Duration").Child("SubTasks"))
    .DurationUnit(Syncfusion.EJ2.Gantt.DurationUnit.Hour)
    .Render()
```

Set per-task via a `DurationUnit` field in data:

```csharp
new GanttData { TaskId = 2, TaskName = "Quick task", StartDate = ..., Duration = 4, DurationUnit = "hour" }
```

Map in TaskFields:

```cshtml
.TaskFields(tf => tf.Id("TaskId").Name("TaskName").StartDate("StartDate").Duration("Duration").DurationUnit("DurationUnit").Child("SubTasks"))
```

Or embed unit in duration string: `"4 hours"`, `"30 minutes"`.

> Default unit is `day`. Edit type for duration column is string when mixing units.

---

## Baseline

Show planned vs. actual task dates side-by-side:

```csharp
public class GanttData
{
    // ... other fields
    public DateTime BaselineStartDate { get; set; }
    public DateTime BaselineEndDate { get; set; }
    public int? BaselineDuration { get; set; }
}
```

```cshtml
@Html.EJS().Gantt("gantt")
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate").EndDate("EndDate").Duration("Duration")
        .BaselineStartDate("BaselineStartDate")
        .BaselineEndDate("BaselineEndDate")
        .Child("SubTasks")
    )
    .RenderBaseline(true)
    .BaselineColor("#fc7b00")   // optional color override
    .Render()
```

> To show a baseline milestone, set `BaselineDuration = 0` explicitly. Matching start/end dates without duration = 0 renders a 1-day baseline task.

---

## Task Constraints

Task constraints define scheduling rules that restrict when a task is allowed to start or finish.

| Constraint Type | Code | Description |
|---|---|---|
| As Soon As Possible | `0` | Starts as soon as its dependencies allow (default) |
| As Late As Possible | `1` | Delays until the last possible moment without affecting successors |
| Must Start On | `2` | Must begin on an exact date |
| Must Finish On | `3` | Must end on an exact date |
| Start No Earlier Than | `4` | Cannot start before the constraint date |
| Start No Later Than | `5` | Must start on or before the constraint date |
| Finish No Earlier Than | `6` | Cannot finish before the constraint date |
| Finish No Later Than | `7` | Must finish on or before the constraint date |

**Model:**

```csharp
public class GanttData
{
    public int TaskId { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public DateTime EndDate { get; set; }
    public int Duration { get; set; }
    public int Progress { get; set; }
    public string Predecessor { get; set; }
    public int? ParentID { get; set; }
    public int ConstraintType { get; set; }   // e.g., 2 (Must Start On), 7 (Finish No Later Than), 4 (Start No Earlier Than)
    public DateTime? ConstraintDate { get; set; }
}
```

**View:**

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.GanttData)
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate").EndDate("EndDate").Duration("Duration")
        .Progress("Progress").Dependency("Predecessor").ParentID("ParentID")
        .ConstraintType("ConstraintType").ConstraintDate("ConstraintDate")
    )
    .EditSettings(es => es.AllowAdding(true).AllowEditing(true).AllowDeleting(true).AllowTaskbarEditing(true))
    .Columns(col =>
    {
        col.Field("TaskId").Visible(false).Add();
        col.Field("TaskName").HeaderText("Job Name").Width("200").Add();
        col.Field("StartDate").Add();
        col.Field("Duration").Add();
        col.Field("ConstraintType").Width("180").Add();
        col.Field("ConstraintDate").Add();
        col.Field("EndDate").Add();
        col.Field("Predecessor").Add();
        col.Field("Progress").Add();
    })
    .Render()
```

> `ConstraintType` accepts numeric values: `0` (ASAP), `1` (ALAP), `2` (MSO), `3` (MFO), `4` (SNET), `5` (SNLT), `6` (FNET), `7` (FNLT). `ConstraintDate` is required for all constraint types except `0` (ASAP) and `1` (ALAP).

---

## Task Constraint Conflict Management

When a scheduling change violates a strict constraint (`2`/MSO, `3`/MFO, `5`/SNLT, `7`/FNLT), the Gantt shows a violation popup. Use the `ActionBegin` event with `requestType === "validateTaskViolation"` to intercept and handle violations silently.

| `validateMode` Flag | Constraint Enforced Silently |
|---|---|
| `respectMustStartOn` | Must Start On (2) |
| `respectMustFinishOn` | Must Finish On (3) |
| `respectStartNoLaterThan` | Start No Later Than (5) |
| `respectFinishNoLaterThan` | Finish No Later Than (7) |

> All flags default to `false` (popup shown). Set a flag to `true` to silently cancel the violating user action without displaying a dialog.

```cshtml
@Html.EJS().Gantt("gantt")
    .DataSource((IEnumerable<object>)ViewBag.GanttData)
    .TaskFields(tf => tf
        .Id("TaskId").Name("TaskName").StartDate("StartDate").EndDate("EndDate").Duration("Duration")
        .Progress("Progress").Dependency("Predecessor").ParentID("ParentID")
        .ConstraintType("ConstraintType").ConstraintDate("ConstraintDate")
    )
    .EditSettings(es => es.AllowAdding(true).AllowEditing(true).AllowDeleting(true).AllowTaskbarEditing(true))
    .ActionBegin("actionBegin")
    .Render()

<script>
function actionBegin(args) {
    if (args.requestType === 'validateTaskViolation') {
        args.validateMode.respectMustStartOn = true;
    }
}
</script>
```
