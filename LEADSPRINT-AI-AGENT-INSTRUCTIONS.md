# 🤖 LEADSPRINT AI AGENT - HEALTHCARE AUTOMATION INSTRUCTIONS

## 🔧 CRITICAL FIXES IMPLEMENTED

### ✅ ElevenLabs API Turn Timeout Fix
**ISSUE**: ElevenLabs API validation error - "Turn timeout must be -1 or between 1 and 300 seconds"
**SOLUTION**: Changed `turn_timeout: 10000` to `turn_timeout: 10` (API expects seconds, not milliseconds)
**LOCATION**: `autonomous-agent.js` line 720
```javascript
turn: {
  turn_timeout: 10  // FIXED: Was 10000 (milliseconds), now 10 (seconds)
}
```

---

## 🎤 VOICE AGENT SYSTEM PROMPT TEMPLATE

Use this template for all ElevenLabs voice agent creation. Replace the dynamic fields with actual practice data:

### 📋 **Voice Agent System Prompt Template**

```markdown
## 🏥 **[practice_name] Appointment Scheduler Prompt**

---

**You are the professional, friendly appointment scheduling assistant at [practice_name]. Your role is to efficiently and warmly help clients schedule their treatments while providing clear, detailed, and reassuring information about our services.**

---

### 🎯 **GENERAL INSTRUCTIONS**
- Ask only ONE clear question at a time
- Use friendly, natural, conversational language  
- Acknowledge customer responses before moving to the next question
- Never ask multiple questions in one message
- If the customer seems unsure, briefly explain the treatments to help them choose
- Always collect and confirm the customer's full name, phone number, and email address

---

### 📍 **LOCATION POLICY**
[practice_name] is located at:
**[practice_address]**

Confirm appointments at "[practice_name]" without asking about location preference.

---

### 🏥 **AVAILABLE TREATMENTS & QUESTION FLOWS**

**[practice_name] offers comprehensive services:**

---

#### 1️⃣ **[treatment_1]**
*[treatment_1_description]*

**If customer mentions [treatment_1]:**
- "Excellent choice! Our [treatment_1] treatments focus on natural-looking results. Which areas would you like to address?"
- **Follow-up:** "Have you had [treatment_1] before, or would this be your first visit?"

---

#### 2️⃣ **[treatment_2]**  
*[treatment_2_description]*

**If customer mentions [treatment_2]:**
- "Great choice! Our [treatment_2] treatments enhance your natural features. Which areas are you interested in?"
- **Follow-up:** "Have you experienced [treatment_2] before?"

---

#### 3️⃣ **[treatment_3]**
*[treatment_3_description]*

**If customer mentions [treatment_3]:**
- "Wonderful! Our [treatment_3] treatments promote healthy results. What specific concerns would you like to address?"
- **Follow-up:** "Is this your first professional [treatment_3] treatment?"

---

#### 4️⃣ **CONSULTATIONS**
*Comprehensive assessment and personalized treatment planning*

**If customer mentions consultation:**
- "Perfect choice! Our consultations at [practice_name] provide personalized treatment recommendations. Are you looking to address specific concerns or general health?"
- **Follow-up:** "Have you had a professional consultation before?"

---

### 🗓️ **APPOINTMENT SCHEDULING FLOW**

**✅ Always follow this professional progression:**

1. **Confirm treatment choice and specific details**
2. **Ask about prior experience (if relevant)**  
3. **Confirm preferred date and time**
   - If unsure, suggest 1-2 available options
4. **Collect customer details one at a time:**
   - Full name
   - Phone number  
   - Email address
5. **Repeat details for confirmation:**
   *"Just to confirm, I have your name as [Name], phone number as [Phone], and email as [Email]. Is that correct?"*
6. **Final appointment confirmation:**
   *"Your appointment for [Treatment] is scheduled for [Date/Time] at [practice_name]."*
7. **Professional closing:**
   *"You'll receive a confirmation email shortly. Thank you for choosing [practice_name], and we look forward to helping you achieve your goals!"*

---

### 🌟 **EXAMPLE GOOD RESPONSES**

✅ **Professional and helpful:**
- *"Thank you for calling [practice_name]! Which treatment interests you today?"*
- *"Excellent choice! Which areas would you like to focus on?"*
- *"Perfect! When would you like to schedule your appointment?"*
- *"May I have your full name to book your appointment?"*
- *"Your appointment for [treatment] is scheduled for Tuesday at 2 PM at [practice_name]."*

---

### 🚫 **AVOID THESE MISTAKES**
❌ *"What treatment and when and what's your number?"* (too many questions)
❌ *"Which location do you prefer?"* (only mention practice location)
❌ *"What do you want?"* (unprofessional)

---

### 💙 **TONE GUIDELINES**
- **Warm, professional, and caring**
- **Sound knowledgeable about treatments**  
- **Emphasize expertise and clinic environment**
- **Focus on enhancement and confidence**
- **Use terms like "goals," "enhancement," "natural results"**

---

### 🏥 **KEY MESSAGING POINTS**
- **Expert care:** Professional, experienced practitioners
- **Premium environment:** Professional, comfortable, modern clinic
- **Personalized care:** Customized treatment plans for individual needs
- **Serving:** Local area and surrounding regions
```

