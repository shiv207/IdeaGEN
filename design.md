# ideaGEN: The Invention Engine
## Design Document

---

## 1. System Architecture

### 1.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     User Interface Layer                     │
│  (Web App: React/Next.js + Tailwind CSS)                    │
└─────────────────┬───────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────┐
│                  API Gateway / Backend                       │
│  (Node.js/Python FastAPI + WebSocket for real-time)        │
└─────────────────┬───────────────────────────────────────────┘
                  │
        ┌─────────┴─────────┬──────────────┬─────────────┐
        │                   │              │             │
┌───────▼────────┐  ┌──────▼──────┐  ┌───▼────┐  ┌────▼─────┐
│   Problem      │  │  Solution   │  │ Visual │  │ Learning │
│   Discovery    │  │  Generation │  │ Engine │  │  Engine  │
│   Engine       │  │  Engine     │  │        │  │          │
└───────┬────────┘  └──────┬──────┘  └───┬────┘  └────┬─────┘
        │                  │              │            │
        └──────────────────┴──────────────┴────────────┘
                           │
        ┌──────────────────┴──────────────────┐
        │                                     │
┌───────▼────────┐              ┌────────────▼──────────┐
│  LLM Services  │              │   Data Layer          │
│  (OpenAI/      │              │   - Vector DB         │
│   Anthropic)   │              │   - PostgreSQL        │
│                │              │   - Redis Cache       │
└────────────────┘              └───────────────────────┘
```

### 1.2 Component Breakdown

#### Frontend (User Interface Layer)
- **Technology**: React with Next.js, TypeScript, Tailwind CSS
- **Key Features**:
  - Conversational chat interface
  - Problem/solution card displays
  - Visual mockup viewer
  - Innovation portfolio dashboard
  - Real-time streaming responses

#### Backend (API Layer)
- **Technology**: Python FastAPI or Node.js Express
- **Responsibilities**:
  - Request routing and validation
  - Session management
  - Rate limiting and authentication
  - WebSocket connections for streaming

#### Core AI Engines
- **Problem Discovery Engine**: Analyzes input and identifies latent problems
- **Solution Generation Engine**: Creates innovative solutions
- **Visual Engine**: Generates mockups and diagrams
- **Learning Engine**: Improves over time with feedback

---

## 2. Core Engine Designs

### 2.1 Problem Discovery Engine

#### Architecture
```
User Input → Context Analyzer → Pattern Detector → Problem Ranker → Output
                    ↓
              Knowledge Base
```

#### Process Flow

1. **Context Analysis**
   - Parse user input (text, voice, images)
   - Extract entities, activities, and temporal patterns
   - Build user context model

2. **Pattern Detection**
   - Identify repetitive actions
   - Detect workarounds and compensating behaviors
   - Find time sinks and friction points
   - Recognize normalized inefficiencies

3. **Problem Formulation**
   - Convert patterns into problem statements
   - Calculate impact scores
   - Assess novelty (is this a known problem?)
   - Generate "why this matters" narratives

4. **Ranking & Filtering**
   - Score by: Impact × Novelty × Solvability
   - Filter out obvious/trivial problems
   - Prioritize non-obvious insights

#### AI Prompt Strategy

```python
PROBLEM_DISCOVERY_PROMPT = """
You are an expert at identifying latent problems that people don't realize they have.

User Context: {user_input}

Analyze this information to discover:
1. Hidden inefficiencies the user has normalized
2. Friction points they work around without thinking
3. Opportunities for delight they're missing
4. Problems that don't have obvious names yet

For each problem:
- Describe it in a compelling way
- Explain why it's non-obvious
- Rate impact (1-10) and novelty (1-10)
- Suggest what category it falls into

Focus on problems that would make the user say "I never thought about it that way!"
"""
```

### 2.2 Solution Generation Engine

#### Architecture
```
Problem Input → Ideation Engine → Cross-Domain Mapper → Feasibility Filter → Solution Presenter
                      ↓
              Innovation Database
