# ASP.NET MVC UI Components Skills

Skills for Syncfusion ASP.NET MVC components, designed for use with AI coding assistants.

This repository contains 20+ AI-ready skill guides for working with Syncfusion ASP.NET MVC components. Each skill includes a `SKILL.md` file that AI coding assistants can read automatically, plus a `references/` subfolder with detailed documentation covering setup, usage patterns, customization, and troubleshooting.

## Quick Start

### Option 1: Using npx (Recommended)

```bash
npx skills add https://github.com/syncfusion/aspnetmvc-ui-components-skills
```

This will automatically add the skills to your workspace.

### Option 2: Manual Installation

**1. Clone this repository**
```bash
git clone https://github.com/syncfusion/aspnetmvc-ui-components-skills.git
```

**2. Add it to your VS Code workspace**

Open your `.code-workspace` file (or create one) and add this repo as a second root folder:
```json
{
  "folders": [
    { "path": "/path/to/your-aspnetmvc-app" },
    { "path": "/path/to/aspnetmvc-ui-components-skills" }
  ]
}
```

**3. Start asking questions**

Your AI assistant will automatically detect and apply the relevant skill based on your prompt:
```text
How do I add grouping to the Syncfusion Grid in ASP.NET MVC?
How do I configure the Scheduler for week view?
How do I apply a dark theme to Syncfusion ASP.NET MVC components?
```

No configuration required. Skills are loaded automatically from the workspace.

---

## Prerequisites

