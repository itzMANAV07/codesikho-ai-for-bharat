# CodeSikho - Design Document

## Executive Summary

CodeSikho is an AI-powered multilingual learning platform designed to break the language barrier in programming education for India's 50M+ coding learners. The platform leverages 18 AWS services in a microservices architecture to deliver real-time error explanations, voice-based interactions, and personalized learning paths in 7 Indian languages.

### Key Design Principles
- **Serverless-First**: Minimize operational overhead and optimize costs
- **Microservices Architecture**: Independent, scalable services
- **Multi-Level Caching**: Reduce latency and AI API costs by 40%
- **Event-Driven**: Real-time processing and async communication
- **Security by Design**: Encryption, authentication, and input validation at every layer
- **Mobile-First**: Optimized for Indian users (primarily mobile)
- **Offline-Capable**: PWA architecture for low-connectivity areas

---

## System Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    USER LAYER                           │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │
│  │  Web App    │  │ Mobile PWA  │  │ Native Apps │      │
│  │  (React)    │  │  (React)    │  │  (Future)   │      │
│  └─────────────┘  └─────────────┘  └─────────────┘      │
└──────────────────────────┬──────────────────────────────┘
                           │ HTTPS/WSS
                           ▼
┌─────────────────────────────────────────────────────────┐
│                  CDN & EDGE LAYER                       │
│  ┌───────────────────────────────────────────────────┐  │
│  │ CloudFront CDN | Route 53 | AWS WAF               │  │
│  └───────────────────────────────────────────────────┘  │
└──────────────────────────┬──────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                  API GATEWAY LAYER                      │
│  ┌───────────────────────────────────────────────────┐  │
│  │ API Gateway (REST + WebSocket)                    │  │
│  │ - Authentication (JWT validation)                 │  │
│  │ - Rate Limiting (100 req/min free, unlimited pro) │  │
│  │ - Request Routing                                 │  │
│  └───────────────────────────────────────────────────┘  │
└──────────────────────────┬──────────────────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│Auth Service │    │ AI Service  │    │Code Service │
│  (Lambda)   │    │    (ECS)    │    │  (Lambda)   │
│             │    │             │    │             │
│ + Cognito   │    │  + Bedrock  │    │ Sandboxed   │
│             │    │             │    │  Execution  │
└─────────────┘    └─────────────┘    └─────────────┘
      ▼                  ▼                  ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│Voice Service│    │Learning Svc │    │Content Svc  │
│  (Lambda)   │    │   (ECS)     │    │  (Lambda)   │
│             │    │             │    │             │
│ Transcribe/ │    │ Progress    │    │ Problem     │
│   Polly     │    │ Tracking    │    │ Generator   │
└─────────────┘    └─────────────┘    └─────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ PostgreSQL  │    │  DynamoDB   │    │    Redis    │
│    (RDS)    │    │             │    │             │
│             │    │  Sessions   │    │   Cache     │
│ User Data   │    │ Real-time   │    │  Hot Data   │
└─────────────┘    └─────────────┘    └─────────────┘
      ▼                  ▼
