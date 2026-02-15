# CodeSikho - Requirements Document

## Project Overview

### Project Name
CodeSikho (Code + Seekho/सीखो = Learn to Code)

### Tagline
"AI-powered multilingual learning platform that breaks the language barrier in programming education for India's 50M+ coding learners"

### Vision
An AI mentor that explains code errors and teaches programming concepts in 7 Indian languages with voice support, making coding accessible to non-English speakers across India.

---

## Problem Statement

### The Language Barrier Crisis
- 90% of programming resources are exclusively in English
- 300M+ non-English speaking students in India struggle with coding
- Error messages and documentation are cryptic even for English speakers
- Students in tier-2/3 cities spend 3-5x more time debugging due to language barriers
- 70% of Indian students prefer learning in their native language

### Access & Affordability Issues
- Limited availability of quality mentors, especially in tier-2/3 cities
- Coaching institutes charge ₹50,000-150,000 per course
- Students can't get instant help during late-night coding sessions
- Quality education concentrated in metros (Bangalore, Delhi, Mumbai)
- Rural areas have almost no access to coding mentors

### Learning Inefficiency
- Students copy-paste errors into Google without understanding
- Generic StackOverflow answers don't match their specific context
- No personalized learning path based on individual knowledge gaps
- Practice problems not aligned with Indian curriculum (GATE, JEE, placement patterns)
- High dropout rates due to frustration with English-only ecosystem

### Market Statistics
- 50M+ people learning to code in India currently
- 300M+ students across all education levels (potential expansion)
- ₹10,000 crore edtech market for STEM subjects
- Growing 20% year-over-year
- 500,000+ annual shortage of software developers in India

---

## Target Users

### Primary User Personas

#### Persona 1: Rahul - Computer Science Student
- Age: 19-22
- Location: Tier-2 city (Indore, Jaipur, Coimbatore)
- Education: B.Tech Computer Science, 2nd-3rd year
- Language: Prefers Hindi, comfortable with English
- Pain Points:
  - Struggles with debugging complex errors
  - No mentor access after college hours (6 PM onwards)
  - Can't afford expensive bootcamps (₹1-2 lakhs)
  - Online tutorials too fast, can't pause and ask questions
- Goals:
  - Get campus placement in product-based company (Google, Microsoft, Amazon)
  - Solve 200+ coding problems before placement season
  - Build 2-3 strong projects for resume
- Budget: ₹500-1000/month for learning

#### Persona 2: Priya - Career Switcher
- Age: 23-26
- Location: Small town, transitioning to tech career
- Education: BA/BCom graduate, learning coding via online courses
- Language: Native Tamil speaker, basic English (reading okay, speaking difficult)
- Pain Points:
  - Feels intimidated by English-only coding environment
  - Can't express doubts clearly in English
  - YouTube tutorials move too fast, no way to ask follow-up questions
  - Feels isolated without peer support
- Goals:
  - Switch from non-tech to software development role
  - Get first tech job (₹3-5 lakhs package)
  - Build portfolio of 5+ projects
  - Learn full-stack development in 6-9 months
- Budget: ₹1,000-2,000/month

#### Persona 3: Arjun - School Student
- Age: 14-17
- Location: Tier-3 city or rural area
- Education: Classes 9-12, learning coding as part of school curriculum
- Language: Primarily regional language (Telugu, Kannada, Bengali)
- Pain Points:
  - School CS teacher has limited knowledge, can't answer advanced questions
  - YouTube videos are in English, hard to understand
  - No one to ask when stuck on homework
  - Wants to build projects but doesn't know how to start
- Goals:
  - Build science fair project using coding
  - Score well in Class 12 CS board exam
  - Prepare for JEE (some CS questions)
  - Explore if coding is right career path
- Budget: ₹100-500/month max

### Market Size
- Primary (Coding): 50M+ learners
- Secondary (All subjects): 300M+ students
- Total Addressable Market: ₹60,000 crore (300M × ₹2,000/year)

---

## Functional Requirements

### FR1: Multilingual Error Explanation

