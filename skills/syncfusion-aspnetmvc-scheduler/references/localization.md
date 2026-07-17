# Localization

## Table of Contents
1. [Culture Configuration](#culture-configuration)
2. [Language Support](#language-support)
3. [Date and Time Formats](#date-and-time-formats)
4. [Custom Locale Strings](#custom-locale-strings)
5. [RTL Support](#rtl-support)

## Culture Configuration

Set scheduler culture/language:

```cshtml
@Html.EJS().Schedule("Schedule")
    .Locale("es-ES") // Spanish (Spain)
    .EventSettings(new ScheduleEventSettings { DataSource = ViewBag.datasource })
    .Render()
```

### Culture Code Format
- `en-US` - English (United States)
- `en-GB` - English (Great Britain)
- `es-ES` - Spanish (Spain)
- `es-MX` - Spanish (Mexico)
- `fr-FR` - French (France)
- `de-DE` - German (Germany)
- `it-IT` - Italian (Italy)
- `pt-BR` - Portuguese (Brazil)
- `ja-JP` - Japanese
- `zh-CN` - Chinese (Simplified)
- `zh-TW` - Chinese (Traditional)
- `ru-RU` - Russian
- `ar-SA` - Arabic (Saudi Arabia)

## Language Support

Supported languages:

```cshtml
// English
.Locale("en-US")

// Spanish
.Locale("es-ES")

// French
.Locale("fr-FR")

// German
.Locale("de-DE")

// Portuguese
.Locale("pt-BR")

// Japanese
.Locale("ja-JP")

// Chinese
.Locale("zh-CN")

// Arabic
.Locale("ar-SA")
```

### Load Culture Files
```html
<script src="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/locale/es.js"></script>
<script src="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/locale/fr.js"></script>
<script src="https://cdn.syncfusion.com/ej2/{{ site.ej2version }}/locale/de.js"></script>
```

## Date and Time Formats

Localized date/time display:

```cshtml
@Html.EJS().Schedule("Schedule")
    .Locale("es-ES")
    .TimeFormat("HH:mm") // 24-hour format
    .DateFormat("dd/MM/yyyy")
    .EventSettings(new ScheduleEventSettings { DataSource = ViewBag.datasource })
    .Render()
```

### Format Codes
| Code | Meaning | Example |
|------|---------|---------|
| dd | Day of month (2 digits) | 05 |
| ddd | Day name (abbreviated) | Mon |
| dddd | Day name (full) | Monday |
| MM | Month (2 digits) | 03 |
| MMM | Month name (abbreviated) | Mar |
| MMMM | Month name (full) | March |
| yy | Year (2 digits) | 24 |
| yyyy | Year (4 digits) | 2024 |
| HH | Hours (24-hour) | 14 |
| hh | Hours (12-hour) | 02 |
| mm | Minutes | 30 |
| ss | Seconds | 45 |
| a | AM/PM | PM |

## Custom Locale Strings

Override default translations:

```javascript
ej.base.L10n.load({
    'es': {
        'schedule': {
            'saveButton': 'Guardar',
            'cancelButton': 'Cancelar',
            'deleteButton': 'Eliminar',
            'newEvent': 'Nuevo Evento',
            'editEvent': 'Editar Evento',
            'subject': 'Asunto',
            'location': 'Ubicación',
            'startTime': 'Hora de inicio',
            'endTime': 'Hora de finalización'
        }
    }
});
```

### Set Custom Translations in C#
```csharp
var localeStrings = new Dictionary<string, object>
{
    { "saveButton", "Guardar" },
    { "cancelButton", "Cancelar" },
    { "deleteButton", "Eliminar" },
    { "newEvent", "Nuevo Evento" }
};
```

## RTL Support

Enable right-to-left layout:

```cshtml
@Html.EJS().Schedule("Schedule")
    .Locale("ar-SA") // Arabic
    .EnableRtl(true)
    .EventSettings(new ScheduleEventSettings { DataSource = ViewBag.datasource })
    .Render()
```

### RTL Languages
- Arabic (ar-*)
- Hebrew (he-IL)
- Farsi (fa-IR)
- Urdu (ur-PK)

### RTL CSS
```css
.e-schedule.e-rtl {
    direction: rtl;
}

.e-schedule.e-rtl .e-sidebar {
    right: 0;
    left: auto;
}

.e-schedule.e-rtl .e-toolbar {
    flex-direction: row-reverse;
}
```

### Combined RTL Setup
```cshtml
@Html.EJS().Schedule("Schedule")
    .Locale("ar-SA")
    .EnableRtl(true)
    .Timezone("Asia/Dubai")
    .Views(new string[] { "Day", "Week", "Month" })
    .CurrentView(ScheduleView.Week)
    .Render()
```