┌─────────────┐    ┌─────────────┐
│     S3      │    │ CloudWatch  │
│             │    │             │
│   Storage   │    │ Monitoring  │
│  Backups    │    │   Logging   │
└─────────────┘    └─────────────┘
```


### Architecture Patterns

#### Microservices Architecture
- **6 Core Services**: Auth, AI, Code, Voice, Learning, Content
- **Independent Deployment**: Each service can be deployed independently
- **Technology Diversity**: Lambda for stateless, ECS for stateful/long-running
- **Service Discovery**: API Gateway routes to appropriate service
- **Inter-Service Communication**: REST APIs + Event-driven (future: SNS/SQS)

#### Serverless-First Approach
- **AWS Lambda**: For short-lived, stateless operations (< 15 min)
- **Amazon ECS Fargate**: For long-running AI tasks and stateful services
- **Benefits**: Pay-per-use, auto-scaling, no server management
- **Cost Optimization**: Power tuning for Lambda, spot instances for ECS (future)

#### Event-Driven Architecture
- **DynamoDB Streams**: Real-time activity processing
- **WebSocket**: Real-time chat and notifications
- **Future**: SNS/SQS for async processing (email, analytics)

---

## Microservices Design

### 1. Auth Service

#### Technology Stack
- **Runtime**: Node.js 18
- **Framework**: Express.js
- **Compute**: AWS Lambda (512 MB RAM, 10s timeout)
- **Authentication**: AWS Cognito
- **Storage**: DynamoDB (sessions), Redis (cache)

#### API Endpoints
```
POST   /auth/register          - User registration
POST   /auth/login             - User login
POST   /auth/refresh           - Refresh access token
POST   /auth/logout            - User logout
GET    /auth/me                - Get current user info
POST   /auth/social/google     - Google OAuth
POST   /auth/social/github     - GitHub OAuth
```

#### Authentication Flow
```
1. User submits credentials
2. Lambda validates input
3. Cognito authenticates user
4. Generate JWT tokens (access: 15min, refresh: 7 days)
5. Store session in DynamoDB + Redis
6. Return tokens to client
```

#### JWT Structure
```json
{
  "header": {
    "alg": "RS256",
    "typ": "JWT"
  },
  "payload": {
    "sub": "user_id",
    "email": "user@example.com",
    "iat": 1609459200,
    "exp": 1609460100,
    "tier": "premium",
    "preferred_language": "hi"
  }
}
```

#### Security Features
- Password hashing: bcrypt (cost factor 12)
- Rate limiting: 5 login attempts per 15 minutes
- MFA support: TOTP via Cognito
- Session management: Redis for fast lookup
- Token rotation: Automatic refresh token rotation


### 2. AI Service

#### Technology Stack
- **Runtime**: Python 3.11
- **Framework**: FastAPI
- **Compute**: Amazon ECS Fargate (2 vCPU, 4 GB RAM)
- **AI Model**: Amazon Bedrock (Claude 3.5 Sonnet)
- **Auto-Scaling**: 2-10 tasks based on CPU/RequestCount

#### API Endpoints
```
WS     /ai/chat                - WebSocket for real-time chat
POST   /ai/explain-error       - Explain programming error
POST   /ai/explain-concept     - Explain programming concept
POST   /ai/review-code         - Code review and suggestions
POST   /ai/generate-solution   - Generate solution for problem
```

#### AI Pipeline Architecture
```
User Message
    ↓
API Gateway (WebSocket)
    ↓
AI Service (ECS)
    ↓
Context Manager (retrieve last 5 messages from Redis)
    ↓
Prompt Engineering Layer
    ↓
Amazon Bedrock (Claude 3.5 Sonnet)
    ↓
Response Streaming (via WebSocket)
    ↓
Cache Response (Redis, 24h TTL for common errors)
    ↓
User receives response
```

#### Prompt Engineering Strategy

**System Prompt Template**:
```python
SYSTEM_PROMPT = """
You are CodeSikho AI, an expert programming mentor for Indian students.
User's preferred language: {language}
User's skill level: {skill_level}

Guidelines:
1. Explain in {language} using simple, everyday language
2. Use Indian context in examples (cricket, trains, etc.)
3. Adjust complexity based on skill level: {skill_level}
4. Provide step-by-step solutions
5. Encourage learning, don't just give answers
6. Be supportive and patient
"""
```

**Error Explanation Prompt**:
```python
ERROR_PROMPT = """
The user encountered this error:
Error: {error_message}

Code context:
{code_snippet}

Explain:
1. What this error means in simple terms
2. Why it occurred (root cause)
3. How to fix it (step-by-step)
4. Related concept to learn next
5. Provide corrected code example

Respond in {language}.
"""
```

#### Multi-Model Strategy
```python
def select_model(query_complexity):
    if query_complexity == "simple":
        return "anthropic.claude-3-haiku"  # Fast, cheap
    elif query_complexity == "medium":
        return "anthropic.claude-3-5-sonnet"  # Balanced
    else:
        return "anthropic.claude-3-opus"  # Complex, expensive
```

#### Context Management
- **Context Window**: 4,000 tokens (last 5 messages)
- **Storage**: Redis with 30-minute TTL
- **Key Format**: `conv:{conversation_id}:context`
- **Pruning**: Automatic when exceeding token limit

#### Response Streaming
```python
async def stream_response(prompt, websocket):
    async for chunk in bedrock_client.stream(prompt):
        await websocket.send_json({
            "type": "chunk",
            "content": chunk.text,
            "done": False
        })
    await websocket.send_json({"type": "done", "done": True})
```

#### Caching Strategy
- **Common Errors**: Cache for 24 hours (40% hit rate)
- **Cache Key**: `error:{hash(error_message)}:{language}`
- **Invalidation**: Manual for outdated explanations
- **Cost Savings**: 40% reduction in Bedrock API calls


