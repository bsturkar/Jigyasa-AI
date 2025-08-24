Emergency Dispatcher Training Simulation project. It explains the entire pipeline, from how each component works to how you can get started and extend the system. You can copy and paste this into a file named `README.md` or adapt it to your preferred format.

---

# Emergency Dispatcher Training Simulation

This repository provides a **conversational AI simulation** where a **distressed caller** (played by an AI) interacts with a **dispatcher** (human user) to practice emergency call handling. The system also provides **feedback** on the dispatcher’s performance based on real-world 911 dispatcher protocols and guidelines.

---

## Table of Contents
1. [Overview](#overview)  
2. [Project Architecture](#project-architecture)  
3. [Agents (Core Components)](#agents-core-components)  
   - [TrainerAgent](#1-traineragent)  
   - [HumanBotAgent](#2-humanbotagent)  
   - [FeedbackReportGenerator](#3-feedbackreportgenerator)  
4. [The `app.py` Script](#the-apppy-script)  
5. [Installation & Setup](#installation--setup)    
6. [Workflow Summary](#workflow-summary)  
7. [Advanced Features & Future Enhancements](#advanced-features--future-enhancements)  
8. [License](#license) (optional)  
9. [Contact / Contributing](#contact--contributing) (optional)

---

## Overview
**Emergency Dispatchers** often encounter high-stress calls requiring adherence to strict protocols (e.g., verifying location, assessing the victim’s state). This project **simulates** these conversations and automatically **generates feedback** to improve dispatcher readiness.

### Key Functionalities
- **Generate Realistic Emergency Scenarios**: Tailored to address common dispatcher mistakes.  
- **Simulate Distressed Callers**: An AI that behaves as a panicked caller, forcing dispatchers to ask the right questions under pressure.  
- **Provide Actionable Feedback**: Highlights positive aspects, missed steps, and suggestions for better call handling.

---

## Project Architecture
Below is a high-level architecture of how everything fits together:

```
+----------------------------+
|      TrainerAgent          |
| (Generates Scenario)       |
+-------------+--------------+
              |
     (Scenario Prompt)
              v
+----------------------------+             +------------------------+
|     HumanBotAgent          |  <----->    |   Dispatcher (User)    |
|  (Distressed Caller AI)    |             |    Real-time Input     |
+-------------+--------------+             +-----------+------------+
              |                                     |
     (Conversation Logs)                             |
              v                                     |
+----------------------------+                       |
|  FeedbackReportGenerator   |       <---------------+
| (Analyzes Conversation)    |  
+-------------+--------------+
              |
     (Feedback Report)
              v
   +----------------------------+
   |      Output / Console      |
   | (e.g., final report)       |
   +----------------------------+
```

### Core Flow
1. **TrainerAgent** generates a scenario prompt (e.g., “Late-night call about a heart attack with background noise”).  
2. **HumanBotAgent** takes on the role of the distressed caller using that scenario.  
3. **Dispatcher** (the human user) tries to handle the call.  
4. **Conversation logs** are collected.  
5. **FeedbackReportGenerator** reviews the logs and provides a structured, actionable report.

---

## Agents (Core Components)

### 1. **TrainerAgent**
- **Purpose**: Generates **new emergency call scenarios** based on past feedback reports.  
- **Usage**:
  - Feeds on previous sessions’ issues (e.g., “slow response time”, “missed protocol steps”).  
  - Creates a refined scenario prompt targeting those weaknesses.  
- **Key Method**: `process_request(feedback_reports)`

### 2. **HumanBotAgent**
- **Purpose**: Simulates the **distressed 911 caller**.  
- **Usage**:
  - Takes the scenario prompt from `TrainerAgent`.  
  - Responds dynamically to dispatcher questions (user input) as if it’s a real caller in distress (crying, forgetting info, etc.).  
- **Key Methods**:
  - `initialize_conversation(prompt)`: Kickstarts the simulation with an opening scenario.  
  - `get_bot_response(user_input)`: Generates new responses from the “distressed caller” based on user input.  
  - `get_conversation_logs()`: Retrieves the entire conversation for feedback analysis.

### 3. **FeedbackReportGenerator**
- **Purpose**: **Analyzes** the final conversation logs and produces a **feedback report**.  
- **Usage**:
  - Takes conversation logs (list of role-content messages).  
  - Uses an LLM prompt to identify missed protocol steps, positive aspects, and improvements.  
- **Key Method**: `generate_feedback(conversation_logs)`

---

## Installation & Setup

1. **Clone the Repository**  
   ```bash
   git clone https://github.com/YourUsername/EmergencyDispatcherTraining.git
   cd EmergencyDispatcherTraining
   ```

2. **Install Dependencies**  
   - Create and activate a virtual environment (recommended):
     ```bash
     python3 -m venv venv
     source venv/bin/activate   # Mac/Linux
     # For Windows: venv\Scripts\activate
     ```
   - Install required packages:
     ```bash
     pip install -r requirements.txt
     ```
   > **Note**: You may need API keys if you’re using an external LLM service (e.g., OpenAI).  

3. **Project Structure**:
   ```
   EmergencyDispatcherTraining/
   ├── agents/
   │   ├── __init__.py
   │   ├── trainer_agent.py
   │   ├── human_bot_agent.py
   │   └── feedback_report_agent.py
   ├── rouetrs/
   │   ├── __init__.py
   │   ├── auth.py
   │   ├── setup.py
   │   └── users.py
   ├── services/
   │   ├── sql_connection.py
   ├── utility/
   │   ├── auth_bearer.py
   │   ├── auth_helper.py
   │   └── data_store.py
   ├── app.py
   ├── main_audio.py
   ├── main.py
   ├── model.py
   ├── requirements.txt
   ├── README.md
   └── ...
   ```

---

## Workflow Summary

1. **Feedback Reports** (Past Sessions)  
   - “Session #1: Missed verifying location, slow instructions.”  
2. **TrainerAgent**  
   - Incorporates that data into a new scenario prompt (e.g., “Emphasize location confirmation quickly”).  
3. **HumanBotAgent**  
   - Responds as a frantic caller with partial info, background noise, etc.  
4. **Live Interaction**  
   - User asks questions → AI replies → logs accumulate.  
5. **FeedbackReportGenerator**  
   - Takes the entire conversation → Analyzes with LLM → Identifies missed steps or best practices → Returns a structured feedback summary.

---

## Advanced Features & Future Enhancements

1. **Multi-Lingual Support**  
   - Real emergencies happen in various languages. Integrate translation for bilingual or multilingual calls.
2. **Gamification & Scoring**  
   - Assign a numerical score for each call, highlight repeated patterns, or track progress over multiple sessions.
3. **Protocol Library Integration**  
   - For advanced setups, integrate standard emergency protocols (like MPDS) and check specifically if each step is followed.
4. **Dashboard & Analytics**  
   - Build a web interface to track each dispatcher’s improvement over time, generate performance graphs, etc.
5. **Role-Based Access**  
   - Differentiate between Trainee Dispatchers, Trainers, and Admins. Provide each role with relevant data and permissions.

---

## License
*(Optional)*  
You may want to add a license (e.g., MIT, Apache 2.0) to clarify usage rights.

---

## Contact / Contributing
*(Optional)*  
- **Contact**: If you have questions or suggestions, reach out to:
   - [Bhaskar Turkar](https://www.linkedin.com/in/bhaskar-turkar-472291141/)
   - [Sanket Bodake](https://www.linkedin.com/in/sanket-b-3ab030245/)
   - [Vivek Kumar](https://www.linkedin.com/in/vivek-kumar-ds/)
- **Contributions**: PRs are welcome! Fork the repo, add features, and open a pull request.

---

### End of README

By following this documentation, you’ll understand each **component** of the pipeline, how to **run** it, and how to **extend** it for more realistic, robust dispatcher training. Good luck with your **Emergency Dispatcher Training Simulation**!

---

## How GPT-5 Capabilities Are Utilized in This Project

- **Advanced Reasoning & Multi-Step Problem Solving:**  
  GPT-5 powers the simulation’s core agents, enabling them to handle complex, multi-turn emergency scenarios. The AI caller (HumanBotAgent) can maintain context, adapt its responses based on dispatcher input, and simulate realistic, evolving emergencies that require nuanced, step-by-step handling.

- **Nuanced Understanding & Context Awareness:**  
  The system uses GPT-5’s deep contextual understanding to interpret dispatcher questions, simulate distressed caller behavior authentically, and ensure that the conversation feels natural and challenging. This helps trainees practice under realistic, high-pressure conditions.

- **Seamless Human-AI Collaboration:**  
  The dispatcher (user) interacts directly with the AI-powered caller, creating a dynamic partnership where the AI adapts in real time to the user’s approach, mistakes, and questions—mirroring real-world unpredictability.

- **High-Quality, Adaptive Output Generation:**  
  GPT-5 generates detailed feedback reports (via FeedbackReportGenerator), identifying missed protocol steps, highlighting strengths, and suggesting actionable improvements. This feedback is tailored to each session, leveraging the model’s ability to analyze and summarize complex interactions.

- **Creativity & Scenario Generation:**  
  The TrainerAgent uses GPT-5 to craft diverse, challenging emergency scenarios, incorporating past performance data to target specific weaknesses. This ensures continuous learning and exposure to a wide range of situations.

- **Scalability & Versatility:**  
  The architecture allows for easy extension—new protocols, languages, or emergency types can be added, leveraging GPT-5’s broad knowledge base and adaptability.

**In summary:**  
Your project harnesses GPT-5’s advanced reasoning, contextual understanding, and adaptive generation to create a realistic, interactive training environment for emergency dispatchers—delivering both challenging simulations and

**Project Snapshots**

![Login page](./jigyasa-AI/login.png)
![Welcome Page](./jigyasa-AI/welcome_page.png)
![Simulation Types](./jigyasa-AI/Simulation_types.png)