#### FR1.1 Language Support
- Support 7 Indian languages: Hindi, Tamil, Telugu, Bengali, Kannada, Malayalam, English
- Each language must have native explanations, not just translations
- Use Indian context in examples (cricket, trains, GST calculations)

#### FR1.2 Error Analysis
- Accept error messages via text input or code snippet paste
- Analyze code context to understand root cause
- Provide explanation in user's preferred language including:
  - Simple explanation in everyday language
  - Root cause identification
  - Step-by-step fix with code examples
  - Related concept to learn next
  - Links to practice problems

#### FR1.3 Response Time
- Text-based error explanation: < 2 seconds (P95)
- Maintain conversation context for follow-up questions
- Support multi-turn conversations with 4,000 token context window

### FR2: Voice-Based Interaction

#### FR2.1 Speech-to-Text
- Accept voice input in all 7 supported languages
- Support Indian accents and dialects
- Custom vocabulary for coding terms (Python, JavaScript, IndexError, etc.)
- Transcription accuracy: > 90% for Indian accents

#### FR2.2 Text-to-Speech
- Generate audio responses in user's preferred language
- Use neural voices for natural-sounding speech
- Adjustable speech rate (0.75x - 1.5x)
- Provide both text and audio responses simultaneously

#### FR2.3 Voice Pipeline Performance
- End-to-end latency: < 3 seconds (P95)
- Support both web and mobile platforms
- Enable listen-only mode for commuting users

### FR3: Context-Aware Code Assistant

#### FR3.1 Code Analysis
- Accept single file, multiple files, or GitHub repository link
- Parse and understand project structure and dependencies
- Support Python, JavaScript, Java, C++, C, Go
- Identify logical errors, not just syntax mistakes
- Provide skill-level-appropriate explanations

#### FR3.2 Code Execution
- Run code in secure sandboxed environment
- Resource limits: 0.5 vCPU, 512 MB RAM, 10-second timeout
- No network access, no file system write access (except /tmp)
- Blacklist dangerous imports (os, subprocess, eval, exec)
- Return output in < 3 seconds

#### FR3.3 Security
- Code not stored permanently, ephemeral processing only
- Input sanitization to prevent code injection
- Automatic container cleanup after execution

### FR4: Personalized Learning Paths

#### FR4.1 Skill Assessment
- 15-20 question assessment covering core CS topics
- Analyze answers to identify:
  - Current skill level (beginner, intermediate, advanced)
  - Strong topics (arrays, loops, etc.)
  - Weak topics (recursion, dynamic programming, etc.)

#### FR4.2 Learning Path Generation
- Generate custom roadmap based on assessment results
- Support goal templates:
  - Campus placement in product-based company (3-4 months)
  - Clear GATE CS with top rank (6 months)
  - Build full-stack web application (4 months)
  - Learn Data Science and ML (5 months)
  - Switch from testing to development role (4 months)

#### FR4.3 Adaptive Learning
- Track progress and adapt difficulty
- Provide weekly reminders and motivation
- Visual progress dashboards
- Recommend resources based on learning patterns

### FR5: Practice Problem Generator

#### FR5.1 Problem Generation
- AI-generated problems tailored to skill level
- Use Indian context (cricket scores, train bookings, GST calculations)
- Align with competitive exam patterns (GATE, JEE, company interviews)
- Support difficulty levels: Easy, Medium, Hard

#### FR5.2 Problem Categories
- Data Structures (arrays, linked lists, trees, graphs, heaps)
- Algorithms (sorting, searching, dynamic programming, greedy)
- Object-Oriented Programming
- System Design (simplified for beginners)
- Database Queries (SQL)

#### FR5.3 Solution Validation
- Instant validation of submitted code
- Explain mistakes in user's preferred language
- Track problems solved and attempted
- Adapt difficulty based on success rate

### FR6: Offline Capability (PWA)

#### FR6.1 Progressive Web App
- Install as standalone app on mobile/desktop
- Work in low-connectivity areas
- Cache previously accessed explanations locally
- Download practice problems for offline solving

#### FR6.2 Offline Features
- Basic code syntax validation without AI
- Queue voice inputs for processing when online
- Sync progress when connection restored
- Smart cache invalidation (7-day TTL)

