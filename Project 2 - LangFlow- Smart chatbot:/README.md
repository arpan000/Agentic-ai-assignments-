# LangFlow – Smart Chatbot for Machine Learning and Deep Learning

## Overview

**LangFlow – Smart Chatbot** is a prompt-based educational chatbot designed to help students learn Machine Learning (ML) and Deep Learning (DL). It uses a visual LangFlow workflow and a Large Language Model (LLM) to answer questions in a clear, approachable way.

The chatbot is configured as **LearnML**, a friendly learning companion that can explain concepts step by step, provide practical examples, help with Python code, compare ML methods, and create quizzes or interview questions when asked.

## Project Objectives

- Make ML and DL concepts easier for beginners to understand.
- Provide clear explanations supported by practical examples.
- Help students explore Python-based ML tasks.
- Support revision with comparisons, practice questions, and interview preparation.
- Demonstrate how prompt engineering and an LLM can be connected through a visual LangFlow workflow.

## Technology Stack

| Technology / Component | Purpose |
| --- | --- |
| **LangFlow** | Builds and connects the chatbot workflow visually. |
| **Prompt Template** | Defines the chatbot's role, instructions, and response style. |
| **Llama 3.2 (3B)** | Generates responses to students' questions. |
| **Chat Input** | Receives the student's message. |
| **Chat Output** | Displays the generated answer. |
| **Python and scikit-learn** | Used in code examples when relevant to a question. |

The workflow screenshot uses the model `llama3.2:3b`. The model is commonly run locally through Ollama; the exact model-provider configuration should match the setup in your LangFlow installation.

## Workflow Architecture

```text
Student Question
      |
      v
  Chat Input
      |
      v
 Prompt Template
(role + instructions
 + {input} question)
      |
      v
 Language Model
  (llama3.2:3b)
      |
      v
  Chat Output
      |
      v
  Answer to Student
```

### How It Works

1. **Chat Input:** The student enters a question in the LangFlow Playground.
2. **Prompt Template:** LangFlow inserts the student's message into the `{input}` variable and combines it with the chatbot's learning instructions.
3. **Language Model:** The configured LLM processes the prepared prompt and generates a response.
4. **Chat Output:** The response is shown to the student in the Playground.

## Key Features

- **Beginner-friendly explanations:** Starts with plain language and builds up to technical details.
- **Step-by-step learning:** Breaks complex topics into smaller ideas.
- **Practical examples:** Connects theory with familiar or real-world applications.
- **Python assistance:** Can provide code examples and explain important lines.
- **Concept comparisons:** Can describe differences, strengths, limitations, and use cases.
- **Practice support:** Can generate quizzes, practice tasks, or interview questions when requested.
- **Focused scope:** Prioritizes Machine Learning and Deep Learning topics.

## Project Files

```text
LangFlow-Smart-Chatbot/
├── prompt.txt       # Instructions used in the Prompt Template node
├── Output.txt       # Example response explaining supervised learning
└── README.md        # Project documentation
```

`Output.txt` is an example response, not a fixed response used by the chatbot. Actual answers are generated dynamically and may vary between runs.

## Setup and Usage

### Prerequisites

- Python and LangFlow installed in your environment.
- Access to the LLM provider configured in the workflow.
- If using Ollama locally, Ollama must be installed and the `llama3.2:3b` model must be available.

### 1. Prepare the model (Ollama setup)

If your LangFlow Language Model component is configured to use Ollama, install Ollama from its official website and pull the model:

```bash
ollama pull llama3.2:3b
```

Make sure the Ollama service is running before testing the workflow. If you use a different provider, follow that provider's setup instructions instead.

### 2. Open LangFlow

Start LangFlow using the method appropriate for your installation. For a pip-based installation, the usual command is:

```bash
langflow run
```

Open the local address printed in the terminal by LangFlow.

### 3. Build or update the workflow

Connect these components in the following order:

1. `Chat Input`
2. `Prompt Template`
3. `Language Model` configured for `llama3.2:3b`
4. `Chat Output`

Copy the contents of `prompt.txt` into the Prompt Template component. Keep the `{input}` variable unchanged so the question from Chat Input is inserted into the prompt. Ensure the component connections match the workflow diagram above.

> **Note:** This repository includes the prompt and an example output. A LangFlow exported workflow file is not included unless you add one separately.

### 4. Test the chatbot

Open the LangFlow Playground and try questions such as:

- `Explain supervised learning using a daily-life example.`
- `Compare classification and regression.`
- `Show a small Python example of a classification model.`
- `Create five beginner-level interview questions on deep learning.`
- `Explain overfitting and suggest ways to reduce it.`

Review the answers for correctness, clarity, and relevance.

## Example Output

The included `Output.txt` demonstrates a possible answer to the question **“What is supervised learning?”** It describes the concept, outlines a typical workflow, compares classification and regression, and includes a small scikit-learn example.

The chatbot generates answers at runtime; the example file is provided for reference and does not guarantee identical output.

## Current Scope and Limitations

- This is a **prompt-based chatbot workflow**.
- The workflow shown does not include a document knowledge base, vector database, or Retrieval-Augmented Generation (RAG) pipeline.
- Response quality depends on the configured model and the prompt.
- AI-generated explanations and code should be checked before use, especially for academic or production work.
- The chatbot is configured to focus on ML and DL, rather than act as a general-purpose assistant.

## Possible Future Improvements

- Add a curated ML/DL document collection and retrieval to ground answers in trusted course material.
- Add conversation memory for multi-turn tutoring.
- Include user-selectable levels such as beginner, intermediate, and advanced.
- Add a quiz mode with answers and explanations.
- Export and include the LangFlow workflow JSON so others can import it directly.
- Add a set of verified evaluation questions to assess answer quality.

## Author / Credits

Created as a learning project to explore visual AI workflow building, prompt engineering, and LLM-powered educational assistance.

---

**Disclaimer:** This chatbot is an educational aid. Verify important technical details and test generated code before relying on it.
