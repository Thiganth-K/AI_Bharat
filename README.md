# SkillBridge – The Informal-to-Formal AI Career Gateway

SkillBridge is a voice-first AI platform designed to unlock the economic potential of the informal workforce (street vendors, gig workers, homemakers) by transforming lived experience into structured, professional skill profiles.

Instead of requiring a written resume, SkillBridge interviews users in their native language, extracts implicit skills using AI, and generates a professional “Shadow Resume” matched to job opportunities.

---

## 🚀 Problem

Traditional job platforms such as LinkedIn, Indeed, and Naukri require:
- English proficiency
- Written CV formatting
- Formal education records

This excludes millions of skilled informal workers who lack documentation but possess real-world competencies.

SkillBridge solves the **Language of Employability Gap**.

---

## 💡 Solution

SkillBridge uses:

1. 🎤 Voice-first onboarding  
2. 🧠 AI-based contextual skill mining  
3. 📄 Automatic resume generation  
4. 🤝 Intelligent job matching  

The system translates informal narratives into formal competencies and maps them to structured job roles.

---

## 🏗 System Architecture

Client → FastAPI Backend → AI Processing Layer → Database → Matching Engine

### AI Processing Stack

- Speech-to-Text: OpenAI Whisper
- Translation Layer
- LLM Skill Extraction (Gemini / GPT-4o via LangChain)
- Resume Generator (ReportLab)

---

## 🛠 Tech Stack

Backend:
- Python 3.10+
- FastAPI
- LangChain
- Supabase (PostgreSQL)

Frontend:
- Streamlit (Hackathon version)
- Flutter (Scalable mobile version)

AI Models:
- Google Gemini 1.5 Flash OR OpenAI GPT-4o
- OpenAI Whisper

Deployment:
- Render / Vercel / Streamlit Cloud
- Supabase Free Tier

---

## 📦 Repository Structure

skillbridge/
│
├── backend/
│ ├── main.py
│ ├── services/
│ ├── models/
│ └── utils/
│
├── requirements.md
├── design.md
└── README.md


---

## ⚙️ Installation

```bash
git clone https://github.com/your-username/skillbridge.git
cd skillbridge
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
