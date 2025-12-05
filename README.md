# AIWF VS Code Extension

**A VS Code extension demonstrating UI integration patterns for AI workflow tooling**

> ⚠️ **Project Status: Active Development (Phase 1 – Foundation)**  
> This extension is part of a project demonstrating frontend integration, VS Code extension architecture, and TypeScript patterns for tool development.

---

## Overview

The AIWF VS Code Extension integrates with the **AI Workflow Engine** to provide in-editor commands, workflow management, and session tools for running multi-phase AI-assisted development workflows directly from VS Code.

The extension communicates with the `aiwf` CLI to orchestrate workflows that:
1. Plan code structure  
2. Generate implementation  
3. Review generated code  
4. Execute revision cycles  

This extension demonstrates practical VS Code Extension API usage, CLI subprocess management, and developer tool UX design.

---

## What This Extension Demonstrates

- **VS Code Extension Architecture**  
  Command registration, activation events, lifecycle management  
- **CLI Integration Patterns**  
  Subprocess management, structured output parsing, error handling  
- **Stateful UI Components**  
  Session tracking, TreeView providers, status bar indicators  
- **TypeScript Best Practices**  
  Modular service architecture, type safety, async/await patterns  
- **Developer Tool UX**  
  Contextual commands, workflow visualization, artifact navigation  

---

## Features (Planned)

### Workflow Commands
- **New Workflow** – Initialize a session with profile and entity configuration  
- **Run Full Workflow** – Execute full plan → generate → review → revise cycle  
- **Step-by-Step Execution** – Run individual phases (interactive mode)  
- **Resume Workflow** – Continue from saved checkpoint  

### Session Management
- View active and completed workflow sessions  
- Inspect workflow state and phase progression  
- Navigate between prompts, plans, and generated artifacts  
- Session history with resume capability  

### Editor Integration
- Status bar indicator for active session and current phase  
- TreeView for browsing sessions and artifacts  
- Quick navigation to workflow files  
- Output channel for CLI logs and progress  

---

## Requirements

- **AI Workflow Engine** installed and accessible on PATH  
  https://github.com/scottcm/ai-workflow-engine  
  Minimum version: 1.0.0
- Python 3.11+ (for the engine)  
- Node.js + npm (for extension development)  

