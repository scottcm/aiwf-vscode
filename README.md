# AIWF VS Code Extension

A Visual Studio Code extension for the AI Workflow Engine (aiwf). It enables developers to run and monitor multi-phase AI-assisted workflows directly from the editor.

## Overview
The extension communicates with the engine via subprocess calls and consumes JSON output. Prompts and AI responses are passed using disk files because these artifacts contain metadata, templates, and multi-file bundles unsuitable for stdin streaming.

## Features (Planned/Phased)
- New workflow initialization
- Run full workflow or individual phases
- Interactive mode support (prompt/response file workflow)
- Session-aware UI elements (TreeView, status bar)
- Log and artifact viewing

## Requirements
- aiwf CLI on PATH
- Python 3.11+
- Node.js for extension development

## Commands
AIWF: New Workflow  
AIWF: Run Full Workflow  
AIWF: Run Planning Phase  
AIWF: Run Generation Phase  
AIWF: Run Review Phase  
AIWF: Run Revision Phase  
AIWF: Resume Workflow

## Architecture
VS Code Extension → (subprocess) → aiwf CLI → engine core

## Status
Extension development begins after engine CLI contract stabilizes.

