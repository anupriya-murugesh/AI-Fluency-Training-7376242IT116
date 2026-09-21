# AI-Fluency-Training-7376242IT116

AI Fluency Training DAY 1 Lab – Chatbot vs Rule-Based Workflow vs AI Agent

# AI Fluency Training – Day 1

## 1. Project Overview

This project demonstrates three different approaches to answering questions about private college course-fee data:

1. **Chatbot** – sends the question directly to an LLM.
2. **Rule-Based Workflow** – uses predefined Python rules without an LLM.
3. **AI Agent** – allows the LLM to decide when to use tools such as fee lookup and calculation.

The project also includes a challenge task to compare how the systems handle a new type of request.

---

## 2. Problem Statement

The college has private course-fee information that should not be guessed by an AI model.

### Course Fee Data

| Course Code | Fee |
|---|---:|
| CS101 | Rs. 12,000 |
| AI202 | Rs. 18,000 |
| DS303 | Rs. 15,000 |

The systems were tested using the following questions:

1. What is the fee for AI202?
2. What is the total fee for CS101 and AI202 after a 10% scholarship?
3. Is DS303 more expensive than CS101, and by how much?
4. Write a two-line welcome message for new AI students.

---

## 3. Systems Implemented

### System 1 – Chatbot

The chatbot sends the user's question directly to the LLM.

**Flow:**

```text
User Question
      ↓
     LLM
      ↓
   Answer
```

It does not have direct access to the private course-fee data.

---

### System 2 – Rule-Based Workflow

The workflow uses Python rules and the stored course-fee dictionary.

**Flow:**

```text
User Question
      ↓
Extract Course Codes
      ↓
Apply Python Rules
      ↓
Calculate Result
      ↓
   Answer
```

It does not use an LLM.

---

### System 3 – AI Agent

The AI agent uses the LLM to decide whether a tool is required.

Available tools:

- `get_course_fee` – retrieves the fee for a course.
- `calculator` – performs arithmetic calculations.

**Flow:**

```text
User Question
      ↓
     LLM
      ↓
Does it need a tool?
   ↙          ↘
 Yes           No
  ↓             ↓
Call Tool    Final Answer
  ↓
Observe Result
  ↓
LLM decides next action
  ↓
Final Answer
```

The agent follows a **Reason → Act → Observe → Repeat** pattern when tools are required.

---

## 4. Tools Used

### `get_course_fee`

Looks up the fee for a course code.

Example:

```text
get_course_fee("AI202")
→ 18000
```

### `calculator`

Performs arithmetic calculations using supported mathematical operations.

Example:

```text
calculator("(12000 + 18000) * 0.9")
→ 27000
```

---

## 5. Agent Trace – Question 2

**Question:**

> What is the total fee for CS101 and AI202 after a 10% scholarship?

The agent used three tool calls:

| Step | Tool Called | Arguments | Result |
|---|---|---|---:|
| 1 | `get_course_fee` | `{"course_code": "CS101"}` | 12000 |
| 2 | `get_course_fee` | `{"course_code": "AI202"}` | 18000 |
| 3 | `calculator` | `{"expression": "(12000+18000)*0.9"}` | 27000.0 |

The final answer was:

> The total fee for CS101 and AI202 after a 10% scholarship is ₹27,000.

### Calculation

```text
CS101 = ₹12,000
AI202 = ₹18,000

Total = ₹30,000

10% scholarship = ₹3,000

Final fee = ₹27,000
```

---

## 6. Observations

| Criterion | Chatbot | Rule-Based Workflow | AI Agent |
|---|---|---|---|
| Q1 correct? | No | Yes | Yes |
| Q2 correct? | No | Yes | Yes |
| Q3 correct? | No | No | Yes |
| Q4 handled well? | Yes | No | Yes |
| Challenge question handled? | Not Tested | No | Yes |
| Same output on repeat run? | No | Yes | No |
| Number of LLM calls per question | 1 | 0 | 1 or more |
| Private course data accessed? | No | Yes | Yes |
| Tool calls used? | No | No | Yes |