```

#### Process Flow

1. **Ideation Phase**
   - Generate 10-20 raw solution concepts
   - Use multiple creative techniques:
     - Analogical reasoning (how is this solved elsewhere?)
     - Inversion (what if we did the opposite?)
     - Combination (merge two unrelated ideas)
     - Elimination (what if we removed the constraint?)

2. **Cross-Domain Mapping**
   - Find solutions from other industries
   - Apply biological/natural analogies
   - Leverage emerging technologies

3. **Refinement**
   - Develop top 3-5 concepts in detail
   - Add implementation hints
   - Generate value propositions
   - Create "before/after" scenarios

4. **Presentation**
   - Craft compelling narratives
   - Generate visual representations
   - Explain the "aha" moment

#### AI Prompt Strategy

```python
SOLUTION_GENERATION_PROMPT = """
You are a visionary product designer and inventor.

Problem: {problem_description}
Context: {user_context}

Generate 5 innovative, non-obvious solutions that would make people say "Why didn't this exist before?"

For each solution:
1. Give it a catchy name
2. Describe how it works in simple terms
3. Explain why it's innovative (not just an obvious fix)
4. Describe the user experience transformation
5. Rate feasibility (1-10) and wow-factor (1-10)

Think like Apple, Dyson, or Tesla - make the solution elegant and surprising.
Use cross-domain inspiration and challenge assumptions.
"""
```

### 2.3 Visual Engine

#### Capabilities
- Generate UI mockups for digital solutions
- Create concept sketches for physical products
- Produce diagrams showing before/after workflows
- Design infographics explaining the innovation

#### Implementation Options

**Option A: AI Image Generation**
- Use DALL-E 3 or Stable Diffusion
- Generate from detailed text prompts
- Fast but less control

**Option B: Programmatic Generation**
- Use libraries like Excalidraw, Mermaid, or D3.js
- Template-based with AI-filled content
- More consistent, faster, cheaper

**Recommended**: Hybrid approach
- Use programmatic for diagrams and workflows
- Use AI generation for product mockups

### 2.4 Learning Engine

#### Feedback Loop
```
User Feedback → Rating Analysis → Pattern Learning → Model Update
                      ↓
              Feedback Database
```

#### Learning Mechanisms

1. **Problem Quality Learning**
   - Track which problems users find most insightful
   - Learn characteristics of "non-obvious" problems
   - Improve pattern detection

2. **Solution Effectiveness Learning**
   - Monitor which solutions get highest ratings
   - Identify successful creative techniques
   - Refine cross-domain mappings

3. **Personalization**
   - Learn user preferences and interests
   - Adapt communication style
   - Tailor problem domains

---

## 3. Data Models

### 3.1 Core Entities

#### User
```typescript
interface User {
  id: string;
  email: string;
  createdAt: Date;
  preferences: UserPreferences;
  innovationPortfolio: string[]; // IDs of saved ideas
}
```

#### Session
```typescript
interface Session {
  id: string;
  userId: string;
  startedAt: Date;
  context: UserContext;
  discoveredProblems: Problem[];
  generatedSolutions: Solution[];
}
```

#### Problem
```typescript
interface Problem {
  id: string;
  sessionId: string;
  title: string;
  description: string;
  category: ProblemCategory;
  impactScore: number; // 1-10
  noveltyScore: number; // 1-10
  reasoning: string;
  userFeedback?: Feedback;
  createdAt: Date;
}