---

## 🔧 DYNAMIC FIELD REPLACEMENT

The LeadSprint agent automatically replaces these dynamic fields when creating voice agents:

### Required Dynamic Fields:
- `[practice_name]` - Practice/clinic name
- `[practice_address]` - Full practice address
- `[treatment_1]` - Primary treatment/service
- `[treatment_1_description]` - Description of primary treatment
- `[treatment_2]` - Secondary treatment/service  
- `[treatment_2_description]` - Description of secondary treatment
- `[treatment_3]` - Third treatment/service
- `[treatment_3_description]` - Description of third treatment

### Implementation Notes:
- **NO doctor names** - Only practice name and treatments
- **Treatments are extracted** from scraped website content
- **Address is scraped** from practice website
- **Template is applied** during ElevenLabs agent creation phase
- **Voice agent creation** happens in Phase 2 of the workflow

---

## 📝 AGENT CONFIGURATION

### ElevenLabs REST API Settings:
```javascript
conversation_config: {
  agent: {
    prompt: { prompt: systemPrompt },  // Uses template above
    first_message: firstMessage,
    language: "en"
  },
  tts: {
    voice_id: "21m00Tcm4TlvDq8ikWAM",
    model: "eleven_turbo_v2_5", 
    stability: 0.8,
    similarity_boost: 0.7,
    optimize_streaming_latency: 2
  },
  asr: {
    quality: "high"
  },
  turn: {
    turn_timeout: 10  // CRITICAL: Must be 10 seconds (not 10000ms)
  }
}
```

---

## 🚀 COMPLETE IMPLEMENTATION WORKFLOW

### 📋 DELTA CLINICS TEMPLATE - EXACT REPLICATION

The LeadSprint agent uses the **delta-clinics-demo** as the exact template. Only variable data changes per lead:
- **Practice name** (e.g., "Delta Clinics" → "Advanced Spine Care")
- **Treatments/Services** (extracted from scraped website)
- **Location/Address** (extracted from scraped website)

**IMPORTANT**: Doctor names are NO LONGER used - only practice name and treatments.

---

## 🏗️ PHASE 0: WEBSITE SCRAPING & DATA EXTRACTION

**Purpose**: Extract practice-specific data from healthcare website

**Implementation**: Uses EXA API search + direct web scraping
```javascript
const scrapedData = {
  practiceId: 'delta-clinics',           // URL-safe identifier
  company: 'Delta Clinics',              // Practice name
  location: '96 Harley Street, London', // Full address
  services: [
    'Cosmetic Surgery',
    'Non-Surgical Treatments', 
    'Aesthetic Procedures'
  ],
  phone: '+44 20 7xxx xxxx',            // If found
  email: 'info@deltaclinics.com'        // If found
};
```

---

## 🏗️ PHASE 1: ELEVENLABS VOICE AGENT CREATION

**Purpose**: Create voice agent with practice-specific prompts

**Template Application**: Dynamic field replacement in voice agent template
```javascript
// Replace template variables with scraped data
const systemPrompt = voiceAgentTemplate
  .replace(/\[practice_name\]/g, scrapedData.company)
  .replace(/\[practice_address\]/g, scrapedData.location)
  .replace(/\[treatment_1\]/g, scrapedData.services[0])
  .replace(/\[treatment_2\]/g, scrapedData.services[1])
  .replace(/\[treatment_3\]/g, scrapedData.services[2] || 'Consultation');
```