### Explanation

- **Chatbot:** It could not answer the three fee-related questions because it did not have access to the private course-fee data. It successfully generated a two-line welcome message for Q4.
- **Rule-Based Workflow:** It correctly answered Q1 and Q2 because those cases were implemented in the Python rules. It could not handle Q3 because a comparison rule was not implemented. It also rejected Q4 because the workflow only handles course-fee questions.
- **AI Agent:** It correctly answered all four questions. It used `get_course_fee` to retrieve private fee data and `calculator` for arithmetic operations. It also successfully handled the challenge question by checking the possible course combinations within the Rs. 30,000 budget.
- **Repeat run:** The rule-based workflow produced exactly the same output on both runs. The chatbot and AI agent produced slightly different natural-language wording on the second run, while the agent produced the same correct results.

---

## 7. Challenge Question

**Question:**

> I can pay Rs. 30,000. Which two courses can I take together within this budget?

### Rule-Based Workflow

The workflow could not handle this question and returned:

```text
Sorry, I can only answer course-fee questions.
```

### AI Agent

The agent retrieved the fees of all three courses and calculated the possible combinations.

| Course 1 | Course 2 | Combined Fee |
|---|---|---:|
| CS101 | AI202 | ₹30,000 |
| CS101 | DS303 | ₹27,000 |

The combination **AI202 + DS303** costs ₹33,000 and exceeds the budget.

Therefore, the two valid combinations within the ₹30,000 budget are:

- CS101 + AI202 = ₹30,000
- CS101 + DS303 = ₹27,000

---

## 8. Comparison

| Feature | Chatbot | Rule-Based Workflow | AI Agent |
|---|---|---|---|
| Uses LLM | Yes | No | Yes |
| Uses private fee data | No | Yes | Yes |
| Uses tools | No | No | Yes |
| Handles calculations | LLM | Python | Calculator tool |
| Dynamic decision making | No | No | Yes |
| General conversation | Yes | No | Yes |
| Handles predefined fee questions | No | Yes | Yes |
| Handles new tool-based questions | No | No | Yes |

---

## 9. Project Structure

```text
AI training Day 1/
│
├── agent.py
├── chatbot.py
├── challenge.py
├── check_setup.py
├── config.py
├── tools.py
├── workflow.py
├── requirements.txt
├── README.md
├── .gitignore
│
└── output-screenshots/
    ├── check_setup.png
    ├── chatbot.png
    ├── workflow.png
    ├── toola_agent.png
    └── challenge.png
```

---

## 10. Key Learning

This exercise demonstrates the difference between a simple LLM chatbot, a deterministic rule-based workflow, and a tool-using AI agent.

The chatbot depends only on the LLM and does not have access to private course data.

The rule-based workflow gives predictable results for the cases that have been explicitly programmed.

The AI agent combines an LLM with external tools. The LLM can decide when to retrieve private data and when to perform calculations.

The main learning is:

```text
LLM
 +
Private Data
 +
Tools
 =
Tool-Using AI Agent
```

---

## 11. Security

The Groq API key is stored in a `.env` file and is excluded from Git using `.gitignore`.

The following files and folders are ignored:

```text
.env
.venv/
__pycache__/
*.pyc
```

The API key should never be committed to GitHub.

---

## 12. Conclusion

The three systems demonstrate different approaches to solving the same problem.

The **chatbot** is suitable for general conversational responses but does not have access to the private course-fee data.

The **rule-based workflow** provides predictable results for predefined cases but cannot handle cases for which rules have not been implemented.

The **AI agent** combines an LLM with tools, allowing it to retrieve private data, perform calculations, and handle questions that require multiple steps.

The project demonstrates the basic architecture and working principle of a **tool-using AI agent**.