#### FR6.3 Storage Management
- IndexedDB for local data storage (5MB limit)
- Service Workers for intelligent caching
- Background sync API for queued operations

### FR7: User Management

#### FR7.1 Authentication
- Email/password registration and login
- Social authentication (Google, GitHub)
- JWT token-based authentication (15-min access, 7-day refresh)
- MFA support (TOTP)

#### FR7.2 User Preferences
- Select preferred language
- Set skill level
- Choose learning goals
- Customize notification settings

#### FR7.3 Subscription Management
- Free tier: 10 questions/day, text only
- Premium tier: ₹99/month, unlimited questions, voice support, offline mode
- Referral program: Invite 3 friends → 1 month premium

---

## Non-Functional Requirements

### NFR1: Performance

#### NFR1.1 Response Time (P95)
- Authentication: < 200ms
- Chat message (text): < 2s
- Code analysis: < 1s
- Voice transcription: < 3s
- Problem generation: < 5s

#### NFR1.2 Throughput
- Support 10,000+ concurrent users
- Handle 100+ messages per second
- Process 500+ code executions per minute

#### NFR1.3 Scalability
- Auto-scale based on load (CPU > 70% or RequestCount > 100/min)
- Support horizontal scaling for all services
- Handle 10x traffic spikes during exam seasons

### NFR2: Availability & Reliability

#### NFR2.1 Uptime
- Target: 99.5% uptime (43 minutes downtime/month max)
- Multi-AZ deployment for critical services
- Auto-healing for failed containers
- Graceful degradation when AI services unavailable

#### NFR2.2 Error Handling
- Error rate: < 1% of all requests
- Automatic retry with exponential backoff
- User-friendly error messages in preferred language

### NFR3: Security

#### NFR3.1 Data Protection
- Encryption at rest (RDS, S3, DynamoDB)
- Encryption in transit (TLS 1.3, WSS)
- No permanent storage of user code
- GDPR-compliant data handling

#### NFR3.2 Authentication & Authorization
- Secure password hashing (bcrypt, cost factor 12)
- JWT token validation on all protected endpoints
- Role-based access control (user, premium, admin)

#### NFR3.3 Rate Limiting
- Free tier: 100 requests/hour, 1,000 requests/day
- Premium tier: 1,000 requests/hour, unlimited daily
- Voice: 20 requests/hour (free), 100/hour (premium)

#### NFR3.4 Input Validation
- SQL injection prevention via parameterized queries
- XSS prevention via React auto-escaping + CSP headers
- CSRF protection via SameSite cookies
- Code input sanitization (blacklist dangerous patterns)

### NFR4: Maintainability

#### NFR4.1 Code Quality
- Microservices architecture for modularity
- Comprehensive API documentation (OpenAPI/Swagger)
- Unit test coverage: > 80%
- Integration test coverage: > 60%

#### NFR4.2 Monitoring & Logging
- Centralized logging via CloudWatch
- Distributed tracing via X-Ray
- Real-time alerts for critical errors
- Performance metrics dashboard

#### NFR4.3 Deployment
- CI/CD pipeline for automated deployments
- Blue-green deployment for zero-downtime updates
- Automated rollback on deployment failures

### NFR5: Usability

#### NFR5.1 User Interface
- Mobile-first responsive design
- Support screen sizes from 320px to 4K
- Accessibility compliance (WCAG 2.1 Level AA target)
- Dark mode support

#### NFR5.2 Localization
- UI text in all 7 supported languages
- Right-to-left (RTL) support where applicable
- Culturally appropriate icons and imagery

#### NFR5.3 User Experience
- Onboarding flow for new users (< 2 minutes)
- Contextual help and tooltips
- Keyboard shortcuts for power users
- Voice commands for hands-free operation

### NFR6: Cost Efficiency

#### NFR6.1 Infrastructure Costs
- Target: < $0.35 per user per month
- Multi-level caching to reduce AI API calls by 40%
- Serverless-first architecture to minimize idle costs
- Reserved capacity for predictable workloads

#### NFR6.2 AI Costs
- Multi-model strategy (Haiku for simple, Sonnet for medium, Opus for complex)
- Cache common error explanations (24-hour TTL)
- Batch processing for non-real-time operations