**ElevenLabs API Configuration**:
```javascript
conversation_config: {
  agent: {
    prompt: { prompt: systemPrompt },
    first_message: `Hi! Welcome to ${scrapedData.company}. I'm here to help you book your appointment. Which treatment interests you today?`,
    language: "en"
  },
  tts: {
    voice_id: "21m00Tcm4TlvDq8ikWAM",
    model: "eleven_turbo_v2_5", 
    stability: 0.8,
    similarity_boost: 0.7,
    optimize_streaming_latency: 2
  },
  asr: { quality: "high" },
  turn: { turn_timeout: 10 }  // CRITICAL: 10 seconds, NOT 10000ms
}
```

---

## 🏗️ PHASE 2: GITHUB REPOSITORY CREATION

**Purpose**: Create personalized healthcare demo repository

### 📁 EXACT REPOSITORY STRUCTURE (Delta Clinics Template)

```
{practice-id}-demo-{timestamp}/
├── package.json                    # Next.js project configuration
├── next.config.ts                  # Next.js build configuration  
├── tsconfig.json                   # TypeScript configuration
├── tailwind.config.js              # Tailwind CSS with healthcare theme
├── postcss.config.mjs              # PostCSS configuration
├── railway.toml                    # Railway deployment configuration
├── .env.local                      # Environment variables (local)
├── public/
│   └── (static assets)
└── src/
    ├── app/
    │   ├── layout.tsx              # Root layout with metadata
    │   ├── page.tsx                # Main demo page
    │   ├── globals.css             # Global styles
    │   └── api/
    │       └── chat/
    │           └── route.ts        # AI chat API endpoint
    └── lib/
        └── practice-config.ts      # Practice-specific configuration
```

### 📄 CRITICAL FILES - EXACT IMPLEMENTATION

#### 1. **package.json** - Next.js Healthcare Demo
```json
{
  "name": "{practice-id}-demo",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build", 
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "react": "19.0.0",
    "react-dom": "19.0.0",
    "next": "15.3.3",
    "lucide-react": "^0.263.1",
    "axios": "^1.9.0",
    "openai": "^4.75.1"
  },
  "devDependencies": {
    "typescript": "^5",
    "@types/node": "^20",
    "@types/react": "^18",
    "@types/react-dom": "^18",
    "autoprefixer": "^10.0.1",
    "postcss": "^8",
    "tailwindcss": "^3.4.0",
    "eslint": "^8",
    "eslint-config-next": "15.3.3"
  }
}
```

#### 2. **src/lib/practice-config.ts** - Practice Configuration
```typescript
export interface PracticeConfig {
  id: string;
  name: string;
  location: string;
  type: 'cosmetic' | 'chiropractic' | 'wellness' | 'beauty';
  chat: {
    assistantName: string;
    initialMessage: string;
    systemPrompt: string;
  };
  services: string[];
  primaryColor: string;
  tagline: string;
  elevenLabsAgentId: string;  // NEW: ElevenLabs voice agent integration
}

export const practiceConfigs: Record<string, PracticeConfig> = {
  '{practice-id}': {
    id: '{practice-id}',
    name: '{practice-name}',
    location: '{practice-address}',
    type: 'cosmetic',
    chat: {
      assistantName: '{practice-name} Assistant',
      initialMessage: 'Hello! How can I help you with your {treatment-1} needs today?',
      systemPrompt: 'You are a professional appointment scheduler for {practice-name}...'
    },
    services: ['{treatment-1}', '{treatment-2}', '{treatment-3}'],
    primaryColor: '#0066cc',
    tagline: 'Professional {treatment-1} Excellence',
    elevenLabsAgentId: '{elevenlabs-agent-id}'  // Linked to created voice agent
  }
};
```

#### 3. **src/app/page.tsx** - Main Demo Page (1:1 Copy)
- **Complete React component** with practice configuration
- **Dynamic theming** based on practice colors
- **Interactive voice and chat demos**
- **Responsive design** with Tailwind CSS
- **Practice-specific content** via configuration
- **🎤 ElevenLabs Voice Agent Integration**: Uses `config.elevenLabsAgentId` for seamless voice demo connection

#### 4. **src/app/layout.tsx** - Root Layout
```typescript
import { Metadata } from 'next';
import { practiceConfigs } from '@/lib/practice-config';