enum ProblemCategory {
  PRODUCTIVITY = "productivity",
  DAILY_ROUTINE = "daily_routine",
  DIGITAL_EXPERIENCE = "digital_experience",
  PHYSICAL_SPACE = "physical_space",
  SOCIAL_INTERACTION = "social_interaction",
  HEALTH_WELLNESS = "health_wellness"
}
```

#### Solution
```typescript
interface Solution {
  id: string;
  problemId: string;
  name: string;
  description: string;
  howItWorks: string;
  whyInnovative: string;
  beforeAfter: {
    before: string;
    after: string;
  };
  feasibilityScore: number; // 1-10
  wowFactorScore: number; // 1-10
  visualAssets: VisualAsset[];
  userFeedback?: Feedback;
  createdAt: Date;
}
```

#### VisualAsset
```typescript
interface VisualAsset {
  id: string;
  type: "mockup" | "diagram" | "sketch" | "infographic";
  url: string;
  prompt: string;
  generatedAt: Date;
}
```

#### Feedback
```typescript
interface Feedback {
  rating: number; // 1-5 stars
  isNonObvious?: boolean;
  isInnovative?: boolean;
  comments?: string;
  submittedAt: Date;
}
```

---

## 4. User Experience Flow

### 4.1 Onboarding Flow

1. **Welcome Screen**
   - Explain the concept: "Discover problems you didn't know you had"
   - Show example transformations
   - CTA: "Start Discovering"

2. **Context Gathering**
   - Conversational prompts:
     - "Tell me about your typical day"
     - "What tasks do you do repeatedly?"
     - "What do you wish was easier?"
   - Optional: Upload images of workspace, screenshots, etc.

3. **Discovery Phase**
   - Show loading animation: "Analyzing your patterns..."
   - Stream discovered problems one by one
   - Each problem card shows:
     - Title
     - Description
     - Impact/Novelty scores
     - "Why this matters" explanation

### 4.2 Solution Exploration Flow

1. **Problem Selection**
   - User clicks on a problem card
   - Transition to solution generation

2. **Solution Generation**
   - Loading: "Inventing solutions..."
   - Stream solutions as they're generated
   - Each solution card shows:
     - Catchy name
     - One-line hook
     - Visual mockup
     - "Learn more" button

3. **Solution Deep Dive**
   - Expanded view with:
     - Full description
     - How it works
     - Before/after comparison
     - Feasibility assessment
     - Related solutions

4. **Feedback & Iteration**
   - Rate the solution (1-5 stars)
   - Mark as "Mind-blowing" or "Needs work"
   - Request variations: "Make it simpler" / "Make it bolder"
   - Save to portfolio

### 4.3 Portfolio View

- Grid of saved problems and solutions
- Filter by category, rating, date
- Share individual ideas (generate shareable link)
- Export as PDF presentation

---

## 5. Technical Implementation

### 5.1 Technology Stack

#### Frontend
- **Framework**: Next.js 14 (React 18)
- **Language**: TypeScript
- **Styling**: Tailwind CSS + Framer Motion (animations)
- **State Management**: Zustand or React Context
- **API Client**: TanStack Query (React Query)

#### Backend
- **Framework**: Python FastAPI (async support)
- **Language**: Python 3.11+
- **API Documentation**: Auto-generated OpenAPI/Swagger

#### AI/ML
- **LLM**: OpenAI GPT-4 or Anthropic Claude 3
- **Image Generation**: DALL-E 3 or Stable Diffusion XL
- **Embeddings**: OpenAI text-embedding-3-small
- **Vector Search**: Pinecone or Weaviate

#### Data Storage
- **Primary Database**: PostgreSQL (user data, sessions)
- **Vector Database**: Pinecone (semantic search)
- **Cache**: Redis (session cache, rate limiting)
- **File Storage**: AWS S3 or Cloudflare R2 (images)

#### Infrastructure
- **Hosting**: Vercel (frontend) + Railway/Render (backend)
- **Monitoring**: Sentry (errors) + PostHog (analytics)

### 5.2 API Design

#### Core Endpoints

```
POST   /api/sessions                    # Create new session
GET    /api/sessions/:id                # Get session details
POST   /api/sessions/:id/analyze        # Analyze user input
GET    /api/sessions/:id/problems       # List discovered problems
POST   /api/problems/:id/solutions      # Generate solutions
POST   /api/solutions/:id/feedback      # Submit feedback
GET    /api/portfolio                   # Get user's saved ideas
POST   /api/visuals/generate            # Generate visual asset
```

#### WebSocket Events

```
// Client → Server
{
  "type": "analyze_input",
  "data": { "text": "...", "context": {...} }
}

// Server → Client (streaming)
{
  "type": "problem_discovered",
  "data": { "problem": {...} }
}

{
  "type": "solution_generated",
  "data": { "solution": {...} }
}
```

### 5.3 AI Integration Architecture

```python
class ProblemDiscoveryEngine:
    def __init__(self, llm_client, vector_db):
        self.llm = llm_client
        self.vector_db = vector_db
    
    async def discover_problems(self, user_input: str, context: dict):
        # 1. Enrich context with similar past discoveries
        similar_contexts = await self.vector_db.search(user_input)
        
        # 2. Generate problems using LLM
        prompt = self._build_discovery_prompt(user_input, context, similar_contexts)
        response = await self.llm.generate(prompt, temperature=0.8)
        
        # 3. Parse and structure problems
        problems = self._parse_problems(response)
        
        # 4. Calculate scores and rank
        ranked_problems = self._rank_problems(problems)
        
        return ranked_problems
```

### 5.4 Prompt Engineering Strategy

#### Key Principles
1. **Few-shot learning**: Include 2-3 examples of great problems/solutions
2. **Structured output**: Request JSON format for easy parsing
3. **Temperature tuning**: Higher (0.8-0.9) for creativity, lower (0.3-0.5) for analysis
4. **Chain-of-thought**: Ask LLM to explain reasoning
5. **Iterative refinement**: Use multi-step prompts for complex tasks

#### Example Prompt Template

```python
DISCOVERY_PROMPT_TEMPLATE = """
You are an expert at discovering latent problems.

<examples>
{few_shot_examples}
</examples>

<user_context>
{user_input}
</user_context>

Discover 3-5 non-obvious problems. For each, provide:
- title: Short, catchy problem name
- description: 2-3 sentences explaining the problem
- category: One of {categories}
- impact_score: 1-10 (how much this affects quality of life)
- novelty_score: 1-10 (how non-obvious this problem is)
- reasoning: Why this is a latent problem worth solving

Output as JSON array.
"""
```

---

## 6. Visual Design System

### 6.1 Design Principles

1. **Clarity**: Complex ideas presented simply
2. **Delight**: Micro-interactions and smooth animations
3. **Focus**: Minimal UI, maximum content
4. **Inspiration**: Evoke curiosity and excitement

### 6.2 Color Palette

```css
/* Primary - Innovation Blue */
--primary-50: #eff6ff;
--primary-500: #3b82f6;
--primary-700: #1d4ed8;

