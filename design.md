# SkillBridge – System Design Document

---

# 1. System Overview

SkillBridge is a voice-first AI system that converts informal spoken narratives into structured professional skill profiles and matches users to jobs.

Architecture follows a layered microservice design.

---

# 2. High-Level Architecture

Client Layer → API Gateway → Processing Layer → Data Layer → Matching Engine

---

# 3. Architecture Components

## 3.1 Client Layer

- Streamlit Web App (Hackathon)
- Flutter Mobile App (Scalable version)
- Captures audio input
- Displays resume & job matches

---

## 3.2 API Gateway

- FastAPI backend
- Routes requests:
  - /transcribe
  - /extract-skills
  - /generate-resume
  - /match-jobs

---

## 3.3 Processing Layer

### Service A: Speech-to-Text
- Model: OpenAI Whisper
- Converts audio to raw text transcript

### Service B: Translation
- Google Translate API
- Converts local dialect → English

### Service C: Skill Extraction Engine
- LLM (Gemini 1.5 Flash / GPT-4o)
- Uses structured prompt to output JSON:

Example Output:
{
  "skills": [
    "Inventory Management",
    "Customer Service",
    "Sales",
    "Financial Planning"
  ]
}

---

## 3.4 Matching Engine

- Rule-based + similarity matching
- Skill-to-job mapping CSV
- Optional vector similarity search

Logic Example:
IF user.skills contains ["Driving", "Time Management"]
→ Match "Delivery Partner"

---

## 3.5 Data Layer

### PostgreSQL (Supabase)
Stores:
- User Profile
- Extracted Skills
- Resume Metadata
- Job Listings

### Optional Vector DB (Pinecone)
Stores:
- Skill embeddings
- Semantic job matching

---

# 4. Data Flow

1. User speaks into microphone.
2. Audio sent to backend.
3. Whisper converts audio → text.
4. Translation layer converts → English.
5. LLM extracts structured skills.
6. Skills stored in PostgreSQL.
7. Matching engine retrieves suitable jobs.
8. Resume generated (PDF via ReportLab).
9. Results displayed to user.

---

# 5. UI Design

## Screen 1 – Interview Screen

- Large central microphone button
- Minimal text
- Prompt: “Tell me what work you did yesterday”

## Screen 2 – Profile Screen

- User photo + name
- Skill badges
- Verified checkmark
- Matched jobs list
- Download Resume button

---

# 6. Database Schema (Simplified)

Table: users
- id (UUID)
- name
- language
- transcript
- created_at

Table: skills
- id
- user_id (FK)
- skill_name

Table: jobs
- id
- title
- required_skills (array)

---

# 7. Resume Generation

- Library: ReportLab
- Structured PDF layout:
  - Name
  - Summary
  - Skills
  - Recommended Roles

---

# 8. Security Considerations

- API Keys stored in .env
- Input validation
- Rate limiting
- Encrypted database connection

---

# 9. Scalability Plan

Future Enhancements:
- Replace rule-based matching with vector similarity.
- Deploy on AWS/GCP.
- Add NSQF skill mapping registry.
- Add employer dashboard.

---

# 10. Risk Analysis

Risk: Accent recognition failure  
Mitigation: Fine-tune Whisper model

Risk: Incorrect skill inference  
Mitigation: Add human verification layer

Risk: API cost overrun  
Mitigation: Use Gemini free tier

---

# 11. Future Roadmap

- Government Scheme Integration
- Micro-loan eligibility engine
- Employer verification badge system
- Offline-first Android app