export async function generateMetadata(): Promise<Metadata> {
  const practiceId = process.env.NEXT_PUBLIC_PRACTICE_ID || '{practice-id}';
  const config = practiceConfigs[practiceId];
  
  return {
    title: `${config.name} - AI Voice Assistant Demo`,
    description: `Experience ${config.name}'s advanced AI voice assistant for ${config.services[0]} appointments.`,
    keywords: config.services.join(', '),
    openGraph: {
      title: `${config.name} - AI Voice Assistant`,
      description: `Book your ${config.services[0]} appointment at ${config.name}`,
      url: `https://{domain}`,
      siteName: config.name,
      locale: 'en_US',
      type: 'website'
    }
  };
}
```

#### 5. **src/app/api/chat/route.ts** - AI Chat API
- **OpenAI GPT-4o-mini integration**
- **Practice detection** via environment variables
- **Dynamic context switching** per practice
- **Robust error handling**

#### 6. **tailwind.config.js** - Healthcare Styling System
```javascript
module.exports = {
  content: [
    './src/pages/**/*.{js,ts,jsx,tsx,mdx}',
    './src/components/**/*.{js,ts,jsx,tsx,mdx}',
    './src/app/**/*.{js,ts,jsx,tsx,mdx}',
    './src/lib/**/*.{js,ts,jsx,tsx,mdx}'
  ],
  safelist: [
    // Healthcare color gradients
    'from-blue-500', 'to-blue-700', 'from-green-500', 'to-green-700',
    'from-purple-500', 'to-purple-700', 'from-pink-500', 'to-pink-700',
    // Comprehensive utility classes for dynamic generation
    'bg-blue-500', 'bg-green-500', 'bg-purple-500', 'bg-pink-500',
    'text-blue-600', 'text-green-600', 'text-purple-600', 'text-pink-600',
    'hover:bg-blue-700', 'hover:bg-green-700', 'hover:bg-purple-700'
  ],
  theme: {
    extend: {
      colors: {
        primary: '#0066cc',
        secondary: '#004499', 
        accent: '#0080ff',
        success: '#10b981',
        warning: '#f59e0b',
        error: '#ef4444'
      },
      fontFamily: {
        sans: ['var(--font-geist-sans)', 'Inter', 'system-ui'],
        mono: ['var(--font-geist-mono)', 'monospace']
      }
    }
  },
  plugins: []
};
```

### 🔧 REPOSITORY CREATION PROCESS

```javascript
async createPersonalizedRepository(practiceData, agentId) {
  const timestamp = Date.now();
  const repoName = `${practiceData.practiceId}-demo-${timestamp}`;
  
  // 1. Create GitHub repository
  const repoResponse = await axios.post('https://api.github.com/user/repos', {
    name: repoName,
    description: `Personalized healthcare demo for ${practiceData.company} - Auto-generated`,
    private: false,
    auto_init: true
  }, {
    headers: {
      'Authorization': `Bearer ${process.env.GITHUB_TOKEN}`,
      'Accept': 'application/vnd.github.v3+json'
    }
  });
  
  // 2. Clone and personalize with delta-clinics template
  const repoPath = `/tmp/${repository.name}`;
  const authenticatedUrl = repository.clone_url.replace('https://github.com/', 
    `https://${process.env.GITHUB_TOKEN}@github.com/`);
  execSync(`git clone ${authenticatedUrl} ${repoPath}`);
  
  // 3. Generate complete template (exact delta-clinics structure)
  await this.generateCompleteTemplate(repoPath, practiceData, agentId);
  
  // 4. Commit and push
  execSync(`cd ${repoPath} && git add .`);
  execSync(`cd ${repoPath} && git commit -m "🚀 Healthcare AI Voice Agent: ${practiceData.company}"`);
  execSync(`cd ${repoPath} && git push origin main`);
  
  return repository;
}
```

---

## 🏗️ PHASE 3: RAILWAY DEPLOYMENT

**Purpose**: Deploy healthcare demo to Railway with custom domain

### 🚂 RAILWAY DEPLOYMENT CONFIGURATION

#### 1. **railway.toml** - Deployment Settings
```toml
[build]
builder = "nixpacks"
nixpacksConfigPath = "nixpacks.toml"

[build.nixpacksConfig]
install = "npm install"
build = "npm run build"
start = "npm start"

[deploy]
startCommand = "npm start"
healthcheckPath = "/"
healthcheckTimeout = 100
restartPolicyType = "on_failure"
restartPolicyMaxRetries = 3

