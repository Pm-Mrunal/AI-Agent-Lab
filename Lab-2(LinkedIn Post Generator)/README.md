# Lab 2 – LinkedIn Post Generator

> Build an AI Agent that automatically generates and posts engaging LinkedIn content from any webpage — powered by MindStudio.

---

## 1. Lab Overview

In this lab, you will create a **LinkedIn Post Generator** AI Agent that can:
- Analyze the content of any webpage or article.
- Automatically generate a **LinkedIn-ready post** summarizing and commenting on that content.
- Let a human review and approve the post.
- Finally, post it directly to LinkedIn using the MindStudio **Browser Extension** integration.

---

## 2. Learning Objectives

By completing this lab, you’ll learn how to:
1. Use the **MindStudio Browser Extension** to trigger an agent from a webpage.
2. Configure an **AI workflow** that analyzes webpage content and generates a professional LinkedIn post.
3. Use a **Checkpoint** for human review before automated publishing.
4. Connect and use **LinkedIn actions** for content posting.

---

## 3. AI Agent Spec

**Agent Trigger:**  
> The agent should be triggered via the **Browser Extension**.

**Functionality:**
- Analyze content from an article or webpage.
- Write a LinkedIn Post and include the link to the original source.
- Allow for **human revisions** and **final post approval**.
- If approved → automatically post to LinkedIn.  
- If rejected → end the workflow.

---

## 4. Workflow Diagram

Below is the visual logic of your MindStudio flow:


---

## 5. Step-by-Step Workflow Logic

### 🟦 Step 1 – Start Block
**Purpose:**  
Initial trigger when the user activates the agent from a webpage using the Browser Extension.

**Behavior:**  
- Captures webpage context automatically (page title, meta description, or full text depending on the browser extension configuration).
- Passes the text to the **Generate Text** block.

---

### 🟩 Step 2 – Generate Text Block
**Purpose:**  
Analyze the captured content and draft a LinkedIn post.

**Inputs:**
- `webpage_content` (captured automatically)
- `webpage_url`

**Prompt Example:**
> “Analyze the content of this webpage.  
> Create a well-formatted and engaging LinkedIn post about it.  
> Use markdown and symbols to make it easy to read.  
> Include the link to the original webpage at the bottom of the post.  
> Reply with only the LinkedIn post text — nothing else.”

**Settings:**
| Setting | Value |
|----------|--------|
| Output Behavior | Save to variable |
| Variable Name | `content` |
| Output Schema | Text (Default) |
| Model | `Gemini 2.0 Flash Lite (google/gemini-2.0-flash-lite-001)` |
| Temperature | 2 |
| Max Response Size | 4000 |

**Output:**  
- A ready-to-review LinkedIn post saved to variable `content`.

---

### 🟨 Step 3 – Checkpoint Block (Human Review)
**Purpose:**  
Pause the flow to allow a human to review or edit the generated post.

**Functionality:**
- Displays the AI-generated post (`content`) to the human reviewer.
- Reviewer chooses:
  - ✅ **Approve** → proceed to post.
  - ❌ **Reject** → stop the workflow.

**Checkpoint UI Text Example:**
> “Here’s the draft LinkedIn post.  
> Please review and approve or reject before posting.”

---

### 🟦 Step 4 – Create LinkedIn Post Block
**Purpose:**  
Publish the approved post directly to LinkedIn.

**Inputs:**
- `content` (approved LinkedIn post text)
- Optional: `webpage_url` (append at end)

**Behavior:**
- Posts the content to the connected LinkedIn account via the MindStudio integration.
- Confirms completion and transitions to **End** block.

---

### 🟥 Step 5 – End Block
**Purpose:**  
Conclude the workflow.

**Branches:**
- **Approved path:** End after successful posting.
- **Rejected path:** End after displaying a message such as  
  _“Post rejected — workflow ended without publishing.”_

---

## 7. Error & Exit Handling

| Scenario | Workflow Behavior |
|-----------|------------------|
| Browser Extension fails to capture text | Display error “No webpage content found.” |
| Human rejects post | Workflow ends immediately. |
| LinkedIn post creation fails | Display system error and log failure. |

---

## 8. Test Scenarios

### ✅ Scenario 1 – Standard Flow
- Trigger via Browser Extension on a webpage.
- Agent generates post → human approves → posts to LinkedIn.  
**Expected:** Post appears successfully on LinkedIn.

### ❌ Scenario 2 – Rejection Flow
- Trigger agent → human reviewer rejects content.  
**Expected:** Workflow ends with message “Post rejected — no action taken.”

### ⚠️ Scenario 3 – No Webpage Content
- Trigger on a blank or restricted page.  
**Expected:** Error message “No readable content found on this page.”

---

## 9. Deliverables

By the end of this lab, you should have:

- A **LinkedIn Post Generator** agent in MindStudio that:
  - Is triggered via Browser Extension.
  - Generates and formats a LinkedIn post.
  - Allows human approval before posting.
  - Integrates directly with LinkedIn publishing API.

---

## 11. Reflection Questions

1. How does integrating a **human approval checkpoint** improve trust and quality in automation workflows?  
2. What are some ways you could personalize LinkedIn posts using additional metadata from the webpage (author, tags, etc.)?  
3. How would you adapt this agent to generate **Twitter/X** posts instead?

---

### 🧩 Summary

This lab demonstrates how to create a **browser-triggered AI publishing assistant** in MindStudio.  
You built an agent that:
- Understands webpage content.  
- Creates human-like social media posts.  
- Involves a human in the loop for review.  
- Automates publishing to LinkedIn.

---

**Author:** Mrunal Surve 
**Lab:** AI Agent Lab #10 – LinkedIn Post Generator  
**Platform:** [MindStudio](https://mindstudio.ai)