---

## Integration Requirements

### INT1: AWS Services Integration

#### INT1.1 AI/ML Services
- Amazon Bedrock (Claude 3.5 Sonnet) for AI intelligence
- Amazon Transcribe for speech-to-text (7 languages)
- Amazon Polly for text-to-speech (Neural voices)
- Amazon Translate for backup translation

#### INT1.2 Compute Services
- AWS Lambda for serverless functions (Auth, Code, Voice, Content)
- Amazon ECS (Fargate) for containerized services (AI, Learning)
- Auto-scaling configuration for all compute resources

#### INT1.3 Storage Services
- Amazon RDS (PostgreSQL) for relational data
- Amazon DynamoDB for NoSQL data (sessions, real-time activity)
- Amazon ElastiCache (Redis) for caching
- Amazon S3 for object storage

#### INT1.4 Networking Services
- Amazon CloudFront for CDN
- Amazon API Gateway for API management
- Amazon Route 53 for DNS
- AWS WAF for security

#### INT1.5 Security Services
- AWS Cognito for user authentication
- AWS Secrets Manager for credential management
- AWS Certificate Manager for SSL/TLS certificates

#### INT1.6 Monitoring Services
- Amazon CloudWatch for logs and metrics
- AWS X-Ray for distributed tracing
- CloudWatch Alarms for alerting

### INT2: Third-Party Integrations

#### INT2.1 Version Control
- GitHub API for repository analysis
- Support for public and private repositories
- OAuth authentication for GitHub access

#### INT2.2 Payment Gateway
- Razorpay for Indian payment methods (UPI, cards, wallets)
- Subscription management and recurring billing
- Webhook integration for payment events

#### INT2.3 Analytics
- Google Analytics for user behavior tracking
- Mixpanel for product analytics
- Custom event tracking for learning outcomes

---

## Data Requirements

### DR1: User Data

#### DR1.1 User Profile
- User ID (UUID, primary key)
- Email (unique, indexed)
- Cognito Sub (unique, indexed)
- Preferred language (default: 'en')
- Skill level (beginner, intermediate, advanced)
- Created at, Last login timestamps
- Subscription status (free, premium)
- Subscription end date

#### DR1.2 User Preferences
- Notification settings
- Theme preference (light, dark)
- Voice settings (speed, volume)
- Privacy settings

### DR2: Conversation Data

#### DR2.1 Conversations
- Conversation ID (UUID, primary key)
- User ID (foreign key)
- Started at, Last message at timestamps
- Message count
- Topic (debugging, learning, practice)
- Programming language

#### DR2.2 Messages
- Message ID (UUID, primary key)
- Conversation ID (foreign key)
- Role (user, assistant)
- Content (text)
- Content language
- Tokens used
- Created at timestamp
- Partitioned by month for performance

### DR3: Learning Data

#### DR3.1 User Progress
- User ID + Topic (composite primary key)
- Proficiency score (0.00 to 1.00)
- Problems solved, Problems attempted
- Last practiced timestamp
- Time spent (minutes)

#### DR3.2 Problems
- Problem ID (UUID, primary key)
- Title (JSONB, multilingual)
- Description (JSONB, multilingual)
- Difficulty (easy, medium, hard)
- Topic
- Constraints, Sample input/output (JSONB)
- Solution template (JSONB, per language)
- Created at, Created by (ai, human)

#### DR3.3 Submissions
- Submission ID (UUID, primary key)
- User ID, Problem ID (foreign keys)
- Code, Language
- Status (correct, wrong_answer, runtime_error, time_limit)
- Execution time (ms), Memory used (KB)
- Test cases passed/total
- Submitted at timestamp

### DR4: Session Data (DynamoDB)

#### DR4.1 Sessions
- Session ID (partition key)
- User ID
- Conversation context (last 5 messages)
- Expires at (TTL enabled)
- Created at

#### DR4.2 User Activity
- User ID (partition key)
- Timestamp (sort key)
- Action (message_sent, code_executed, problem_solved)
- Details (map)
- IP address

### DR5: Cache Data (Redis)

