# SkillBridge – System Design Document

## Document Information

| Field | Value |
|-------|-------|
| Version | 1.0 |
| Last Updated | February 2026 |
| Status | Draft |
| Author | SkillBridge Team |

---

## 1. System Overview

### 1.1 Purpose

SkillBridge is a voice-first AI system that converts informal spoken narratives into structured professional skill profiles and matches users to relevant job opportunities. The platform addresses the employability gap for informal workers by removing language and literacy barriers.

### 1.2 Architectural Approach

The system follows a layered microservice architecture with clear separation of concerns:

- **Client Layer**: User interface and interaction
- **API Gateway**: Request routing and authentication
- **Processing Layer**: AI-powered business logic
- **Data Layer**: Persistent storage and retrieval
- **Matching Engine**: Job recommendation system

---

## 2. High-Level Architecture

```
┌─────────────────┐
│  Client Layer   │  (Streamlit/Flutter)
└────────┬────────┘
         │
┌────────▼────────┐
│  API Gateway    │  (FastAPI)
└────────┬────────┘
         │
┌────────▼────────────────────────────┐
│      Processing Layer               │
│  ┌──────────┐  ┌──────────────┐   │
│  │ Speech   │  │ Translation  │   │
│  │ to Text  │  │   Service    │   │
│  └──────────┘  └──────────────┘   │
│  ┌──────────────────────────────┐ │
│  │  Skill Extraction Engine     │ │
│  └──────────────────────────────┘ │
└────────┬────────────────────────────┘
         │
┌────────▼────────┐  ┌──────────────┐
│   Data Layer    │  │   Matching   │
│  (PostgreSQL)   │  │    Engine    │
└─────────────────┘  └──────────────┘
```

---

## 3. Architecture Components

### 3.1 Client Layer

The client layer provides the user interface for interacting with the system.

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Web Application | Streamlit | Hackathon MVP and rapid prototyping |
| Mobile Application | Flutter | Production-ready scalable mobile app |

**Responsibilities:**
- Capture audio input from user
- Display extracted skills and generated resume
- Show matched job opportunities
- Provide intuitive voice-first interface

---

### 3.2 API Gateway

The API gateway serves as the central entry point for all client requests.

**Technology**: FastAPI

**Key Endpoints:**

| Endpoint | Method | Description | Request | Response |
|----------|--------|-------------|---------|----------|
| `/transcribe` | POST | Convert audio to text | Audio file | Transcript text |
| `/extract-skills` | POST | Extract skills from transcript | Transcript | Structured skills JSON |
| `/generate-resume` | POST | Generate PDF resume | User profile + skills | PDF file |
| `/match-jobs` | POST | Find matching jobs | Skills array | Job listings |

**Features:**
- Request validation and sanitization
- Rate limiting (100 requests/minute per user)
- Authentication and authorization
- Error handling and logging

---

### 3.3 Processing Layer

The processing layer contains the core AI services that transform user input into structured data.

#### Service A: Speech-to-Text

**Technology**: OpenAI Whisper

**Function**: Converts audio recordings to raw text transcripts

**Input**: Audio file (WAV, MP3, M4A)  
**Output**: Text transcript

**Performance Target**: < 5 seconds latency

#### Service B: Translation Service

**Technology**: Google Translate API

**Function**: Translates local dialects and vernacular languages to English

**Supported Languages** (Initial):
- Tamil
- Hindi
- English (passthrough)

**Input**: Text in native language  
**Output**: English text

#### Service C: Skill Extraction Engine

**Technology**: LLM (Google Gemini 1.5 Flash or OpenAI GPT-4o)

**Function**: Extracts structured skills from informal narratives using contextual understanding

**Prompt Engineering**: Uses carefully crafted prompts to identify:
- Technical skills
- Soft skills
- Domain expertise
- Work experience indicators

**Example Input:**
```
"I sell vegetables at the market every day. I wake up at 4 AM to buy fresh produce 
from the wholesale market. I manage my inventory, negotiate prices with suppliers, 
and handle customer complaints."
```

**Example Output:**
```json
{
  "skills": [
    "Inventory Management",
    "Customer Service",
    "Negotiation",
    "Early Morning Operations",
    "Sales",
    "Financial Planning"
  ],
  "experience_years": "5+",
  "domain": "Retail/Sales"
}
```