- An AI coding assistant that supports skills/context files (e.g., Syncfusion Code Studio, GitHub Copilot, Cursor, or similar tools)
- [.NET Framework 4.7+ or .NET 8+](https://www.microsoft.com/net/download)
- A Syncfusion license key ([free community license available](https://www.syncfusion.com/products/communitylicense))
- Basic knowledge of ASP.NET MVC views, controllers, and models.

## How These Skills Work

Each `SKILL.md` file contains a `description` field in its YAML frontmatter. AI coding assistants read this description to decide when to automatically apply a skill during a conversation. When you ask about a specific Syncfusion Component - for example, "How do I add sorting to my Grid?" - the AI assistant detects the match and loads the corresponding skill to guide its response.

You can also reference a skill explicitly by mentioning the component or control by name in your prompt.

### Example Prompts

```text
How do I bind data to the Syncfusion Grid in ASP.NET MVC?
```
→ The AI assistant loads the Grid skill and uses its getting-started and data-binding reference docs.

```text
Show me how to implement master-detail in the TreeGrid.
```
→ The AI assistant loads the TreeGrid skill.

### Using Reference Files

Each `references/` subfolder contains deeper implementation guides. When the AI assistant loads a skill, it can also pull in these files when you ask follow-up questions:

```text
Show me how to export the DataGrid to Excel.
```
→ The AI assistant uses `references/advanced-features.md` from the Grid skill for the detailed answer.

## Skill File Structure

Every skill folder follows this layout:

```text
skills/
└── syncfusion-aspnetmvc-<component>/
    ├── SKILL.md                  ← Loaded by AI assistant; contains When to Use, Component Overview, and navigation links
    └── references/
        ├── getting-started.md    ← Installation, setup, NuGet packages, basic configuration
        ├── advanced-features.md  ← In-depth feature guides and code samples
        └── ...                   ← Additional reference files per component
```

`SKILL.md` sections:
- **When to Use This Skill** - trigger phrases and scenarios that activate this skill
- **Component Overview** - NuGet package, namespace, key capabilities at a glance
- **Documentation and Navigation Guide** - links to all reference files in the skill

## Repository Structure

```text
README.md
skills/
    syncfusion-aspnetmvc-common/
    syncfusion-aspnetmvc-theme/
    syncfusion-aspnetmvc-license/
    syncfusion-aspnetmvc-richtexteditor/
    syncfusion-aspnetmvc-grid/
    syncfusion-aspnetmvc-scheduler/
    ... (15+ total skills)
```

## Skill Index

> **Tip:** Start with [Common](skills/syncfusion-aspnetmvc-common/SKILL.md) if you are setting up a new project. For all other tasks, find the skill that matches the specific component below.

### Foundation & Setup

- [Common](skills/syncfusion-aspnetmvc-common/SKILL.md) - installation, setup, HTML Helpers, script/style references, localization
- [Theme](skills/syncfusion-aspnetmvc-theme/SKILL.md) - Bootstrap, Material, Tailwind, Fluent themes; dark mode, size modes, icon integration
- [License](skills/syncfusion-aspnetmvc-license/SKILL.md) - license key registration, troubleshooting, error resolution
- [Security](skills/syncfusion-aspnetmvc-security/SKILL.md) - Content Security Policy (CSP) headers, nonces, inline scripts/styles protection

### Grids & Data Management

- [Grid](skills/syncfusion-aspnetmvc-grid/SKILL.md) - sorting, filtering, grouping, aggregates, editing, export, virtual/infinite scrolling
- [TreeGrid](skills/syncfusion-aspnetmvc-treegrid/SKILL.md) - hierarchical parent-child data, editing, virtual scrolling, export
- [Kanban](skills/syncfusion-aspnetmvc-kanban/SKILL.md) - task management, drag-drop, swimlane grouping, column-based layouts
- [Pivot Table](skills/syncfusion-aspnetmvc-pivot-table/SKILL.md) - pivot data analysis, OLAP/cube setup, drill-down, conditional formatting

### Scheduling & Project Management

- [Scheduler](skills/syncfusion-aspnetmvc-scheduler/SKILL.md) - calendar views, appointments, recurring events, resources, drag-drop, timezone handling
- [Gantt Chart](skills/syncfusion-aspnetmvc-gantt-chart/SKILL.md) - project timelines, task dependencies, resource allocation, filtering, export

### Editors & Content Components

- [Rich Text Editor](skills/syncfusion-aspnetmvc-richtexteditor/SKILL.md) - WYSIWYG and Markdown editing, toolbar, media insertion, smart features, paste cleanup
- [Block Editor](skills/syncfusion-aspnetmvc-blockeditor/SKILL.md) - block-based editing, block types (paragraph, heading, list, code, table), drag-drop, context menus
- [Markdown Converter](skills/syncfusion-aspnetmvc-markdown-converter/SKILL.md) - Markdown to HTML conversion, integration with Rich Text Editor

### Data Visualization

- [Diagram](skills/syncfusion-aspnetmvc-diagram/SKILL.md) - flowcharts, organizational charts, BPMN, UML, swimlane diagrams, mind maps, data binding
- [Barcode](skills/syncfusion-aspnetmvc-barcodes/SKILL.md) - Code39, Code128, Codabar, QR codes, DataMatrix generation and export

### User Interface & Navigation

- [Ribbon](skills/syncfusion-aspnetmvc-ribbon/SKILL.md) - Office-style ribbon with tabs, groups, buttons, dropdowns, backstage view, keytips

### AI & Conversational Components

- [AI AssistView](skills/syncfusion-aspnetmvc-ai-assistview/SKILL.md) - conversational AI chat, prompt suggestions, file attachments, speech-to-text, backend integrations (OpenAI, Gemini, Ollama)
- [Chat UI](skills/syncfusion-aspnetmvc-chat-ui/SKILL.md) - real-time messaging, bot integrations, typing indicators, mentions, file attachments, speech-to-text
- [Inline AI Assist](skills/syncfusion-aspnetmvc-inline-ai-assist/SKILL.md) - inline text editing, prompt-response UI, command popups, toolbar customization, response actions

### Accessibility & Input

- [Speech-to-Text](skills/syncfusion-aspnetmvc-speech-to-text/SKILL.md) - voice input, Web Speech API integration, speech recognition, transcription, error handling

---

## Getting Help

- Check the [Syncfusion ASP.NET MVC documentation](https://www.syncfusion.com/aspnet-mvc-ui-controls)
- Review skill reference files in the `references/` folders
- Visit [Syncfusion community forums](https://www.syncfusion.com/forums/aspnetmvc-js2)

---

## License

These skills are provided as educational resources for working with Syncfusion ASP.NET MVC components. Syncfusion components require a valid license key. To acquire a license, you can quote a purchase at https://www.syncfusion.com/sales/pricing.