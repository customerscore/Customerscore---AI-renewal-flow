# CustomerScore.io Renewal Flow

AI-powered **customer renewal analysis and engagement automation
workflow** built with n8n.

This workflow evaluates customer health before renewal using AI,
categorizes them into strategic segments (churn risk, expansion,
neutral), and automatically triggers the appropriate customer success
actions.

Depending on the risk and engagement level, it either: - Triggers
**high-touch Customer Success Manager (CSM) intervention** - Launches
**automated retention or expansion campaigns**

------------------------------------------------------------------------

# Overview

The workflow runs on a schedule and performs the following steps:

1.  Pulls customer data from a **CustomerScore workflow**
2.  Uses AI to analyze engagement and renewal risk
3.  Classifies customers into strategic segments
4.  Routes them to either high-touch or low-touch engagement
5.  Executes outreach through email, Slack, HubSpot, or Customer.io

This enables Customer Success teams to **prioritize human effort while
automating scalable lifecycle engagement**.

------------------------------------------------------------------------

# Workflow Architecture

    Schedule Trigger
          │
          ▼
    Customer Data Pull (CustomerScore)
          │
          ▼
    AI Strategist Analysis
    (OpenAI + Structured Output)
          │
          ▼
    Routing Logic
          │
     ┌────┴───────────────┐
     │                    │
     ▼                    ▼
    High Touch           Low Touch
    (CSM Intervention)   (Automation)
     │                    │
     ▼                    ▼
    Gmail Draft           Customer.io Campaign
    HubSpot Task
    Slack Alert

------------------------------------------------------------------------

# Customer Segmentation Logic

The AI strategist analyzes customer data and assigns:

### Strategic Segment

-   `churn_risk`
-   `expansion`
-   `neutral`

### Engagement Level

-   `high_touch` → Customer Success Manager intervention required
-   `low_touch` → handled via automation

### Urgency

-   `immediate`
-   `high`
-   `moderate`
-   `low`

------------------------------------------------------------------------

# High Touch Flow (CSM Intervention)

For high-value or critical customers, the workflow generates
**personalized outreach and internal alerts**.

## Actions Generated

1.  **Personalized Email Draft**
    -   Created in Gmail
    -   Tailored to customer usage patterns
2.  **HubSpot Task**
    -   Internal CSM follow-up task
    -   Includes full customer context
3.  **Slack Alert**
    -   Notifies Customer Success team
    -   Includes urgency indicator and summary

## AI Model

Used to generate personalized communication and context-aware messaging
for the CSM team.

------------------------------------------------------------------------

# Low Touch Flow (Automated Campaigns)

Lower-risk or scalable accounts are handled automatically through
**Customer.io campaigns**.

## Campaign Types

### Churn Prevention

  Campaign                Trigger
  ----------------------- ---------------------------------------
  `churn_save_urgent`     \<14 days to renewal with severe risk
  `churn_save_gentle`     Moderate churn signals
  `churn_save_adoption`   Low usage or feature adoption issues

### Expansion

  Campaign                     Trigger
  ---------------------------- --------------------------------------
  `expansion_capacity_alert`   Seat utilization \>85%
  `expansion_upsell_offer`     Strong engagement and growth signals

The AI chooses the optimal campaign and sends structured event data to
Customer.io.

------------------------------------------------------------------------

# Customer Health Signals

The workflow evaluates several engagement and product usage indicators.

### Engagement Metrics

-   Engagement score
-   Time spent in product
-   Login frequency

### Product Adoption

-   Active integrations/plugins
-   Feature usage

### Team Usage

-   Seat utilization
-   Active users

### Sales Activity

-   Deals tracked
-   Deal activity trends

### Support Activity

-   Support ticket volume
-   Resolution rate

------------------------------------------------------------------------

# Inputs

Customer data is pulled from another workflow:

`Customerscore - pull customers`

Typical fields include:

    customer_name
    mrr
    plan
    days_until_renewal
    engagement_score
    seat_utilization_percent
    active_plugins
    deals

------------------------------------------------------------------------

# Outputs

## High Touch

  Action           Tool
  ---------------- ---------
  Email draft      Gmail
  Follow‑up task   HubSpot
  Internal alert   Slack

## Low Touch

  Action                            Tool
  --------------------------------- -------------
  Personalized lifecycle campaign   Customer.io

------------------------------------------------------------------------

# Required Integrations

The workflow requires credentials for the following tools:

-   OpenAI API
-   Gmail
-   Slack
-   HubSpot
-   Customer.io
-   n8n instance

------------------------------------------------------------------------

# Setup Instructions

## 1. Import Workflow

In n8n:

    Import → From File

Upload the workflow JSON.

## 2. Configure Credentials

Add credentials for:

-   OpenAI
-   Gmail
-   Slack
-   HubSpot
-   Customer.io

## 3. Configure Slack Channel

Update the Slack node channel ID.

## 4. Connect CustomerScore Workflow

Ensure the following workflow exists in your instance:

    Customerscore - pull customers

------------------------------------------------------------------------

# Use Cases

This workflow is ideal for:

-   SaaS **renewal management**
-   Customer Success **churn prevention**
-   **Expansion opportunity detection**
-   **AI-assisted CSM workflows**
-   Automated lifecycle marketing

------------------------------------------------------------------------

# Benefits

-   AI-driven customer health analysis
-   Prioritizes human CSM attention where it matters
-   Automates large-scale customer engagement
-   Identifies expansion opportunities automatically
-   Reduces churn risk before renewal

------------------------------------------------------------------------

# License

You can adapt and modify this workflow for internal or commercial use.
