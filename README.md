# 🤖 AI-Powered Renewal Automation Flow

> **Generates tens of thousands in MRR** by automating customer renewal outreach with AI-driven personalisation, churn detection, and intelligent routing.

![AI Renewal Flow Overview](./flow-overview.png)

---

## 📖 Overview

This automation flow uses AI agents to identify at-risk and high-potential customer accounts, craft personalised renewal strategies, and execute outreach — all without manual intervention. It routes customers into **high-touch** or **low-touch** tracks based on engagement level, ensuring the right message reaches the right account at the right time.

Built on [Customerscore.io](https://customerscore.io), the flow integrates with OpenAI, HubSpot, Gmail, and Slack to close the loop from data to action.

---

## 🗺️ Flow Architecture

The flow is composed of three main stages:

1. **Data Ingestion** — Pull customer health and context data
2. **Strategy & Filtering** — AI analyses engagement, scores risk, and routes accounts
3. **Execution** — Personalised outreach via high-touch or low-touch tracks

---

## 🔢 Stage 1 — Data Ingestion

![Data Ingestion](./step1-data-ingestion.png)

| Node | Description |
|------|-------------|
| **Trigger** | Scheduled or event-based trigger that kicks off the flow |
| **Company description & Playbooks** | Manually supplied company context and renewal playbooks used to ground the AI |
| **Customerscore.io** | Pulls live customer health scores and static account data |

The **Get customer data** step collects both real-time health metrics and historical static data for each account — providing the AI agents with full context before any analysis begins.

---

## 🧠 Stage 2 — Strategy & Filtering

![Strategy and Filter](./step2-strategy-filter.png)

### Retention Strategist Agent

The **Retention Strategist** is an AI agent (powered by OpenAI Chat Model + Structured Output Parser) that:

- Analyses engagement data and trends
- Identifies churn risk signals and expansion opportunities
- Crafts a personalised retention strategy per account

Sub-components:
- `Chat Mode` + `Memory` — maintains context across reasoning steps
- `Tool` + `Output Parser` — produces structured, machine-readable strategy outputs

### Filter

Only accounts with **churn risk** or **expansion potential** pass through to execution. Neutral-risk accounts are dropped, keeping the flow focused on accounts that matter.

### Route by Engagement Level

A rules-based router segments accounts into three tracks:

| Route | Description |
|-------|-------------|
| `high_touch` | High MRR accounts requiring human involvement |
| `low_touch` | Lower MRR accounts suitable for automated sequences |
| `neutral_risk` | No action taken |

---

## ⚡ Stage 3 — Execution

![Execution](./step3-execution.png)

These agents take the strategy from the Retention Strategist and craft personalised messaging for each track.

---

### 🤝 High Touch Track — Human Touch

> For high MRR accounts, nothing is sent automatically. Only drafts and alerts are created to help the CS team react quickly.

The **High Touch AI Agent** (OpenAI model + structured output parser) generates account-specific messaging and triggers:

| Action | Tool | Details |
|--------|------|---------|
| Create Gmail Draft | Gmail | Drafts a personalised renewal email for CS review |
| Create HubSpot Task | HubSpot | Creates an engagement task in the CRM (`create: engagement`) |
| Send Slack Alert | Slack | Posts a real-time alert to the CS team (`post: message`) |

---

### 📧 Low Touch Track — Tech Touch

> Low touch customers are sent a personalised renewal sequence with specific offers based on their engagement level.

The **Low Touch AI Agent** generates account-specific messaging and triggers:

| Action | Tool | Details |
|--------|------|---------|
| Run low touch personalised sequence | Automation | Enrols the customer in a targeted renewal sequence (`track: event`) |

---

## 🛠️ Tech Stack

| Component | Tool |
|-----------|------|
| Automation platform | [n8n](https://n8n.io) |
| Customer data | [Customerscore.io](https://customerscore.io) |
| AI model | OpenAI GPT-4 (Chat Model) |
| CRM | HubSpot |
| Email | Gmail |
| Alerts | Slack |
| Sequence runner | Customer engagement platform (track: event) |

---

## 🚀 Getting Started

### Prerequisites

- Access to [Customerscore.io](https://customerscore.io) with API credentials
- OpenAI API key
- HubSpot account with API access
- Gmail OAuth credentials
- Slack bot token with `chat:write` permission
- n8n instance (self-hosted or cloud)

### Setup

1. **Clone this repository**
   ```bash
   git clone https://github.com/your-org/ai-renewal-flow.git
   cd ai-renewal-flow
   ```

2. **Import the workflow** into your n8n instance using the provided JSON file.

3. **Configure credentials** in n8n for:
   - Customerscore.io
   - OpenAI
   - HubSpot
   - Gmail
   - Slack

4. **Update the Company description & Playbooks node** with your company's context, renewal playbooks, and tone guidelines.

5. **Set your trigger** — configure the schedule or event that should kick off the flow (e.g. 90 days before renewal date).

6. **Activate** the workflow.

---

## 🔄 Flow Summary

```
Trigger
  └─► Company description & Playbooks
        └─► Customerscore.io (Get customer data)
              └─► Retention Strategist AI Agent
                    └─► Filter (churn risk / expansion potential only)
                          └─► Route by Engagement Level
                                ├─► high_touch ─► High Touch AI Agent
                                │                     ├─► Create Gmail Draft
                                │                     ├─► Create HubSpot Task
                                │                     └─► Send Slack Alert
                                ├─► low_touch  ─► Low Touch AI Agent
                                │                     └─► Run personalised sequence
                                └─► neutral_risk ─► (no action)
```

---

## 📄 License

MIT License — see [LICENSE](./LICENSE) for details.

---

Built with ❤️ by [Customerscore.io](https://customerscore.io)