---

### 3.4 Matching Engine

The matching engine connects user skills to relevant job opportunities.

**Approach**: Hybrid matching system

#### Rule-Based Matching
- Skill-to-job mapping database
- Minimum skill threshold requirements
- Priority-based ranking

**Example Logic:**
```
IF user.skills contains ["Driving", "Time Management", "Navigation"]
  AND user.experience > 1 year
THEN Match: "Delivery Partner" (Score: 0.95)
```

#### Semantic Matching (Optional Enhancement)
- Vector embeddings for skills and job descriptions
- Cosine similarity scoring
- Technology: Pinecone vector database

**Matching Algorithm:**
1. Extract user skill embeddings
2. Query vector database for similar job requirements
3. Rank by similarity score
4. Filter by minimum threshold (0.7)
5. Return top 5 matches

---

### 3.5 Data Layer

The data layer provides persistent storage for all system data.

#### Primary Database: PostgreSQL (Supabase)

**Stores:**
- User profiles and authentication
- Extracted skills and competencies
- Resume metadata and versions
- Job listings and requirements
- Matching history and analytics

**Features:**
- Row-level security
- Real-time subscriptions
- Automatic backups
- RESTful API access

#### Vector Database: Pinecone (Optional)

**Stores:**
- Skill embeddings (768-dimensional vectors)
- Job description embeddings
- Semantic search indices

**Use Case**: Enhanced job matching through semantic similarity

---

## 4. Data Flow

### 4.1 End-to-End User Journey

```
┌─────────────┐
│    User     │
│   Speaks    │
└──────┬──────┘
       │ Audio Recording
       ▼
┌─────────────────────┐
│  Client Application │
└──────┬──────────────┘
       │ POST /transcribe
       ▼
┌─────────────────────┐
│  Speech-to-Text     │
│  (Whisper)          │
└──────┬──────────────┘
       │ Raw Transcript
       ▼
┌─────────────────────┐
│  Translation        │
│  Service            │
└──────┬──────────────┘
       │ English Text
       ▼
┌─────────────────────┐
│  Skill Extraction   │
│  Engine (LLM)       │
└──────┬──────────────┘
       │ Structured Skills JSON
       ▼
┌─────────────────────┐
│  PostgreSQL         │
│  Database           │
└──────┬──────────────┘
       │ User Profile Saved
       ▼
┌─────────────────────┐
│  Matching Engine    │
└──────┬──────────────┘
       │ Job Matches
       ▼
┌─────────────────────┐
│  Resume Generator   │
│  (ReportLab)        │
└──────┬──────────────┘
       │ PDF Resume
       ▼
┌─────────────────────┐
│  Client Display     │
│  Results            │
└─────────────────────┘
```

### 4.2 Detailed Process Steps

1. **Audio Capture**: User speaks into microphone describing their work experience
2. **Audio Upload**: Client sends audio file to backend via `/transcribe` endpoint
3. **Transcription**: Whisper model converts audio to text (< 5 seconds)
4. **Translation**: Google Translate converts native language to English
5. **Skill Extraction**: LLM analyzes transcript and extracts structured skills (< 4 seconds)
6. **Data Persistence**: Skills and profile stored in PostgreSQL database
7. **Job Matching**: Matching engine queries database for relevant opportunities
8. **Resume Generation**: ReportLab creates professional PDF resume (< 3 seconds)
9. **Result Display**: Client shows skills, resume preview, and job matches
10. **User Review**: User can edit skills and download resume

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

## 6. Database Schema

### 6.1 Entity Relationship Diagram

```
┌─────────────────┐       ┌─────────────────┐
│     users       │       │     skills      │
├─────────────────┤       ├─────────────────┤
│ id (PK)         │───┐   │ id (PK)         │
│ name            │   └──<│ user_id (FK)    │
│ phone           │       │ skill_name      │
│ language        │       │ category        │
│ transcript      │       │ confidence      │
│ audio_url       │       │ created_at      │
│ created_at      │       └─────────────────┘
│ updated_at      │
└─────────────────┘
        │
        │
        ▼
┌─────────────────┐       ┌─────────────────┐
│    resumes      │       │      jobs       │
├─────────────────┤       ├─────────────────┤
│ id (PK)         │       │ id (PK)         │
│ user_id (FK)    │       │ title           │
│ pdf_url         │       │ description     │
│ version         │       │ required_skills │
│ created_at      │       │ salary_range    │
└─────────────────┘       │ location        │
                          │ company         │
        ┌─────────────────│ created_at      │
        │                 └─────────────────┘
        │
        ▼
┌─────────────────┐
│   job_matches   │
├─────────────────┤
│ id (PK)         │
│ user_id (FK)    │
│ job_id (FK)     │
│ match_score     │
│ matched_skills  │
│ created_at      │
└─────────────────┘
```

