---
id: telemetry-pipeline-privacy-tos-accuracy-boundaries
type: excalidraw
excalidraw-plugin: parsed
tags:
  - excalidraw
  - telemetry
  - accuracy
  - privacy
  - oauth
  - pipeline
created: 2026-11-27
updated: 2026-11-27
sources:
  - "[[concepts/two-tier-telemetry-accuracy]]"
  - "[[sources/local-cli-telemetry]]"
  - "[[sources/oauth-tos-constraints]]"
---
==⚠ Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==

# Excalidraw Data
## Text Elements
Tier 1: Local Ingestion & Caveat Guards ^grp-title-grp-ingest

Tier 2: ToS-Compliant Sync (v2 Opt-in) ^grp-title-grp-tier2

UI Trust & Confidence Badges ^grp-title-grp-badge

Raw CLI Logs
(Gemini / Claude / Codex) ^t-logs-text

Guard: Gemini 30d/50 Purge
+ Hash 3-Step Mapping ^t-warn-gemini-text

Guard: Claude Dedup
& Placeholder Skew ^t-warn-claude-text

SQLite (Metadata Only)
+ Memory-Only 80c Preview ^t-norm-text

v2 Legal Bridge
Opt-in Enabled? ^t-optin-text

Claude Statusline stdin /
Codex Probe / GCP API ^t-probe-text

Policy Guard: No Consumer
OAuth Scraping / Secrets ^t-warn-tos-text

Badge: Exact Local
(API Reqs, Turns, Cost) ^t-exact-text

Adaptive Baseline
(Countdown, Cost, Peak Bar) ^t-est-text

Badge: Official Verified
(Server Remaining %) ^t-verified-text

immediate copy ^edge-0-t-logs-to-t-warn-gemini-label

dedup + flag ^edge-1-t-logs-to-t-warn-claude-label

persist ^edge-2-t-warn-gemini-to-t-norm-label

persist ^edge-3-t-warn-claude-to-t-norm-label

v1 exact ^edge-4-t-norm-to-t-exact-label

quota check ^edge-5-t-norm-to-t-optin-label

No (v1 default) ^edge-6-t-optin-to-t-est-label

Yes (v2) ^edge-7-t-optin-to-t-probe-label

enforces ^edge-8-t-warn-tos-to-t-probe-label

reconcile ^edge-9-t-probe-to-t-verified-label

## Element Links
^t-logs: [[sources/local-cli-telemetry]]
^t-warn-gemini: [[entities/gemini-cli]]
^t-warn-claude: [[entities/claude-code]]
^t-norm: [[concepts/two-tier-telemetry-accuracy]]
^t-optin: [[sources/oauth-tos-constraints]]
^t-probe: [[sources/oauth-tos-constraints]]
^t-warn-tos: [[sources/oauth-tos-constraints]]
^t-exact: [[concepts/prompt-efficiency-scoring]]
^t-est: [[concepts/two-tier-telemetry-accuracy]]
^t-verified: [[concepts/plan-roi-recommendation]]

