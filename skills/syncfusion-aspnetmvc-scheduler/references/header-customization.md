# Header Customization

## Table of Contents
1. [Header Bar Configuration](#header-bar-configuration)
2. [Custom Toolbar Items](#custom-toolbar-items)
3. [Date Picker Integration](#date-picker-integration)
4. [Navigation Controls](#navigation-controls)
5. [Header Styling](#header-styling)

## Header Bar Configuration

Customize toolbar appearance:

```cshtml
@using Syncfusion.EJ2.Schedule

@(Html.EJS().Schedule("schedule")
    .Width("100%")
    .Height("550px")
    .Toolbar(new string[] { 
        "Previous", "Today", "Next", "Views", 
        "DatePicker", "Settings", "Print", "Export" 
    })
    .EventSettings(new ScheduleEventSettings { DataSource = ViewBag.datasource })
    .SelectedDate(new DateTime(2018, 2, 15))
    .Render()
)
```

### Toolbar Items
| Item | Purpose |
|------|---------|
| Previous | Go to previous period |
| Today | Go to current date |
| Next | Go to next period |
| Views | Switch between views |
| DatePicker | Select date directly |
| Settings | Show settings menu |
| Print | Print scheduler |
| Export | Export appointments |

## Custom Toolbar Items

Add custom buttons:

```cshtml
.ToolbarItemRendered("onToolbarItemRendered")
```

```javascript
function onToolbarItemRendered(args) {
    if (args.item && args.item.id === 'schedule_views') {
        // Customize views dropdown
        args.element.style.minWidth = '150px';
    }
}
```

### Custom Button
```cshtml
@using Syncfusion.EJ2.Schedule

@(Html.EJS().Schedule("schedule")
    .Width("100%")
    .Height("550px")
    .Toolbar(new object[] { 
        "Previous", "Today", "Next", "Views",
        new { text = "Refresh", tooltipText = "Refresh appointments", prefixIcon = "e-icon-refresh" }
    })
    .ToolbarClick("onToolbarClick")
    .EventSettings(new ScheduleEventSettings { DataSource = ViewBag.datasource })
    .SelectedDate(new DateTime(2018, 2, 15))
    .Render()
)
```

```javascript
function onToolbarClick(args) {
    if (args.item.text === 'Refresh') {
        var schedule = document.getElementById('schedule').ej2_instances[0];
        schedule.refreshEvents();
    }
}
```

## Date Picker Integration

Add date navigation:

```cshtml
@using Syncfusion.EJ2.Schedule

@(Html.EJS().Schedule("schedule")
    .Width("100%")
    .Height("550px")
    .Toolbar(new string[] { "Previous", "Today", "Next", "DatePicker", "Views" })
    .EventSettings(new ScheduleEventSettings { DataSource = ViewBag.datasource })
    .SelectedDate(new DateTime(2018, 2, 15))
    .Render()
)
```

### Custom Date Picker
```html
<div class="e-toolbar-custom-date">
    <input type="date" id="dateSelector" />
</div>

<script>
document.getElementById('dateSelector').addEventListener('change', function(e) {
    var selectedDate = new Date(e.target.value);
    var schedule = document.getElementById('schedule').ej2_instances[0];
    schedule.selectedDate = selectedDate;
});
</script>
```

## Navigation Controls

Control date navigation:

```javascript
var schedule = document.getElementById('schedule').ej2_instances[0];

// Previous period
schedule.previousDate();

// Next period
schedule.nextDate();

// Go to today
schedule.today();

// Set specific date
schedule.selectedDate = new Date(2024, 0, 15);
```

### Month Navigation
```javascript
function goToMonth(monthOffset) {
    var schedule = document.getElementById('schedule').ej2_instances[0];
    var newDate = new Date(schedule.selectedDate);
    newDate.setMonth(newDate.getMonth() + monthOffset);
    schedule.selectedDate = newDate;
}

// Go 3 months forward
goToMonth(3);

// Go 1 month back
goToMonth(-1);
```

## Header Styling

Customize header appearance:

```css
.e-schedule .e-toolbar {
    background: linear-gradient(90deg, #1a73e8 0%, #1e3a8a 100%);
    padding: 12px 16px;
    border-bottom: 2px solid #1565c0;
}

.e-schedule .e-toolbar .e-btn {
    color: white;
    border: none;
    margin: 0 4px;
}

.e-schedule .e-toolbar .e-btn:hover {
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 4px;
}

.e-schedule .e-toolbar .e-btn.e-active {
    background-color: rgba(255, 255, 255, 0.3);
    border-bottom: 3px solid white;
}

.e-schedule .e-date-range {
    font-size: 18px;
    font-weight: 600;
    color: white;
}

.e-schedule .e-date-range .e-tbar-btn:hover {
    background: transparent;
}
```

### Toolbar Spacing
```css
.e-schedule .e-toolbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.e-schedule .e-toolbar-left {
    display: flex;
    gap: 8px;
}

.e-schedule .e-toolbar-right {
    display: flex;
    gap: 8px;
}
```

### Responsive Toolbar
```css
@media (max-width: 768px) {
    .e-schedule .e-toolbar {
        flex-wrap: wrap;
    }
    
    .e-schedule .e-toolbar .e-btn {
        font-size: 12px;
        padding: 6px 8px;
    }
}
```