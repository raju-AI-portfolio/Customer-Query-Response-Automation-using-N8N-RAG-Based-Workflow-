# 🤖 Customer Query & Response Automation using N8N (RAG-Based Workflow)
RAG-based customer support automation built using N8N. The workflow captures customer queries via Telegram, retrieves relevant product information from documents using Pinecone vector search, generates context-aware responses with OpenAI, logs interactions in Google Sheets, and delivers instant AI-powered replies automatically.

**Retrieval-Augmented Generation (RAG) Automation Pipeline**  

---

## 🎯 Objective

Build a **RAG-based automation workflow** using N8N that:

- Captures customer queries via Telegram Bot  
- Retrieves relevant product information from uploaded documents  
- Generates context-aware responses using AI  
- Logs query–response pairs into Google Sheets  
- Sends automated replies back to customers  

This workflow is designed for **XSound Pro Headphones** (or any product knowledge base).

---

## 🚨 Problem Statement

Customer support teams frequently:

- Manually search documents to answer queries  
- Draft responses repeatedly  
- Maintain records manually  
- Face inconsistent response quality  

This leads to:

❌ Delayed communication  
❌ Higher operational workload  
❌ Poor customer experience  

---

## ✅ Solution: RAG-Based Automation

This project implements a **Retrieval-Augmented Generation (RAG)** pipeline using:

- N8N (Workflow Automation)
- Telegram Bot (Query Capture + Response Delivery)
- Google Drive (Document Storage)
- Pinecone (Vector Database)
- OpenAI (Embeddings + Chat Model)
- Google Sheets (Traceability)

---

## 🧠 How RAG Works in This Project

```
Customer Query (Telegram)
            ↓
Telegram Trigger (N8N)
            ↓
Retriever (Pinecone Vector Store)
            ↓
AI Agent (OpenAI Chat Model)
            ↓
Google Sheets (Log Query-Response)
            ↓
Telegram Send Message (Auto Reply)
```
---
<img width="1589" height="672" alt="image" src="https://github.com/user-attachments/assets/b48d7a0d-df20-4627-a43c-109c84500546" />

---

## 🛠️ Tech Stack

- N8N (Self-hosted via GitHub Codespaces)
- OpenAI:
  - text-embedding-3-small (512 dimensions)
  - gpt-4o-mini
- Pinecone Vector Database
- Telegram Bot
- Google Drive
- Google Sheets
- GitHub Codespaces

---

# 🚀 Implementation Steps

---

## 🔹 Step 1: Start Self-Hosted N8N on GitHub Codespaces

### 1️⃣ Create Codespace

- Login to GitHub
- Open GitHub Codespaces
- Start Blank Template

### 2️⃣ Install N8N

```bash
node -v
npm install n8n -g
```

### 3️⃣ Forward Port 5678

- Open Ports tab  
- Forward port 5678  
- Change visibility to Public  

### 4️⃣ Create `.env` File

```env
N8N_HOST=0.0.0.0
N8N_PORT=5678
N8N_PROTOCOL=https

WEBHOOK_URL=YOUR_FORWARDED_ADDRESS
N8N_EDITOR_BASE_URL=YOUR_FORWARDED_ADDRESS
N8N_WEBHOOK_TUNNEL_URL=YOUR_FORWARDED_ADDRESS
```

### 5️⃣ Start N8N

```bash
source .env
n8n start
```

Access N8N via forwarded public URL.

---

## 🔹 Step 2: Import Workflow JSON

- Create New Workflow
- Click ⋮ → Import from File
- Upload provided RAG_Workflow_file.json
- Rename workflow
- Save

---

## 🔹 Step 3: Google Drive Integration

- Create folder: `DOCS_FOR_RAG`
- Upload Product Document (PDF)
- Configure:
  - Google Drive File Created
  - Google Drive File Updated
  - Download File Node

Authenticate using OAuth2.

---

## 🔹 Step 4: Add OpenAI API Keys

Add API keys to:

- Embedding OpenAI Node  
  - Model: text-embedding-3-small  
  - Dimensions: 512  

- OpenAI Chat Model  
  - Model: gpt-4o-mini  

---

## 🔹 Step 5: Pinecone Vector Store (Insert Mode)

### Create Index:

- Index Name: `my-product-files`
- Embedding Model: text-embedding-3-small
- Dimension: 512
- Mode: Serverless
- Cloud: AWS
- Region: Virginia

### Node Configuration:

```
Operation Mode: Insert Documents
Index: my-product-files
```

Execute node to store vectors.

---

## 🔹 Step 6: Create Telegram Bot

1. Open Telegram Web  
2. Search for BotFather  
3. Use command:

```
/newbot
```

4. Set:
   - Bot Name
   - Username
5. Copy API Access Token
6. Open bot URL and click Start

---

## 🔹 Step 7: Telegram Trigger Node

- Add Telegram credentials
- Paste Bot Access Token
- Trigger on: Message
- Execute step
- Test by sending message to bot

---

## 🔹 Step 8: Pinecone Retrieval Node

Configuration:

```
Operation Mode: Retrieve Documents
Index: my-product-files
```

No separate test required.

---

## 🔹 Step 9: AI Agent Node

System Message:

```
You are a helpful Product Support Assistant designed to answer customer queries based on XSound Pro Headphones Product documentation.

Retrieve relevant information from the provided internal document and provide a concise, accurate, and informative answer.

Use the tool "company_documents_tool".

If the answer cannot be found:
"I couldn’t find this information in the product documentation. Please reach out to xsound.support@abccompany.com"

If unrelated question:
"I can only answer questions about XSound Pro Headphones."
```

Execute and test.

---

## 🔹 Step 10: Google Sheets Logging

Create Sheet:

```
Name: Product Query Responses
Columns:
- Query
- Response
```

Node Configuration:

```
Resource: Sheet with Document
Operation: Append Row
Mapping Mode: Manual
```

Map:

- Query → Telegram Trigger Message
- Response → AI Agent Output

---

## 🔹 Step 11: Telegram Send Message Node

Configuration:

```
Resource: Message
Operation: Send Message
Chat ID: From Telegram Trigger
Text: From AI Agent
Append n8n Attribution: OFF
```

---

## 🔹 Step 12: Activate & Test

Activate workflow.

Test queries like:

- What colors are available?
- What ANC modes are available?
- How long does the battery last?
- What noise cancellation options exist?

Verify:

✅ Telegram receives response  
✅ Google Sheet logs Query-Response pair  

---

# 📊 Business Impact

- Instant query resolution
- Knowledge-grounded AI responses
- Reduced support workload
- Improved response consistency
- Full traceability via Google Sheets

---

# 📂 Repository Structure

```
/RAG_Workflow_file.json
/README.md
/screenshots/
/product-document/
/.env.example
```

---

# 🔮 Future Enhancements

- Multi-product support
- Role-based access
- Dashboard analytics
- Slack/WhatsApp integration
- Multi-language query support
- Feedback scoring system

---

# 📜 License

Developed for educational purposes under  
GenAI Automation Mini-Project (RAG-Based Workflow)

---

## 🧠 Key Learning Outcomes

- RAG architecture implementation
- Vector database integration (Pinecone)
- AI-powered automation workflows
- Telegram bot integration
- Document grounding for LLMs
- End-to-end no-code AI deployment

---
Architecture Screenshot

<img width="1623" height="661" alt="image" src="https://github.com/user-attachments/assets/5b1a1ae2-2458-4e7b-b261-4ee2497a74ab" />


🚀 This project demonstrates how businesses can automate customer support using Retrieval-Augmented Generation with N8N.