%%
## Drawing
```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [
    {
      "id": "grp-grp-ingest",
      "type": "rectangle",
      "x": 56.0,
      "y": 47.2,
      "width": 1057.3,
      "height": 248.0,
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
      "seed": 1450911117,
      "version": 1,
      "versionNonce": 1450911118,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false
    },
    {
      "id": "grp-title-grp-ingest",
      "type": "text",
      "x": 68.0,
      "y": 57.2,
      "width": 316.7,
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
      "seed": 1086895885,
      "version": 1,
      "versionNonce": 1086895886,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Tier 1: Local Ingestion & Caveat Guards",
      "originalText": "Tier 1: Local Ingestion & Caveat Guards",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "grp-grp-tier2",
      "type": "rectangle",
      "x": 56.0,
      "y": 36.0,
      "width": 1788.0,
      "height": 259.2,
      "angle": 0,
      "strokeColor": "#c2255c",
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
      "seed": 546791047,
      "version": 1,
      "versionNonce": 546791048,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false
    },
    {
      "id": "grp-title-grp-tier2",
      "type": "text",
      "x": 68.0,
      "y": 46.0,
      "width": 308.6,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#c2255c",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 2143984474,
      "version": 1,
      "versionNonce": 2143984475,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Tier 2: ToS-Compliant Sync (v2 Opt-in)",
      "originalText": "Tier 2: ToS-Compliant Sync (v2 Opt-in)",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "grp-grp-badge",
      "type": "rectangle",
      "x": 1175.3,
      "y": 105.2,
      "width": 1038.7,
      "height": 201.2,
      "angle": 0,
      "strokeColor": "#e8590c",
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
      "seed": 1241209920,
      "version": 1,
      "versionNonce": 1241209921,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false
    },
    {
      "id": "grp-title-grp-badge",
      "type": "text",
      "x": 1187.3,
      "y": 115.2,
      "width": 227.4,
      "height": 17.5,
      "angle": 0,
      "strokeColor": "#e8590c",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 380017121,
      "version": 1,
      "versionNonce": 380017122,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "UI Trust & Confidence Badges",
      "originalText": "UI Trust & Confidence Badges",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "t-logs",
      "type": "rectangle",
      "x": 80.0,
      "y": 91.2,
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
      "seed": 1240145026,
      "version": 1,
      "versionNonce": 1240145027,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "t-logs-text",
          "type": "text"
        },
        {
          "id": "edge-0-t-logs-to-t-warn-gemini",
          "type": "arrow"
        },
        {
          "id": "edge-1-t-logs-to-t-warn-claude",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[sources/local-cli-telemetry]]",
      "locked": false
    },
    {
      "id": "t-logs-text",
      "type": "text",
      "x": 94.0,
      "y": 103.2,
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
      "seed": 1283081777,
      "version": 1,
      "versionNonce": 1283081778,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Raw CLI Logs\n(Gemini / Claude / Codex)",
      "originalText": "Raw CLI Logs\n(Gemini / Claude / Codex)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "t-logs",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "t-warn-gemini",
      "type": "rectangle",
      "x": 450.0,
      "y": 91.2,
      "width": 269.3,
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
      "seed": 716973126,
      "version": 1,
      "versionNonce": 716973127,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "t-warn-gemini-text",
          "type": "text"
        },
        {
          "id": "edge-0-t-logs-to-t-warn-gemini",
          "type": "arrow"
        },
        {
          "id": "edge-2-t-warn-gemini-to-t-norm",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[entities/gemini-cli]]",
      "locked": false
    },
    {
      "id": "t-warn-gemini-text",
      "type": "text",
      "x": 464.0,
      "y": 103.2,
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
      "seed": 1112791093,
      "version": 1,
      "versionNonce": 1112791094,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Guard: Gemini 30d/50 Purge\n+ Hash 3-Step Mapping",
      "originalText": "Guard: Gemini 30d/50 Purge\n+ Hash 3-Step Mapping",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "t-warn-gemini",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "t-warn-claude",
      "type": "rectangle",
      "x": 482.5,
      "y": 207.2,
      "width": 204.3,
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
      "seed": 277759280,
      "version": 1,
      "versionNonce": 277759281,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "t-warn-claude-text",
          "type": "text"
        },
        {
          "id": "edge-1-t-logs-to-t-warn-claude",
          "type": "arrow"
        },
        {
          "id": "edge-3-t-warn-claude-to-t-norm",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[entities/claude-code]]",
      "locked": false
    },
    {
      "id": "t-warn-claude-text",
      "type": "text",
      "x": 496.5,
      "y": 219.2,
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
      "seed": 269338682,
      "version": 1,
      "versionNonce": 269338683,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Guard: Claude Dedup\n& Placeholder Skew",
      "originalText": "Guard: Claude Dedup\n& Placeholder Skew",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "t-warn-claude",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "t-norm",
      "type": "rectangle",
      "x": 829.3,
      "y": 149.2,
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
      "seed": 777523070,
      "version": 1,
      "versionNonce": 777523071,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "t-norm-text",
          "type": "text"
        },
        {
          "id": "edge-2-t-warn-gemini-to-t-norm",
          "type": "arrow"
        },
        {
          "id": "edge-3-t-warn-claude-to-t-norm",
          "type": "arrow"
        },
        {
          "id": "edge-4-t-norm-to-t-exact",
          "type": "arrow"
        },
        {
          "id": "edge-5-t-norm-to-t-optin",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/two-tier-telemetry-accuracy]]",
      "locked": false
    },
    {
      "id": "t-norm-text",
      "type": "text",
      "x": 843.3,
      "y": 161.2,
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
      "seed": 1857470005,
      "version": 1,
      "versionNonce": 1857470006,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "SQLite (Metadata Only)\n+ Memory-Only 80c Preview",
      "originalText": "SQLite (Metadata Only)\n+ Memory-Only 80c Preview",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "t-norm",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "t-optin",
      "type": "diamond",
      "x": 1207.1,
      "y": 80.0,
      "width": 225.7,
      "height": 86.4,
      "angle": 0,
      "strokeColor": "#c2255c",
      "backgroundColor": "#ffdeeb",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 65610132,
      "version": 1,
      "versionNonce": 65610133,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "t-optin-text",
          "type": "text"
        },
        {
          "id": "edge-5-t-norm-to-t-optin",
          "type": "arrow"
        },
        {
          "id": "edge-6-t-optin-to-t-est",
          "type": "arrow"
        },
        {
          "id": "edge-7-t-optin-to-t-probe",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[sources/oauth-tos-constraints]]",
      "locked": false
    },
    {
      "id": "t-optin-text",
      "type": "text",
      "x": 1250.4,
      "y": 103.2,
      "width": 139.2,
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
      "seed": 1720338693,
      "version": 1,
      "versionNonce": 1720338694,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "v2 Legal Bridge\nOpt-in Enabled?",
      "originalText": "v2 Legal Bridge\nOpt-in Enabled?",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "t-optin",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "t-probe",
      "type": "rectangle",
      "x": 1560.0,
      "y": 91.2,
      "width": 260.0,
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
      "seed": 788899226,
      "version": 1,
      "versionNonce": 788899227,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "t-probe-text",
          "type": "text"
        },
        {
          "id": "edge-7-t-optin-to-t-probe",
          "type": "arrow"
        },
        {
          "id": "edge-8-t-warn-tos-to-t-probe",
          "type": "arrow"
        },
        {
          "id": "edge-9-t-probe-to-t-verified",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[sources/oauth-tos-constraints]]",
      "locked": false
    },
    {
      "id": "t-probe-text",
      "type": "text",
      "x": 1574.0,
      "y": 103.2,
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
      "seed": 1472737956,
      "version": 1,
      "versionNonce": 1472737957,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Claude Statusline stdin /\nCodex Probe / GCP API",
      "originalText": "Claude Statusline stdin /\nCodex Probe / GCP API",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "t-probe",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "t-warn-tos",
      "type": "rectangle",
      "x": 80.0,
      "y": 207.2,
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
      "seed": 786658016,
      "version": 1,
      "versionNonce": 786658017,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "t-warn-tos-text",
          "type": "text"
        },
        {
          "id": "edge-8-t-warn-tos-to-t-probe",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[sources/oauth-tos-constraints]]",
      "locked": false
    },
    {
      "id": "t-warn-tos-text",
      "type": "text",
      "x": 94.0,
      "y": 219.2,
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
      "seed": 873405299,
      "version": 1,
      "versionNonce": 873405300,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Policy Guard: No Consumer\nOAuth Scraping / Secrets",
      "originalText": "Policy Guard: No Consumer\nOAuth Scraping / Secrets",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "t-warn-tos",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "t-exact",
      "type": "rectangle",
      "x": 1199.3,
      "y": 218.4,
      "width": 241.4,
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
      "seed": 294478263,
      "version": 1,
      "versionNonce": 294478264,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "t-exact-text",
          "type": "text"
        },
        {
          "id": "edge-4-t-norm-to-t-exact",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/prompt-efficiency-scoring]]",
      "locked": false
    },
    {
      "id": "t-exact-text",
      "type": "text",
      "x": 1213.3,
      "y": 230.4,
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
      "seed": 2014722602,
      "version": 1,
      "versionNonce": 2014722603,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Badge: Exact Local\n(API Reqs, Turns, Cost)",
      "originalText": "Badge: Exact Local\n(API Reqs, Turns, Cost)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "t-exact",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "t-est",
      "type": "rectangle",
      "x": 1550.7,
      "y": 207.2,
      "width": 278.6,
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
      "seed": 89018411,
      "version": 1,
      "versionNonce": 89018412,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "t-est-text",
          "type": "text"
        },
        {
          "id": "edge-6-t-optin-to-t-est",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/two-tier-telemetry-accuracy]]",
      "locked": false
    },
    {
      "id": "t-est-text",
      "type": "text",
      "x": 1564.7,
      "y": 219.2,
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
      "seed": 422613169,
      "version": 1,
      "versionNonce": 422613170,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Adaptive Baseline\n(Countdown, Cost, Peak Bar)",
      "originalText": "Adaptive Baseline\n(Countdown, Cost, Peak Bar)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "t-est",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "t-verified",
      "type": "rectangle",
      "x": 1939.3,
      "y": 149.2,
      "width": 250.7,
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
      "seed": 99682321,
      "version": 1,
      "versionNonce": 99682322,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "t-verified-text",
          "type": "text"
        },
        {
          "id": "edge-9-t-probe-to-t-verified",
          "type": "arrow"
        }
      ],
      "updated": 1,
      "link": "[[concepts/plan-roi-recommendation]]",
      "locked": false
    },
    {
      "id": "t-verified-text",
      "type": "text",
      "x": 1953.3,
      "y": 161.2,
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
      "seed": 161035965,
      "version": 1,
      "versionNonce": 161035966,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Badge: Official Verified\n(Server Remaining %)",
      "originalText": "Badge: Official Verified\n(Server Remaining %)",
      "fontSize": 16,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "t-verified",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-0-t-logs-to-t-warn-gemini",
      "type": "arrow",
      "x": 340.0,
      "y": 123.2,
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
      "seed": 824227427,
      "version": 1,
      "versionNonce": 824227428,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-0-t-logs-to-t-warn-gemini-label",
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
        "elementId": "t-logs",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "t-warn-gemini",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-0-t-logs-to-t-warn-gemini-label",
      "type": "text",
      "x": 338.1,
      "y": 114.5,
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
      "seed": 492135363,
      "version": 1,
      "versionNonce": 492135364,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "immediate copy",
      "originalText": "immediate copy",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-0-t-logs-to-t-warn-gemini",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-1-t-logs-to-t-warn-claude",
      "type": "arrow",
      "x": 340.0,
      "y": 123.2,
      "width": 142.5,
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
      "seed": 781797127,
      "version": 1,
      "versionNonce": 781797128,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-1-t-logs-to-t-warn-claude-label",
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
          142.5,
          116.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "t-logs",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "t-warn-claude",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-1-t-logs-to-t-warn-claude-label",
      "type": "text",
      "x": 362.6,
      "y": 172.4,
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
      "seed": 1834068415,
      "version": 1,
      "versionNonce": 1834068416,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "dedup + flag",
      "originalText": "dedup + flag",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-1-t-logs-to-t-warn-claude",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-2-t-warn-gemini-to-t-norm",
      "type": "arrow",
      "x": 719.3,
      "y": 123.2,
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
      "seed": 1571593248,
      "version": 1,
      "versionNonce": 1571593249,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-2-t-warn-gemini-to-t-norm-label",
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
        "elementId": "t-warn-gemini",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "t-norm",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-2-t-warn-gemini-to-t-norm-label",
      "type": "text",
      "x": 745.9,
      "y": 143.4,
      "width": 56.8,
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
      "seed": 653224512,
      "version": 1,
      "versionNonce": 653224513,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "persist",
      "originalText": "persist",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-2-t-warn-gemini-to-t-norm",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-3-t-warn-claude-to-t-norm",
      "type": "arrow",
      "x": 686.8,
      "y": 239.2,
      "width": 142.5,
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
      "seed": 1040294740,
      "version": 1,
      "versionNonce": 1040294741,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-3-t-warn-claude-to-t-norm-label",
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
          142.5,
          -58.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "t-warn-claude",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "t-norm",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-3-t-warn-claude-to-t-norm-label",
      "type": "text",
      "x": 729.6,
      "y": 201.4,
      "width": 56.8,
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
      "seed": 1772167263,
      "version": 1,
      "versionNonce": 1772167264,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "persist",
      "originalText": "persist",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-3-t-warn-claude-to-t-norm",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-4-t-norm-to-t-exact",
      "type": "arrow",
      "x": 1089.3,
      "y": 181.2,
      "width": 110.0,
      "height": 69.2,
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
      "seed": 1275982486,
      "version": 1,
      "versionNonce": 1275982487,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-4-t-norm-to-t-exact-label",
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
          69.2
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "t-norm",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "t-exact",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-4-t-norm-to-t-exact-label",
      "type": "text",
      "x": 1111.8,
      "y": 207.0,
      "width": 65.0,
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
      "seed": 1475683211,
      "version": 1,
      "versionNonce": 1475683212,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "v1 exact",
      "originalText": "v1 exact",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-4-t-norm-to-t-exact",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-5-t-norm-to-t-optin",
      "type": "arrow",
      "x": 1089.3,
      "y": 181.2,
      "width": 117.8,
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
      "seed": 160117481,
      "version": 1,
      "versionNonce": 160117482,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-5-t-norm-to-t-optin-label",
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
          117.8,
          -58.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "t-norm",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "t-optin",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-5-t-norm-to-t-optin-label",
      "type": "text",
      "x": 1103.5,
      "y": 143.4,
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
      "seed": 926905219,
      "version": 1,
      "versionNonce": 926905220,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "quota check",
      "originalText": "quota check",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-5-t-norm-to-t-optin",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-6-t-optin-to-t-est",
      "type": "arrow",
      "x": 1432.8,
      "y": 123.2,
      "width": 117.8,
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
      "seed": 744823902,
      "version": 1,
      "versionNonce": 744823903,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-6-t-optin-to-t-est-label",
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
          117.8,
          116.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "t-optin",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "t-est",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-6-t-optin-to-t-est-label",
      "type": "text",
      "x": 1430.8,
      "y": 172.4,
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
      "seed": 200266274,
      "version": 1,
      "versionNonce": 200266275,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "No (v1 default)",
      "originalText": "No (v1 default)",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-6-t-optin-to-t-est",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-7-t-optin-to-t-probe",
      "type": "arrow",
      "x": 1432.8,
      "y": 123.2,
      "width": 127.1,
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
      "seed": 1433818495,
      "version": 1,
      "versionNonce": 1433818496,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-7-t-optin-to-t-probe-label",
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
          127.1,
          0.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "t-optin",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "t-probe",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-7-t-optin-to-t-probe-label",
      "type": "text",
      "x": 1463.9,
      "y": 114.5,
      "width": 65.0,
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
      "seed": 663564208,
      "version": 1,
      "versionNonce": 663564209,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "Yes (v2)",
      "originalText": "Yes (v2)",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-7-t-optin-to-t-probe",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-8-t-warn-tos-to-t-probe",
      "type": "arrow",
      "x": 340.0,
      "y": 239.2,
      "width": 1220.0,
      "height": 116.0,
      "angle": 0,
      "strokeColor": "#495057",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "dotted",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "seed": 1093520828,
      "version": 1,
      "versionNonce": 1093520829,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-8-t-warn-tos-to-t-probe-label",
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
          1220.0,
          -116.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "t-warn-tos",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "t-probe",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-8-t-warn-tos-to-t-probe-label",
      "type": "text",
      "x": 917.5,
      "y": 172.4,
      "width": 65.0,
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
      "seed": 1640518903,
      "version": 1,
      "versionNonce": 1640518904,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "enforces",
      "originalText": "enforces",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-8-t-warn-tos-to-t-probe",
      "lineHeight": 1.25,
      "autoResize": true
    },
    {
      "id": "edge-9-t-probe-to-t-verified",
      "type": "arrow",
      "x": 1820.0,
      "y": 123.2,
      "width": 119.3,
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
      "seed": 1784870988,
      "version": 1,
      "versionNonce": 1784870989,
      "isDeleted": false,
      "boundElements": [
        {
          "id": "edge-9-t-probe-to-t-verified-label",
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
          58.0
        ]
      ],
      "lastCommittedPoint": null,
      "startBinding": {
        "elementId": "t-probe",
        "focus": 0,
        "gap": 4
      },
      "endBinding": {
        "elementId": "t-verified",
        "focus": 0,
        "gap": 4
      },
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "elbowed": false
    },
    {
      "id": "edge-9-t-probe-to-t-verified-label",
      "type": "text",
      "x": 1843.1,
      "y": 143.4,
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
      "seed": 1794415476,
      "version": 1,
      "versionNonce": 1794415477,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1,
      "link": null,
      "locked": false,
      "text": "reconcile",
      "originalText": "reconcile",
      "fontSize": 14,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "edge-9-t-probe-to-t-verified",
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
