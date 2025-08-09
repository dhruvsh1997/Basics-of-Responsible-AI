# 📚 Responsible AI Library Chatbot Repository

Welcome to the **Responsible AI Library Chatbot Repository**! This project showcases three Jupyter notebooks that implement secure and ethical chatbots using **LangChain**, **Guardrails-AI**, and **OpenAI** APIs. Each chatbot is designed for a specific domain (library, politics, furniture) with robust guardrails to ensure **Responsible AI** principles, prevent **prompt injection attacks**, and maintain safe, ethical interactions.

---

## Table of Contents
- [Overview](#overview)
- [What is Responsible AI?](#what-is-responsible-ai)
  - [Key Principles of Responsible AI](#key-principles-of-responsible-ai)
  - [Responsible AI Practices](#responsible-ai-practices)
- [Guardrails in Responsible AI](#guardrails-in-responsible-ai)
  - [Why Use Guardrails?](#why-use-guardrails)
  - [How Guardrails Are Implemented](#how-guardrails-are-implemented)
- [Prompt Injection Prevention Techniques](#prompt-injection-prevention-techniques)
- [Notebooks Overview](#notebooks-overview)
  - [📖 Notebook 1: Library Chatbot](#notebook-1-library-chatbot)
  - [🏛 Notebook 2: Politics Chatbot](#notebook-2-politics-chatbot)
  - [🪑 Notebook 3: Furniture Chatbot](#notebook-3-furniture-chatbot)
- [Setup Instructions](#setup-instructions)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

This repository demonstrates the development of three domain-specific chatbots with a focus on **Responsible AI**. The chatbots leverage **LangChain** for structuring LLM interactions, **Guardrails-AI** for output validation, and additional safety mechanisms like input sanitization and moderation APIs. Each chatbot is restricted to its domain, ensuring ethical behavior and preventing misuse through prompt injection or harmful content.

🔑 **Key Features**:
- Domain-specific responses (library, politics, furniture).
- Multi-layered guardrails for safety and compliance.
- Robust prompt injection prevention.
- Logging for accountability and debugging.
- User-friendly error messages for transparency.

---

## What is Responsible AI? 🌐

**Responsible AI** refers to the development and deployment of artificial intelligence systems in a manner that is ethical, transparent, and aligned with societal values. It ensures AI systems are safe, fair, and accountable while minimizing risks like bias, harm, or misuse.

### Key Principles of Responsible AI
- **🛡️ Safety**: AI systems must not cause harm, either through malicious outputs or unintended consequences.
- **⚖️ Fairness**: AI should avoid discrimination and ensure equitable treatment across diverse groups.
- **📢 Transparency**: Users should understand how AI makes decisions and when it refuses to respond.
- **🔍 Accountability**: Developers must track and audit AI behavior to ensure compliance with ethical standards.
- **🔒 Privacy**: AI must protect user data and avoid unauthorized access or leaks.
- **⚙️ Robustness**: AI systems should be reliable, secure, and resistant to manipulation.

### Responsible AI Practices
The following practices are implemented in this repository to uphold Responsible AI principles:
1. **Domain Restriction**: Each chatbot is limited to a specific topic (e.g., books, politics, furniture) to prevent off-topic or sensitive responses.
2. **Input Validation**: Inputs are checked for relevance and safety using allowlists and sanitization.
3. **Output Validation**: Outputs are validated using Pydantic schemas or `.rail` specs to ensure compliance with predefined rules.
4. **Content Moderation**: The OpenAI Moderation API (in the furniture chatbot) checks for harmful content like hate speech or violence.
5. **Logging and Monitoring**: Comprehensive logging tracks errors and interactions for debugging and accountability.
6. **Polite Refusals**: Out-of-scope or restricted queries receive clear, user-friendly error messages.
7. **User Preference Storage**: The politics chatbot uses an `InMemoryStore` to store user preferences, enhancing personalization while maintaining privacy.

---

## Guardrails in Responsible AI 🛑

### Why Use Guardrails?
Guardrails are constraints and validation mechanisms that ensure AI systems operate within ethical and operational boundaries. They are critical for:
- **Preventing Harm**: Blocking inappropriate or harmful responses.
- **Enforcing Compliance**: Ensuring outputs align with predefined rules and ethical guidelines.
- **Mitigating Prompt Injection**: Preventing malicious inputs from manipulating the AI.
- **Maintaining Consistency**: Ensuring responses are relevant and formatted correctly.
- **Enhancing Trust**: Providing users with reliable and safe interactions.

### How Guardrails Are Implemented
This repository uses **Guardrails-AI** and custom validation techniques to enforce Responsible AI:
- **System Prompts**: Define strict roles and restrictions (e.g., "Only answer book-related questions").
- **Allowlist Filtering**: Inputs must contain domain-specific keywords (e.g., "book," "furniture") to be processed.
- **Deny Constraints**: Explicitly block sensitive topics (e.g., Israel-Palestine in the politics chatbot).
- **Pydantic Schemas**: Validate output structure and content (e.g., `LibraryAnswer`, `PoliticsAnswer`).
- **Moderation API**: Checks inputs and outputs for harmful content (furniture chatbot).
- **Regex-Based Sanitization**: Detects and blocks prompt injection attempts.

---

## Prompt Injection Prevention Techniques 🚨

**Prompt injection** occurs when a user crafts input to bypass AI restrictions or elicit unintended behavior (e.g., "Ignore all previous instructions"). The following techniques are used across the notebooks to prevent prompt injection:

1. **🔍 Input Sanitization (All Notebooks)**:
   - **Description**: Regular expressions detect suspicious patterns in user inputs.
   - **Patterns Checked**:
     - `r"(?i)ignore.*instructions"`: Blocks attempts to override system prompts.
     - `r"(?i)system role is now"`: Prevents role changes.
     - `r"(?i)run this code"`: Blocks code execution attempts.
     - `r"(?i)show.*finance"`, `r"(?i)how much.*earn"`, `r"(?i)how much.*invest"`: Blocks finance-related queries.
   - **Implementation**: The `sanitize_input` function raises an error if a pattern is detected, stopping further processing.

2. **📋 Allowlist Filtering (Library and Furniture Chatbots)**:
   - **Description**: Inputs must contain domain-specific keywords to be processed.
   - **Library Chatbot**: Keywords like "book," "novel," "author."
   - **Furniture Chatbot**: Keywords like "wood," "furniture," "chair."
   - **Implementation**: The `check_allowlist` (library) and `check_furniture_related` (furniture) functions reject inputs lacking relevant keywords.

3. **🚫 Deny Constraints (Politics Chatbot)**:
   - **Description**: The `.rail` spec explicitly denies inputs containing restricted keywords (e.g., "Israel," "Palestine," "Gaza").
   - **Implementation**: Guardrails-AI raises an error if restricted keywords are detected.

4. **🛡️ System Prompt Restrictions (All Notebooks)**:
   - **Description**: System prompts explicitly limit the chatbot’s scope and instruct it to refuse certain topics (e.g., finances, internal operations).
   - **Implementation**: `ChatPromptTemplate` with `SystemMessagePromptTemplate` defines strict roles.

5. **🔎 Moderation API (Furniture Chatbot)**:
   - **Description**: OpenAI’s Moderation API checks inputs and outputs for harmful content (e.g., hate speech, violence).
   - **Implementation**: The `moderate_content` function flags and rejects unsafe content.

6. **✅ Output Validation (Library and Politics Chatbots)**:
   - **Description**: Guardrails-AI validates LLM outputs against Pydantic schemas or `.rail` specs to ensure compliance with expected format and content.
   - **Implementation**: `LibraryAnswer` (library) and `PoliticsAnswer` (politics) schemas enforce structured outputs.

---

## Notebooks Overview

### 📖 Notebook 1: Library Chatbot
**File**: `library_chatbot.ipynb`

**Description**: A chatbot for answering questions about books, novels, and learning resources using **LangChain**, **Guardrails-AI**, and the **Grok** model (LLaMA3-8b-8192).

**Workflow**:
1. **Initialization**: The `ExecuteLibraryChatbot` class sets up the Grok model with a low temperature (0.2) for consistent responses.
2. **Prompt Setup**: A `ChatPromptTemplate` restricts responses to book-related topics and defines user input handling.
3. **Input Validation**:
   - **Allowlist Check**: Ensures inputs contain keywords like "book," "novel," or "author."
   - **Sanitization**: Uses regex to block prompt injection patterns.
4. **Processing**: The input is processed by the LLM pipeline, and the output is validated using a Pydantic schema (`LibraryAnswer`).
5. **Response**: Returns the validated answer or a polite refusal.

**Block Diagram**:
```
[User Input] --> [Allowlist Check] --> [Sanitize Input] --> [LLM Pipeline] --> [Guardrails Validation] --> [Output]
   |                |                     |                     |                    |
   |                Reject if no         Reject if             Process input        Validate output
   |                book keywords       injection detected    with Grok            with Pydantic
   V                                                                    |
Reject with error message                                             Return validated answer
```

**Guardrails and Prompt Injection Prevention**:
- **System Prompt**: Restricts responses to books and learning resources.
- **Allowlist**: Requires book-related keywords.
- **Sanitization**: Blocks patterns like "ignore instructions."
- **Guardrails-AI**: Validates outputs using `LibraryAnswer` schema.

**Responsible AI**:
- **Safety**: Sanitization and allowlist prevent harmful responses.
- **Transparency**: Polite refusals for out-of-scope queries.
- **Accountability**: Logs errors to console and file (`library_chatbot.log`).

**Example Usage**:
```python
chatbot = ExecuteLibraryChatbot(groq_api_key)
print(chatbot.ask("Tell me about the plot of 'Pride and Prejudice'."))
# Output: [Description of the plot]
print(chatbot.ask("How much does your library make every month?"))
# Output: "Sorry, I can't help with that."
```

---

### 🏛 Notebook 2: Politics Chatbot
**File**: `politics_bot.ipynb`

**Description**: A chatbot for general politics questions using **LangChain**, **Guardrails-AI**, and **OpenAI GPT-4o-mini**, with restrictions on sensitive topics like Israel-Palestine.

**Workflow**:
1. **Initialization**: Sets up GPT-4o-mini and a custom `GuardrailsParserWithAPI` for Guardrails integration.
2. **Prompt Setup**: A `ChatPromptTemplate` restricts responses to general politics and enforces JSON output.
3. **Guardrails Setup**: A `.rail` spec defines the output format (`PoliticsAnswer`) and denies inputs with restricted keywords.
4. **Processing**: The LLM chain processes the input, and Guardrails validates the output.
5. **Memory Store**: An `InMemoryStore` stores user preferences (e.g., short answers).
6. **Response**: Returns a JSON response or a refusal for restricted topics.

**Block Diagram**:
```
[User Input] --> [Guardrails Deny Check] --> [LLM Chain] --> [Guardrails Output Parser] --> [JSON Output]
   |                     |                       |                     |
   |                     Reject if              Process with         Validate output
   |                     Israel/Palestine      GPT-4o-mini          with .rail spec
   V                                           |
Reject with error message                    Store preferences in InMemoryStore
```

**Guardrails and Prompt Injection Prevention**:
- **System Prompt**: Limits responses to general politics and enforces JSON format.
- **Guardrails .rail Spec**: Denies inputs with "Israel," "Palestine," or "Gaza."
- **Custom Parser**: Ensures Guardrails compatibility with LangChain.
- **No Explicit Sanitization**: Relies on Guardrails’ deny constraints.

**Responsible AI**:
- **Fairness**: Avoids sensitive topics to prevent bias.
- **Transparency**: Clear refusals for restricted topics.
- **Accountability**: Stores user preferences for auditing.

**Example Usage**:
```python
question = "What are the main political parties in the USA?"
response = llm_chain.invoke({"question": question})
# Output: {"answer": "The main political parties in the USA are the Democratic Party and the Republican Party."}
```

---

### 🪑 Notebook 3: Furniture Chatbot
**File**: `furniture_chatbot.ipynb`

**Description**: A chatbot for furniture and woodworking questions using **LangChain**, **OpenAI GPT-5-mini** (hypothetical), and **OpenAI Moderation API**.

**Workflow**:
1. **Initialization**: Sets up GPT-5-mini and the Moderation API client.
2. **Prompt Setup**: A `ChatPromptTemplate` restricts responses to furniture and woodworking.
3. **Input Validation**:
   - **Topic Check**: Requires furniture-related keywords.
   - **Sanitization**: Blocks prompt injection patterns.
   - **Moderation**: Checks input with Moderation API.
4. **Processing**: The LLM pipeline processes the input, and the output is checked with the Moderation API.
5. **Response**: Returns a safe response or a refusal.

**Block Diagram**:
```
[User Input] --> [Furniture Keyword Check] --> [Sanitize Input] --> [Moderation API] --> [LLM Pipeline] --> [Moderation API] --> [Output]
   |                     |                       |                    |                   |                    |
   |                     Reject if no           Reject if           Reject if           Process with        Reject if
   |                     furniture keywords    injection detected  input flagged       GPT-5-mini          output flagged
   V                                                                          |
Reject with error message                                                  Return safe response
```

**Guardrails and Prompt Injection Prevention**:
- **System Prompt**: Restricts responses to furniture and woodworking.
- **Allowlist**: Requires keywords like "wood," "furniture."
- **Sanitization**: Blocks patterns like "ignore instructions."
- **Moderation API**: Flags harmful content in inputs and outputs.

**Responsible AI**:
- **Safety**: Moderation API ensures no harmful content.
- **Transparency**: Polite refusals for out-of-scope or flagged content.
- **Accountability**: Logs to console and file (`furniture_chatbot.log`).

**Example Usage**:
```python
print(ask_furniture_chatbot("What type of wood is best for a dining table?"))
# Output: [Description of wood types]
print(ask_furniture_chatbot("How much does your furniture store make every month?"))
# Output: "Sorry, I can only answer questions about woods, furniture, products, brands, or materials."
```

---

## Setup Instructions 🛠

1. **Clone the Repository**:
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
   Required packages: `langchain`, `langchain_groq`, `langchain_openai`, `guardrails-ai`, `openai`, `python-dotenv`, `langgraph`.

3. **Set Up API Keys**:
   - Add **GROQ_API_KEY** and **OPENAI_API_KEY** to Colab secrets or a `.env` file:
     ```
     GROQ_API_KEY=your_groq_api_key
     OPENAI_API_KEY=your_openai_api_key
     ```

4. **Run Notebooks**:
   Open the notebooks in Jupyter or Google Colab and execute the cells.

---

## Usage 🚀

1. **Library Chatbot**:
   - Run `library_chatbot.ipynb`.
   - Ask book-related questions (e.g., "Tell me about the plot of 'Pride and Prejudice'.").
   - Non-book queries are rejected.

2. **Politics Chatbot**:
   - Run `politics_bot.ipynb`.
   - Ask general politics questions (e.g., "What are the main political parties in the USA?").
   - Restricted topics (e.g., Israel-Palestine) are blocked.

3. **Furniture Chatbot**:
   - Run `furniture_chatbot.ipynb`.
   - Ask furniture-related questions (e.g., "What type of wood is best for a dining table?").
   - Non-furniture or harmful queries are rejected.

---

## Contributing 🤝

We welcome contributions! Follow these steps:
1. Fork the repository.
2. Create a new branch (`git checkout -b feature/your-feature`).
3. Commit changes (`git commit -m "Add your feature"`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request.

---

## License 📜

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.