The extension communicates with the engine via CLI subprocess calls. 
See the [Engine API Contract](https://github.com/scottcm/ai-workflow-engine/blob/main/API-CONTRACT.md) 
for the complete interface specification.

All AI provider configuration is handled by the **engine**, not the extension.

---

## Architecture Overview

```
UI Layer (Commands, TreeViews, Status Bar)
      ↓
Service Layer (CLI Integration, State Management)
      ↓
Model Layer (Session, Workflow, Config)
      ↓
Utils (Process Management, Path Resolution)
```

### Project Structure

```
aiwf-vscode-extension/
├── src/
│   ├── extension.ts               # Activation + command registration
│   ├── commands/
│   │   ├── workflow.ts            # Workflow lifecycle commands
│   │   └── session.ts             # Session management commands
│   ├── services/
│   │   ├── cli.ts                 # aiwf CLI subprocess integration
│   │   ├── config.ts              # Extension configuration
│   │   └── state.ts               # Session state tracking
│   ├── models/
│   │   ├── session.ts             # Session data model
│   │   └── workflow.ts            # Workflow state model
│   ├── ui/
│   │   ├── sessionTreeProvider.ts # TreeView for sessions
│   │   └── statusBar.ts           # Status bar integration
│   └── utils/
│       ├── process.ts             # Child process helpers
│       └── paths.ts               # Path resolution
├── test/
│   ├── unit/
│   └── integration/
├── package.json                   # Extension manifest
├── tsconfig.json                  # TypeScript configuration
├── .vscodeignore
└── README.md
```

---

## Development Setup

Clone and install dependencies:

```bash
git clone https://github.com/scottcm/aiwf-vscode-extension.git
cd aiwf-vscode-extension
npm install
```

Build:

```bash
npm run build
```

Run tests:

```bash
npm test
```

Launch the extension:

1. Open the project in VS Code  
2. Press `F5` to launch Extension Development Host  
3. Run commands via Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`)

---

## Commands (Planned)

| Command | Description |
|---------|-------------|
| `AIWF: New Workflow` | Initialize new workflow session |
| `AIWF: Run Full Workflow` | Execute complete workflow |
| `AIWF: Run Planning Phase` | Execute planning only |
| `AIWF: Run Generation Phase` | Execute generation only |
| `AIWF: Run Review Phase` | Execute review only |
| `AIWF: Run Revision Phase` | Execute revision only |
| `AIWF: Resume Workflow` | Continue existing session |
| `AIWF: Open Session Directory` | Navigate to session artifacts |
| `AIWF: List Active Sessions` | Show all active workflow sessions |

Each command maps to an `aiwf` CLI command as specified in the engine's API contract.

---

## Configuration (Planned)

- **aiwf.executablePath** – Path to aiwf CLI executable  
- **aiwf.defaultProfile** – Default workflow profile (e.g., `jpa-mt-domain`)  
- **aiwf.showStatusBar** – Show/hide status bar indicator  
- **aiwf.sessionStoragePath** – Custom session storage location  
- **aiwf.outputVerbosity** – CLI output detail level  

---

## Current Implementation Status

- [ ] Extension scaffold and manifest (`package.json`)  
- [ ] Command registration system  
- [ ] CLI service for `aiwf` subprocess integration  
- [ ] Session state management service  
- [ ] TreeView provider for session navigation  
- [ ] Status bar integration  
- [ ] Output channel for workflow logs  
- [ ] Configuration handling  

---

## Relationship to the AI Workflow Engine

This extension is a companion to the **AI Workflow Engine**:  
https://github.com/scottcm/ai-workflow-engine

**Division of Responsibilities:**
- **Engine** – All workflow orchestration, AI provider integration, state persistence  
- **Extension** – UI/UX layer, command surface, editor integration  

The extension communicates with the engine exclusively through its CLI interface, following the workflow contract defined in the engine's `API-CONTRACT.md`.

---

## Profile Support

The extension supports all engine profiles through the CLI interface. Users can:
- Select profile via command palette
- Configure default profile in settings
- Switch profiles between sessions

**Current Engine Profiles:**
- `jpa-mt-domain` – Multi-tenant JPA domain layer (Entity + Repository)

**Planned Engine Profiles:**
- `jpa-mt-vertical` – Multi-tenant full-stack generation

The extension automatically adapts to available profiles and their capabilities.

---

## Execution Modes

### Interactive Mode (Current Focus)

1. Extension triggers CLI to generate prompt files  
2. User manually copies prompt to AI web interface  
3. User pastes AI response back into designated file  
4. Extension triggers CLI to process response  
5. Repeat for each phase  

**Extension UX:**
- Opens generated prompt file automatically
- Provides "Copy to Clipboard" button
- Shows clear instructions for paste-back workflow
- Tracks phase progression
- Validates responses before processing

### Automated Mode (Future)

1. Extension triggers CLI with `--automated` flag  
2. CLI directly calls AI provider APIs  
3. Results streamed back to extension  
4. Minimal user interaction required  

The extension UI adapts to whichever mode is active for the session.

---

## Session Management

Sessions are stored in filesystem with the following structure:

```
.aiwf/
  sessions/
    <session-id>/
      session.json          # Session metadata and state
      prompts/              # Generated prompts for each phase
      plans/                # AI-generated plans
      artifacts/            # Generated code and outputs
      responses/            # AI responses (interactive mode)
```

The extension provides:
- TreeView for browsing session hierarchy
- Quick navigation to any session file
- Session state indicators (active, completed, failed)
- Resume capability for interrupted sessions

---

## Project Goals

- **VS Code Extension API proficiency**  
- **TypeScript architectural patterns**  
- **Robust subprocess integration**  
- **Developer-friendly UX**  
- **Modular, testable components**  

---

## Tech Stack

- **TypeScript 5.x**  
- **VS Code Extension API**  
- **Node.js**  
- **child_process** for CLI integration  
- **VS Code Test Framework**  

---

## License

MIT License
