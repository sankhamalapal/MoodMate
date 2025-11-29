
# 🌟 MoodMate – AI Mental Wellness Check-In & Action Assistant

**MoodMate** is a multi-agent AI system designed to provide empathetic mental wellness check-ins and generate personalized, evidence-based action plans. Built using the **Google Agent Development Kit (ADK)** and **Gemini 2.5**, it moves beyond simple conversation to provide structured wellness analysis and concrete steps for improvement.

This project was developed as a Capstone for the **5-Day AI Agents Intensive Course with Google (Agents for Good track)**.

---

## 🚩 The Problem
Mental wellness support often faces two extremes: generic, impersonal advice found in search engines, or the high barrier of entry for professional therapy. Individuals feeling overwhelmed, stressed, or having trouble sleeping often need immediate, accessible, and structured guidance to help them navigate their current state before issues escalate.

## 💡 The Solution
MoodMate acts as a compassionate wellness partner using a **Sequential Multi-Agent System**. It replicates a triage and support process by:
1.  **Listening:** Conducting an empathetic intake interview.
2.  **Analyzing:** Quantifying wellness metrics (Mood, Stress, Sleep) to determine severity.
3.  **Planning:** Generating a specific, feasible action plan using custom tools (Breathing exercises, Supplements, Sleep hygiene).
4.  **Evaluating:** Self-correcting and grading the plan to ensure it is safe, feasible, and personalized.

---

## 🏗️ Architecture

MoodMate utilizes the `SequentialAgent` pattern from the Google ADK to chain four specialized agents together. Each agent passes its context to the next, ensuring a cohesive workflow.

### The Agent Chain:
1.  **🗣️ CheckInAgent:**
    * **Role:** Empathetic listener.
    * **Task:** Greets the user, gathers data on mood/stress/sleep, and summarizes input.
2.  **🧠 AnalysisAgent:**
    * **Role:** Diagnostician.
    * **Task:** Calculates a "Wellness Score" and severity level; identifies key themes.
    * **Tool:** `calculate_wellness_score`.
3.  **📝 ActionPlannerAgent:**
    * **Role:** Wellness Coach.
    * **Task:** Creates a prioritized action plan (Immediate, Nutrition, Sleep, Habits).
    * **Tools:** `get_breathing_exercises`, `get_vitamin_recommendations`, `get_sleep_hygiene_tips`.
4.  **⚖️ EvaluatorAgent:**
    * **Role:** Quality Assurance.
    * **Task:** Reviews the generated plan for coverage and feasibility; assigns a grade (A-F).

---

## 🛠️ Tech Stack

* **Framework:** [Google Agent Development Kit (ADK)](https://pypi.org/project/google-adk/)
* **Model:** Gemini 2.5 Flash Lite (`gemini-2.5-flash-lite`)
* **Language:** Python 3.x
* **Environment:** Jupyter Notebook / Google Colab

---

## 🚀 Setup & Installation

### Prerequisites
* A Google Cloud Project with the Gemini API enabled.
* A Google AI Studio API Key.

### Steps

1.  **Install Dependencies:**
    ```bash
    pip install google-adk google-genai
    ```

2.  **Configure API Key:**
    Set your Google API key in your environment variables.
    ```python
    import os
    os.environ["GOOGLE_API_KEY"] = "YOUR_API_KEY_HERE"
    ```

3.  **Run the System:**
    Open the notebook `MoodMate_Agent.ipynb` and run all cells.

---

## 💻 Usage

To start an interactive session, run the `main()` function within the notebook:

```python
await main()
```

## 🧪 Evaluation

The project includes a test harness to validate agent logic against synthetic scenarios (e.g., "High Stress," "Mild Wellness Dip").

```
# Run evaluation test cases
run_evaluation()
```

##### Disclaimer : MoodMate is an AI assistant for wellness tracking and is not a substitute for professional medical or psychiatric advice. If you are in crisis, please contact emergency services or a dedicated crisis hotline.


