# SkillBridge – Requirements Specification

## 1. Executive Summary

### 1.1 Problem Statement

Millions of informal workers—including street vendors, gig workers, and homemakers—possess valuable skills but lack formal documentation, certifications, or resumes. Traditional employment platforms like LinkedIn, Indeed, and Naukri require:

- Written resumes
- English proficiency
- Structured employment history
- Formal education credentials

This creates a **"Language of Employability Gap"** that systematically excludes skilled workers from formal employment opportunities.

### 1.2 Solution Overview

SkillBridge bridges this gap using a voice-first AI interface that converts lived experience into formal skill representations mapped to industry standards. The platform democratizes access to employment opportunities by removing language and literacy barriers.

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

### 3.1 Performance Requirements

| ID | Requirement | Target | Measurement |
|----|-------------|--------|-------------|
| NFR1 | Transcription Latency | < 5 seconds | Time from audio upload to text output |
| NFR2 | Skill Extraction Response | < 4 seconds | Time from transcript to structured skills |
| NFR3 | Resume Generation | < 3 seconds | Time to generate PDF |
| NFR4 | End-to-End Processing | < 15 seconds | Complete workflow from audio to resume |

### 3.2 Scalability Requirements

| ID | Requirement | Target |
|----|-------------|--------|
| NFR5 | Concurrent Users | Minimum 500 users (hackathon scale) |
| NFR6 | Database Growth | Support 10,000+ user profiles |
| NFR7 | API Rate Limiting | 100 requests/minute per user |

### 3.3 Security Requirements

| ID | Requirement | Implementation |
|----|-------------|----------------|
| NFR8 | Data Transmission | HTTPS/TLS 1.3 for all API communication |
| NFR9 | API Key Management | Secure storage in environment variables |
| NFR10 | Database Access | Role-based access control (RBAC) |
| NFR11 | Data Encryption | Encrypted storage for sensitive user data |
| NFR12 | Input Validation | Sanitize all user inputs to prevent injection attacks |

### 3.4 Accessibility Requirements

| ID | Requirement | Description |
|----|-------------|-------------|
| NFR13 | Voice-First Interface | Primary interaction through voice, minimal text input |
| NFR14 | Multilingual Support | Tamil and Hindi initially, expandable to other languages |
| NFR15 | Low-Literacy Design | Visual icons and audio feedback for navigation |
| NFR16 | Low-Bandwidth Support | Optimized for 2G/3G networks |

---

## 4. Technical Requirements

### 4.1 Backend Technology Stack

| Component | Technology | Version | Purpose |
|-----------|-----------|---------|---------|
| Runtime | Python | 3.10+ | Core application runtime |
| Web Framework | FastAPI | Latest | RESTful API development |
| AI Orchestration | LangChain | Latest | LLM integration and chaining |
| Database | Supabase (PostgreSQL) | Latest | User data and profiles |
| PDF Generation | ReportLab | Latest | Resume PDF creation |

### 4.2 AI Model Requirements

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Large Language Model | Google Gemini 1.5 Flash or OpenAI GPT-4o | Skill extraction and understanding |
| Speech-to-Text | OpenAI Whisper | Audio transcription |
| Translation | Google Translate API | Language translation |
| Embeddings (Optional) | OpenAI text-embedding-3-small | Semantic job matching |

### 4.3 Frontend Technology Stack

| Version | Technology | Purpose |
|---------|-----------|---------|
| MVP (Hackathon) | Streamlit | Rapid prototyping and demo |
| Production | Flutter | Cross-platform mobile application |

### 4.4 Data Storage Requirements

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Primary Database | PostgreSQL (Supabase) | User profiles, skills, job listings |
| Vector Database | Pinecone (Optional) | Semantic search and matching |
| File Storage | Supabase Storage | Audio files and generated PDFs |

---

## 5. System Constraints

### 5.1 Technical Constraints

| ID | Constraint | Impact |
|----|-----------|--------|
| C1 | Free-tier API limits | Must optimize API calls and implement caching |
| C2 | Low-bandwidth environments | Must minimize data transfer and support offline features |
| C3 | Mobile device compatibility | Must support devices with limited processing power |

### 5.2 User Constraints

