# Templates — Syncfusion ASP.NET MVC AI AssistView

## Table of Contents
- [Banner Template](#banner-template)
- [Prompt Item Template](#prompt-item-template)
- [Response Item Template](#response-item-template)
- [Prompt Suggestion Item Template](#prompt-suggestion-item-template)
- [Footer Template](#footer-template)

All templates use **JsRender** syntax (`type="text/x-jsrender"`) and are referenced by their script element ID (e.g., `"#bannerContent"`).

---

## Banner Template

Use `BannerTemplate` to display welcome content, branding, or onboarding text at the top of the conversation area. Visible before any prompts are submitted.

```razor
@using Syncfusion.EJ2.InteractiveChat

<div class="aiassist-container" style="height: 350px; width: 650px;">
    @Html.EJS().AIAssistView("aiAssistView")
        .BannerTemplate("#bannerContent")
        .PromptRequest("onPromptRequest")
        .Created("onCreated")
        .Render()
</div>

<script>
    var assistObj;
    function onCreated() { assistObj = this; }
    function onPromptRequest(args) {
        setTimeout(function () {
            assistObj.addPromptResponse('Connect to your AI service for real-time responses.');
        }, 2000);
    }
</script>

<!-- JsRender template -->
<script id="bannerContent" type="text/x-jsrender">
    <div class="banner-content">
        <div class="e-icons e-assistview-icon"></div>
        <h3>AI Assistance</h3>
        <div>Your everyday AI companion.</div>
    </div>
</script>

<style>
    .aiassist-container .e-view-container { margin: auto; }
    .aiassist-container .e-banner-view { margin-left: 0; }
    .banner-content .e-assistview-icon:before { font-size: 35px; }
    .banner-content { text-align: center; }
</style>
```

> **Positioning:** The banner sits at the top of the prompt/response area. It disappears once the first prompt is submitted.

---

## Prompt Item Template

Use `PromptItemTemplate` to fully customize the display of each user prompt bubble. Template context exposes: `${prompt}`, `${toolbarItems}`, `${index}`.

```razor
@using Syncfusion.EJ2.InteractiveChat
@using Newtonsoft.Json

@{
    var promptsData = new[]
    {
        new {
            prompt = "What is AI?",
            response = "<div>AI stands for Artificial Intelligence, enabling machines to mimic human intelligence.</div>",
            suggestionData = new List<string>()
        }
    };
    var promptsJson = Html.Raw(JsonConvert.SerializeObject(promptsData));
}

<div class="aiassist-container" style="height: 350px; width: 650px;">
    @Html.EJS().AIAssistView("aiAssistView")
        .Prompts(promptsData)
        .PromptItemTemplate("#promptItemTemplate")
        .PromptRequest("onPromptRequest")
        .Created("onCreated")
        .Render()
</div>

<script>
    var assistObj;
    var prompts = @Html.Raw(promptsJson);
    function onCreated() { assistObj = this; }
    function onPromptRequest(args) {
        setTimeout(function () {
            var found = prompts.find(p => p.prompt === args.prompt);
            assistObj.addPromptResponse(found ? found.response : 'Default response.');
        }, 2000);
    }
</script>

<script id="promptItemTemplate" type="text/x-jsrender">
    <div class="promptItemContent">
        <div class="prompt-header">
            You
            <span class="e-icons e-user"></span>
        </div>
        <div class="content">${prompt}</div>
    </div>
</script>

<style>
    .promptItemContent {
        display: flex;
        flex-direction: column;
        gap: 10px;
        align-items: flex-end;
        margin-right: 20px;
    }
    .promptItemContent .prompt-header {
        font-size: 20px;
        font-weight: bold;
        display: flex;
        align-items: center;
    }
    .promptItemContent .prompt-header span { margin-left: 10px; }
    .promptItemContent .content { margin-right: 35px; }
</style>
```

---

## Response Item Template

Use `ResponseItemTemplate` to customize the display of each AI response bubble. Template context exposes: `${prompt}`, `${response}`, `${index}`, `${toolbarItems}`, `${output}`.

```razor
@using Syncfusion.EJ2.InteractiveChat
@using Newtonsoft.Json

@{
    var promptsData = new[]
    {
        new {
            prompt = "What is AI?",
            response = "<div>AI stands for Artificial Intelligence, enabling machines to mimic human intelligence.</div>",
            suggestionData = new List<string>()
        }
    };
    var promptsJson = Html.Raw(JsonConvert.SerializeObject(promptsData));
}

<div class="aiassist-container" style="height: 400px; width: 650px;">
    @Html.EJS().AIAssistView("aiAssistView")
        .Prompts(promptsData)
        .ResponseItemTemplate("#responseItemTemplate")
        .PromptRequest("onPromptRequest")
        .Created("onCreated")
        .Render()
</div>

<script>
    var assistObj;
    var prompts = @Html.Raw(promptsJson);
    function onCreated() { assistObj = this; }
    function onPromptRequest(args) {
        setTimeout(function () {
            var found = prompts.find(p => p.prompt === args.prompt);
            assistObj.addPromptResponse(found ? found.response : 'Default response.');
        }, 2000);
    }
</script>

<script id="responseItemTemplate" type="text/x-jsrender">
    <div class="responseItemContent">
        <div class="response-header">
            <span class="e-icons e-assistview-icon"></span>
            AI Assist
        </div>
        <div class="responseContent">${response}</div>
    </div>
</script>

<style>
    .responseItemContent {
        display: flex;
        flex-direction: column;
        gap: 10px;
        margin-left: 20px;
    }
    .responseItemContent .response-header {
        font-size: 20px;
        font-weight: bold;
        display: flex;
        align-items: center;
    }
    .responseItemContent .responseContent { margin-left: 35px; }
    .responseItemContent .response-header .e-assistview-icon:before { margin-right: 10px; }
    .aiassist-container .e-response-item-template .e-toolbar-items { margin-left: 35px; }
</style>
```

---

## Prompt Suggestion Item Template

Use `PromptSuggestionItemTemplate` to customize the appearance of suggestion chips. Template context exposes: `${index}`, `${promptSuggestion}`.

```razor
@using Syncfusion.EJ2.InteractiveChat

@{
    var defaultSuggestions = new string[] {
        "Best practices for clean, maintainable code?",
        "How to optimize code editor for speed?"
    };
}

<div class="aiassist-container" style="height: 350px; width: 650px;">
    @Html.EJS().AIAssistView("aiAssistView")
        .PromptSuggestions(defaultSuggestions)
        .PromptSuggestionItemTemplate("#promptSuggestionItemTemplate")
        .PromptRequest("onPromptRequest")
        .Created("onCreated")
        .Render()
</div>

<script>
    var assistObj;
    function onCreated() { assistObj = this; }
    function onPromptRequest(args) {
        setTimeout(function () {
            assistObj.addPromptResponse('Connect to your AI service for real-time responses.');
        }, 2000);
    }
</script>

<script id="promptSuggestionItemTemplate" type="text/x-jsrender">
    <div class="suggestion-item active">
        <span class="e-icons e-circle-info"></span>
        <div class="content">${promptSuggestion}</div>
    </div>
</script>

<style>
    /* Remove default suggestion item styling to use custom */
    .e-aiassistview .e-views .e-suggestions li {
        padding: 0;
        border: none;
        box-shadow: none;
    }
    .suggestion-item {
        display: flex;
        align-items: center;
        background-color: #686868;
        color: white;
        padding: 4px 10px;
        opacity: 0.8;
        gap: 5px;
        height: 35px;
        border-radius: 5px;
    }
    .suggestion-item .content {
        text-overflow: ellipsis;
        white-space: nowrap;
        overflow: hidden;
    }
</style>
```

---

## Footer Template

Use `FooterTemplate` to replace the default footer (textarea + send button) with a completely custom input area. Wire the custom submit button manually via JavaScript.

```razor
@using Syncfusion.EJ2.InteractiveChat
@using Newtonsoft.Json

@{
    var suggestions = new string[] {
        "How do I prioritize my tasks?",
        "How can I improve my time management skills?"
    };
    var prompts = new[]
    {
        new { prompt = "How do I prioritize my tasks?",
              response = "Prioritize tasks by urgency and impact: tackle high-impact tasks first.", suggestionData = new List<string>() },
        new { prompt = "How can I improve my time management skills?",
              response = "Set clear goals, use a planner, prioritize tasks, and minimize distractions.", suggestionData = new List<string>() }
    };
    var promptsJson = Html.Raw(JsonConvert.SerializeObject(prompts));
}

<div class="aiassist-container" style="height: 350px; width: 650px;">
    @Html.EJS().AIAssistView("aiAssistView")
        .FooterTemplate("#footerContent")
        .PromptRequest("onPromptRequest")
        .Created("onCreated")
        .Render()
</div>

<script>
    var assistObj;
    var prompts = @Html.Raw(promptsJson);

    function onCreated() { assistObj = this; }

    function onPromptRequest() {
        setTimeout(function () {
            assistObj.addPromptResponse('Connect to your AI service for real-time responses.');
        }, 2000);
    }

    // Wire custom send button
    document.addEventListener('click', function (event) {
        if (event.target && event.target.id === 'sendPrompt') {
            const textArea = document.getElementById('promptTextArea');
            if (textArea) {
                textArea.value = '';
                assistObj.addPromptResponse('Connect to your AI service for real-time responses.');
            }
        }
    });
</script>

<script id="footerContent" type="text/x-jsrender">
    <div class="custom-footer">
        <textarea id="promptTextArea" class="e-input" rows="2"
                  placeholder="Enter your prompt here"></textarea>
        <button id="sendPrompt" class="e-btn e-primary">Generate</button>
    </div>
</script>

<style>
    .custom-footer {
        display: flex;
        gap: 10px;
        padding: 10px;
        background-color: transparent;
    }
    #promptTextArea {
        width: 100%;
        padding: 10px;
        border-radius: 5px;
        border: 1px solid #ccc;
    }
    #sendPrompt {
        padding: 5px 15px;
        align-self: flex-end;
    }
</style>
```

> **Note:** When using `FooterTemplate`, the built-in `PromptRequest` event is not triggered by the custom button. You must call `assistObj.addPromptResponse()` directly from the custom button's click handler, or use `assistObj.executePrompt()` to trigger the event pipeline.
