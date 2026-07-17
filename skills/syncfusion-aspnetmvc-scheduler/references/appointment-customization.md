# Appointment Customization

## Table of Contents
1. [Event Templates](#event-templates)
2. [EventRendered Event](#eventrendered-event)
3. [Custom CSS Styling](#custom-css-styling)
4. [Tooltips](#tooltips)
5. [Colors and Indicators](#colors-and-indicators)
6. [Quick Info Customization](#quick-info-customization)

## Event Templates

Create custom HTML templates for appointment display:

```cshtml
@using Syncfusion.EJ2.Schedule

@(Html.EJS().Schedule("schedule")
    .Width("100%")
    .Height("550px")
    .EventSettings(new ScheduleEventSettings { DataSource = ViewBag.datasource, Template = ViewBag.template })
    .Readonly(true)
    .SelectedDate(new DateTime(2018, 2, 15))
    .Render()
)

<style>
    .e-schedule .e-vertical-view .e-content-wrap .e-appointment {
        border-radius: 8px;
    }

    .e-schedule .template-wrap {
        height: 100%;
        white-space: normal;
    }

    .e-schedule .template-wrap .subject {
        font-weight: 600;
        font-size: 15px;
        padding: 4px;
        text-overflow: ellipsis;
        white-space: nowrap;
        overflow: hidden;
    }

    .e-schedule .template-wrap .time {
        height: 50px;
        font-size: 12px;
        padding: 4px 6px;
        overflow: hidden;
    }
</style>
```

## EventRendered Event

Customize appointments after rendering:

```cshtml
.EventRendered("onEventRendered")
```

```javascript
function onEventRendered(args) {
    if (args.data.Status === 'Pending') {
        args.element.style.backgroundColor = '#FFD700';
        args.element.style.color = '#000';
    }
}
```

## Custom CSS Styling

Apply CSS classes to appointments:

```cshtml
@using Syncfusion.EJ2.Schedule

@(Html.EJS().Schedule("schedule")
    .Width("100%")
    .Height("550px")
    .CssClass("custom-class")
    .EventSettings(new ScheduleEventSettings { DataSource = ViewBag.datasource})
    .SelectedDate(new DateTime(2018, 2, 15))
    .Render()
)

<style>
    .custom-class.e-schedule .e-vertical-view .e-all-day-appointment-wrapper .e-appointment,
    .custom-class.e-schedule .e-vertical-view .e-day-wrapper .e-appointment,
    .custom-class.e-schedule .e-month-view .e-appointment{
        background: green;
    }
</style>
```

## Tooltips

Add tooltips to appointments:

```javascript
$('.e-appointment').tooltip({
    content: function() {
        var data = $(this).data('data');
        return data.Subject + ' - ' + data.Location;
    }
});
```

## Colors and Indicators

Customize appointment appearance:

```csharp
new ScheduleEvent {
    Subject = "Project Deadline",
    StartTime = new DateTime(2024, 1, 15, 9, 0, 0),
    EndTime = new DateTime(2024, 1, 15, 10, 0, 0),
    CategoryColor = "#FF0000"
}
```

### Color Codes
- Success: `#28A745`
- Warning: `#FFC107`
- Error: `#DC3545`
- Info: `#17A2B8`

## Quick Info Customization

Customize the popup info template:

```cshtml
.QuickInfoTemplate("#quickInfoTemplate")
```

```html
<script id="quickInfoTemplate" type="text/x-template">
    <div class="e-quick-popup-content">
        <div class="e-title">${Subject}</div>
        <div class="e-content">
            <div><strong>Time:</strong> ${StartTime}</div>
            <div><strong>Location:</strong> ${Location}</div>
            <div><strong>Status:</strong> ${Status}</div>
        </div>
    </div>
</script>
```

### Disable Quick Info

```cshtml
.ShowQuickInfo(false)
```
