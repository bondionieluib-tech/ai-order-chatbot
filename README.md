<img width="1320" height="685" alt="image" src="https://github.com/user-attachments/assets/f8892180-7fc6-431f-8b8d-045817ba8235" />


AI Customer Support Agent — Facebook Messenger (Cake Business)

An n8n workflow that automates customer support for a cake ordering business on Facebook Messenger — answering common questions, handling orders, and escalating to a human agent when needed, 24/7.

Overview

Customers messaging a business page often go unanswered outside working hours, which risks lost sales and lost trust. This workflow keeps every conversation covered by routing incoming Messenger messages through an AI agent that can answer questions, look up the cake menu, and hand off to a human when a live agent is needed — with no messages falling through the cracks.

How It Works
A webhook (via the Facebook Graph API) receives incoming Messenger messages.
Postgres checks the conversation's status (bot_active or human_active) to decide whether the AI agent should respond or stay out of the way.
If active, the AI agent (Groq, with Gemini as fallback) responds using the Google Sheets cake menu and stored customer/order data.
If the customer asks for a human, a Telegram approval step notifies a live agent with the conversation context; once approved, Postgres flips the status to human_active and the bot steps back.

See docs/ai-customer-support.md for the full write-up, including architecture, problems solved, and known limitations.

Tools
Tool	Role
n8n (self-hosted, Docker)	Workflow orchestration
Facebook Graph API	Messenger webhook
Groq	Primary conversation model
Gemini	Fallback conversation model
Google Sheets	Cake menu source
PostgreSQL	Customer, order, and conversation state
Telegram	Human handoff approval
Status

Actively used and maintained. A RAG pipeline for operating hours and policy documents is planned but not yet implemented.

Related

Part of a two-workflow system — see the companion Automated Order Process Flow and Follow-Up workflow, which handles what happens after an order is placed.
