# Blimp Marketing Agency – AI Proposal/Audit/Content Calendar/Admin Agent (n8n)

This repository contains an **n8n chat-based AI agent workflow** that automatically generates:

- ✅ **Marketing Proposal**
- ✅ **Digital Audit Report**
- ✅ **30-Day Content Calendar**
- ✅ **Admin outputs** (emails, follow-ups, meeting agenda, onboarding checklist, simple reports)

It works by receiving a chat message, extracting key details (client name, budget, links, etc.), routing to the correct task, generating content using an LLM (Groq OpenAI-compatible API), and exporting the final output as an **HTML file**.

---

## What this workflow does (simple)

1. A user sends a message in chat (natural language or JSON).
2. The workflow extracts:
   - task type (proposal / audit / content_calendar / admin)
   - client details (name, industry, goal, budget, links, email)
3. A **Switch** node routes the request to the correct branch.
4. The workflow calls the LLM API (Groq) to generate output.
5. Output is saved as **HTML file** using “Convert to File”.

---

## Workflow Overview (Nodes)

### 1) `When chat message received`
- Trigger node (LangChain Chat Trigger).
- Starts workflow when you send a message to the chat endpoint.

### 2) `Code in JavaScript`
- Parses the chat input.
- Detects task automatically:
  - If message contains words like `audit` → task = audit
  - If contains `calendar` → task = content_calendar
  - If contains `email/admin` or email address → task = admin
  - Otherwise default = proposal
- Extracts fields like: client name, budget, website link, timeline, etc.

### 3) `Switch`
Routes based on:
- `proposal`
- `audit`
- `content_calendar`
- `admin`

### 4) Knowledge nodes
Each branch has a `(Blimp Knowledge)` Set node that provides:
- `blimp_services`
- `pricing_rules`
- `terms`
- `tone`
- `blimp_website` (in proposal branch)

### 5) HTTP Request nodes (LLM generation)
Each branch hits Groq API (`/openai/v1/chat/completions`) using OpenAI-style payload:

- `http_Proposal` → generates proposal format
- `HTTP Request (Audit)1` → generates audit report format
- `HTTP Request (Calender))` → generates 30-day calendar table
- `HTTP Request (Admin)` → generates admin task output format

### 6) Extract nodes
After LLM response, the workflow extracts text from:
- `choices[0].message.content`

### 7) File Content nodes
Cleans up output for file generation.

### 8) Convert to File nodes
Creates HTML files:
- `Client - proposal.html`
- `Client - Audit.html`
- `Client - calender.html`
- `Client - admin.html`

---

## Requirements

- **n8n** (self-hosted or cloud)
- Groq API key (OpenAI-compatible endpoint)
- Optional: n8n Credentials manager for storing API keys securely

---

## IMPORTANT Security Note (Fix this before using)
Your workflow JSON currently shows a **hardcoded Groq API key** inside the HTTP headers:

```json
"Authorization": "Bearer gsk_..."
