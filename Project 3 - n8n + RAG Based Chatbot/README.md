# PCI DSS 4.0 RAG Chatbot using n8n

A simple document-based chatbot built with **n8n**, **Ollama**, and **Retrieval-Augmented Generation (RAG)**. It answers questions using information retrieved from a PCI DSS 4.0 PDF.

## What It Does

- Downloads the PCI DSS 4.0 PDF from Google Drive.
- Extracts the document content and creates embeddings using Ollama.
- Stores document chunks in n8n Simple Vector Store.
- Accepts questions through the n8n chat interface.
- Retrieves relevant passages and uses an AI Agent to generate an answer.

## Tools Used

- **n8n** — workflow automation and chat interface
- **Ollama `qwen3:4b`** — chat model
- **Ollama `mxbai-embed-large:latest`** — embeddings model
- **Google Drive** — PDF source
- **Simple Vector Store** — temporary vector storage

## How It Works

### 1. Load the PDF

`Manual Trigger → Google Drive: Download file → Simple Vector Store (Insert Documents)`

The Default Data Loader reads the PDF, and Ollama Embeddings converts its content into vectors. The document chunks are stored under the key `vector_store_key`.

### 2. Chat with the PDF

`Chat Trigger → AI Agent`

The AI Agent uses `qwen3:4b` to answer questions and can call the upper Simple Vector Store node to retrieve relevant document passages. Simple Memory provides conversation context.

## Setup

### Requirements

- A working n8n installation
- [Ollama](https://ollama.com/) installed and running
- Access to the source PDF in Google Drive
- Google Drive and Ollama credentials configured in n8n

Download the models in a terminal:

```bash
ollama pull qwen3:4b
ollama pull mxbai-embed-large
```

### Run the Workflow

1. Import `Workflow of Rag based chat model using n8n.json` into n8n.
2. Open the **Download file** node and configure your Google Drive credentials and PDF selection.
3. Check the **Ollama Chat Model** and **Embeddings Ollama** nodes and confirm the models are available.
4. Confirm both Simple Vector Store nodes use the key `vector_store_key`; the lower node inserts documents and the upper node retrieves documents as an AI Agent tool.
5. Run the lower ingestion workflow using **Execute workflow** and confirm the PDF content is stored.
6. Open the chat panel and ask a question about PCI DSS 4.0.
7. Check the execution details to confirm the AI Agent used the vector-store retrieval tool.

## Example Question and Expected Answer

**Question:**

> According to the PCI DSS 4.0 PDF, what is PCI DSS, what is its purpose, and which entities or environments does it apply to?

**Expected answer:**

PCI DSS (Payment Card Industry Data Security Standard) is an information security standard that sets technical and operational requirements to protect payment card account data. Its purpose is to improve cardholder data security and promote consistent security practices.

According to the PDF, it applies to entities that store, process, or transmit cardholder data or sensitive authentication data; systems and environments that can affect the security of the Cardholder Data Environment (CDE); and entities responsible for protecting account data. The CDE includes the relevant system components, people, and processes involved in handling this data.

## Troubleshooting

- **Answers mention loans, LTV, or FOIR:** Check that the AI Agent's System Message and the vector-store tool description contain PCI DSS instructions, not old loan-related instructions. Start a new chat session after updating them.
- **The retrieval tool is not used:** Confirm the upper Simple Vector Store is connected to the AI Agent's **Tool** port and its description tells the agent to search the PCI DSS PDF.
- **No relevant documents are found:** Run the ingestion workflow again and check the Google Drive download, vector-store key, and embedding settings.
- **Ollama errors:** Ensure Ollama is running and both models are installed.

## Notes

- The n8n **Simple Vector Store stores data in memory** and is intended for demos. Data may be lost when n8n restarts, so run ingestion again when needed.
- Response quality depends on the content successfully extracted and retrieved from the PDF.
- Configure your own credentials after importing the workflow. **Never commit passwords, OAuth secrets, or API keys to GitHub.**
- Include the PDF in the repository only if you have permission to redistribute it.

## Repository Files

```text
.
├── README.md
└── Workflow of Rag based chat model using n8n.json
```

## Purpose

This project demonstrates how RAG can connect a language model to a document knowledge source so users can ask questions and receive answers grounded in the supplied PDF.