### 6.2 Table Definitions

#### Table: users

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique user identifier |
| name | VARCHAR(255) | NOT NULL | User's full name |
| phone | VARCHAR(20) | UNIQUE | Contact number |
| language | VARCHAR(10) | NOT NULL | Preferred language code |
| transcript | TEXT | | Raw transcript of interview |
| audio_url | VARCHAR(500) | | URL to stored audio file |
| created_at | TIMESTAMP | DEFAULT NOW() | Account creation time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update time |

#### Table: skills

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique skill identifier |
| user_id | UUID | FOREIGN KEY | Reference to users table |
| skill_name | VARCHAR(100) | NOT NULL | Name of the skill |
| category | VARCHAR(50) | | Skill category (technical, soft, domain) |
| confidence | DECIMAL(3,2) | | AI confidence score (0.00-1.00) |
| created_at | TIMESTAMP | DEFAULT NOW() | Extraction time |

#### Table: jobs

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique job identifier |
| title | VARCHAR(255) | NOT NULL | Job title |
| description | TEXT | | Detailed job description |
| required_skills | TEXT[] | | Array of required skills |
| salary_range | VARCHAR(50) | | Salary information |
| location | VARCHAR(255) | | Job location |
| company | VARCHAR(255) | | Company name |
| created_at | TIMESTAMP | DEFAULT NOW() | Listing creation time |

#### Table: resumes

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique resume identifier |
| user_id | UUID | FOREIGN KEY | Reference to users table |
| pdf_url | VARCHAR(500) | NOT NULL | URL to generated PDF |
| version | INTEGER | DEFAULT 1 | Resume version number |
| created_at | TIMESTAMP | DEFAULT NOW() | Generation time |

#### Table: job_matches

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique match identifier |
| user_id | UUID | FOREIGN KEY | Reference to users table |
| job_id | UUID | FOREIGN KEY | Reference to jobs table |
| match_score | DECIMAL(3,2) | NOT NULL | Match percentage (0.00-1.00) |
| matched_skills | TEXT[] | | Array of matched skills |
| created_at | TIMESTAMP | DEFAULT NOW() | Match creation time |

---

## 7. Resume Generation

### 7.1 Technology

**Library**: ReportLab (Python PDF generation)

### 7.2 Resume Template Structure

```
┌─────────────────────────────────────┐
│         [User Name]                 │
│    [Phone] | [Language]              │
├─────────────────────────────────────┤
│                                     │
│  PROFESSIONAL SUMMARY               │
│  [AI-generated summary based on     │
│   extracted skills and experience]  │
│                                     │
├─────────────────────────────────────┤
│                                     │
│  CORE COMPETENCIES                  │
│  • Skill 1    • Skill 4             │
│  • Skill 2    • Skill 5             │
│  • Skill 3    • Skill 6             │
│                                     │
├─────────────────────────────────────┤
│                                     │
│  WORK EXPERIENCE                    │
│  [Inferred from narrative]          │
│  • Key responsibility 1             │
│  • Key responsibility 2             │
│  • Key responsibility 3             │
│                                     │
├─────────────────────────────────────┤
│                                     │
│  RECOMMENDED ROLES                  │
│  1. [Job Title 1] - Match: 95%      │
│  2. [Job Title 2] - Match: 88%      │
│  3. [Job Title 3] - Match: 82%      │
│                                     │
├─────────────────────────────────────┤
│  Generated by SkillBridge AI        │
│  [Date] | [QR Code to Profile]      │
└─────────────────────────────────────┘
```

### 7.3 Generation Process

1. **Data Collection**: Gather user profile, skills, and job matches
2. **Summary Generation**: LLM creates professional summary from transcript
3. **Layout Rendering**: ReportLab formats content into PDF
4. **QR Code Addition**: Generate QR code linking to digital profile
5. **Storage**: Upload PDF to Supabase Storage
6. **URL Return**: Provide download link to user

