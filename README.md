# SkillBridge – Empowering Bharat's Informal Workforce

> **AI for Bharat Hackathon 2026**  
> *Bridging the Language of Employability Gap*

SkillBridge is a voice-first AI platform designed to unlock the economic potential of India's 450+ million informal workers—street vendors, gig workers, homemakers, and daily wage earners—by transforming lived experience into structured, professional skill profiles.

Instead of requiring written resumes or English proficiency, SkillBridge interviews users in their native language (Tamil, Hindi, and more), extracts implicit skills using AI, and generates a professional "Shadow Resume" matched to relevant job opportunities.

---

## 🚀 The Problem: India's Hidden Workforce

**90% of India's workforce is in the informal sector**, yet they remain invisible to formal employment opportunities.

Traditional job platforms require:
- English proficiency (only 10% of Indians speak English)
- Written CV formatting (literacy barriers)
- Formal education records (most lack documentation)
- Digital literacy (limited smartphone skills)

**The Impact:**
- 450M+ skilled workers excluded from formal job market
- ₹10 lakh crore in lost economic potential annually
- Perpetuation of poverty cycles despite possessing valuable skills
- No pathway to upskilling or government schemes

SkillBridge addresses this **Language of Employability Gap** by making employment accessible to Bharat, not just India.

---

## 💡 Our Solution: AI-Powered Dignity of Work

SkillBridge leverages cutting-edge AI to democratize employment access:

1. **🎤 Voice-First in Native Language** – Speak in Tamil, Hindi, or any Indian language
2. **🧠 AI Skill Recognition** – Extract hidden competencies from daily work narratives
3. **📄 Instant Professional Resume** – Generate industry-standard resumes in seconds
4. **🤝 Smart Job Matching** – Connect to relevant opportunities with NSQF alignment
5. **🔊 Audio Job Descriptions** – Hear opportunities in your own language

**Real-World Example:**
> *"मैं सुबह 4 बजे उठकर सब्जी मंडी जाती हूं..."*  
> (I wake up at 4 AM to go to the vegetable market...)

**SkillBridge Extracts:**
- Inventory Management
- Customer Service
- Financial Planning
- Early Operations Management
- Negotiation Skills

**Matched Jobs:** Retail Associate, Warehouse Supervisor, Supply Chain Assistant

---

## 🏗 System Architecture

```
Client Layer → API Gateway → AI Processing Layer → Data Layer → Matching Engine
```

### AI Processing Stack

- **Speech-to-Text**: OpenAI Whisper
- **Translation Layer**: Google Translate API
- **LLM Skill Extraction**: Google Gemini 1.5 Flash / OpenAI GPT-4o via LangChain
- **Resume Generator**: ReportLab (PDF generation)

---

## 🛠 Tech Stack

### Backend
- Python 3.10+
- FastAPI
- LangChain
- Supabase (PostgreSQL)
- ReportLab (PDF generation)

### Frontend
- Streamlit (Hackathon/MVP version)
- Flutter (Scalable mobile version)

### AI Models
- **LLM**: Google Gemini 1.5 Flash or OpenAI GPT-4o
- **Speech-to-Text**: OpenAI Whisper
- **Translation**: Google Translate API

### Deployment
- Backend: Render / Vercel
- Frontend: Streamlit Cloud
- Database: Supabase Free Tier

---

## 🎯 Key Features