#### DR5.1 Cache Keys
- session:{session_id} → User session data (30 min TTL)
- conv:{conversation_id}:context → Conversation context (30 min TTL)
- error:{error_hash}:{language} → Cached AI responses (24 hour TTL)
- progress:{user_id}:topics → User progress (1 hour TTL)
- ratelimit:{user_id}:{endpoint} → Rate limit counters (1 min TTL)

---

## Compliance & Regulatory Requirements

### CR1: Data Privacy
- Comply with Indian IT Act 2000
- GDPR compliance for international users
- User data deletion on request (within 30 days)
- Data retention policy (7 years for financial, 1 year for activity logs)

### CR2: Content Moderation
- Filter inappropriate content in user inputs
- Prevent generation of harmful code
- Report abuse and spam

### CR3: Accessibility
- Target WCAG 2.1 Level AA compliance
- Screen reader compatibility
- Keyboard navigation support
- Voice-first interface for accessibility

---

## Success Metrics

### User Engagement Metrics
- Daily Active Users (DAU): 30% of MAU
- Weekly Active Users (WAU): 60% of MAU
- Session duration: 15-30 minutes average
- Messages per session: 3-5 interactions
- Return rate: 60% within 7 days

### Learning Outcome Metrics
- Problem-solving success rate: 50% of attempted problems
- Debugging time reduction: 40-60% vs. without CodeSikho
- Skill level progression: Beginner → Intermediate in 3 months
- Placement success rate: 20% increase for final-year students

### Platform Performance Metrics
- Response time: Text P95 < 2s, Voice P95 < 3s
- Error rate: < 1% of all requests
- Uptime: 99.5%
- Voice accuracy: > 90% transcription accuracy

### Business Metrics
- User Acquisition Cost (CAC): ₹50-150 per user
- Customer Lifetime Value (LTV): ₹1,800 (18 months × ₹99)
- LTV:CAC ratio: > 3:1
- Conversion rate (Free → Premium): 20%
- Net Promoter Score (NPS): > 50
- Monthly churn rate: < 5% for premium users

---

## Constraints & Assumptions

### Technical Constraints
- AWS as primary cloud provider
- Budget: $3,227/month for 10,000 users
- AWS credits: $10,000-15,000 from hackathon
- Initial deployment: Single region (ap-south-1, Mumbai)

### Business Constraints
- MVP timeline: 8-10 weeks
- Initial target: 10,000 active users in 6 months
- Pricing: ₹99/month for premium (fixed for first year)
- Break-even: 12,625 total users (20% conversion)

### Assumptions
- Users have smartphones or computers with internet access
- Users comfortable with basic app navigation
- Indian language speakers prefer native language over English
- Voice input preferred by 30-40% of users
- 20% free-to-premium conversion rate achievable
- AWS credits will cover infrastructure for 6 months

---

## Out of Scope (Future Phases)

### Phase 2 (Months 7-9)
- Multi-subject expansion (Math, Physics, Chemistry)
- Live doubt solving sessions
- Mock interviews with AI interviewer
- Native mobile apps (Android, iOS)

### Phase 3 (Months 10-12)
- All STEM subjects
- Collaboration features (pair programming)
- Teacher tools (assignment creation, auto-grading)
- Advanced analytics for institutions

### Phase 4 (Year 2+)
- Complete K-12 curriculum
- International expansion (Bangladesh, Nepal, Sri Lanka)
- Video content generation
- AR/VR learning experiences

---

## Glossary

- MAU: Monthly Active Users
- DAU: Daily Active Users
- WAU: Weekly Active Users
- P95: 95th percentile (95% of requests faster than this)
- P99: 99th percentile
- TTL: Time To Live (cache expiration)
- JWT: JSON Web Token
- MFA: Multi-Factor Authentication
- PWA: Progressive Web App
- CDN: Content Delivery Network
- AST: Abstract Syntax Tree
- NPS: Net Promoter Score
- CAC: Customer Acquisition Cost
- LTV: Lifetime Value
- WCAG: Web Content Accessibility Guidelines

---

**Document Version:** 1.0  
**Last Updated:** February 15, 2026  
**Status:** Draft for AI for Bharat Hackathon 2026

