# 🌟 MoodMate – AI Mental Wellness Check-In & Action Assistant

<div align="center">

![MoodMate Banner](https://github.com/sankhamalapal/MoodMate/blob/main/MoodMate.png)

**An empathetic multi-agent AI system providing structured mental wellness support through personalized check-ins and evidence-based action plans.**

*Capstone Project for the 5-Day AI Agents Intensive Course with Google (Agents for Good track)*

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
3. **📝 Plan** – Generate personalized actions: breathing exercises, vitamins/supplements, sleep strategies, weekly habits
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
                  ┌──────────────────┐
                  │ActionPlannerAgent│ → Creates personalized plan
                  │ Tools:           │
                  │ • breathing      │
                  │ • vitamins       │
                  │ • sleep_hygiene  │
                  └──────────────────┘
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
| **CheckInAgent** | Empathetic Listener | None | Structured wellness data |
| **AnalysisAgent** | Diagnostic Analyst | `calculate_wellness_score` | Wellness score + severity |
| **ActionPlannerAgent** | Wellness Coach | `get_breathing_exercises`<br>`get_vitamin_recommendations`<br>`get_sleep_hygiene_tips` | Personalized action plan |
| **EvaluatorAgent** | Quality Reviewer | None | Quality score + grade |

### Technology Stack

- **Google ADK** – Multi-agent orchestration framework
- **Gemini 2.5 Flash Lite** – Efficient LLM with tool-calling capabilities
- **Python 3.8+** – Core implementation
- **Jupyter/Colab** – Interactive environment

---

## 🚀 Setup & Installation

### Prerequisites

- Python 3.8+
- Google Cloud account
- Gemini API key ([Get one here](https://aistudio.google.com/apikey))

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/moodmate.git
   cd moodmate
   ```

2. **Install dependencies**
   ```bash
   pip install google-adk google-genai
   ```

3. **Configure your Google API Key**

   To use this project, you need to configure your **Google API key**.

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
   jupyter notebook MoodMate_Agent.ipynb
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

---

## 📊 Example Output

**Input:** Mood: 4/10, Stress: 8/10, Sleep: 4/10

**Output:**
```
Wellness Score: 3.3/10
Severity: CRITICAL

IMMEDIATE ACTIONS:
✓ Box Breathing (5 min)
✓ 10-minute outdoor walk

SUPPLEMENTS (HIGH PRIORITY):
• Magnesium 200-400mg before bed
• B-Complex vitamins
• Vitamin D3 1000-2000 IU

SLEEP HYGIENE:
• No screens 1hr before bed
• Cool bedroom 60-67°F
• Consistent bedtime

EVALUATION: 90/100 (Grade A)
```

---

## 🔭 Future Enhancements

- Iterative plan refinement based on evaluator feedback
- Longitudinal progress tracking across sessions
- Wearable data integration (sleep, HRV)
- Crisis detection and automatic escalation

---

## ⚠️ Disclaimer

**MoodMate is an AI wellness assistant for self-tracking and is NOT a substitute for professional medical or psychiatric advice.** If you are in crisis, please contact:

- **988 Suicide & Crisis Lifeline:** Call/text 988 or visit [988lifeline.org](https://988lifeline.org/)
- **Emergency Services:** Call 911 (US) or your local emergency number
- **Crisis Text Line:** Text HOME to 741741

Always consult healthcare providers before starting new supplements or treatments.

---

## 🙏 Acknowledgments

This project was developed as part of the **5-Day AI Agents Intensive Course with Google (Agents for Good track)**, a collaborative educational initiative by **Google** and **Kaggle**.

Special thanks to:
- **Google** for developing the Agent Development Kit (ADK) and Gemini models that power this system
- **Kaggle** for hosting the AI Agents Intensive course and providing learning resources
- The **Google ADK team** for their comprehensive documentation and support
- All instructors and mentors from the 5-Day AI Agents Intensive program

**Powered by:**
- [Google Agent Development Kit (ADK)](https://github.com/google/genai-agent-framework)
- [Gemini 2.5 Flash Lite](https://ai.google.dev/)
