# 🔍 Smart Research Summarizer — AI-Powered n8n Workflow

## What It Does

A 3-node automation that takes any topic from a web form, sends it to Google Gemini 2.5 Flash with a structured prompt, and delivers a fully formatted HTML research summary directly to the user’s inbox — in under 30 seconds.

-----

## The Problem It Solves

Manually researching and summarizing topics is slow and produces inconsistent output. For anyone in L&D, training, or operations who needs quick, readable briefings on unfamiliar topics — compliance updates, regulatory concepts, process terminology — this removes the manual step entirely.

A user submits the form once. The AI handles research depth, audience calibration, and email formatting automatically.

-----

## Workflow Architecture

```
[Form Trigger] → [Gemini 2.5 Flash] → [Gmail: Send HTML Email]
```

### Node 1 — Form Trigger (On Form Submission)

A public n8n form collects 5 inputs:

|Field                                 |Type    |Purpose                                   |
|--------------------------------------|--------|------------------------------------------|
|Your Name                             |Text    |Personalizes the email                    |
|Your Email                            |Email   |Delivery address                          |
|What topic do you want to learn about?|Textarea|Research subject                          |
|How deep should the summary be?       |Dropdown|Quick Overview / Detailed / In-Depth      |
|Explain it like I’m a…                |Dropdown|Beginner / Student / Professional / Expert|

### Node 2 — Google Gemini 2.5 Flash (Message a Model)

- Passes all 5 form fields into a master prompt via n8n expression mode
- Prompt instructs the model to output a complete, styled HTML email (no markdown, no code fences)
- Email structure includes: The Big Idea, Full Picture, Key Takeaways, Did You Know, Now You Can Say, Go Deeper resources, and a personalized footer

### Node 3 — Gmail (Send a Message)

- Recipient pulled dynamically from form input
- Subject line personalized with user’s name
- Email Type set to HTML — the Gemini output renders directly as a designed email

-----

## Tech Stack

- **Automation platform:** n8n Cloud
- **AI model:** Google Gemini 2.5 Flash (via Google AI Studio API)
- **Email delivery:** Gmail (OAuth2)
- **Prompt engineering:** Expression mode with dynamic `$json` variable injection

-----

## How to Import and Run

1. Download `My workflow.json` from this repo
1. Go to your n8n instance → **Create Workflow** → **⋯ menu → Import from file**
1. Add your credentials:
- **Gemini:** Create a Google Gemini (PaLM) API credential → paste your key from [aistudio.google.com/apikey](https://aistudio.google.com/apikey)
- **Gmail:** Connect via OAuth2 (Sign in with Google in n8n Cloud)
1. In the Gemini node, verify the Content field is in **expression mode** (not plain text)
1. Toggle the workflow to **Active**
1. Open the Form URL from the Form Trigger node and submit a test

-----

## Key Design Decisions

- **Gemini outputs the entire HTML email** — no post-processing, no string concatenation. The Gmail node delivers it as-is, keeping the workflow lean at 3 nodes
- **Expression mode for dynamic prompting** — without this, Gemini sees literal `{{ $json['Your Name'] }}` instead of the actual form values
- **Audience-level calibration** — the depth and complexity of the output adjusts based on what the user selects, making the same workflow useful for both beginners and subject experts

-----

## Built By

Shrushti Yadav  
LSSBB | Operational Excellence & Quality Operations  
[LinkedIn](https://linkedin.com) | [GitHub](https://github.com)
