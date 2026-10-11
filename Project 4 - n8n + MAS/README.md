# n8n Multi-Agent Research Assistant

A simple multi-agent workflow built in **n8n** for collecting information from the web and turning it into a useful answer. It uses two AI agents in sequence: the first researches the question, and the second prepares the final response.

> **Status:** Academic prototype. Add your own Tavily and Ollama credentials after importing the workflow, then test it in your n8n environment.

## Problem Statement

Finding and organizing information from the web can take time. This project divides the work between two AI agents so that web research and final analysis happen in separate steps.

## Workflow

```text
User question
     ↓
Chat Trigger
     ↓
Agent 1 — Information Collection
     ↕
Tavily Search
     ↓
Agent 2 — Final Analysis and Response
     ↓
Final answer in chat
```

### Agent 1 — Information Collection

- Receives the user's question.
- Uses Tavily to search the web for relevant information.
- Collects findings related to the main keywords in the question.
- Passes its output to the next agent.

### Agent 2 — Final Analysis

- Receives the information collected by Agent 1.
- Organizes the findings into a clear response.
- Can provide a summary, sentiment overview, or conclusion when requested by the user.

Both agents use the configured Ollama chat model. Agent 2 depends on Agent 1's output, so the workflow follows a **sequential multi-agent architecture**.

## Tools Used

- **n8n** — workflow automation and agent orchestration
- **Tavily Search** — web search for Agent 1
- **Ollama** — chat model connection
- **`gemma4:31b-cloud`** — model configured in the exported workflow

## Import and Run

1. Open your n8n instance.
2. Import `My workflow 2 copy.json` into n8n.
3. Set up your own **Tavily API credentials** and select them in the Tavily Search node.
4. Set up your **Ollama credentials** and make sure the configured model is available to your account or endpoint.
5. Open the chat trigger and test the workflow with a question.
6. Check that Agent 1 returns research findings and Agent 2 uses those findings in its final answer.

The exported workflow is inactive by default. Test it in the n8n editor before activating it for regular use. Node labels or credential settings may need to be adjusted for your n8n version.

## Example Query

```text
Research the impact of generative AI on entry-level jobs.
Summarize the main findings, describe the overall sentiment, and give a balanced conclusion.
```

The first agent searches for relevant information. The second agent uses that output to create the final response.

## Limitations

- The quality of the result depends on the search results and the configured language model.
- The workflow does not independently verify every claim, so check important facts and source links before relying on the answer.
- It does not currently save results to a database or export them to a report file.
- Results may vary depending on the question, model, and available web sources.

## Future Improvements

- Add a source-checking step before the final answer.
- Improve the output format with headings and source links.
- Save research results to Google Sheets or export them as a report.

## Repository Contents

```text
.
├── README.md
└── My workflow 2 copy.json
```

## Security Note

Do not upload API keys, passwords, or exported credentials to GitHub. Configure credentials directly in n8n and review workflow exports before sharing them publicly.
