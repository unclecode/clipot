# Clipot - Project Overview

> Polish your thoughts with AI - Smart clipboard assistant PWA

## Quick Reference

| Attribute | Value |
|-----------|-------|
| **Project Name** | Clipot |
| **Type** | Progressive Web App (PWA) |
| **Version** | v1.2.0 |
| **Status** | Production-ready |
| **Primary Tech** | Vanilla JavaScript, Anthropic Claude API |

## Description

Clipot is a Progressive Web App that uses AI to polish and refine text. Designed as a mobile-first application with offline support, it enables users to quickly improve their writing with a single tap. The app integrates directly with Anthropic's Claude API for text refinement.

## Key Features

- **Auto-process on paste**: Automatically refines text when pasted
- **Language support**: Maintains input language or forces specific output language
- **Offline capable**: Works offline after first load via Service Worker
- **Installable**: Add to home screen on iOS/Android as a native-like app
- **Zero dependencies**: Pure vanilla JavaScript, no build process required

## Architecture

```
Single-Page Application (no server required)
├── index.html (All-in-one: HTML + CSS + JavaScript)
├── manifest.json (PWA configuration)
├── service-worker.js (Offline support)
└── icons (192x192, 512x512 SVG)
```

**Data Flow:**
1. User pastes or inputs text
2. Text sent to Anthropic Claude API
3. Refined text returned and displayed
4. Auto-copied to clipboard

## Subprojects

| Subproject | Status | Description |
|------------|--------|-------------|
| [main](./main/INDEX.md) | Active | Core PWA application |

## Active Subproject

**Current Focus:** `main` (the core application)

## High-Level TODOs

- [ ] Consider adding more language models (GPT, etc.)
- [ ] Add text history feature
- [ ] Explore desktop PWA optimizations

## External Dependencies

| Dependency | Purpose | Notes |
|------------|---------|-------|
| Anthropic API | Text refinement AI | Requires user API token |

## Quick Links

- [Subproject: main](./main/INDEX.md)
- [Workflow Guide](./WORKFLOW.md)
- [Principles](./PRINCIPLES.md)
- [Lessons Learned](./LESSONS.md)
