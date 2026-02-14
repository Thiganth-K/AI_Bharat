# SkillBridge – The Informal-to-Formal AI Career Gateway

## 1. Problem Statement

Millions of informal workers (street vendors, gig workers, homemakers) possess valuable skills but lack formal documentation, certifications, or resumes. Traditional platforms like LinkedIn, Indeed, and Naukri require written resumes, English proficiency, and structured employment history.

This creates a "Language of Employability Gap."

SkillBridge bridges this gap using a voice-first AI interface that converts lived experience into formal skill representations mapped to industry standards.

---

## 2. Functional Requirements

### 2.1 User Requirements

- FR1: User should be able to provide input through voice in native language.
- FR2: System should transcribe audio into text.
- FR3: System should translate vernacular language into English.
- FR4: System should extract structured skills from narrative text.
- FR5: System should generate a professional “Shadow Resume”.
- FR6: System should match user skills with suitable jobs.
- FR7: System should provide audio job feed in native language.
- FR8: System should store user profile in database.
- FR9: System should generate downloadable PDF resume.

---

### 2.2 Admin / System Requirements

- FR10: Admin should be able to manage job listings.
- FR11: System should maintain skill-to-job mapping logic.
- FR12: System should log AI outputs for validation and debugging.

---

## 3. Non-Functional Requirements

### Performance
- Transcription latency < 5 seconds.
- Skill extraction response < 4 seconds.

### Scalability
- Support minimum 500 concurrent users (hackathon scale).

### Security
- HTTPS API communication.
- Secure API key storage (environment variables).
- Role-based database access.

### Accessibility
- Voice-first interface.
- Multilingual support (Tamil, Hindi initially).

---

## 4. Technical Requirements

### Backend
- Python 3.10+
- FastAPI
- LangChain
- Supabase (PostgreSQL)

### AI Models
- LLM: Google Gemini 1.5 Flash or OpenAI GPT-4o
- Speech-to-Text: OpenAI Whisper

### Frontend
- Streamlit (Hackathon version)
  OR
- Flutter (Mobile scalable version)

### Storage
- PostgreSQL (User profiles)
- Vector DB (Optional: Pinecone)

---

## 5. Constraints

- Must operate within free-tier API limits.
- Must work with low-bandwidth environments.
- Must support low-literacy users.

---

## 6. Assumptions

- User has access to a smartphone.
- User can speak clearly in local dialect.
- Internet connection is available.

---

## 7. Success Criteria

- User can generate a resume within 2 minutes.
- System extracts minimum 5 meaningful skills.
- At least 3 relevant job matches returned.