### 7.4 Customization Options

- Multiple language support (resume in native language)
- Different templates (formal, creative, minimal)
- Skill highlighting based on target job
- Optional photo inclusion

---

## 8. Security Considerations

### 8.1 Authentication and Authorization

| Component | Implementation |
|-----------|----------------|
| User Authentication | JWT tokens with 24-hour expiration |
| API Key Management | Environment variables (.env file) |
| Database Access | Row-level security (RLS) in Supabase |
| Admin Access | Role-based access control (RBAC) |

### 8.2 Data Protection

| Measure | Implementation |
|---------|----------------|
| Data in Transit | HTTPS/TLS 1.3 for all API communication |
| Data at Rest | AES-256 encryption in Supabase |
| PII Handling | Minimal collection, encrypted storage |
| Audio Files | Temporary storage, auto-deletion after 30 days |

### 8.3 Input Validation

- Sanitize all user inputs to prevent injection attacks
- File type validation for audio uploads
- Size limits on audio files (max 10MB)
- Rate limiting: 100 requests/minute per user

### 8.4 API Security

- API key rotation policy (every 90 days)
- Request signing for sensitive operations
- CORS configuration for allowed origins
- DDoS protection via rate limiting

### 8.5 Privacy Compliance

- GDPR compliance for international users
- Indian data protection laws compliance
- User consent management
- Right to deletion (data erasure)
- Data portability support

---

## 9. Scalability Plan

### 9.1 Current Architecture (Hackathon MVP)

| Component | Technology | Capacity |
|-----------|-----------|----------|
| Backend | FastAPI on Render | 500 concurrent users |
| Database | Supabase Free Tier | 500MB storage |
| AI APIs | Free tier limits | ~1000 requests/day |

### 9.2 Scaling Strategy

#### Phase 1: Immediate Optimizations (0-1000 users)

- Implement caching for common queries
- Optimize database indices
- Add CDN for static assets
- Compress audio files before upload

#### Phase 2: Horizontal Scaling (1000-10,000 users)

- Deploy on AWS/GCP with auto-scaling
- Upgrade to Supabase Pro tier
- Implement Redis caching layer
- Add load balancer for API gateway
- Separate read/write database replicas

#### Phase 3: Microservices Architecture (10,000+ users)

- Split services into independent microservices:
  - Transcription service
  - Translation service
  - Skill extraction service
  - Matching service
  - Resume generation service
- Implement message queue (RabbitMQ/Kafka)
- Add monitoring and observability (Prometheus, Grafana)
- Implement circuit breakers for fault tolerance

### 9.3 Future Enhancements

| Enhancement | Timeline | Impact |
|-------------|----------|--------|
| Vector-based semantic matching | Post-hackathon | Improved job matching accuracy |
| NSQF skill mapping integration | 3 months | Standardized skill classification |
| Employer dashboard | 6 months | Two-sided marketplace |
| Offline-first mobile app | 9 months | Support for low-connectivity areas |
| Multi-modal input (text + voice) | 12 months | Increased accessibility |

---

## 10. Risk Analysis and Mitigation

### 10.1 Technical Risks

| Risk | Probability | Impact | Mitigation Strategy |
|------|------------|--------|---------------------|
| Accent recognition failure | Medium | High | Fine-tune Whisper model on local accents; add manual correction option |
| Incorrect skill inference | Medium | High | Implement human verification layer; user review and edit capability |
| API cost overrun | Low | Medium | Use Gemini free tier; implement caching; set budget alerts |
| Database performance degradation | Low | Medium | Optimize queries; add indices; implement connection pooling |
| Third-party API downtime | Low | High | Implement retry logic; fallback to alternative providers |

### 10.2 User Experience Risks

| Risk | Probability | Impact | Mitigation Strategy |
|------|------------|--------|---------------------|
| User confusion with interface | Medium | High | Conduct user testing; provide audio instructions; simplify UI |
| Privacy concerns | Medium | High | Clear privacy policy; transparent data usage; secure storage |
| Low adoption rate | Medium | Medium | Community outreach; partnerships with NGOs; user education |
| Language translation errors | Medium | Medium | Support multiple translation providers; manual review option |

### 10.3 Business Risks