[[services]]
name = "web"
environment = "production"
port = 8080
```

### 🔧 RAILWAY MCP DEPLOYMENT PROCESS

```javascript
async deployToRailwayFromRepo(practiceData, repository) {
  // 1. Create Railway project using MCP
  const project = await this.railwayMCPCreateProject(`${practiceData.practiceId}-demo`);
  
  // 2. Get production environment
  const environments = await this.railwayMCPGetEnvironments(project.id);
  const prodEnv = environments.find(env => env.name === 'production');
  
  // 3. Create service from GitHub repository  
  const service = await this.railwayMCPCreateService(project.id, repository.full_name);
  
  // 4. Set environment variables
  const variables = {
    NEXT_PUBLIC_PRACTICE_ID: practiceData.practiceId,
    NEXT_PUBLIC_COMPANY_NAME: practiceData.company,
    NEXT_PUBLIC_PRACTICE_LOCATION: practiceData.location,
    NEXT_PUBLIC_ELEVENLABS_AGENT_ID: agentId,  // NEW: Connect voice agent to demo
    NODE_ENV: 'production'
  };
  await this.railwayMCPSetVariables(project.id, prodEnv.id, service.id, variables);
  
  // 5. Create custom domain
  const domain = await this.railwayMCPCreateDomain(project.id, prodEnv.id, service.id);
  
  return {
    url: `https://${domain.domain}`,
    status: 'deployed',
    projectId: project.id,
    serviceId: service.id,
    domain: domain.domain,
    repositoryUrl: repository.html_url
  };
}
```

### 📊 ENVIRONMENT VARIABLES (Railway)

**Critical Variables for Practice Personalization**:
```bash
NEXT_PUBLIC_PRACTICE_ID=delta-clinics
NEXT_PUBLIC_COMPANY_NAME=Delta Clinics
NEXT_PUBLIC_PRACTICE_LOCATION=96 Harley Street, London
NEXT_PUBLIC_ELEVENLABS_AGENT_ID=ag_xxxxx  # Links demo to voice agent
NODE_ENV=production
PORT=8080
```

---

## 🏗️ PHASE 4: NOTION DATABASE STORAGE

**Purpose**: Store complete lead data in Notion after successful deployment

### 📊 FINAL NOTION UPDATE

**Only after complete successful deployment**, create the Notion entry:

```javascript
async storeCompletedLead(practiceData, agentId, repository, deployment) {
  const finalLeadData = {
    'Practice Name': practiceData.company,
    'Location': practiceData.location,
    'Services': practiceData.services.join(', '),
    'Demo URL': deployment.url,
    'Repository': repository.html_url,
    'ElevenLabs Agent ID': agentId,
    'Railway Project': `https://railway.app/project/${deployment.projectId}`,
    'Status': 'Deployed',
    'Deployment Date': new Date().toISOString(),
    'Phone': practiceData.phone || '',
    'Email': practiceData.email || '',
    'Notes': '✅ Complete workflow executed successfully'
  };
  
  // Create Notion page only after everything is working
  await this.notionCreatePage(finalLeadData);
}
```

**Critical**: This phase only executes if all previous phases completed successfully.

---

## 🏗️ PHASE 5: FINAL INTEGRATION & TESTING

**Purpose**: Validate complete deployment and confirm functionality

### ✅ DEPLOYMENT VALIDATION

1. **Demo Site Accessibility**: Verify `https://{domain}` loads correctly
2. **Practice Personalization**: Confirm practice name, doctor, services display
3. **Voice Agent Integration**: Test ElevenLabs voice agent functionality
4. **Chat Functionality**: Validate OpenAI chat API responses
5. **Responsive Design**: Check mobile/desktop layouts

**Note**: Notion database storage is handled in Phase 4 after successful deployment validation.

---

## 🎯 AUTOMATION WORKFLOW EXECUTION

This complete workflow is triggered by:

**API Endpoint**: `POST /create-leads`
**Payload**: `{ "count": 1, "practice_url": "https://deltaclinics.com" }`

**Complete Execution**:
1. **Phase 0**: Scrape practice data → Extract name, services, location (**NO DOCTOR**)
2. **Phase 1**: Create voice agent → Apply template with scraped data → Return `agentId`
3. **Phase 2**: Generate repository → 1:1 delta-clinics template + voice agent ID
4. **Phase 3**: Deploy to Railway → MCP deployment + voice agent environment variable
5. **Phase 4**: Store in Notion → Create tracking entry with deployment results
6. **Phase 5**: Final validation → Confirm deployment + voice integration success

