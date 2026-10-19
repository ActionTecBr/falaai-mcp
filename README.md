# FalaAI MCP — Model Context Protocol server for Conversation Intelligence, Speech Analytics & Compliance

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![MCP Registry](https://img.shields.io/badge/MCP%20Registry-io.github.ActionTecBr%2Ffalaai--mcp-blue)](https://registry.modelcontextprotocol.io/v0.1/servers?search=falaai-mcp)
[![Transport: Streamable HTTP](https://img.shields.io/badge/transport-streamable--http-purple.svg)](https://mcp-falaai.action.tec.br/mcp)
[![API docs](https://img.shields.io/badge/docs-falaai.action.tec.br%2Fapi-lightgrey.svg)](https://falaai.action.tec.br/api)
[![FalaAI API (MCP) MCP connector – tool definition quality and endpoint health on Glama](https://glama.ai/mcp/connectors/io.github.ActionTecBr/falaai-mcp/badges/score.svg)](https://glama.ai/mcp/connectors/io.github.ActionTecBr/falaai-mcp)

Official **MCP server** for the **FalaAI API** — plug it into an AI assistant (Gemini, Claude, Cursor, VS Code, …) and let the assistant **transcribe audio, diagnose conversations and audit compliance** from natural language. The analysis tools use the **same `fai_` API key** you already use with the REST API; the `health` tool works **without a key**.

> The assistant calls the FalaAI tools on your behalf. You get speaker-separated transcripts, summaries, reasons, actions, sentiment and a **compliance risk score** — with the same results as the REST API.

## Endpoint

| Item | Value |
|---|---|
| **URL** | `https://mcp-falaai.action.tec.br/mcp` |
| **Alternate URL** | `https://api01-falaai.action.tec.br/mcp` |
| **Transport** | Streamable HTTP (stateless) |
| **Auth** | `Authorization: Bearer fai_<key>` — **optional** for `health`, **required** for the analysis tools |
| **Server name** | `FalaAI API` |
| **Source code** | `https://github.com/ActionTecBr/falaai-mcp` |
| **API docs** | `https://falaai.action.tec.br/api` |

## Use any FalaAI API independently

FalaAI is a set of **independent REST APIs**. You **do not** need FalaAI Transcription to use analysis or compliance auditing.

| If you have... | Use |
|---|---|
| Audio but no transcript | `transcribe_audio` |
| An existing transcript | `diagnostic` |
| A transcript needing compliance analysis | `risk_audit` |
| An existing transcript needing both | `diagnostic` + `risk_audit` |
| A WhatsApp export (`.zip`/`.txt`) | `extract_whatsapp` |
| Your own STT provider (Whisper, Deepgram...) | **skip** `transcribe_audio` |

## What is FalaAI API?

FalaAI API is an **AI conversation-intelligence API** for customer-service, contact-center, sales, messaging and other business conversations — speech-to-text (diarization + audio events), conversation analysis (summary, reason, action, topic, sentiment) and a **compliance/risk audit** (COPC CX, ISO 18295-1). Zero-storage. The **MCP server exposes the four analysis tools plus a free `health` check** over the Model Context Protocol so an AI assistant can use them directly.

## Tools

| Tool | Use it for | Required | Optional | Returns |
|---|---|---|---|---|
| `health` | Test the connection / API version (**free**) | — | — | health + version (status, version, uptime, database, phase, launch date) |
| `transcribe_audio` | Turn an audio file into a diarized transcript | `file_base64` | `filename`, `model`, `language`, `client_reference_id` | transcription: text, dialog, audio events, input metadata |
| `diagnostic` | Fast diagnosis of a conversation | `language` (+ `dialog` or `text`) | `response_language`, `duration_seconds`, `participants`, `call_direction`, `audio_events`, `client_reference_id` | summary, reason, action, topic, sentiment, participants |
| `risk_audit` | Compliance / quality audit of a call | `dialog`, `duration_seconds`, `language` | `response_language`, `participants`, `call_direction`, `audio_events`, `response_format`, `client_reference_id` | compliance risk score, verdict, detections and an auditable HTML report |
| `extract_whatsapp` | Extract and segment a WhatsApp export | `file_base64`, `timezone`, `date_format` | `filename`, `start`, `end`, `windows`, `gap_minutes`, `min_messages`, `voice_note`, `expression`, `dry_run`, `client_reference_id` | segmented conversations with speakers, dialogs, counts and usage |

> Uploads are sent as `file_base64` (base64 of the file bytes). `extract_whatsapp` supports a **free preview** with `dry_run=true` (validates, segments and estimates credits without calling the AI and without charging).
>
> **Start with `health`** to confirm the connection and the API version (free, no credits) before any billable call.

## Why FalaAI

- **Audit 100%, not a sample** — every conversation gets a score, not a random QA sample.
- **Auditable by design** — a compliance risk score with detections/violations and an HTML report you can hand to an auditor.
- **Credits, not tokens** — every call consumes credits and the response tells you exactly how many. Monthly plans + non-expiring top-up packs.
- **Zero-storage** — audio/text/results are processed and discarded; only usage/audit records remain. TLS in transit + at rest, per-account isolation (multitenancy), LGPD (you are the controller; Action Tec is the processor).
- **Multilingual** — transcription in **31 languages**, diagnosis in **21 locales**, risk audit and HTML reports in **3** (PT/EN/ES) · **start free** (no card).
- **B2B ready** — one Enterprise plan serves N clients, no per-user fee (ideal for ISVs).

## Use cases

- **Contact center / Quality** — audit 100% of conversations instead of a sample.
- **Compliance / Legal** — auditable evidence for audits and disputes.
- **CX / Operations** — risk score, sentiment and reason per conversation.
- **Sales** — analyze sales calls and extract structured outcomes.
- **BI / Data** — typed JSON ready for your database or analytics stack.

## Integrations

The MCP server is **client-agnostic** — any assistant that speaks Model Context Protocol can use it, at **any point** of your pipeline (capture, transcription, analysis or audit).

| Ecosystem | How |
|---|---|
| **AI assistants (Gemini, Claude, Cursor, VS Code…)** | Add the MCP server and let the assistant call the tools. |
| **PABX / telephony (3CX, Asterisk, Genesys)** | Send the recording to `transcribe_audio`, or the transcript to `diagnostic`/`risk_audit`. |
| **CRM / ERP (Odoo, Salesforce, SAP Service Cloud, HubSpot)** | Attach the structured JSON (summary, reason, sentiment, risk score) to the record. |
| **Contact center / QA (Zendesk, Twilio Flex, Take Blip)** | Batch-audit conversations and feed the score into your QA dashboard. |
| **Chatbots / messaging (WhatsApp, Telegram, Microsoft Teams)** | Extract conversations from an export and analyze them. |

> These are **integration examples**, not certified native integrations. Any platform can integrate through **REST / cURL** — the standard, language-independent path.

## BYOT — Bring Your Own Transcription

Already have speech-to-text (Whisper, Deepgram, AssemblyAI, your own)? **Skip `transcribe_audio`** and send the text straight to **`diagnostic`** and/or **`risk_audit`**. You only pay for what you use.

## Connect your MCP client

Add the server with the **Name** and **URL** above. You can add it **without a key** and call `health` to confirm the connection; to use the **analysis tools**, configure the **Authorization header** `Bearer fai_<your_key>`. **Client UIs vary by product/version** — the two values that matter are the **URL** and the **Authorization header**.

### Gemini (custom MCP server)

1. Open your account/interface **Settings**.
2. Go to **Connected apps / Tools / MCP (Custom MCP Servers)**.
3. Add a new custom server:
   - **Name:** `FalaAI MCP`
   - **Endpoint URL:** `https://mcp-falaai.action.tec.br/mcp`
   - **Authentication:** `Authorization: Bearer fai_<your_key>` (optional for `health`; required for the analysis tools)
4. Save — the FalaAI tools become available to the assistant.

### Other clients (Claude, Cursor, VS Code, …)

Add an MCP server with type **Streamable HTTP** (remote), URL `https://mcp-falaai.action.tec.br/mcp`, and the header `Authorization: Bearer fai_<your_key>`. Some clients use a JSON config; the two values that matter are the **URL** and the **Authorization header**.

## Credits & limits

- **`health` is free** — it tests the connection/version and never consumes credits.
- Every analysis call **consumes credits**; the response returns exactly how many (`usage.credits_consumed`). Check plans, credits and usage on the [Dashboard](https://falaai.action.tec.br/api/dashboard).
- `extract_whatsapp` with `dry_run=true` is a **free preview** (no AI, no charge).
- Uploads travel as `file_base64` inside the request (keep large recordings within the MCP request size); transcription supports audio up to **3 hours**, and files are **processed and discarded** (zero-storage).
- Need the full file-size limits, formats and plans? See the [API reference](https://api01-falaai.action.tec.br/docs) and the [product page](https://falaai.action.tec.br/api).

## Security

- Authenticate every request with `Authorization: Bearer fai_<key>`.
- **Never** commit or share real keys — store them in your assistant's secret/config.
- The MCP server is **stateless** and **zero-storage** (LGPD); only billing/usage metadata is logged.

## Links

- Website: <https://falaai.action.tec.br>
- API base URL: <https://api01-falaai.action.tec.br>
- API reference (Swagger UI): <https://api01-falaai.action.tec.br/docs>
- OpenAPI contract: <https://api01-falaai.action.tec.br/openapi.json>
- Sandbox: <https://falaai.action.tec.br/api#playground>
- Dashboard (keys/credits/usage): <https://falaai.action.tec.br/api/dashboard>
- GitHub organization: <https://github.com/ActionTecBr>