---
id: system-architecture-agents-usage-mac-v1-2
type: excalidraw
excalidraw-plugin: parsed
tags:
  - excalidraw
  - architecture
  - macos
  - swiftui
  - widgetkit
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[concepts/macos-menubar-widget-architecture]]"
  - "[[syntheses/prd-agents-usage-mac]]"
---
==⚠ Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==

# Excalidraw Data
## Text Elements
Local CLI Logs (v1) ^grp-title-grp-cli

Non-Sandboxed Main App Engine ^grp-title-grp-core

Local Storage & App Group ^grp-title-grp-store

macOS Presentation Surfaces ^grp-title-grp-ui

Gemini CLI
(~/.gemini/tmp/... JSON) ^n-gemini-text

Claude Code
(~/.claude/projects JSONL) ^n-claude-text

Codex CLI
(~/.codex/sessions JSONL) ^n-codex-text

FSEvents + Wake Catch-up
Dual Parser (Offset / Delta) ^n-watcher-text

3-Tier Turn Efficiency
& Cost Engine ^n-eff-text

Plan ROI & Legal Tier 2
(v2.0 Engine) ^n-roi-text

SQLite (Turns + Daily Rollup)
+ Opt-in Pricing Catalog ^n-sqlite-text

App Group Snapshot
(widget-snapshot.json, 0B prompt) ^n-appgroup-text

Notch-Safe MenuBarExtra
(Adaptive Baseline Popover) ^n-menubar-text

Dashboard Window (EN/KO)
(Charts + Memory 80c Preview) ^n-dashboard-text

WidgetKit Extension
(Sandboxed, 40-70/day) ^n-widget-text

mtime delta ^edge-0-n-gemini-to-n-watcher-label

byte_offset ^edge-1-n-claude-to-n-watcher-label

byte_offset ^edge-2-n-codex-to-n-watcher-label

upsert + rollup ^edge-3-n-watcher-to-n-sqlite-label

turn metrics ^edge-4-n-sqlite-to-n-eff-label

V_api & V_adj ^edge-5-n-eff-to-n-roi-label

live state ^edge-6-n-eff-to-n-menubar-label

scores & coaching ^edge-7-n-eff-to-n-dashboard-label

plan advice ^edge-8-n-roi-to-n-dashboard-label

debounced write ^edge-9-n-sqlite-to-n-appgroup-label

read snapshot ^edge-10-n-appgroup-to-n-widget-label

## Element Links
^n-gemini: [[entities/gemini-cli]]
^n-claude: [[entities/claude-code]]
^n-codex: [[entities/codex-cli]]
^n-watcher: [[concepts/macos-menubar-widget-architecture]]
^n-eff: [[concepts/prompt-efficiency-scoring]]
^n-roi: [[concepts/plan-roi-recommendation]]
^n-sqlite: [[sources/pricing-quotas-2026]]
^n-appgroup: [[concepts/macos-menubar-widget-architecture]]
^n-menubar: [[concepts/macos-menubar-widget-architecture]]
^n-dashboard: [[syntheses/prd-agents-usage-mac]]
^n-widget: [[sources/macos-architecture-constraints]]

