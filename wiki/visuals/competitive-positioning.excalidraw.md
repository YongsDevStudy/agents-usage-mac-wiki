---
id: competitive-positioning-monitor-to-optimize
type: excalidraw
excalidraw-plugin: parsed
tags:
  - excalidraw
  - competitors
  - positioning
  - strategy
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[entities/competitor-landscape]]"
  - "[[sources/market-competitors]]"
---
==⚠ Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==

# Excalidraw Data
## Text Elements
Existing Tools (Quota Monitor Only) ^grp-title-grp-existing

Agents Usage Mac (3-Pillar Loop) ^grp-title-grp-target

ClaudeBar / UsageBar
(Single Vendor Gauge) ^c-claudebar-text

CodexBar / OpenUsage
(Multi-Vendor Quota Bar) ^c-codexbar-text

ccusage (CLI)
(Daily/Block JSONL Stats) ^c-ccusage-text

Pillar 1: Real-time Monitor
(Menu Bar + WidgetKit
Gemini 1st + Claude + Codex) ^c-p1-text

Pillar 2: Prompt Efficiency
(Turn Cost, Cache Hit,
/compact & /clear Coaching) ^c-p2-text

Pillar 3: Plan ROI Engine
(API Equivalent vs Plan,
Upgrade/Downgrade Advice) ^c-p3-text

expand multi-CLI ^edge-0-c-claudebar-to-c-p1-label

native WidgetKit + local-first ^edge-1-c-codexbar-to-c-p1-label

turn-level diagnosis ^edge-2-c-ccusage-to-c-p2-label

live logs ^edge-3-c-p1-to-c-p2-label

waste-adjusted ROI ^edge-4-c-p2-to-c-p3-label

## Element Links
^c-claudebar: [[entities/competitor-landscape]]
^c-codexbar: [[entities/competitor-landscape]]
^c-ccusage: [[entities/competitor-landscape]]
^c-p1: [[concepts/macos-menubar-widget-architecture]]
^c-p2: [[concepts/prompt-efficiency-scoring]]
^c-p3: [[concepts/plan-roi-recommendation]]