| ID | Constraint | Impact |
|----|-----------|--------|
| C4 | Low-literacy users | Interface must be primarily voice and icon-based |
| C5 | Limited English proficiency | Must support native language throughout |
| C6 | Limited technical knowledge | Must provide intuitive, guided user experience |

### 5.3 Business Constraints

| ID | Constraint | Impact |
|----|-----------|--------|
| C7 | Hackathon timeline | MVP must be completed within event timeframe |
| C8 | Budget limitations | Must use free-tier services where possible |

---

## 6. Assumptions and Dependencies

### 6.1 User Assumptions

- User has access to a smartphone with microphone
- User can speak clearly in their local dialect
- User has internet connection (2G or better)
- User is willing to share work experience verbally

### 6.2 Technical Assumptions

- AI models will accurately extract skills from informal narratives
- Translation services will handle local dialects adequately
- Third-party APIs will maintain >99% uptime
- Database will scale within free-tier limits during hackathon

### 6.3 Dependencies

- OpenAI API availability and rate limits
- Google Translate API functionality
- Supabase service reliability
- Internet connectivity for users

---

## 7. Success Criteria and KPIs

### 7.1 User Experience Metrics

| Metric | Target | Measurement Method |
|--------|--------|-------------------|
| Time to Resume | < 2 minutes | End-to-end workflow timing |
| Skill Extraction Accuracy | Minimum 5 meaningful skills | Manual validation |
| Job Match Relevance | Minimum 3 relevant matches | User feedback |
| User Satisfaction | > 80% positive feedback | Post-interaction survey |

### 7.2 Technical Performance Metrics

| Metric | Target | Measurement Method |
|--------|--------|-------------------|
| System Uptime | > 95% | Monitoring tools |
| API Response Time | < 5 seconds average | Application logs |
| Error Rate | < 5% | Error tracking |
| Transcription Accuracy | > 90% | Sample validation |

### 7.3 Business Impact Metrics

| Metric | Target | Measurement Method |
|--------|--------|-------------------|
| User Registrations | 100+ during hackathon | Database count |
| Resume Generations | 50+ | System logs |
| Job Matches Created | 150+ | Database analytics |

---

## 8. Risk Analysis and Mitigation

### 8.1 Technical Risks

| Risk | Probability | Impact | Mitigation Strategy |
|------|------------|--------|---------------------|
| Accent recognition failure | Medium | High | Fine-tune Whisper model; add manual correction |
| Incorrect skill inference | Medium | High | Implement human verification layer |
| API cost overrun | Low | Medium | Use Gemini free tier; implement rate limiting |
| Database performance issues | Low | Medium | Optimize queries; implement caching |

### 8.2 User Experience Risks

| Risk | Probability | Impact | Mitigation Strategy |
|------|------------|--------|---------------------|
| User confusion with interface | Medium | High | Conduct user testing; simplify UI |
| Privacy concerns | Medium | High | Clear privacy policy; secure data handling |
| Low adoption rate | Medium | Medium | User education; community outreach |

### 8.3 Business Risks

| Risk | Probability | Impact | Mitigation Strategy |
|------|------------|--------|---------------------|
| Regulatory compliance issues | Low | High | Legal review; data protection compliance |
| Competition from existing platforms | Medium | Medium | Focus on unique value proposition |
| Scalability challenges | Medium | Medium | Plan for infrastructure scaling |

---

## 9. Future Enhancements

### Phase 2 (Post-Hackathon)
- Vector-based semantic job matching
- NSQF (National Skills Qualifications Framework) integration
- Employer dashboard for job posting
- Mobile application (Flutter)

### Phase 3 (Long-term)
- Government scheme integration (PMKVY, etc.)
- Micro-loan eligibility assessment
- Employer verification badge system
- Offline-first Android application
- Multi-modal input (text + voice)
- Skill verification through practical tests

---

## 10. Compliance and Standards

### 10.1 Data Protection
- GDPR compliance for international users
- Indian data protection laws compliance
- User consent management

### 10.2 Accessibility Standards
- Voice-first design principles
- Support for users with disabilities
- Multilingual accessibility

### 10.3 Industry Standards
- NSQF skill classification alignment
- ISO/IEC 27001 security practices
- RESTful API design standards
