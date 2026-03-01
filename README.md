# 🌟 MoodMate – AI Mental Wellness Check-In & Action Assistant

<div align="center">

![MoodMate Banner](https://github.com/sankhamalapal/MoodMate/blob/main/MoodMate.png)

**An empathetic multi-agent AI system providing structured mental wellness support through personalized check-ins and evidence-based action plans.**

*Capstone Project for the 5-Day AI Agents Intensive Course with Google (Agents for Good track)*

[▶️ Watch Demo Video](https://www.youtube.com/watch?v=0eQ-OA83HFk&list=PL6De2ZbS1kOxY843MrFj2RevGYmrnjXv8&index=2&pp=gAQBiAQB)

</div>

---

## 🚩 The Problem

Mental wellness support exists at two extremes with a critical gap in between:

**Generic Digital Tools** provide one-size-fits-all advice without personalization or severity assessment.

**Professional Services** have barriers like cost, wait times, and the psychological threshold of seeking formal treatment.

**The Gap:** Many people experiencing stress, anxiety, sleep disruption, or emotional fatigue need immediate, accessible, personalized guidance with concrete action steps—but have nowhere to turn.

**MoodMate bridges this gap.**

---

## 💡 The Solution

MoodMate provides structured wellness support through a **four-stage sequential workflow**:

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   LISTEN    │ --> │   ANALYZE   │ --> │    PLAN     │ --> │  EVALUATE   │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
 Empathetic          Compute              Generate            Quality
 Check-In            Wellness Score       Personalized        Assurance
 Interview           & Severity           Action Plan         Review
```

### The Four Stages

1. **🗣️ Listen** – Empathetic interview gathering mood, stress, sleep, energy, stressors, and symptoms
2. **🧠 Analyze** – Compute wellness score `(mood + (10-stress) + sleep)/3` and classify severity (CRITICAL/HIGH/MODERATE/LOW)
3. **📝 Plan** – Generate personalized actions: breathing exercises, vitamins/supplements, sleep strategies, weekly habits — grounded in RAG-retrieved evidence
4. **⚖️ Evaluate** – Validate plan quality across coverage, feasibility, evidence-base, personalization, and balance

---

## 🏗️ Architecture

MoodMate uses a **Sequential Multi-Agent System** built with Google ADK:

### System Flow

```
                         User Input
                             │
                             ▼
                  ┌──────────────────┐
                  │  🛡️ PromptGuard  │ → Sanitises input, blocks injection attacks
                  └──────────────────┘
                             │
                             ▼
                  ┌──────────────────┐
                  │ CheckInAgent     │ → Gathers wellness data
                  │ (No tools)       │
                  └──────────────────┘
                             │
                             ▼
                  ┌──────────────────┐
                  │ AnalysisAgent    │ → Computes wellness score
                  │ Tool: calculate_ │   & severity classification
                  │ wellness_score   │
                  └──────────────────┘
                             │
                             ▼
                  ┌──────────────────────────┐
                  │ ActionPlannerAgent       │ → Creates personalized plan
                  │ Tools:                   │   grounded in retrieved evidence
                  │ • breathing              │
                  │ • vitamins               │
                  │ • sleep_hygiene          │
                  │ • retrieve_wellness_     │ ← RAG: in-memory vector store
                  │   knowledge (RAG)        │
                  │ • web_search_wellness    │ ← Live Google Search + auto-index
                  └──────────────────────────┘
                             │
                             ▼
                  ┌──────────────────┐
                  │ EvaluatorAgent   │ → Validates plan quality
                  │ (No tools)       │   & assigns grade
                  └──────────────────┘
                             │
                             ▼
                      Final Output
```

### Agent Responsibilities

| Agent | Role | Tools | Key Output |
|-------|------|-------|-----------|
| **🛡️ PromptGuard** | Security Layer | — | Sanitised, safe user input |
| **CheckInAgent** | Empathetic Listener | None | Structured wellness data |
| **AnalysisAgent** | Diagnostic Analyst | `calculate_wellness_score` | Wellness score + severity |
| **ActionPlannerAgent** | Wellness Coach | `get_breathing_exercises`<br>`get_vitamin_recommendations`<br>`get_sleep_hygiene_tips`<br>`retrieve_wellness_knowledge`<br>`web_search_wellness` | Evidence-grounded action plan |
| **EvaluatorAgent** | Quality Reviewer | None | Quality score + grade |

### Technology Stack

- **Google ADK** – Multi-agent orchestration framework
- **Gemini 2.5 Flash Lite** – Efficient LLM with tool-calling capabilities
- **Python 3.8+** – Core implementation
- **Jupyter/Colab** – Interactive environment

---

## 🔐 Security – Prompt Injection Protection

All user inputs are passed through `PromptGuard` **before** reaching any agent. The guard:

- Detects and redacts **14 injection patterns**: role-overrides, jailbreaks, delimiter tricks (`### SYSTEM:`), DAN keywords, base64/hex payloads, and system-prompt leakage attempts
- Enforces a **2,000-character input length limit**
- Strips **null bytes and control characters**
- **Logs every flagged event** with a timestamp and input preview for auditability
- Returns a cleaned string to the agent — the original adversarial payload is never forwarded

```python
safe_input, was_flagged, reason = prompt_guard.sanitise(user_input)
# e.g. "Ignore all previous instructions..." → flagged, redacted, logged
```

---

## 📚 RAG – Retrieval-Augmented Generation

`ActionPlannerAgent` grounds its recommendations in retrieved evidence via two tools:

| Tool | Description |
|------|-------------|
| `retrieve_wellness_knowledge(query, top_k)` | TF-IDF cosine search over **14 curated evidence-based passages** (stress, sleep, mood, anxiety, energy, social connection) |
| `web_search_wellness(query)` | Wraps ADK's `google_search`, returns top 5 live results, and **auto-indexes snippets** back into the RAG store for future retrieval |

The in-memory `InMemoryRAGStore` uses a zero-dependency TF-IDF engine — no external ML libraries required. The store grows richer as web results are indexed during a session.

---

## 🚀 Setup & Installation

### Prerequisites

- Python 3.8+
- Google Cloud account
- Gemini API key ([Get one here](https://aistudio.google.com/apikey))

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/sankhamalapal/MoodMate.git
   cd MoodMate
   ```

2. **Install dependencies**
   ```bash
   pip install google-adk google-genai
   ```

3. **Configure your Google API Key**

   #### 1. Create an API key
   If you don't already have one, generate a new API key in [**Google AI Studio**](https://aistudio.google.com/apikey).

   #### 2. Add the API key as a Secret in Google Colab
   - Open **Secrets** from the Colab menu (🔑 key icon in left sidebar)
   - Click **Add new secret**
   - Set the **Name** to: `GOOGLE_API_KEY`
   - Paste your key into the **Value** field and click **Save**
   - Make sure the secret is enabled (**checkbox checked**) so it attaches to your notebook

   #### 3. Important
   ⚠️ **Do not hard-code or paste your API key directly into the notebook**, especially if you plan to share it publicly.

4. **Run the notebook**
   ```bash
   jupyter notebook MoodMate_Agent_Enhanced.ipynb
   ```
   Or upload to [Google Colab](https://colab.research.google.com/)

---

## 💻 Usage

### Interactive Chat Mode

```python
# Natural conversation with MoodMate
await chat_interface()
```

**Commands:**
- Answer wellness questions naturally
- Type `generate plan` when ready for your action plan
- Type `quit` or `exit` to end

### Automated Demo Mode

```python
# Full pipeline with pre-populated data
await main()
```

### Run Evaluation Tests

```python
# Test system with synthetic scenarios
run_evaluation()
```

### Inspect Security Audit Log

```python
# View all flagged injection attempts from the session
prompt_guard.show_audit_log()
```

---

## 📊 Example Output

**Input:** Mood: 4/10, Stress: 8/10, Sleep: 4/10

**Output:**
```
Wellness Score: 3.3/10
Severity: CRITICAL

RAG RETRIEVAL (evidence grounding):
 ✓ "Chronic stress elevates cortisol... Box breathing lowers heart rate within 90s"
 ✓ "Magnesium glycinate 200–400mg increases GABA activity, reduces insomnia"
 ✓ "B-complex vitamins depleted by stress; essential for dopamine synthesis"

IMMEDIATE ACTIONS:
✓ Box Breathing (5 min)
✓ 10-minute outdoor walk

SUPPLEMENTS (HIGH PRIORITY):
• Magnesium 200–400mg before bed
• B-Complex vitamins
• Vitamin D3 1000–2000 IU

SLEEP HYGIENE:
• No screens 1hr before bed
• Cool bedroom 60–67°F
• Consistent bedtime

EVALUATION: 92/100 (Grade A)
```

### Plan Quality Breakdown

| Dimension | Score | Notes |
|-----------|-------|-------|
| Coverage | 92/100 | Addresses all identified concerns including RAG-sourced evidence |
| Feasibility | 95/100 | Most actions < 20 min and readily available |
| Evidence-Based | 93/100 | Recommendations grounded in retrieved research passages |
| Personalization | 90/100 | Directly references user's specific scores and stressors |
| Balance | 90/100 | Blends immediate relief with sustainable long-term habits |
| **Overall** | **92/100** | **Grade A** |

---

## 🔭 Future Enhancements

- Iterative plan refinement based on evaluator feedback
- Longitudinal progress tracking across sessions
- Wearable data integration (sleep, HRV)
- Crisis detection and automatic escalation
- Persistent RAG store across sessions (database-backed)

---

## 🎯 Conclusion

**MoodMate directly addresses the gap between generic digital tools and professional services** by providing what was missing: immediate, personalized wellness support with concrete action steps—no cost barriers, no wait times, no psychological threshold of seeking formal treatment.

The four-stage sequential workflow transforms vague wellness concerns into **quantified severity assessments** and **actionable, evidence-based intervention plans** grounded in RAG-retrieved research. Prompt injection protection ensures user safety throughout, while live Google Search keeps recommendations current.

With consistent evaluation scores averaging **92/100 (Grade A)**, MoodMate demonstrates reliable, high-quality output that validates this approach. This is the bridge that makes quality mental wellness support accessible to anyone, anywhere, immediately.

---

## ⚠️ Disclaimer

**MoodMate is an AI wellness assistant for self-tracking and is NOT a substitute for professional medical or psychiatric advice.**

Always consult healthcare providers before starting new supplements or treatments.

---

## 🙏 Acknowledgments

This project was developed as part of the **5-Day AI Agents Intensive Course with Google (Agents for Good track)**, a collaborative educational initiative by **Google** and **Kaggle**.

Special thanks to:
- **Google** for developing the Agent Development Kit (ADK) and Gemini models that power this system
- **Kaggle** for hosting the AI Agents Intensive course and providing learning resources
- The **Google ADK team** for their comprehensive documentation and support
- All instructors and mentors from the 5-Day AI Agents Intensive program
