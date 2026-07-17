# Events — Syncfusion ASP.NET MVC AI AssistView

## Table of Contents
- [created](#created)
- [promptRequest](#promptrequest)
- [promptChanged](#promptchanged)
- [Attachment Events](#attachment-events)
  - [beforeAttachmentUpload](#beforeattachmentupload)
  - [attachmentUploadSuccess](#attachmentuploadsuccess)
  - [attachmentUploadFailure](#attachmentuploadfailure)
  - [attachmentRemoved](#attachmentremoved)
  - [attachmentClick](#attachmentclick)

---

## created

Fires when the AI AssistView control has finished rendering. Use this event to store the control reference (`this`) for later programmatic access.

```razor
@using Syncfusion.EJ2.InteractiveChat

<div class="aiassist-container" style="height: 350px; width: 650px;">
    @Html.EJS().AIAssistView("aiAssistView")
        .PromptRequest("onPromptRequest")
        .Created("onCreated")
        .Render()
</div>

<script>
    var assistObj;

    function onCreated() {
        assistObj = this; // 'this' refers to the AIAssistView instance
        // Safe to call assistObj methods here
    }

    function onPromptRequest(args) {
        setTimeout(function () {
            assistObj.addPromptResponse('Connect to your AI service for real-time responses.');
        }, 2000);
    }
</script>
```

> **Best practice:** Always store `this` in `onCreated` — it's the only reliable way to get the control reference for calling methods like `addPromptResponse()` and `executePrompt()`.

---

## promptRequest

Fires when the user submits a prompt (via the send button, pressing Enter, or `executePrompt()`). The event argument `args.prompt` contains the submitted text.

```razor
@Html.EJS().AIAssistView("aiAssistView")
    .PromptRequest("onPromptRequest")
    .Created("onCreated")
    .Render()
```

```javascript
function onPromptRequest(args) {
    // args.prompt — the submitted prompt string
    setTimeout(function () {
        var defaultResponse = 'Connect to your AI service for real-time responses.';
        assistObj.addPromptResponse(defaultResponse);
    }, 2000);
}
```

**Typical pattern — call server AI endpoint:**

```javascript
function onPromptRequest(args) {
    fetch('/Home/GetAIResponse', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ prompt: args.prompt })
    })
    .then(r => r.json())
    .then(responseText => {
        assistObj.addPromptResponse(responseText.trim() || 'No response received.');
    })
    .catch(() => {
        assistObj.addPromptResponse('⚠️ Error connecting to AI service.');
    });
}
```

---

## promptChanged

Fires whenever the text in the prompt textarea changes (on each keystroke). Use this for real-time validation, character counts, or dynamic suggestion updates.

```razor
@Html.EJS().AIAssistView("aiAssistView")
    .PromptChanged("onPromptChanged")
    .PromptRequest("onPromptRequest")
    .Created("onCreated")
    .Render()
```

```javascript
function onCreated() { assistObj = this; }

function onPromptChanged(args) {
    // args contains updated prompt text
    // e.g., live character counter, enable/disable send button
}

function onPromptRequest(args) {
    setTimeout(function () {
        assistObj.addPromptResponse('Connect to your AI service for real-time responses.');
    }, 2000);
}
```

---

## Attachment Events

All attachment events require `EnableAttachments(true)` and `AttachmentSettings` with `SaveUrl` and `RemoveUrl`.

### beforeAttachmentUpload

Fires before a file upload begins. Use to validate, cancel, or modify the upload request.

```razor
@Html.EJS().AIAssistView("aiAssistView")
    .EnableAttachments(true)
    .BeforeAttachmentUpload("beforeAttachmentUpload")
    .AttachmentSettings(new AIAssistViewAttachmentSettings()
    {
        SaveUrl = @Url.Content("https://services.syncfusion.com/aspnet/production/api/FileUploader/Save"),
        RemoveUrl = @Url.Content("https://services.syncfusion.com/aspnet/production/api/FileUploader/Remove")
    })
    .PromptRequest("onPromptRequest")
    .Created("onCreated")
    .Render()
```

```javascript
function beforeAttachmentUpload(args) {
    // args.fileData — file details (name, size, type)
    // Set args.cancel = true to prevent upload
}
```

---

### attachmentUploadSuccess

Fires when a file has been successfully uploaded to the server.

```razor
@Html.EJS().AIAssistView("aiAssistView")
    .EnableAttachments(true)
    .AttachmentUploadSuccess("attachmentUploadSuccess")
    .AttachmentSettings(new AIAssistViewAttachmentSettings()
    {
        SaveUrl = @Url.Content("https://services.syncfusion.com/aspnet/production/api/FileUploader/Save"),
        RemoveUrl = @Url.Content("https://services.syncfusion.com/aspnet/production/api/FileUploader/Remove")
    })
    .PromptRequest("onPromptRequest")
    .Created("onCreated")
    .Render()
```

```javascript
function attachmentUploadSuccess(args) {
    // args.file — uploaded file details
    // args.response — server response
}
```

---

### attachmentUploadFailure

Fires when a file upload fails (network error, server rejection, size exceeded, etc.).

```razor
@Html.EJS().AIAssistView("aiAssistView")
    .EnableAttachments(true)
    .AttachmentUploadFailure("attachmentUploadFailure")
    .AttachmentSettings(new AIAssistViewAttachmentSettings()
    {
        SaveUrl = @Url.Content("https://services.syncfusion.com/aspnet/production/api/FileUploader/Save"),
        RemoveUrl = @Url.Content("https://services.syncfusion.com/aspnet/production/api/FileUploader/Remove")
    })
    .PromptRequest("onPromptRequest")
    .Created("onCreated")
    .Render()
```

```javascript
function attachmentUploadFailure(args) {
    // args.file — file that failed
    // args.e — error details
    console.error('Upload failed for:', args.file.name);
}
```

---

### attachmentRemoved

Fires when the user removes an attached file from the pending list.

```razor
@Html.EJS().AIAssistView("aiAssistView")
    .EnableAttachments(true)
    .AttachmentRemoved("attachmentRemoved")
    .AttachmentSettings(new AIAssistViewAttachmentSettings()
    {
        SaveUrl = @Url.Content("https://services.syncfusion.com/aspnet/production/api/FileUploader/Save"),
        RemoveUrl = @Url.Content("https://services.syncfusion.com/aspnet/production/api/FileUploader/Remove")
    })
    .PromptRequest("onPromptRequest")
    .Created("onCreated")
    .Render()
```

```javascript
function attachmentRemoved(args) {
    // args.file — removed file details
}
```

---

### attachmentClick

Fires when the user clicks on an already-attached file preview. Wire it via `AttachmentSettings.AttachmentClick`.

```razor
@Html.EJS().AIAssistView("aiAssistView")
    .EnableAttachments(true)
    .AttachmentSettings(new AIAssistViewAttachmentSettings()
    {
        SaveUrl = @Url.Content("https://services.syncfusion.com/aspnet/production/api/FileUploader/Save"),
        RemoveUrl = @Url.Content("https://services.syncfusion.com/aspnet/production/api/FileUploader/Remove"),
        AttachmentClick = "attachmentClick"
    })
    .PromptRequest("onPromptRequest")
    .Created("onCreated")
    .Render()
```

```javascript
function attachmentClick(args) {
    // Use to preview, open, or download the clicked file
    // args.file — file details
}
```