**Result**: Complete healthcare demo deployed at custom Railway domain with:
- ✅ Practice-specific branding and content
- ✅ Functional ElevenLabs voice agent
- ✅ AI chat powered by OpenAI
- ✅ Responsive design with practice colors
- ✅ SEO optimized metadata
- ✅ Professional healthcare UI/UX

**Template Guarantee**: Every deployment uses exact delta-clinics structure with only variable data changed (practice name, treatments, location). **NO DOCTOR NAMES** - only clinic name and treatments.

---

## 🎤 ELEVENLABS VOICE AGENT SEAMLESS INTEGRATION

### 🔗 **Voice Agent → Demo Site Connection**

The created ElevenLabs voice agent is seamlessly integrated into the demo site:

#### 1. **Agent ID Injection** 
```javascript
// During repository creation (Phase 3)
const practiceConfig = {
  id: practiceData.practiceId,
  name: practiceData.company,
  location: practiceData.location,
  services: practiceData.services,
  elevenLabsAgentId: agentId  // ← Voice agent ID from Phase 2
};

// Environment variable injection (Phase 4)
const variables = {
  NEXT_PUBLIC_ELEVENLABS_AGENT_ID: agentId  // ← Links demo to voice agent
};
```

#### 2. **Demo Page Voice Integration**
```javascript
// In src/app/page.tsx - Voice Demo Component
const VoiceDemo = () => {
  const config = practiceConfigs[practiceId];
  const agentId = config.elevenLabsAgentId;
  
  const startVoiceCall = () => {
    // Direct connection to created ElevenLabs agent
    window.ElevenLabs?.startConversation(agentId, {
      onConnect: () => console.log('Connected to voice agent'),
      onMessage: (message) => console.log('Voice message:', message)
    });
  };
  
  return (
    <button onClick={startVoiceCall} className="voice-demo-btn">
      🎤 Start Voice Demo with {config.name}
    </button>
  );
};
```

#### 3. **Automatic Configuration Flow**
1. **Phase 2**: Create ElevenLabs voice agent → Returns `agentId`
2. **Phase 3**: Generate practice config → Include `elevenLabsAgentId: agentId`
3. **Phase 4**: Set environment variable → `NEXT_PUBLIC_ELEVENLABS_AGENT_ID=agentId`
4. **Runtime**: Demo page → Load config → Connect to voice agent

#### 4. **Voice Agent Configuration Update**
```javascript
async generateCompleteTemplate(repoPath, practiceData, agentId) {
  // Generate practice-config.ts with voice agent ID
  const configContent = `
export const practiceConfigs = {
  '${practiceData.practiceId}': {
    id: '${practiceData.practiceId}',
    name: '${practiceData.company}',
    location: '${practiceData.location}',
    services: ${JSON.stringify(practiceData.services)},
    elevenLabsAgentId: '${agentId}',  // ← Critical integration
    primaryColor: '#0066cc',
    tagline: 'Professional ${practiceData.services[0]} Excellence'
  }
};`;
  
  await fs.writeFile(`${repoPath}/src/lib/practice-config.ts`, configContent);
}
```

### ✅ **Integration Verification**

The voice agent integration is verified by:
1. **Config File**: `src/lib/practice-config.ts` contains correct `elevenLabsAgentId`
2. **Environment Variable**: `NEXT_PUBLIC_ELEVENLABS_AGENT_ID` is set in Railway
3. **Demo Functionality**: Voice demo button connects to created agent
4. **Practice Context**: Voice agent uses practice-specific prompts and data

**Result**: Visitors to the demo site can immediately use the voice agent with practice-specific context and appointments scheduling functionality.

---

## 🚀 WORKFLOW INTEGRATION

**Template Integration Points**:

1. **Phase 0**: Website scraping extracts practice data
2. **Phase 1**: **Voice Agent Creation** ← Voice template applied here
3. **Phase 2**: GitHub repository creation ← Delta-clinics template applied here
4. **Phase 3**: Railway deployment ← Environment variables applied here  
5. **Phase 4**: Data stored in Notion database ← Final storage after deployment
6. **Phase 5**: Final validation and testing

The agent dynamically fills in all `[field_name]` placeholders with actual scraped data before sending to ElevenLabs API.
