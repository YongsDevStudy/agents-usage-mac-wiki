---
id: swiftpm-modules-sqlite-data-flow-tdd-v1-0
type: excalidraw
excalidraw-plugin: parsed
tags:
  - excalidraw
  - tdd
  - sqlite
  - swiftpm
  - erd
  - architecture
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[syntheses/tdd-agents-usage-mac]]"
  - "[[concepts/adr-core-decisions]]"
---
==⚠ Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==

# Excalidraw Data
## Text Elements
AgentsParsers (Adapters) ^grp-title-grp-parsers

AgentsCore (GRDB + Engines) ^grp-title-grp-core

SQLite WAL Tables (9 Tables) ^grp-title-grp-db

AgentsApp & AgentsWidget ^grp-title-grp-app

GeminiCLIParser
(.jsonl $set / .json) ^p-gem-text

ClaudeCodeParser
(requestId / Sidechain) ^p-cld-text

CodexCLIParser
(last_token_usage) ^p-cdx-text

actor IngestionCoordinator
(1.5s Debounce + Catch-up) ^c-coord-text

PricingEngine &
EfficiencyScorer ^c-calc-text

projects -> sessions
-> user_turns ^d-sess-text

api_requests (SSOT)
-> daily_rollups ^d-req-text

ingestion_cursors,
rate_limit_events, errors ^d-aux-text

AgentsApp
(MenuBarExtra + Charts) ^a-ui-text

AgentsWidget
(WidgetReloadBudgetGuard) ^a-wgt-text

ParsedLogDelta ^edge-0-p-gem-to-c-coord-label

ParsedLogDelta ^edge-1-p-cld-to-c-coord-label

ParsedLogDelta ^edge-2-p-cdx-to-c-coord-label

price & score ^edge-3-c-coord-to-c-calc-label

upsert turns ^edge-4-c-calc-to-d-sess-label

upsert + rollup ^edge-5-c-calc-to-d-req-label

cursors & limits ^edge-6-c-coord-to-d-aux-label

GRDB read ^edge-7-d-req-to-a-ui-label

80c mem preview ^edge-8-d-sess-to-a-ui-label

widget-snapshot.json ^edge-9-c-calc-to-a-wgt-label

## Element Links
^p-gem: [[entities/gemini-cli]]
^p-cld: [[entities/claude-code]]
^p-cdx: [[entities/codex-cli]]
^c-coord: [[concepts/adr-core-decisions]]
^c-calc: [[concepts/prompt-efficiency-scoring]]
^d-sess: [[syntheses/tdd-agents-usage-mac]]
^d-req: [[syntheses/tdd-agents-usage-mac]]
^d-aux: [[syntheses/tdd-agents-usage-mac]]
^a-ui: [[concepts/macos-menubar-widget-architecture]]
^a-wgt: [[concepts/macos-menubar-widget-architecture]]

