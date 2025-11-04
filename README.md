# 🧠 “Ask the Docs” AI Agent – Workflow Guide

> This GitHub README describes the complete step-by-step logic for the **Ask the Docs** AI Agent workflow implemented in **MindStudio**.

---

## 🧩 Overview

The **Ask the Docs** agent allows users to ask questions about any uploaded product documentation (PDF).  
It uses **RAG (Retrieval-Augmented Generation)** to retrieve relevant information from the docs and answer accurately.

The agent performs three main functions:

1. **Input & Relevance Check** – captures user questions and filters out unrelated ones.  
2. **RAG Pipeline** – enhances the query, retrieves content, and generates an answer.  
3. **Error Handling** – provides friendly feedback for off-topic questions.

---

## ⚙️ Workflow Summary

### Canvas Flow (as shown in MindStudio)


---

## 🧭 Step-by-Step Logic

### 🟦 Step 1 – Start Block
- **Purpose:** Entry point for each user interaction.
- **Action:** Triggers the workflow automatically when the user sends a message.
- **No configuration** needed.

---

### 🟩 Step 2 – User Input Block
- **Block Name:** `User Input`
- **Group:** Input
- **Goal:** Capture user’s question dynamically during runtime.

**Configuration:**
- **Label:** “Ask a question about the documentation”
- **Variable Name:** `user_question`
- **Type:** Text

**Output:**  
`user_question` → passed to `Logic` block for relevance checking.

---

### 🟨 Step 3 – Logic Block (Relevance Check)
- **Block Name:** `Logic`
- **Group:** Input
- **Goal:** Check if the user’s question is relevant to the specified topic or product.

**Prompt Example:**
> “You are a classifier. Determine if the question is related to the product documentation.  
> Reply with one word only: `RELEVANT` or `NOT_RELEVANT`.”

**Inputs:**  
- `user_question`

**Outputs:**
- `relevance_label` = `"RELEVANT"` or `"NOT_RELEVANT"`

**Routing Logic:**
- If `RELEVANT` → Proceed to RAG section (`Generate Text` → `Query Data Source` → `Generate Text` → `End`)
- If `NOT_RELEVANT` → Route to `Display Content` (Error Handling)

---

### ⚠️ Step 4 – Error Handling Block (Display Content)
- **Block Name:** `Display Content`
- **Group:** Error Handling
- **Goal:** Display message when the user asks an off-topic question.

**Triggered When:** `relevance_label == "NOT_RELEVANT"`

**Configuration:**
- **Message Example:**
  > “I can only answer questions about the product documentation.  
  > Please ask a question related to the product or its features.”

**End Behavior:**  
Flow terminates after displaying the message.

---

### 🔍 Step 5 – Generate Enriched Query (RAG Step 1)
- **Block Name:** `Generate Text`
- **Group:** Generate Queries, Search DB with RAG
- **Goal:** Rewrite the user’s question into a concise and context-rich search query.

**Inputs:**
- `user_question`

**Prompt Example:**
> “Rewrite the user’s question into a short, keyword-rich query suitable for searching a product documentation database.  
> Do not answer the question.”

**Output Variable:**
- `enhanced_query`

**Next Step:**  
`enhanced_query` → `Query Data Source`

---

### 📚 Step 6 – Query Data Source (RAG Step 2)
- **Block Name:** `Query Data Source`
- **Group:** Generate Queries, Search DB with RAG
- **Goal:** Retrieve relevant information from the uploaded documentation using vector search.

**Inputs:**
- `enhanced_query`
- Linked Data Source: the uploaded product documentation PDF (e.g., `MindStudio_Documentation_compressed.pdf`)

**Configuration:**
- **Search Type:** Vector query
- **Top-K Results:** 3–5
- **Return Fields:** Text snippets, sections, or metadata from docs.

**Outputs:**
- `retrieved_context` – relevant content extracted from the document.

**Next Step:**  
`retrieved_context` → `Generate Text` (final answer generation)

---

### 💬 Step 7 – Generate Final Answer (RAG Step 3)
- **Block Name:** `Generate Text`
- **Group:** Generate Queries, Search DB with RAG
- **Goal:** Generate a grounded response to the user’s question using retrieved documentation.

**Inputs:**
- `user_question`
- `enhanced_query`
- `retrieved_context`

**Prompt Example:**
> “You are a documentation assistant. Answer the user’s question **only** using the provided context.  
> If the information is not in the context, say that you’re unsure and suggest where to find it.  
>  
> Question: {{user_question}}  
> Search Query: {{enhanced_query}}  
> Context: {{retrieved_context}}”

**Output Variable:**
- `final_answer`

**Next Step:**  
Connect to `End` block to output the answer.

---

### 🟢 Step 8 – End Block
- **Block Name:** `End`
- **Goal:** Return the AI’s final answer to the chat interface.

**Input:**  
`final_answer` (from previous block)

**Action:**  
Displays the generated answer to the user.  
Conversation remains open for follow-up questions (multi-turn chat).

---

## 🧠 Workflow Summary (Text Form)

1. **User asks a question** → captured as `user_question`.
2. **Logic check:** Classify as `RELEVANT` or `NOT_RELEVANT`.
   - ❌ Not Relevant → Show error message.
   - ✅ Relevant → Continue.
3. **Enhance query** for vector search.
4. **Query data source** (PDF documentation).
5. **Generate grounded answer** using retrieved context.
6. **Return answer** to user and wait for next question.

---

## 🧪 Testing Scenarios

| Scenario | Input | Expected Behavior |
|-----------|--------|-------------------|
| Relevant | “How do I connect a data source in MindStudio?” | Fetch answer from docs and display. |
| Irrelevant | “Tell me a joke.” | Display relevance warning message. |
| Multi-turn | “How do I set up the agent?” → “Can it handle follow-up questions?” | Maintains conversation and answers contextually. |

---

## 🚀 Tips for Customization

- **Change Data Source:** Replace with your own documentation PDF.  
- **Adjust Relevance Sensitivity:** Modify classification prompt to be stricter or more lenient.  
- **Improve Answers:** Add examples in the final LLM prompt to improve style or format.  
- **Add Source Citations:** Include page or section references in the `final_answer`.

---

## 🧾 Deliverables for the Lab

By the end of this exercise, you should have:

- A fully functional **Ask the Docs Agent** in MindStudio.
- Working RAG flow using:
  - `Generate Text`
  - `Query Data Source`
  - `Logic` and `Display Content` for error handling.
- Uploaded documentation as a vectorized Data Source.


---

### ✅ Summary

This workflow demonstrates how to build a **RAG-powered documentation assistant** using MindStudio with:
- Dynamic user input  
- Intelligent topic filtering  
- Vector-based retrieval  
- Context-aware answer generation  

It’s a foundation you can expand for:
- API doc chatbots  
- Product FAQ bots  
- Customer support assistants  

---

**Author:** Your Name  
**Lab:** AI Agent Lab #8 – “Ask the Docs” Chat Bot  
**Platform:** [MindStudio](https://mindstudio.ai)
