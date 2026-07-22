# 🚀 AI Lead Generation & Sales Outreach Automation System

An end-to-end AI-powered Lead Generation workflow built in **n8n** that automatically finds businesses, audits their websites, qualifies leads, generates personalized cold emails, schedules follow-ups, creates professional AI proposals, updates CRM, and notifies the sales team.

---

# 📌 Features

- 🔍 Google Business Search using SerpAPI
- 🌐 Website Crawling using Jina AI
- 🤖 AI Website Audit
- ⭐ AI Lead Scoring
- 📧 Personalized Cold Email Generation
- ⏳ Automatic Follow-up Emails
- 📄 AI Proposal Generator
- 📑 Professional PDF Proposal
- ☁️ Google Drive Upload
- 📊 Google Sheets CRM
- 💬 Slack Notifications
- 📩 Gmail Integration
- 🔄 Duplicate Lead Detection
- 📈 ROI Estimation
- 💡 AI Automation Recommendations

---

# 🏗 Workflow Overview

```
Webhook

↓

Validate Request

↓

Google Search (SerpAPI)

↓

Extract Websites

↓

Website Crawl

↓

AI Website Audit

↓

Lead Qualification

↓

Cold Email Generator

↓

Google Sheets CRM

↓

Send Email

↓

Schedule Follow-up

↓

Slack Notification

↓

Proposal Generator

↓

HTML Proposal

↓

PDF

↓

Google Drive Upload

↓

Send Proposal

↓

Webhook Response
```

---

# 🧠 AI Agents Included

## 1. Website Audit Agent

Analyzes:

- Website Design
- UI/UX
- Mobile Friendliness
- SEO
- Trust Signals
- Lead Capture
- CRM Readiness
- AI Opportunities
- Sales Funnel
- Conversion Rate

Returns structured JSON.

---

## 2. Lead Score Agent

Calculates:

- Website Score
- Buying Intent
- Estimated Budget
- Closing Probability
- Priority
- Recommended Offer

---

## 3. Cold Email Agent

Generates:

- Personalized Subject
- Email Body
- CTA
- Follow-up Days

---

## 4. Follow-up Agent

Creates short follow-up emails after the configured waiting period.

---

## 5. Proposal Generator

Creates:

- Executive Summary
- AI Solutions
- ROI
- Timeline
- Pricing
- Benefits
- Next Steps

---

# 🛠 Tech Stack

- n8n
- Groq AI (Llama 3.3 70B)
- SerpAPI
- Jina AI
- Google Sheets
- Gmail API
- Slack API
- Google Drive API
- PDFBolt

---

# 📂 Folder Structure

```
assets/
docs/
prompts/
templates/
examples/
screenshots/
workflow.json
README.md
```

---

# 📥 Webhook Request

```json
{
  "search_query": "Dentists",
  "location": "New York",
  "industry": "Healthcare",
  "business_type": "Dental Clinic"
}
```

---

# 📤 Output

- Website Audit
- Lead Score
- Personalized Cold Email
- Follow-up Email
- Proposal PDF
- CRM Entry
- Slack Notification

---

# 🔄 Automation Flow

```
Search Businesses

↓

Analyze Website

↓

Score Lead

↓

Generate Email

↓

Store CRM

↓

Send Email

↓

Wait

↓

Follow-up

↓

Generate Proposal

↓

Upload PDF

↓

Send Proposal

↓

Notify Team
```

---

# 📊 Integrations

✅ SerpAPI

✅ Groq AI

✅ Google Sheets

✅ Gmail

✅ Google Drive

✅ Slack

✅ PDFBolt

---

# 🎯 Business Use Cases

- AI Agencies
- Marketing Agencies
- Lead Generation Companies
- CRM Automation
- Sales Teams
- Consultants
- Freelancers

---

# 🚀 Future Improvements

- Apollo Integration
- Hunter.io Email Finder
- BuiltWith Tech Detection
- LinkedIn Enrichment
- Multi-step Email Sequences
- HubSpot CRM
- Airtable CRM
- PostgreSQL Database
- WhatsApp Outreach
- Loom Video Generation
- AI Voice Outreach

---

# 👨‍💻 Author

**Piyush Kumar**

Founder — **AutoNode**

AI Automations & AI Agents for Business

LinkedIn

https://www.linkedin.com/in/piyush-ai

GitHub

https://github.com/autonode-x

---

⭐ If you found this project useful, don't forget to star the repository.