| Risk | Probability | Impact | Mitigation Strategy |
|------|------------|--------|---------------------|
| Regulatory compliance issues | Low | High | Legal review; GDPR compliance; data protection officer |
| Competition from existing platforms | Medium | Medium | Focus on unique value proposition; underserved market |
| Scalability challenges | Medium | Medium | Cloud infrastructure; auto-scaling; performance monitoring |
| Funding limitations | Medium | High | Seek grants; partnerships; freemium model |

---

# 11. Future Roadmap

- Government Scheme Integration
- Micro-loan eligibility engine
- Employer verification badge system
- Offline-first Android app


---

## 11. Future Roadmap

### Phase 1: Hackathon MVP (Current)

**Timeline**: 2-3 days

**Deliverables:**
- ✅ Voice input and transcription
- ✅ Basic skill extraction
- ✅ Resume generation
- ✅ Simple rule-based job matching
- ✅ Streamlit web interface

### Phase 2: Post-Hackathon Enhancements

**Timeline**: 1-3 months

**Features:**
- Flutter mobile application (Android/iOS)
- Vector-based semantic job matching
- NSQF (National Skills Qualifications Framework) integration
- Employer dashboard for job posting
- Enhanced UI/UX based on user feedback
- Multi-language support expansion

### Phase 3: Production Scale

**Timeline**: 3-6 months

**Features:**
- Government scheme integration (PMKVY, PMEGP, etc.)
- Micro-loan eligibility assessment engine
- Employer verification badge system
- Skill verification through practical tests
- Community features (peer endorsements)
- Analytics dashboard for admins

### Phase 4: Advanced Features

**Timeline**: 6-12 months

**Features:**
- Offline-first Android application
- Multi-modal input (text + voice + video)
- AI-powered interview preparation
- Career path recommendations
- Integration with payment gateways for premium features
- Partnerships with training providers
- Blockchain-based skill certification

---

## 12. Deployment Architecture

### 12.1 Development Environment

```
Developer Machine
├── Backend (localhost:8000)
├── Frontend (localhost:8501)
└── Local PostgreSQL (optional)
```

### 12.2 Staging Environment

```
Render/Vercel
├── Backend API (staging.skillbridge.app/api)
├── Supabase Staging Database
└── Test data and mock jobs
```

### 12.3 Production Environment

```
Cloud Provider (AWS/GCP)
├── Load Balancer
├── API Gateway (multiple instances)
├── Processing Services (auto-scaling)
├── Supabase Production Database
├── CDN for static assets
└── Monitoring and logging
```

---

## 13. Monitoring and Observability

### 13.1 Key Metrics

| Metric | Tool | Alert Threshold |
|--------|------|-----------------|
| API Response Time | Application logs | > 5 seconds |
| Error Rate | Sentry | > 5% |
| Database Connections | Supabase Dashboard | > 80% capacity |
| API Cost | Cloud billing | > $100/day |
| User Registrations | Analytics | Track daily |

### 13.2 Logging Strategy

- Application logs: Structured JSON format
- Error tracking: Sentry integration
- User analytics: Mixpanel or Google Analytics
- Performance monitoring: New Relic or Datadog

---

## 14. Testing Strategy

### 14.1 Unit Testing

- Test individual functions and services
- Mock external API calls
- Target: >80% code coverage

### 14.2 Integration Testing

- Test API endpoints end-to-end
- Validate database operations
- Test AI service integrations

### 14.3 User Acceptance Testing

- Test with real users from target demographic
- Validate voice recognition accuracy
- Assess UI/UX intuitiveness
- Gather feedback on job match relevance

---

## 15. Conclusion

SkillBridge represents a paradigm shift in how informal workers access formal employment opportunities. By leveraging voice-first AI technology, the platform removes traditional barriers of language and literacy, democratizing access to the job market.

The system architecture is designed for rapid prototyping during the hackathon phase while maintaining a clear path to production scalability. The modular microservice design allows for independent scaling and enhancement of individual components.

Key success factors:
- Accurate skill extraction from informal narratives
- Relevant job matching algorithms
- Intuitive voice-first user experience
- Secure and scalable infrastructure
- Clear path to monetization and sustainability

With proper execution, SkillBridge has the potential to impact millions of informal workers across India and beyond, bridging the employability gap and creating economic opportunities for underserved communities.