### For Workers (Bharat)
- **भारतीय भाषाओं में** (Indian Languages): Tamil, Hindi, Telugu, Bengali, Marathi support
- **बोलो, लिखो मत** (Speak, Don't Write): Zero typing required
- **2 मिनट में Resume**: Complete profile in under 2 minutes
- **Free Forever**: No hidden costs, accessible to all

### For Employers (India Inc.)
- **Untapped Talent Pool**: Access 450M+ skilled workers
- **NSQF-Aligned Skills**: Standardized skill classifications
- **Verified Profiles**: AI-validated skill assessments
- **Inclusive Hiring**: Meet CSR and diversity goals

### Technology Excellence
- **State-of-the-art AI**: OpenAI Whisper + Google Gemini/GPT-4o
- **Privacy-First**: End-to-end encryption, GDPR compliant
- **Offline-Ready**: Works on 2G networks
- **Scalable Architecture**: Built for millions of users

---

## 📦 Repository Structure

```
skillbridge/
│
├── backend/
│   ├── main.py              # FastAPI application entry point
│   ├── services/            # Business logic and AI services
│   ├── models/              # Database models and schemas
│   └── utils/               # Helper functions and utilities
│
├── frontend/
│   └── app.py               # Streamlit application
│
├── requirements.txt         # Python dependencies
├── requirements.md          # Functional and technical requirements
├── design.md                # System design documentation
├── IMPACT.md                # Social impact analysis and vision
├── README.md                # Project overview (this file)
└── .env                     # Environment variables (not in repo)
```

---

## ⚙️ Installation

### Prerequisites
- Python 3.10 or higher
- pip package manager
- Git

### Setup Instructions

```bash
# Clone the repository
git clone https://github.com/your-username/skillbridge.git
cd skillbridge

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure environment variables
cp .env.example .env
# Edit .env with your API keys

# Run the application
python backend/main.py
```

---

## 🔑 Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
# AI Model API Keys
OPENAI_API_KEY=your_openai_api_key
GOOGLE_API_KEY=your_gemini_api_key

# Database
SUPABASE_URL=your_supabase_url
SUPABASE_KEY=your_supabase_key

# Optional
PINECONE_API_KEY=your_pinecone_key
```

---

## 🚦 Usage

### Running the Backend

```bash
cd backend
uvicorn main:app --reload
```

The API will be available at `http://localhost:8000`

### Running the Frontend

```bash
streamlit run frontend/app.py
```

The web interface will open at `http://localhost:8501`

---

## 📡 API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/transcribe` | POST | Convert audio to text |
| `/extract-skills` | POST | Extract skills from transcript |
| `/generate-resume` | POST | Generate PDF resume |
| `/match-jobs` | POST | Find matching job opportunities |

---

## 🌟 Social Impact

SkillBridge targets India's 450+ million informal workers, unlocking ₹10 lakh crore in economic potential. See [IMPACT.md](IMPACT.md) for detailed social impact analysis, user personas, and scalability vision.

**Key Impact Areas:**
- Financial inclusion for marginalized communities
- Women's economic empowerment
- Rural employment generation
- Alignment with Skill India and Digital India missions

---

## 🛣 Roadmap

### Phase 1 (Current - Hackathon MVP)
- ✅ Voice input and transcription
- ✅ Basic skill extraction
- ✅ Resume generation
- ✅ Simple job matching

### Phase 2 (Post-Hackathon)
- 🔄 Flutter mobile application
- 🔄 Vector-based semantic matching
- 🔄 NSQF skill mapping integration
- 🔄 Employer dashboard

### Phase 3 (Future)
- 📋 Government scheme integration
- 📋 Micro-loan eligibility engine
- 📋 Employer verification badge system
- 📋 Offline-first Android app

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 👥 Team

SkillBridge is developed for **AI for Bharat Hackathon 2026** with a mission to democratize employment access for India's informal workforce.

**Our Commitment:** Technology that serves Bharat, not just India.

---

## 📞 Contact

For questions or feedback, please reach out to:
- Email: [your-email@example.com]
- GitHub Issues: [https://github.com/your-username/skillbridge/issues]

---

## 🙏 Acknowledgments

- **AI for Bharat** for organizing this impactful hackathon
- **OpenAI** for Whisper and GPT models enabling voice-first AI
- **Google** for Gemini AI and translation services
- **Supabase** for database infrastructure
- **India's informal workforce** for inspiring this solution and teaching us the true meaning of skill and resilience

---

## 🏆 Why SkillBridge Deserves to Win

1. **Massive Impact Potential**: Addresses 450M+ workers, not just a niche problem
2. **India-First Solution**: Built for Indian languages, Indian context, Indian challenges
3. **Technically Sound**: Production-ready architecture with clear scalability path
4. **Socially Responsible**: Aligns with national priorities (Skill India, Digital India)
5. **Sustainable Business Model**: Clear path to revenue without exploiting users
6. **Immediate Deployment**: Can launch within weeks post-hackathon

**This is not just a hackathon project—it's a movement to democratize employment in India.**

---

*"हर हुनर को पहचान मिले, हर कामगार को मौका मिले"*  
*(Every skill deserves recognition, every worker deserves opportunity)*