/* Accent - Spark Orange */
--accent-400: #fb923c;
--accent-600: #ea580c;

/* Neutrals */
--gray-50: #f9fafb;
--gray-900: #111827;

/* Semantic */
--success: #10b981;
--warning: #f59e0b;
--error: #ef4444;
```

### 6.3 Typography

- **Headings**: Inter (bold, modern)
- **Body**: Inter (regular, readable)
- **Code/Technical**: JetBrains Mono

### 6.4 Component Library

- Problem Card (with hover effects)
- Solution Card (flip animation)
- Loading States (skeleton screens)
- Toast Notifications
- Modal Dialogs
- Rating Component (stars)
- Tag/Badge System

---

## 7. Demo Strategy

### 7.1 Demo Scenario

**Persona**: Sarah, a product manager

**Input**: "I spend my mornings checking Slack, email, Jira, and Notion to figure out what I need to do today. Then I have back-to-back meetings. I take notes in meetings but rarely look at them again."

**Expected Output**:

**Problem 1**: "The Morning Context Reconstruction Ritual"
- You're rebuilding your mental model of work every morning
- Impact: 8/10, Novelty: 9/10

**Solution**: "Overnight Context Compiler"
- AI that watches your work tools and creates a personalized morning briefing
- Shows: what changed, what needs attention, what can wait
- Visual: Mockup of a beautiful morning dashboard

**Problem 2**: "The Meeting Note Graveyard"
- Notes are write-only memory
- Impact: 7/10, Novelty: 8/10

**Solution**: "Living Notes"
- Notes that automatically resurface when relevant
- Appear in Slack when someone mentions the topic
- Visual: Diagram showing note → context → notification flow

### 7.2 Presentation Flow

1. **Hook** (30 seconds)
   - "How many problems do you have that you don't even know about?"
   - Show the Apple quote

2. **Demo** (3 minutes)
   - Live input from audience member
   - Stream problems in real-time
   - Generate solutions with visuals
   - Show "wow" reactions

3. **Vision** (1 minute)
   - This is the future of innovation
   - Everyone becomes an inventor
   - Problems are opportunities

4. **Q&A** (1 minute)

---

## 8. Risk Mitigation

### 8.1 Technical Risks

| Risk | Mitigation |
|------|------------|
| LLM API downtime during demo | Cache pre-generated responses for demo scenario |
| Slow response times | Implement streaming, show progress indicators |
| Poor quality outputs | Fine-tune prompts extensively, add quality filters |
| Cost overruns from API usage | Set rate limits, use caching aggressively |

### 8.2 Product Risks

| Risk | Mitigation |
|------|------------|
| Problems seem obvious | Extensive prompt engineering, add novelty filter |
| Solutions seem impractical | Balance feasibility with innovation in scoring |
| User doesn't understand value | Strong onboarding, clear examples |
| Demo doesn't resonate | Prepare multiple personas, practice extensively |

---

## 9. Success Metrics

### 9.1 Demo Success
- Audience engagement (applause, questions)
- Judge feedback scores
- Social media mentions
- Award placement

### 9.2 Product Success (if continued)
- % of problems rated "non-obvious"
- % of solutions rated "innovative"
- User retention (return visits)
- Ideas saved to portfolio
- Viral coefficient (shares)

---

## 10. Future Roadmap

### Phase 1: Post-Hackathon (Week 1-2)
- Polish UI/UX based on feedback
- Improve prompt quality
- Add more examples to knowledge base

### Phase 2: Beta Launch (Month 1-2)
- User authentication
- Portfolio persistence
- Sharing features
- Mobile responsive design

### Phase 3: Growth (Month 3-6)
- Community features (explore others' ideas)
- Collaboration tools
- Export to pitch deck
- Integration with productivity tools

### Phase 4: Monetization (Month 6+)
- Freemium model (limited discoveries/month)
- Pro features (unlimited, priority generation)
- Enterprise (team innovation workshops)
- API access for developers
