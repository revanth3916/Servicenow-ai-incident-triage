# 🤖 AI Incident Triage Assistant for ServiceNow

A scoped ServiceNow application that uses **Google Gemini** to automatically triage new incidents — suggesting category, impact, urgency, assignment group and fix steps — while keeping a **human in the loop** to accept or reject every suggestion.

Built on a free ServiceNow Personal Developer Instance (PDI) and the Gemini API free tier. **Total cost: $0.**

---

## ✨ Features

- **Automatic triage** — an async Business Rule calls Gemini on every new incident
- **Strict JSON prompt** — reliable, parseable AI responses
- **Guardrails** — only real categories and assignment groups are accepted; impact/urgency limited to 1–3
- **Prompt-injection protection** — ticket text is treated as data, not instructions
- **Automatic model fallback** — if a model returns 503/429, the next model is tried
- **Human-in-the-loop** — Accept / Reject buttons; nothing changes until an agent decides
- **Audit log** — every AI call stores prompt, response, model, duration and decision
- **Dashboard** — acceptance rate, success vs error, average response time

## 📊 Results (demo data)

| Metric | Value |
|---|---|
| AI acceptance rate | 75% |
| Average AI response time | ~2.9 seconds |
| Cost | $0 |

## 🔄 How it works

1. Agent creates an incident
2. Async Business Rule triggers `AITriageHelper` (Script Include)
3. Prompt is built with the instance's real categories and groups
4. Gemini API is called via `RESTMessageV2` (with fallback models)
5. Response is validated and saved to AI fields on the incident
6. Agent clicks **Accept** (values copied + work note) or **Reject** (no changes)
7. Decision is logged and shown on the dashboard

## 🧱 Components

| Type | Name |
|---|---|
| Scoped app | AI Incident Triage (`x_1518641_ai_inc_0`) |
| Script Include | `AITriageHelper` |
| Business Rule | AI Triage on Insert (async) |
| UI Actions | Accept AI Suggestion, Reject AI Suggestion |
| Custom table | AI Triage Log |
| Incident fields | AI Category, Impact, Urgency, Assignment Group, Suggested Fix, Confidence, Reasoning, Triage Status |
| System properties | `gemini_api_key`, `gemini_model` |
| Dashboard | AI Incident Triage (Platform Analytics) |

## 🛠️ Tech stack

ServiceNow (Scoped App, Script Include, Business Rules, UI Actions, GlideRecord, Platform Analytics) · JavaScript · REST API · Google Gemini API · Prompt Engineering

## 🚀 Installation

1. Download the `sys_remote_update_set_*.xml` file from this repo
2. In your instance: **System Update Sets → Retrieved Update Sets → Import Update Set from XML**
3. Open the imported set → **Preview** → **Commit**
4. Get a free API key from [Google AI Studio](https://aistudio.google.com)
5. Set system property `x_1518641_ai_inc_0.gemini_api_key` to your key
6. Create an incident and open the **AI Triage** tab ✅

> ⚠️ Use only demo data with the Gemini free tier.

## 🧩 Challenges solved

- Handled Gemini **503 "high demand"** errors with automatic model fallback
- Found **duplicate Business Rules** causing double API calls — removed, halving usage
- Fixed a **misspelled column** that made log writes fail silently
- Resolved **cross-scope access** for a scoped app reading Global tables

## 👤 Author

**Venkata Sai Revanth Alapati** — ServiceNow Developer
[LinkedIn](https://www.linkedin.com/in/revanth-alapati-a43294194)