%%
## Drawing
```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [
    {
      "id": "grp-grp-existing",
      "type": "rectangle",
      "x": 56.0,
      "y": 36.0,
      "width": 308.0,
      "height": 364.0,
      "angle": 0,
      "strokeColor": "#c92a2a",
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
      "seed": 979485222,
      "version": 1,
      "versionNonce": 979485223,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false
    },
    {
      "id": "grp-title-grp-existing",
      "type": "text",
      "x": 68.0,
      "y": 46.0,
      "width": 284.2,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#c92a2a",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 535019190,
      "version": 1,
      "versionNonce": 535019191,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Existing Tools (Quota Monitor Only)",
      "originalText": "Existing Tools (Quota Monitor Only)",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "grp-grp-target",
      "type": "rectangle",
      "x": 426.0,
      "y": 144.0,
      "width": 1094.4,
      "height": 148.0,
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
      "seed": 330851597,
      "version": 1,
      "versionNonce": 330851598,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false
    },
    {
      "id": "grp-title-grp-target",
      "type": "text",
      "x": 438.0,
      "y": 154.0,
      "width": 259.8,
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
      "seed": 84670349,
      "version": 1,
      "versionNonce": 84670350,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Agents Usage Mac (3-Pillar Loop)",
      "originalText": "Agents Usage Mac (3-Pillar Loop)",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "c-claudebar",
      "type": "rectangle",
      "x": 98.5,
      "y": 80.0,
      "width": 222.9,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#f1f3f5",
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
      "seed": 1700960520,
      "version": 1,
      "versionNonce": 1700960521,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "c-claudebar-text",
          "type": "text"
        },
        {
          "id": "edge-0-c-claudebar-to-c-p1",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[entities/competitor-landscape]]",
      "locked": false
    },
    {
      "id": "c-claudebar-text",
      "type": "text",
      "x": 112.5,
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
      "seed": 1141741282,
      "version": 1,
      "versionNonce": 1141741283,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "ClaudeBar / UsageBar\n(Single Vendor Gauge)",
      "originalText": "ClaudeBar / UsageBar\n(Single Vendor Gauge)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "c-claudebar",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "c-codexbar",
      "type": "rectangle",
      "x": 84.7,
      "y": 196.0,
      "width": 250.7,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#f1f3f5",
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
      "seed": 1828471072,
      "version": 1,
      "versionNonce": 1828471073,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "c-codexbar-text",
          "type": "text"
        },
        {
          "id": "edge-1-c-codexbar-to-c-p1",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[entities/competitor-landscape]]",
      "locked": false
    },
    {
      "id": "c-codexbar-text",
      "type": "text",
      "x": 98.7,
      "y": 208.0,
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
      "seed": 1362294120,
      "version": 1,
      "versionNonce": 1362294121,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "CodexBar / OpenUsage\n(Multi-Vendor Quota Bar)",
      "originalText": "CodexBar / OpenUsage\n(Multi-Vendor Quota Bar)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "c-codexbar",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "c-ccusage",
      "type": "rectangle",
      "x": 80.0,
      "y": 312.0,
      "width": 260.0,
      "height": 64.0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#f1f3f5",
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
      "seed": 1130585258,
      "version": 1,
      "versionNonce": 1130585259,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "c-ccusage-text",
          "type": "text"
        },
        {
          "id": "edge-2-c-ccusage-to-c-p2",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[entities/competitor-landscape]]",
      "locked": false
    },
    {
      "id": "c-ccusage-text",
      "type": "text",
      "x": 94.0,
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
      "seed": 857135227,
      "version": 1,
      "versionNonce": 857135228,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "ccusage (CLI)\n(Daily/Block JSONL Stats)",
      "originalText": "ccusage (CLI)\n(Daily/Block JSONL Stats)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "c-ccusage",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "c-p1",
      "type": "rectangle",
      "x": 450.0,
      "y": 188.0,
      "width": 287.8,
      "height": 80.0,
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
      "seed": 1185098639,
      "version": 1,
      "versionNonce": 1185098640,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "c-p1-text",
          "type": "text"
        },
        {
          "id": "edge-0-c-claudebar-to-c-p1",
          "type": "arrow"
        },
        {
          "id": "edge-1-c-codexbar-to-c-p1",
          "type": "arrow"
        },
        {
          "id": "edge-3-c-p1-to-c-p2",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/macos-menubar-widget-architecture]]",
      "locked": false
    },
    {
      "id": "c-p1-text",
      "type": "text",
      "x": 464.0,
      "y": 198.0,
      "width": 259.8,
      "height": 60.0,
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
      "seed": 1631865094,
      "version": 1,
      "versionNonce": 1631865095,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Pillar 1: Real-time Monitor\n(Menu Bar + WidgetKit\nGemini 1st + Claude + Codex)",
      "originalText": "Pillar 1: Real-time Monitor\n(Menu Bar + WidgetKit\nGemini 1st + Claude + Codex)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "c-p1",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "c-p2",
      "type": "rectangle",
      "x": 847.8,
      "y": 188.0,
      "width": 278.6,
      "height": 80.0,
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
      "seed": 1605007925,
      "version": 1,
      "versionNonce": 1605007926,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "c-p2-text",
          "type": "text"
        },
        {
          "id": "edge-2-c-ccusage-to-c-p2",
          "type": "arrow"
        },
        {
          "id": "edge-3-c-p1-to-c-p2",
          "type": "arrow"
        },
        {
          "id": "edge-4-c-p2-to-c-p3",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/prompt-efficiency-scoring]]",
      "locked": false
    },
    {
      "id": "c-p2-text",
      "type": "text",
      "x": 861.8,
      "y": 198.0,
      "width": 250.6,
      "height": 60.0,
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
      "seed": 1741700012,
      "version": 1,
      "versionNonce": 1741700013,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Pillar 2: Prompt Efficiency\n(Turn Cost, Cache Hit,\n/compact & /clear Coaching)",
      "originalText": "Pillar 2: Prompt Efficiency\n(Turn Cost, Cache Hit,\n/compact & /clear Coaching)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "c-p2",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "c-p3",
      "type": "rectangle",
      "x": 1236.4,
      "y": 188.0,
      "width": 260.0,
      "height": 80.0,
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
      "seed": 682445475,
      "version": 1,
      "versionNonce": 682445476,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "c-p3-text",
          "type": "text"
        },
        {
          "id": "edge-4-c-p2-to-c-p3",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/plan-roi-recommendation]]",
      "locked": false
    },
    {
      "id": "c-p3-text",
      "type": "text",
      "x": 1250.4,
      "y": 198.0,
      "width": 232.0,
      "height": 60.0,
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
      "seed": 747431951,
      "version": 1,
      "versionNonce": 747431952,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Pillar 3: Plan ROI Engine\n(API Equivalent vs Plan,\nUpgrade/Downgrade Advice)",
      "originalText": "Pillar 3: Plan ROI Engine\n(API Equivalent vs Plan,\nUpgrade/Downgrade Advice)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "c-p3",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-0-c-claudebar-to-c-p1",
      "type": "arrow",
      "x": 321.4,
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
      "seed": 537121403,
      "version": 1,
      "versionNonce": 537121404,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-0-c-claudebar-to-c-p1-label",
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
        "elementId": "c-claudebar",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "c-p1",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-0-c-claudebar-to-c-p1-label",
      "type": "text",
      "x": 320.8,
      "y": 161.2,
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
      "seed": 1345642671,
      "version": 1,
      "versionNonce": 1345642672,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "expand multi-CLI",
      "originalText": "expand multi-CLI",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-0-c-claudebar-to-c-p1",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-1-c-codexbar-to-c-p1",
      "type": "arrow",
      "x": 335.4,
      "y": 228.0,
      "width": 114.6,
      "height": 0.0,
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
      "seed": 1658843795,
      "version": 1,
      "versionNonce": 1658843796,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-1-c-codexbar-to-c-p1-label",
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
          114.6,
          0.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "c-codexbar",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "c-p1",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-1-c-codexbar-to-c-p1-label",
      "type": "text",
      "x": 270.9,
      "y": 219.2,
      "width": 243.6,
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
      "seed": 357241419,
      "version": 1,
      "versionNonce": 357241420,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "native WidgetKit + local-first",
      "originalText": "native WidgetKit + local-first",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-1-c-codexbar-to-c-p1",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-2-c-ccusage-to-c-p2",
      "type": "arrow",
      "x": 340.0,
      "y": 344.0,
      "width": 507.8,
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
      "seed": 126698901,
      "version": 1,
      "versionNonce": 126698902,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-2-c-ccusage-to-c-p2-label",
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
          507.8,
          -116.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "c-ccusage",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "c-p2",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-2-c-ccusage-to-c-p2-label",
      "type": "text",
      "x": 512.7,
      "y": 277.2,
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
      "seed": 802746418,
      "version": 1,
      "versionNonce": 802746419,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "turn-level diagnosis",
      "originalText": "turn-level diagnosis",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-2-c-ccusage-to-c-p2",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-3-c-p1-to-c-p2",
      "type": "arrow",
      "x": 737.8,
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
      "seed": 1034084754,
      "version": 1,
      "versionNonce": 1034084755,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-3-c-p1-to-c-p2-label",
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
        "elementId": "c-p1",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "c-p2",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-3-c-p1-to-c-p2-label",
      "type": "text",
      "x": 756.2,
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
      "seed": 723486715,
      "version": 1,
      "versionNonce": 723486716,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "live logs",
      "originalText": "live logs",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-3-c-p1-to-c-p2",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-4-c-p2-to-c-p3",
      "type": "arrow",
      "x": 1126.4,
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
      "seed": 384124675,
      "version": 1,
      "versionNonce": 384124676,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-4-c-p2-to-c-p3-label",
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
        "elementId": "c-p2",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "c-p3",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-4-c-p2-to-c-p3-label",
      "type": "text",
      "x": 1108.3,
      "y": 219.2,
      "width": 146.2,
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
      "seed": 2102476965,
      "version": 1,
      "versionNonce": 2102476966,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "waste-adjusted ROI",
      "originalText": "waste-adjusted ROI",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-4-c-p2-to-c-p3",
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