%%
## Drawing
```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [
    {
      "id": "grp-grp-parsers",
      "type": "rectangle",
      "x": 56.0,
      "y": 36.0,
      "width": 289.4,
      "height": 364.0,
      "angle": 0,
      "strokeColor": "#1971c2",
      "backgroundColor": "#f8f9fa",
      "fillStyle": "solid",
      "strokeWidth": 1,
      "strokeStyle": "dashed",
      "roughness": 1,
      "opacity": 60,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 1866823705,
      "version": 1,
      "versionNonce": 1866823706,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false
    },
    {
      "id": "grp-title-grp-parsers",
      "type": "text",
      "x": 68.0,
      "y": 46.0,
      "width": 194.9,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#1971c2",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 48973995,
      "version": 1,
      "versionNonce": 48973996,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "AgentsParsers (Adapters)",
      "originalText": "AgentsParsers (Adapters)",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "grp-grp-core",
      "type": "rectangle",
      "x": 407.4,
      "y": 94.0,
      "width": 645.6,
      "height": 190.0,
      "angle": 0,
      "strokeColor": "#6741d9",
      "backgroundColor": "#f8f9fa",
      "fillStyle": "solid",
      "strokeWidth": 1,
      "strokeStyle": "dashed",
      "roughness": 1,
      "opacity": 60,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 1573231666,
      "version": 1,
      "versionNonce": 1573231667,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false
    },
    {
      "id": "grp-title-grp-core",
      "type": "text",
      "x": 419.4,
      "y": 104.0,
      "width": 219.2,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#6741d9",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 1626308766,
      "version": 1,
      "versionNonce": 1626308767,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "AgentsCore (GRDB + Engines)",
      "originalText": "AgentsCore (GRDB + Engines)",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "grp-grp-db",
      "type": "rectangle",
      "x": 786.7,
      "y": 36.0,
      "width": 654.8,
      "height": 306.0,
      "angle": 0,
      "strokeColor": "#2f9e44",
      "backgroundColor": "#f8f9fa",
      "fillStyle": "solid",
      "strokeWidth": 1,
      "strokeStyle": "dashed",
      "roughness": 1,
      "opacity": 60,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 1673134678,
      "version": 1,
      "versionNonce": 1673134679,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false
    },
    {
      "id": "grp-title-grp-db",
      "type": "text",
      "x": 798.7,
      "y": 46.0,
      "width": 227.4,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#2f9e44",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 146876679,
      "version": 1,
      "versionNonce": 146876680,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "SQLite WAL Tables (9 Tables)",
      "originalText": "SQLite WAL Tables (9 Tables)",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "grp-grp-app",
      "type": "rectangle",
      "x": 1156.7,
      "y": 152.0,
      "width": 659.4,
      "height": 248.0,
      "angle": 0,
      "strokeColor": "#0c8599",
      "backgroundColor": "#f8f9fa",
      "fillStyle": "solid",
      "strokeWidth": 1,
      "strokeStyle": "dashed",
      "roughness": 1,
      "opacity": 60,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 1733964785,
      "version": 1,
      "versionNonce": 1733964786,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false
    },
    {
      "id": "grp-title-grp-app",
      "type": "text",
      "x": 1168.7,
      "y": 162.0,
      "width": 194.9,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#0c8599",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 366168214,
      "version": 1,
      "versionNonce": 366168215,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "AgentsApp & AgentsWidget",
      "originalText": "AgentsApp & AgentsWidget",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "p-gem",
      "type": "rectangle",
      "x": 89.2,
      "y": 80.0,
      "width": 222.9,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#1971c2",
      "backgroundColor": "#d0ebff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 196689812,
      "version": 1,
      "versionNonce": 196689813,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "p-gem-text",
          "type": "text"
        },
        {
          "id": "edge-0-p-gem-to-c-coord",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[entities/gemini-cli]]",
      "locked": false
    },
    {
      "id": "p-gem-text",
      "type": "text",
      "x": 103.2,
      "y": 92.0,
      "width": 194.9,
      "height": 40.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 1915342097,
      "version": 1,
      "versionNonce": 1915342098,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "GeminiCLIParser\n(.jsonl $set / .json)",
      "originalText": "GeminiCLIParser\n(.jsonl $set / .json)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "p-gem",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "p-cld",
      "type": "rectangle",
      "x": 80.0,
      "y": 196.0,
      "width": 241.4,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#1971c2",
      "backgroundColor": "#d0ebff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 615421091,
      "version": 1,
      "versionNonce": 615421092,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "p-cld-text",
          "type": "text"
        },
        {
          "id": "edge-1-p-cld-to-c-coord",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[entities/claude-code]]",
      "locked": false
    },
    {
      "id": "p-cld-text",
      "type": "text",
      "x": 94.0,
      "y": 208.0,
      "width": 213.4,
      "height": 40.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 1156896754,
      "version": 1,
      "versionNonce": 1156896755,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "ClaudeCodeParser\n(requestId / Sidechain)",
      "originalText": "ClaudeCodeParser\n(requestId / Sidechain)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "p-cld",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "p-cdx",
      "type": "rectangle",
      "x": 103.2,
      "y": 312.0,
      "width": 195.0,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#1971c2",
      "backgroundColor": "#d0ebff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 2021016294,
      "version": 1,
      "versionNonce": 2021016295,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "p-cdx-text",
          "type": "text"
        },
        {
          "id": "edge-2-p-cdx-to-c-coord",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[entities/codex-cli]]",
      "locked": false
    },
    {
      "id": "p-cdx-text",
      "type": "text",
      "x": 117.2,
      "y": 324.0,
      "width": 167.0,
      "height": 40.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 51338103,
      "version": 1,
      "versionNonce": 51338104,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "CodexCLIParser\n(last_token_usage)",
      "originalText": "CodexCLIParser\n(last_token_usage)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "p-cdx",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "c-coord",
      "type": "rectangle",
      "x": 431.4,
      "y": 196.0,
      "width": 269.3,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#6741d9",
      "backgroundColor": "#e5dbff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 898254607,
      "version": 1,
      "versionNonce": 898254608,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "c-coord-text",
          "type": "text"
        },
        {
          "id": "edge-0-p-gem-to-c-coord",
          "type": "arrow"
        },
        {
          "id": "edge-1-p-cld-to-c-coord",
          "type": "arrow"
        },
        {
          "id": "edge-2-p-cdx-to-c-coord",
          "type": "arrow"
        },
        {
          "id": "edge-3-c-coord-to-c-calc",
          "type": "arrow"
        },
        {
          "id": "edge-6-c-coord-to-d-aux",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/adr-core-decisions]]",
      "locked": false
    },
    {
      "id": "c-coord-text",
      "type": "text",
      "x": 445.4,
      "y": 208.0,
      "width": 241.3,
      "height": 40.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 316466807,
      "version": 1,
      "versionNonce": 316466808,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "actor IngestionCoordinator\n(1.5s Debounce + Catch-up)",
      "originalText": "actor IngestionCoordinator\n(1.5s Debounce + Catch-up)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "c-coord",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "c-calc",
      "type": "rectangle",
      "x": 852.5,
      "y": 138.0,
      "width": 176.5,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#2f9e44",
      "backgroundColor": "#d3f9d8",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 2072435282,
      "version": 1,
      "versionNonce": 2072435283,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "c-calc-text",
          "type": "text"
        },
        {
          "id": "edge-3-c-coord-to-c-calc",
          "type": "arrow"
        },
        {
          "id": "edge-4-c-calc-to-d-sess",
          "type": "arrow"
        },
        {
          "id": "edge-5-c-calc-to-d-req",
          "type": "arrow"
        },
        {
          "id": "edge-9-c-calc-to-a-wgt",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/prompt-efficiency-scoring]]",
      "locked": false
    },
    {
      "id": "c-calc-text",
      "type": "text",
      "x": 866.5,
      "y": 150.0,
      "width": 148.5,
      "height": 40.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 5577979,
      "version": 1,
      "versionNonce": 5577980,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "PricingEngine &\nEfficiencyScorer",
      "originalText": "PricingEngine &\nEfficiencyScorer",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "c-calc",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "d-sess",
      "type": "rectangle",
      "x": 1203.9,
      "y": 80.0,
      "width": 213.6,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#0c8599",
      "backgroundColor": "#c5f6fa",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 1635650385,
      "version": 1,
      "versionNonce": 1635650386,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "d-sess-text",
          "type": "text"
        },
        {
          "id": "edge-4-c-calc-to-d-sess",
          "type": "arrow"
        },
        {
          "id": "edge-8-d-sess-to-a-ui",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[syntheses/tdd-agents-usage-mac]]",
      "locked": false
    },
    {
      "id": "d-sess-text",
      "type": "text",
      "x": 1217.9,
      "y": 92.0,
      "width": 185.6,
      "height": 40.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 329558863,
      "version": 1,
      "versionNonce": 329558864,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "projects -> sessions\n-> user_turns",
      "originalText": "projects -> sessions\n-> user_turns",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "d-sess",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "d-req",
      "type": "rectangle",
      "x": 1208.5,
      "y": 196.0,
      "width": 204.3,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#0c8599",
      "backgroundColor": "#c5f6fa",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 271921926,
      "version": 1,
      "versionNonce": 271921927,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "d-req-text",
          "type": "text"
        },
        {
          "id": "edge-5-c-calc-to-d-req",
          "type": "arrow"
        },
        {
          "id": "edge-7-d-req-to-a-ui",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[syntheses/tdd-agents-usage-mac]]",
      "locked": false
    },
    {
      "id": "d-req-text",
      "type": "text",
      "x": 1222.5,
      "y": 208.0,
      "width": 176.3,
      "height": 40.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 472060206,
      "version": 1,
      "versionNonce": 472060207,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "api_requests (SSOT)\n-> daily_rollups",
      "originalText": "api_requests (SSOT)\n-> daily_rollups",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "d-req",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "d-aux",
      "type": "rectangle",
      "x": 810.7,
      "y": 254.0,
      "width": 260.0,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#c92a2a",
      "backgroundColor": "#ffe3e3",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 1028359386,
      "version": 1,
      "versionNonce": 1028359387,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "d-aux-text",
          "type": "text"
        },
        {
          "id": "edge-6-c-coord-to-d-aux",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[syntheses/tdd-agents-usage-mac]]",
      "locked": false
    },
    {
      "id": "d-aux-text",
      "type": "text",
      "x": 824.7,
      "y": 266.0,
      "width": 232.0,
      "height": 40.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 784788196,
      "version": 1,
      "versionNonce": 784788197,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "ingestion_cursors,\nrate_limit_events, errors",
      "originalText": "ingestion_cursors,\nrate_limit_events, errors",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "d-aux",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "a-ui",
      "type": "rectangle",
      "x": 1550.7,
      "y": 196.0,
      "width": 241.4,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#e8590c",
      "backgroundColor": "#ffe8cc",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 1953353351,
      "version": 1,
      "versionNonce": 1953353352,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "a-ui-text",
          "type": "text"
        },
        {
          "id": "edge-7-d-req-to-a-ui",
          "type": "arrow"
        },
        {
          "id": "edge-8-d-sess-to-a-ui",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/macos-menubar-widget-architecture]]",
      "locked": false
    },
    {
      "id": "a-ui-text",
      "type": "text",
      "x": 1564.7,
      "y": 208.0,
      "width": 213.4,
      "height": 40.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 2102903999,
      "version": 1,
      "versionNonce": 2102904000,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "AgentsApp\n(MenuBarExtra + Charts)",
      "originalText": "AgentsApp\n(MenuBarExtra + Charts)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "a-ui",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "a-wgt",
      "type": "rectangle",
      "x": 1180.7,
      "y": 312.0,
      "width": 260.0,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#6741d9",
      "backgroundColor": "#e5dbff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "seed": 474146964,
      "version": 1,
      "versionNonce": 474146965,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "a-wgt-text",
          "type": "text"
        },
        {
          "id": "edge-9-c-calc-to-a-wgt",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/macos-menubar-widget-architecture]]",
      "locked": false
    },
    {
      "id": "a-wgt-text",
      "type": "text",
      "x": 1194.7,
      "y": 324.0,
      "width": 232.0,
      "height": 40.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 1975014552,
      "version": 1,
      "versionNonce": 1975014553,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "AgentsWidget\n(WidgetReloadBudgetGuard)",
      "originalText": "AgentsWidget\n(WidgetReloadBudgetGuard)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "a-wgt",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-0-p-gem-to-c-coord",
      "type": "arrow",
      "x": 312.1,
      "y": 112.0,
      "width": 119.2,
      "height": 116.0,
      "angle": 0,
      "strokeColor": "#495057",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "seed": 1019125071,
      "version": 1,
      "versionNonce": 1019125072,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-0-p-gem-to-c-coord-label",
          "type": "text"
        }
      ],
      "updated": 1,
      "link": null,
      "locked": false,
      "points": [
        [
          0,
          0
        ],
        [
          119.2,
          116.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "p-gem",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "c-coord",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-0-p-gem-to-c-coord-label",
      "type": "text",
      "x": 314.9,
      "y": 161.2,
      "width": 113.7,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#343a40",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 1868080863,
      "version": 1,
      "versionNonce": 1868080864,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "ParsedLogDelta",
      "originalText": "ParsedLogDelta",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-0-p-gem-to-c-coord",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-1-p-cld-to-c-coord",
      "type": "arrow",
      "x": 321.4,
      "y": 228.0,
      "width": 110.0,
      "height": 0.0,
      "angle": 0,
      "strokeColor": "#495057",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "seed": 1350341838,
      "version": 1,
      "versionNonce": 1350341839,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-1-p-cld-to-c-coord-label",
          "type": "text"
        }
      ],
      "updated": 1,
      "link": null,
      "locked": false,
      "points": [
        [
          0,
          0
        ],
        [
          110.0,
          0.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "p-cld",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "c-coord",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-1-p-cld-to-c-coord-label",
      "type": "text",
      "x": 319.5,
      "y": 219.2,
      "width": 113.7,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#343a40",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 114339217,
      "version": 1,
      "versionNonce": 114339218,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "ParsedLogDelta",
      "originalText": "ParsedLogDelta",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-1-p-cld-to-c-coord",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-2-p-cdx-to-c-coord",
      "type": "arrow",
      "x": 298.2,
      "y": 344.0,
      "width": 133.2,
      "height": 116.0,
      "angle": 0,
      "strokeColor": "#495057",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "seed": 1777023680,
      "version": 1,
      "versionNonce": 1777023681,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-2-p-cdx-to-c-coord-label",
          "type": "text"
        }
      ],
      "updated": 1,
      "link": null,
      "locked": false,
      "points": [
        [
          0,
          0
        ],
        [
          133.2,
          -116.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "p-cdx",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "c-coord",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-2-p-cdx-to-c-coord-label",
      "type": "text",
      "x": 307.9,
      "y": 277.2,
      "width": 113.7,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#343a40",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 1697987632,
      "version": 1,
      "versionNonce": 1697987633,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "ParsedLogDelta",
      "originalText": "ParsedLogDelta",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-2-p-cdx-to-c-coord",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-3-c-coord-to-c-calc",
      "type": "arrow",
      "x": 700.7,
      "y": 228.0,
      "width": 151.8,
      "height": 58.0,
      "angle": 0,
      "strokeColor": "#495057",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "seed": 96425088,
      "version": 1,
      "versionNonce": 96425089,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-3-c-coord-to-c-calc-label",
          "type": "text"
        }
      ],
      "updated": 1,
      "link": null,
      "locked": false,
      "points": [
        [
          0,
          0
        ],
        [
          151.8,
          -58.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "c-coord",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "c-calc",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-3-c-coord-to-c-calc-label",
      "type": "text",
      "x": 723.8,
      "y": 190.2,
      "width": 105.6,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#343a40",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 910711431,
      "version": 1,
      "versionNonce": 910711432,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "price & score",
      "originalText": "price & score",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-3-c-coord-to-c-calc",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-4-c-calc-to-d-sess",
      "type": "arrow",
      "x": 1029.0,
      "y": 170.0,
      "width": 175.0,
      "height": 58.0,
      "angle": 0,
      "strokeColor": "#495057",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "seed": 1751876943,
      "version": 1,
      "versionNonce": 1751876944,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-4-c-calc-to-d-sess-label",
          "type": "text"
        }
      ],
      "updated": 1,
      "link": null,
      "locked": false,
      "points": [
        [
          0,
          0
        ],
        [
          175.0,
          -58.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "c-calc",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "d-sess",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-4-c-calc-to-d-sess-label",
      "type": "text",
      "x": 1067.8,
      "y": 132.2,
      "width": 97.4,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#343a40",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 1173861723,
      "version": 1,
      "versionNonce": 1173861724,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "upsert turns",
      "originalText": "upsert turns",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-4-c-calc-to-d-sess",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-5-c-calc-to-d-req",
      "type": "arrow",
      "x": 1029.0,
      "y": 170.0,
      "width": 179.6,
      "height": 58.0,
      "angle": 0,
      "strokeColor": "#495057",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "seed": 877776203,
      "version": 1,
      "versionNonce": 877776204,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-5-c-calc-to-d-req-label",
          "type": "text"
        }
      ],
      "updated": 1,
      "link": null,
      "locked": false,
      "points": [
        [
          0,
          0
        ],
        [
          179.6,
          58.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "c-calc",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "d-req",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-5-c-calc-to-d-req-label",
      "type": "text",
      "x": 1057.8,
      "y": 190.2,
      "width": 121.8,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#343a40",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 1289038806,
      "version": 1,
      "versionNonce": 1289038807,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "upsert + rollup",
      "originalText": "upsert + rollup",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-5-c-calc-to-d-req",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-6-c-coord-to-d-aux",
      "type": "arrow",
      "x": 700.7,
      "y": 228.0,
      "width": 110.0,
      "height": 58.0,
      "angle": 0,
      "strokeColor": "#495057",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "dashed",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "seed": 1363375668,
      "version": 1,
      "versionNonce": 1363375669,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-6-c-coord-to-d-aux-label",
          "type": "text"
        }
      ],
      "updated": 1,
      "link": null,
      "locked": false,
      "points": [
        [
          0,
          0
        ],
        [
          110.0,
          58.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "c-coord",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "d-aux",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-6-c-coord-to-d-aux-label",
      "type": "text",
      "x": 690.8,
      "y": 248.2,
      "width": 129.9,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#343a40",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 870569991,
      "version": 1,
      "versionNonce": 870569992,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "cursors & limits",
      "originalText": "cursors & limits",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-6-c-coord-to-d-aux",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-7-d-req-to-a-ui",
      "type": "arrow",
      "x": 1412.8,
      "y": 228.0,
      "width": 137.9,
      "height": 0.0,
      "angle": 0,
      "strokeColor": "#495057",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "seed": 1784584155,
      "version": 1,
      "versionNonce": 1784584156,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-7-d-req-to-a-ui-label",
          "type": "text"
        }
      ],
      "updated": 1,
      "link": null,
      "locked": false,
      "points": [
        [
          0,
          0
        ],
        [
          137.9,
          0.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "d-req",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "a-ui",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-7-d-req-to-a-ui-label",
      "type": "text",
      "x": 1445.2,
      "y": 219.2,
      "width": 73.1,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#343a40",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 2086986385,
      "version": 1,
      "versionNonce": 2086986386,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "GRDB read",
      "originalText": "GRDB read",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-7-d-req-to-a-ui",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-8-d-sess-to-a-ui",
      "type": "arrow",
      "x": 1417.5,
      "y": 112.0,
      "width": 133.2,
      "height": 116.0,
      "angle": 0,
      "strokeColor": "#495057",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "dashed",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "seed": 236177799,
      "version": 1,
      "versionNonce": 236177800,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-8-d-sess-to-a-ui-label",
          "type": "text"
        }
      ],
      "updated": 1,
      "link": null,
      "locked": false,
      "points": [
        [
          0,
          0
        ],
        [
          133.2,
          116.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "d-sess",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "a-ui",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-8-d-sess-to-a-ui-label",
      "type": "text",
      "x": 1423.2,
      "y": 161.2,
      "width": 121.8,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#343a40",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 1201578795,
      "version": 1,
      "versionNonce": 1201578796,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "80c mem preview",
      "originalText": "80c mem preview",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-8-d-sess-to-a-ui",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-9-c-calc-to-a-wgt",
      "type": "arrow",
      "x": 1029.0,
      "y": 170.0,
      "width": 151.8,
      "height": 174.0,
      "angle": 0,
      "strokeColor": "#495057",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "seed": 759070350,
      "version": 1,
      "versionNonce": 759070351,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-9-c-calc-to-a-wgt-label",
          "type": "text"
        }
      ],
      "updated": 1,
      "link": null,
      "locked": false,
      "points": [
        [
          0,
          0
        ],
        [
          151.8,
          174.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "c-calc",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "a-wgt",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-9-c-calc-to-a-wgt-label",
      "type": "text",
      "x": 1023.7,
      "y": 248.2,
      "width": 162.4,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#343a40",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 1743587512,
      "version": 1,
      "versionNonce": 1743587513,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "widget-snapshot.json",
      "originalText": "widget-snapshot.json",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-9-c-calc-to-a-wgt",
      "lineHeight": 1.25,
      "autoResize": true
    }
  ],
  "appState": {
    "gridSize": 20,
    "viewBackgroundColor": "#ffffff"
  },
  "files": {}
}
```
%%