%%
## Drawing
```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [
    {
      "id": "grp-grp-cli",
      "type": "rectangle",
      "x": 56.0,
      "y": 36.0,
      "width": 317.3,
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
      "seed": 1723994628,
      "version": 1,
      "versionNonce": 1723994629,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false
    },
    {
      "id": "grp-title-grp-cli",
      "type": "text",
      "x": 68.0,
      "y": 46.0,
      "width": 154.3,
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
      "seed": 340486503,
      "version": 1,
      "versionNonce": 340486504,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Local CLI Logs (v1)",
      "originalText": "Local CLI Logs (v1)",
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
      "x": 435.3,
      "y": 36.0,
      "width": 1557.1,
      "height": 248.0,
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
      "x": 447.3,
      "y": 46.0,
      "width": 235.5,
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
      "text": "Non-Sandboxed Main App Engine",
      "originalText": "Non-Sandboxed Main App Engine",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "grp-grp-store",
      "type": "rectangle",
      "x": 833.1,
      "y": 152.0,
      "width": 789.3,
      "height": 190.0,
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
      "seed": 1214008406,
      "version": 1,
      "versionNonce": 1214008407,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false
    },
    {
      "id": "grp-title-grp-store",
      "type": "text",
      "x": 845.1,
      "y": 162.0,
      "width": 203.0,
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
      "seed": 385976715,
      "version": 1,
      "versionNonce": 385976716,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Local Storage & App Group",
      "originalText": "Local Storage & App Group",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "grp-grp-ui",
      "type": "rectangle",
      "x": 1684.4,
      "y": 152.0,
      "width": 733.7,
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
      "seed": 665974990,
      "version": 1,
      "versionNonce": 665974991,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false
    },
    {
      "id": "grp-title-grp-ui",
      "type": "text",
      "x": 1696.4,
      "y": 162.0,
      "width": 219.2,
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
      "seed": 1288360863,
      "version": 1,
      "versionNonce": 1288360864,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "macOS Presentation Surfaces",
      "originalText": "macOS Presentation Surfaces",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "n-gemini",
      "type": "rectangle",
      "x": 89.3,
      "y": 80.0,
      "width": 250.7,
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
      "seed": 189838024,
      "version": 1,
      "versionNonce": 189838025,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "n-gemini-text",
          "type": "text"
        },
        {
          "id": "edge-0-n-gemini-to-n-watcher",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[entities/gemini-cli]]",
      "locked": false
    },
    {
      "id": "n-gemini-text",
      "type": "text",
      "x": 103.3,
      "y": 92.0,
      "width": 222.7,
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
      "seed": 112861651,
      "version": 1,
      "versionNonce": 112861652,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Gemini CLI\n(~/.gemini/tmp/... JSON)",
      "originalText": "Gemini CLI\n(~/.gemini/tmp/... JSON)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "n-gemini",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "n-claude",
      "type": "rectangle",
      "x": 80.0,
      "y": 196.0,
      "width": 269.3,
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
      "seed": 828560302,
      "version": 1,
      "versionNonce": 828560303,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "n-claude-text",
          "type": "text"
        },
        {
          "id": "edge-1-n-claude-to-n-watcher",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[entities/claude-code]]",
      "locked": false
    },
    {
      "id": "n-claude-text",
      "type": "text",
      "x": 94.0,
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
      "seed": 1424241120,
      "version": 1,
      "versionNonce": 1424241121,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Claude Code\n(~/.claude/projects JSONL)",
      "originalText": "Claude Code\n(~/.claude/projects JSONL)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "n-claude",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "n-codex",
      "type": "rectangle",
      "x": 84.7,
      "y": 312.0,
      "width": 260.0,
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
      "seed": 1233785335,
      "version": 1,
      "versionNonce": 1233785336,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "n-codex-text",
          "type": "text"
        },
        {
          "id": "edge-2-n-codex-to-n-watcher",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[entities/codex-cli]]",
      "locked": false
    },
    {
      "id": "n-codex-text",
      "type": "text",
      "x": 98.7,
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
      "seed": 125082479,
      "version": 1,
      "versionNonce": 125082480,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Codex CLI\n(~/.codex/sessions JSONL)",
      "originalText": "Codex CLI\n(~/.codex/sessions JSONL)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "n-codex",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "n-watcher",
      "type": "rectangle",
      "x": 459.3,
      "y": 196.0,
      "width": 287.8,
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
      "seed": 1060634830,
      "version": 1,
      "versionNonce": 1060634831,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "n-watcher-text",
          "type": "text"
        },
        {
          "id": "edge-0-n-gemini-to-n-watcher",
          "type": "arrow"
        },
        {
          "id": "edge-1-n-claude-to-n-watcher",
          "type": "arrow"
        },
        {
          "id": "edge-2-n-codex-to-n-watcher",
          "type": "arrow"
        },
        {
          "id": "edge-3-n-watcher-to-n-sqlite",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/macos-menubar-widget-architecture]]",
      "locked": false
    },
    {
      "id": "n-watcher-text",
      "type": "text",
      "x": 473.3,
      "y": 208.0,
      "width": 259.8,
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
      "seed": 111138318,
      "version": 1,
      "versionNonce": 111138319,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "FSEvents + Wake Catch-up\nDual Parser (Offset / Delta)",
      "originalText": "FSEvents + Wake Catch-up\nDual Parser (Offset / Delta)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "n-watcher",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "n-eff",
      "type": "rectangle",
      "x": 1315.2,
      "y": 138.0,
      "width": 232.2,
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
      "seed": 1796360278,
      "version": 1,
      "versionNonce": 1796360279,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "n-eff-text",
          "type": "text"
        },
        {
          "id": "edge-4-n-sqlite-to-n-eff",
          "type": "arrow"
        },
        {
          "id": "edge-5-n-eff-to-n-roi",
          "type": "arrow"
        },
        {
          "id": "edge-6-n-eff-to-n-menubar",
          "type": "arrow"
        },
        {
          "id": "edge-7-n-eff-to-n-dashboard",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/prompt-efficiency-scoring]]",
      "locked": false
    },
    {
      "id": "n-eff-text",
      "type": "text",
      "x": 1329.2,
      "y": 150.0,
      "width": 204.2,
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
      "seed": 1380225385,
      "version": 1,
      "versionNonce": 1380225386,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "3-Tier Turn Efficiency\n& Cost Engine",
      "originalText": "3-Tier Turn Efficiency\n& Cost Engine",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "n-eff",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "n-roi",
      "type": "rectangle",
      "x": 1727.0,
      "y": 80.0,
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
      "seed": 856054649,
      "version": 1,
      "versionNonce": 856054650,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "n-roi-text",
          "type": "text"
        },
        {
          "id": "edge-5-n-eff-to-n-roi",
          "type": "arrow"
        },
        {
          "id": "edge-8-n-roi-to-n-dashboard",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/plan-roi-recommendation]]",
      "locked": false
    },
    {
      "id": "n-roi-text",
      "type": "text",
      "x": 1741.0,
      "y": 92.0,
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
      "seed": 1089756765,
      "version": 1,
      "versionNonce": 1089756766,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Plan ROI & Legal Tier 2\n(v2.0 Engine)",
      "originalText": "Plan ROI & Legal Tier 2\n(v2.0 Engine)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "n-roi",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "n-sqlite",
      "type": "rectangle",
      "x": 857.1,
      "y": 196.0,
      "width": 297.1,
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
      "seed": 122688158,
      "version": 1,
      "versionNonce": 122688159,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "n-sqlite-text",
          "type": "text"
        },
        {
          "id": "edge-3-n-watcher-to-n-sqlite",
          "type": "arrow"
        },
        {
          "id": "edge-4-n-sqlite-to-n-eff",
          "type": "arrow"
        },
        {
          "id": "edge-9-n-sqlite-to-n-appgroup",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[sources/pricing-quotas-2026]]",
      "locked": false
    },
    {
      "id": "n-sqlite-text",
      "type": "text",
      "x": 871.1,
      "y": 208.0,
      "width": 269.1,
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
      "seed": 365404338,
      "version": 1,
      "versionNonce": 365404339,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "SQLite (Turns + Daily Rollup)\n+ Opt-in Pricing Catalog",
      "originalText": "SQLite (Turns + Daily Rollup)\n+ Opt-in Pricing Catalog",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "n-sqlite",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "n-appgroup",
      "type": "rectangle",
      "x": 1264.2,
      "y": 254.0,
      "width": 334.2,
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
      "seed": 267646807,
      "version": 1,
      "versionNonce": 267646808,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "n-appgroup-text",
          "type": "text"
        },
        {
          "id": "edge-9-n-sqlite-to-n-appgroup",
          "type": "arrow"
        },
        {
          "id": "edge-10-n-appgroup-to-n-widget",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/macos-menubar-widget-architecture]]",
      "locked": false
    },
    {
      "id": "n-appgroup-text",
      "type": "text",
      "x": 1278.2,
      "y": 266.0,
      "width": 306.2,
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
      "seed": 2034540865,
      "version": 1,
      "versionNonce": 2034540866,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "App Group Snapshot\n(widget-snapshot.json, 0B prompt)",
      "originalText": "App Group Snapshot\n(widget-snapshot.json, 0B prompt)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "n-appgroup",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "n-menubar",
      "type": "rectangle",
      "x": 1708.4,
      "y": 196.0,
      "width": 278.6,
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
      "seed": 1373521642,
      "version": 1,
      "versionNonce": 1373521643,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "n-menubar-text",
          "type": "text"
        },
        {
          "id": "edge-6-n-eff-to-n-menubar",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/macos-menubar-widget-architecture]]",
      "locked": false
    },
    {
      "id": "n-menubar-text",
      "type": "text",
      "x": 1722.4,
      "y": 208.0,
      "width": 250.6,
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
      "seed": 1521529364,
      "version": 1,
      "versionNonce": 1521529365,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Notch-Safe MenuBarExtra\n(Adaptive Baseline Popover)",
      "originalText": "Notch-Safe MenuBarExtra\n(Adaptive Baseline Popover)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "n-menubar",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "n-dashboard",
      "type": "rectangle",
      "x": 2097.0,
      "y": 196.0,
      "width": 297.1,
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
      "seed": 1763405797,
      "version": 1,
      "versionNonce": 1763405798,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "n-dashboard-text",
          "type": "text"
        },
        {
          "id": "edge-7-n-eff-to-n-dashboard",
          "type": "arrow"
        },
        {
          "id": "edge-8-n-roi-to-n-dashboard",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[syntheses/prd-agents-usage-mac]]",
      "locked": false
    },
    {
      "id": "n-dashboard-text",
      "type": "text",
      "x": 2111.0,
      "y": 208.0,
      "width": 269.1,
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
      "seed": 2145484359,
      "version": 1,
      "versionNonce": 2145484360,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Dashboard Window (EN/KO)\n(Charts + Memory 80c Preview)",
      "originalText": "Dashboard Window (EN/KO)\n(Charts + Memory 80c Preview)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "n-dashboard",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "n-widget",
      "type": "rectangle",
      "x": 1731.6,
      "y": 312.0,
      "width": 232.2,
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
      "seed": 974293712,
      "version": 1,
      "versionNonce": 974293713,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "n-widget-text",
          "type": "text"
        },
        {
          "id": "edge-10-n-appgroup-to-n-widget",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[sources/macos-architecture-constraints]]",
      "locked": false
    },
    {
      "id": "n-widget-text",
      "type": "text",
      "x": 1745.6,
      "y": 324.0,
      "width": 204.2,
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
      "seed": 907469890,
      "version": 1,
      "versionNonce": 907469891,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "WidgetKit Extension\n(Sandboxed, 40-70/day)",
      "originalText": "WidgetKit Extension\n(Sandboxed, 40-70/day)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "n-widget",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-0-n-gemini-to-n-watcher",
      "type": "arrow",
      "x": 340.0,
      "y": 112.0,
      "width": 119.3,
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
      "seed": 500555822,
      "version": 1,
      "versionNonce": 500555823,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-0-n-gemini-to-n-watcher-label",
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
          119.3,
          116.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "n-gemini",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "n-watcher",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-0-n-gemini-to-n-watcher-label",
      "type": "text",
      "x": 355.0,
      "y": 161.2,
      "width": 89.3,
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
      "seed": 902996866,
      "version": 1,
      "versionNonce": 902996867,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "mtime delta",
      "originalText": "mtime delta",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-0-n-gemini-to-n-watcher",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-1-n-claude-to-n-watcher",
      "type": "arrow",
      "x": 349.3,
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
      "seed": 237663947,
      "version": 1,
      "versionNonce": 237663948,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-1-n-claude-to-n-watcher-label",
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
        "elementId": "n-claude",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "n-watcher",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-1-n-claude-to-n-watcher-label",
      "type": "text",
      "x": 359.7,
      "y": 219.2,
      "width": 89.3,
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
      "seed": 548422100,
      "version": 1,
      "versionNonce": 548422101,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "byte_offset",
      "originalText": "byte_offset",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-1-n-claude-to-n-watcher",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-2-n-codex-to-n-watcher",
      "type": "arrow",
      "x": 344.6,
      "y": 344.0,
      "width": 114.7,
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
      "seed": 526918225,
      "version": 1,
      "versionNonce": 526918226,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-2-n-codex-to-n-watcher-label",
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
          114.7,
          -116.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "n-codex",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "n-watcher",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-2-n-codex-to-n-watcher-label",
      "type": "text",
      "x": 357.4,
      "y": 277.2,
      "width": 89.3,
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
      "seed": 2099725325,
      "version": 1,
      "versionNonce": 2099725326,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "byte_offset",
      "originalText": "byte_offset",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-2-n-codex-to-n-watcher",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-3-n-watcher-to-n-sqlite",
      "type": "arrow",
      "x": 747.1,
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
      "seed": 701267432,
      "version": 1,
      "versionNonce": 701267433,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-3-n-watcher-to-n-sqlite-label",
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
        "elementId": "n-watcher",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "n-sqlite",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-3-n-watcher-to-n-sqlite-label",
      "type": "text",
      "x": 741.2,
      "y": 219.2,
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
      "seed": 1131492140,
      "version": 1,
      "versionNonce": 1131492141,
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
      "containerId": "edge-3-n-watcher-to-n-sqlite",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-4-n-sqlite-to-n-eff",
      "type": "arrow",
      "x": 1154.2,
      "y": 228.0,
      "width": 161.0,
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
      "seed": 660422773,
      "version": 1,
      "versionNonce": 660422774,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-4-n-sqlite-to-n-eff-label",
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
          161.0,
          -58.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "n-sqlite",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "n-eff",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-4-n-sqlite-to-n-eff-label",
      "type": "text",
      "x": 1186.0,
      "y": 190.2,
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
      "seed": 1868929979,
      "version": 1,
      "versionNonce": 1868929980,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "turn metrics",
      "originalText": "turn metrics",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-4-n-sqlite-to-n-eff",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-5-n-eff-to-n-roi",
      "type": "arrow",
      "x": 1547.4,
      "y": 170.0,
      "width": 179.6,
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
      "seed": 549614310,
      "version": 1,
      "versionNonce": 549614311,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-5-n-eff-to-n-roi-label",
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
          -58.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "n-eff",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "n-roi",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-5-n-eff-to-n-roi-label",
      "type": "text",
      "x": 1584.4,
      "y": 132.2,
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
      "seed": 1676101966,
      "version": 1,
      "versionNonce": 1676101967,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "V_api & V_adj",
      "originalText": "V_api & V_adj",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-5-n-eff-to-n-roi",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-6-n-eff-to-n-menubar",
      "type": "arrow",
      "x": 1547.4,
      "y": 170.0,
      "width": 161.0,
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
      "seed": 770251938,
      "version": 1,
      "versionNonce": 770251939,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-6-n-eff-to-n-menubar-label",
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
          161.0,
          58.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "n-eff",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "n-menubar",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-6-n-eff-to-n-menubar-label",
      "type": "text",
      "x": 1587.3,
      "y": 190.2,
      "width": 81.2,
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
      "seed": 1341286648,
      "version": 1,
      "versionNonce": 1341286649,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "live state",
      "originalText": "live state",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-6-n-eff-to-n-menubar",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-7-n-eff-to-n-dashboard",
      "type": "arrow",
      "x": 1547.4,
      "y": 170.0,
      "width": 549.6,
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
      "seed": 1294830325,
      "version": 1,
      "versionNonce": 1294830326,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-7-n-eff-to-n-dashboard-label",
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
          549.6,
          58.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "n-eff",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "n-dashboard",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-7-n-eff-to-n-dashboard-label",
      "type": "text",
      "x": 1753.2,
      "y": 190.2,
      "width": 138.0,
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
      "seed": 2081395059,
      "version": 1,
      "versionNonce": 2081395060,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "scores & coaching",
      "originalText": "scores & coaching",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-7-n-eff-to-n-dashboard",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-8-n-roi-to-n-dashboard",
      "type": "arrow",
      "x": 1968.4,
      "y": 112.0,
      "width": 128.6,
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
      "seed": 828778288,
      "version": 1,
      "versionNonce": 828778289,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-8-n-roi-to-n-dashboard-label",
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
          128.6,
          116.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "n-roi",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "n-dashboard",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-8-n-roi-to-n-dashboard-label",
      "type": "text",
      "x": 1988.0,
      "y": 161.2,
      "width": 89.3,
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
      "seed": 2089448242,
      "version": 1,
      "versionNonce": 2089448243,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "plan advice",
      "originalText": "plan advice",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-8-n-roi-to-n-dashboard",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-9-n-sqlite-to-n-appgroup",
      "type": "arrow",
      "x": 1154.2,
      "y": 228.0,
      "width": 110.0,
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
      "seed": 272714869,
      "version": 1,
      "versionNonce": 272714870,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-9-n-sqlite-to-n-appgroup-label",
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
        "elementId": "n-sqlite",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "n-appgroup",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-9-n-sqlite-to-n-appgroup-label",
      "type": "text",
      "x": 1148.3,
      "y": 248.2,
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
      "seed": 1883993610,
      "version": 1,
      "versionNonce": 1883993611,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "debounced write",
      "originalText": "debounced write",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-9-n-sqlite-to-n-appgroup",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-10-n-appgroup-to-n-widget",
      "type": "arrow",
      "x": 1598.4,
      "y": 286.0,
      "width": 133.2,
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
      "seed": 251740893,
      "version": 1,
      "versionNonce": 251740894,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-10-n-appgroup-to-n-widget-label",
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
          58.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "n-appgroup",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "n-widget",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-10-n-appgroup-to-n-widget-label",
      "type": "text",
      "x": 1612.2,
      "y": 306.2,
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
      "seed": 1466956828,
      "version": 1,
      "versionNonce": 1466956829,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "read snapshot",
      "originalText": "read snapshot",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-10-n-appgroup-to-n-widget",
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
