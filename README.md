# RAG Loan Inquiry Agent

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/faith-amanze/rag-loan-inquiry-agent/blob/main/Loan_Inquiry_Agent.ipynb)

**To try it:** click the button, add your own free `GROQ_API_KEY` and `NGROK_AUTHTOKEN` in Colab Secrets, run the cells in order, and open the link the last cell prints.

A loan inquiry chatbot that answers customer questions using only a set of loan policy documents. Built as a hands-on project during **AI Bootcamp 2026 (Day 2: LLM / Agentic AI / AI Engineering)**, delivered by **Data Science Nigeria (DSN), led by Elisha (Jesutofunmi) Odemakinde** in **October 2026**.

> **Author:** Faith Amanze · **Cohort / Class:** AI Bootcamp 2026 · **Instructor:** Elisha (Jesutofunmi) Odemakinde

## About the training

This project was part of a practical training on building agentic AI applications. The goals were to:

- Use a Large Language Model (LLM) through an API without hardcoding credentials.
- Apply **Retrieval-Augmented Generation (RAG)** so the model answers from trusted documents instead of guessing.
- Store and search documents with a vector database (ChromaDB).
- Expose the agent through a web service with Flask and a public tunnel (ngrok).
- Debug real-world errors in notebooks, APIs and deployment.

## What it does

A user asks a question such as *"What documents do I need for a loan?"*. The agent finds the most relevant policy text and answers only from that text. If the answer isn't in the policies, it says so and suggests contacting customer support.

## How it works

```
User question
     │
     ▼
ChromaDB (vector store)  ──►  finds the 2 most relevant policy passages
     │
     ▼
LLM (Groq, openai/gpt-oss-20b)  ──►  answers using ONLY those passages
     │
     ▼
Flask web chat page (shared with ngrok)
```

1. **Retrieval:** policy documents are stored in ChromaDB. The user's question is matched against them by meaning.
2. **Generation:** the retrieved passages and the question go to the LLM with a system prompt that restricts it to the provided context.
3. **Delivery:** a small Flask app serves a chat page, and ngrok creates a temporary public link.

## Tech stack

| Part | Tool |
|---|---|
| Notebook | Google Colab |
| LLM | Groq (OpenAI-compatible API), `openai/gpt-oss-20b` |
| Vector database | ChromaDB |
| Web server | Flask |
| Public link | ngrok (via pyngrok) |
| Language | Python |

## Setup

1. Open `Loan_Inquiry_Agent.ipynb` in Google Colab.
2. Create free accounts at [console.groq.com](https://console.groq.com) and [ngrok.com](https://dashboard.ngrok.com/get-started/your-authtoken).
3. In Colab, open the **Secrets** tab (key icon) and add:
   - `GROQ_API_KEY`
   - `NGROK_AUTHTOKEN`

   Turn on **Notebook access** for both.
4. Run the cells **in order, top to bottom**.
5. Run the web chat cell and open the link it prints. If ngrok shows a warning page, click **Visit Site**.

> Never paste API keys into code cells or screenshots. Always read them with `userdata.get(...)`.

## Customizing

- **Change what the agent knows:** edit the `policies` list in the ChromaDB cell.
- **Change how it behaves:** edit the system prompt in the agent cell.
- **Change the model:** edit `MODEL`. List currently available models with `client.models.list()`.

## Troubleshooting (issues I met and fixed)

| Problem | Cause | Fix |
|---|---|---|
| `SecretNotFoundError` | Secret missing or Notebook access off | Add the secret and turn on access |
| `429 insufficient_quota` | OpenAI account has no credits | Use a free provider such as Groq |
| `400 Invalid Auth key` with Gemini | New `AQ.` keys didn't work with the OpenAI-compatible endpoint | Use Groq instead |
| `404 model does not exist` | Provider removed the model | List models and update `MODEL` |
| `NameError` | Cells run out of order | Restart the session and run from the top |
| `Method Not Allowed` on `/webhook` | A browser sent GET to a POST-only route | Open the chat link instead |
| `SyntaxError` after editing | Text pasted twice into a line | Fix the line and re-run |
| WhatsApp bot gets only the default auto-reply | Free Twilio trial doesn't support custom reply webhooks | Use the web chat, or upgrade Twilio |

## What I learned

- Keeping secrets out of code and screenshots, and revoking a key once it is exposed.
- Why cell order matters in notebooks.
- How RAG reduces made-up answers by grounding the model in documents.
- How free-tier limits and changing model names affect real projects.
- How webhooks, POST requests and public tunnels work.
- Reading error messages to find the real cause.

## Design notes and limitations

- The original brief was a WhatsApp agent using Twilio. The free Twilio trial can't send custom replies, so the final version uses a **web chat** with the same agent logic. The agent function can be reused behind a WhatsApp webhook on a paid Twilio account.
- It runs only while the Colab session is connected, and the ngrok link changes each run.
- Anyone with the link can use the LLM quota, so share it carefully.
- The sample policy figures are for demonstration, not real lending terms.

## Possible next steps

- Load real policy documents (PDF or text files).
- Deploy permanently on a hosting service instead of Colab.
- Add WhatsApp through a paid Twilio account.
- Add conversation memory and basic usage limits.

## Acknowledgements

Thanks to **Elisha (Jesutofunmi) Odemakinde and Data Science Nigeria (DSN)** for the training and guidance.
