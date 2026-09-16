AI Customer Support Agent for Facebook Messenger (Cake Business)

Problem

Customers reaching out through online stores or social media marketplaces often have questions, place inquiries, or follow up on previous concerns. Inevitably, some of these messages go unanswered — during busy hours or after the business closes for the day. Left unattended, this can mean lost sales, lost trust, and unresolved issues piling up.

Automating these conversations solves that gap. With an AI agent handling incoming messages, customers get a response any time of day, common questions about products and services are answered instantly, and the business no longer has to choose between staying responsive and stepping away. The result: faster replies, fewer missed opportunities, and a more consistent customer experience.

Architecture

<img width="1358" height="688" alt="ai-order-chatbot-workflow" src="https://github.com/user-attachments/assets/be8d19c7-d870-42dd-bdb2-c625320bc46d" />


Tools Used
n8n — self-hosted via Docker

Google Sheets — cake menu lookup

PostgreSQL — stores customer, order, and conversation data

Telegram — human handoff approval

Groq — primary conversation model

Gemini — fallback conversation model

Facebook Graph API — webhook integration for Messenger

Problems Solved

Preventing the AI agent from interfering during human handoff A Postgres node placed right after the webhook stores the conversation ID and checks the status column on the conversations table. If the status is bot_active, the message is routed through the AI agent as usual. If it's human_active, a second Postgres lookup combined with an IF node routes the message around the AI agent entirely — so a live agent can take over without the bot jumping back in.

Handling live-agent handoff requests When a customer asks to speak with a human, a Telegram node sends the conversation context — including the customer's name and phone number — and waits for an approval action. Once approved, the workflow updates the status column in the conversations table to human_active, which fully hands the conversation over to a human agent and keeps the AI agent out of it going forward.

Working around free-tier rate limits Groq's free tier comes with rate limits, and some models don't always have enough throughput to keep a structured process running smoothly. To keep conversations moving, the workflow falls back to a secondary model that follows the same instructions and handles the conversation while the primary model's rate limit resets before the next turn.

Learning SQL through real use Rather than using Google Sheets or Airtable, this project uses Postgres — partly to practice core SQL operations (INSERT, UPDATE, CREATE, and general query manipulation) as the workflow required them. It's also proven faster to query and manipulate directly in Postgres than to work through a spreadsheet-style interface.

Model selection for reliability GPT-OSS-120B showed a high hallucination rate during testing. Switching to Qwen models produced more consistent instruction-following and fewer hallucinations. Prompt tightening and monitoring are ongoing to keep the agent aligned with its intended behavior.

Limitations & Next Steps

A RAG pipeline to store operating hours, policies, and workflow-specific guidelines is a planned next step. It's not implemented yet — some of that information is currently handled through prompting instead — but it's a natural extension once time allows, likely on a future project where a RAG setup is a better fit for what's being built.
