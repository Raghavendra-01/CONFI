# 🕯️ CONFI
### *Anonymous Confession Platform — Where Secrets Find Their Voice*

> *"The truth you can't say out loud deserves a place to breathe."*

---

## 🌑 What is CONFI?

**CONFI** is a dual-experience anonymous confession platform — built for the brave, the broken, the curious, and the honest. It lives in two worlds simultaneously:

- 📱 **A mobile app** — your pocket-sized confessional booth, available at 3 AM when the truth feels too heavy
- 🌐 **A 3D interactive web simulation** — a living, breathing galaxy of secrets rendered in real-time, where each confession becomes a glowing orb drifting through a digital cosmos

No names. No faces. No judgment. Just raw, unfiltered human truth.

---

## ✨ Features That Set CONFI Apart

### 🔮 The Confession Universe (3D Web Simulation)
```
Every secret you submit becomes a particle of light.
Every reaction warps the gravity of the universe.
You are anonymous. Your truth is eternal.
```

- **Real-time 3D particle system** — confessions rendered as luminous orbs using Three.js
- **Gravity-based clustering** — similar confessions orbit each other based on emotional AI tagging
- **Interactive exploration** — click any orb to read, react, or send a whisper back
- **Pulse animations** — confessions throb with life when they receive empathy reactions
- **Time-warp mode** — watch the last 24h of confessions burst into existence like a cosmic time-lapse

### 📱 Mobile App (iOS & Android)
- **One-tap anonymous posting** — zero sign-up, zero friction, zero identity
- **Mood-based filters** — explore by emotion: heartbreak, joy, guilt, rage, laughter
- **Whisper threads** — anonymous reply chains where conversations bloom without identities
- **Safe mode** — AI-powered content moderation that protects without censoring
- **Confession reminders** — gentle nudges when the app senses you've been reading but not releasing (opt-in only)
- **Local confession map** — see what people near you are feeling, with location blurred to city-level only

### 🛡️ Privacy Architecture
```
Your secret goes in.
A cryptographic hash comes out.
Even we can't trace it back to you.
```

- **Zero-knowledge identity model** — we don't collect emails, names, or persistent IDs
- **Session-only tokens** — your device generates a temporary key that expires when you close the app
- **End-to-end encrypted whispers** — even our servers can't read reply threads
- **IP scrubbing** — all server logs strip IP addresses before writing to disk
- **No analytics SDKs** — no Firebase, no Mixpanel, no invisible trackers

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                      CONFI PLATFORM                     │
├─────────────────────┬───────────────────────────────────┤
│   📱 Mobile App     │   🌐 3D Web Simulation           │
│   React Native      │   Three.js + WebGL                │
│   Expo SDK          │   React Three Fiber               │
│   Zustand State     │   Socket.io (live feed)           │
└────────┬────────────┴────────────────┬────────────────-─┘
         │                             │
         ▼                             ▼
┌─────────────────────────────────────────────────────────┐
│                    API GATEWAY (GraphQL)                │
│                    Anonymous Auth Layer                 │
│                    Rate Limiting + Abuse Detection      │
└───────────────────────────┬─────────────────────────────┘
                             │
         ┌───────────────────┼────────────────────┐
         ▼                   ▼                    ▼
   ┌───────────┐    ┌────────────────┐    ┌─────────────┐
   │ Confession│    │ Emotion Engine │    │  Moderation │
   │   Store   │    │  (AI tagging)  │    │   Service   │
   │ PostgreSQL│    │   Python ML    │    │   + Queue   │
   └───────────┘    └────────────────┘    └─────────────┘
```

---

## 🚀 Getting Started

### Prerequisites
```bash
node >= 18.0.0
python >= 3.10
docker + docker-compose
expo-cli (for mobile)
```

### Quick Start

```bash
# Clone the universe
git clone https://github.com/Raghavendra-01/CONFI.git
cd CONFI

# Install the silence (dependencies)
npm install

# Configure your secrets (ironic, we know)
cp .env.example .env
# → Fill in your DB connection, AI API keys, etc.

# Launch the cosmos
docker-compose up -d          # Spins up PostgreSQL + Redis
npm run dev:api               # GraphQL API on :4000
npm run dev:web               # 3D web experience on :3000

# Mobile (separate terminal)
cd mobile
expo start
```

### Environment Variables
```env
# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/confi

# Security
JWT_SECRET=something_deeply_unknowable
ENCRYPTION_KEY=32_char_key_here

# AI / Moderation
OPENAI_API_KEY=your_key_here
PERSPECTIVE_API_KEY=your_key_here

# 3D Simulation
NEXT_PUBLIC_SOCKET_URL=ws://localhost:4001
NEXT_PUBLIC_MAX_ORBS=500
```

---

## 📁 Project Structure

```
CONFI/
├── 📱 mobile/                    # React Native (Expo)
│   ├── app/                      # File-based routing
│   │   ├── index.tsx             # Feed screen
│   │   ├── confess.tsx           # Confession composer
│   │   └── explore/              # Category explorer
│   ├── components/               # Reusable UI
│   └── stores/                   # Zustand state
│
├── 🌐 web/                       # Next.js 14
│   ├── app/                      # App router
│   ├── components/
│   │   ├── Universe/             # Three.js 3D scene
│   │   │   ├── ConfessionOrb.tsx # Individual orb component
│   │   │   ├── GravitySystem.tsx # Physics engine wrapper
│   │   │   └── StarField.tsx     # Background cosmos
│   │   └── ui/                   # Interface overlays
│   └── shaders/                  # GLSL shaders for orb glow
│
├── ⚙️  api/                       # GraphQL + REST backend
│   ├── schema/                   # GraphQL schema definitions
│   ├── resolvers/                # Confession resolvers
│   ├── middleware/               # Auth, rate limiting, scrubbing
│   └── services/
│       ├── emotion-engine/       # ML-based mood classification
│       └── moderation/           # Safety layer
│
└── 🐳 docker-compose.yml
```

---

## 🎨 The 3D Simulation — Technical Deep Dive

The web experience is built around a **custom Three.js particle system** where each confession is mapped to:

| Property | Meaning |
|---|---|
| **Orb size** | Age of confession (newer = smaller, grows over time) |
| **Orb color** | Emotional category (blue = sad, amber = hopeful, red = anger...) |
| **Orbit speed** | Engagement velocity (reactions per minute) |
| **Brightness** | Empathy score (more whispers = more light) |
| **Drift direction** | Semantic similarity to nearby confessions |

```typescript
// Example: How a confession becomes an orb
const confession = await createConfessionOrb({
  text: encryptedContent,
  emotion: await classifyEmotion(text),   // 'grief' | 'joy' | 'rage' | ...
  intensity: sentimentScore,              // 0.0 → 1.0
  position: generateOrbitalPosition(),   // Based on emotion cluster
})
```

---

## 🤖 Emotion Engine

CONFI uses a fine-tuned emotion classification model to:

1. **Tag** confessions with 12 primary emotional categories
2. **Cluster** similar confessions into gravitational zones in the 3D space
3. **Detect** crisis language and surface mental health resources (non-intrusively)
4. **Prevent** re-identification through linguistic pattern matching

```python
# Simplified emotion classification pipeline
emotions = [
    "grief", "guilt", "shame", "joy", "love",
    "rage", "fear", "loneliness", "relief", "pride",
    "regret", "hope"
]
```

---

## 🔐 Security Model

CONFI operates on a **Radical Anonymity** principle:

```
Traditional Apps:   User → Account → Content
CONFI:              Device → Session Token (ephemeral) → Content
                              ↑
                    Discarded after session ends.
                    Never stored. Never logged.
```

**What we collect:** The confession text. The emotional tag. A timestamp. That's it.

**What we never collect:** IP address (scrubbed), Device ID, Location (unless explicitly shared, blurred to city), User Agent (stripped), Behavioral analytics.

---

## 🌍 Roadmap

```
✅ Phase 1 — The Void         Core confession + anonymous feed
✅ Phase 2 — The Universe     3D web simulation goes live
🔄 Phase 3 — The Echo         Whisper threads + anonymous replies
📅 Phase 4 — The Mirror       AI-powered reflection (journaling mode)
📅 Phase 5 — The Bridge       Mental health resource integration
📅 Phase 6 — The Archive      Time capsule confessions (open in 1 year)
```

---

## 🧠 Contributing

CONFI is open to contributors who believe in **privacy as a human right** and **truth as a healing force**.

```bash
# Before your first PR, read our principles:
cat CONTRIBUTING.md | grep "privacy first"

# Run tests before pushing
npm run test:all
npm run audit:privacy     # Custom check for accidental data collection
```

**We especially need:**
- 🎨 GLSL shader writers (make those orbs more beautiful)
- 🔒 Security researchers (find our blind spots — responsibly)
- 🌐 Translators (anonymity speaks every language)
- 🧠 ML engineers (emotion engine improvements)

---

## ⚖️ Ethics

CONFI was built with uncomfortable questions in mind:

*What if someone confesses to a crime?* → We have a legal compliance framework that balances anonymity with public safety thresholds.

*What if someone is in crisis?* → Our emotion engine surfaces crisis resources without breaking anonymity or alerting anyone.

*What if it becomes a hate platform?* → Moderation runs on every post before it enters the universe. Hate has no orbit here.

---

## 📜 License

MIT — because secrets shouldn't be locked behind paywalls.

---

## 💬 The Last Word

> *CONFI exists because humans need somewhere to put things down.*
> *Not everything needs a name attached.*
> *Some truths just need to be released into the dark,*
> *and know that somewhere out there,*
> *someone else's orb is drifting close to yours.*

---

<p align="center">
  Built with silence, caffeine, and the belief that honesty sets you free.<br/>
  <strong>Raghavendra-01/CONFI</strong> · MIT License · No data collected · No apologies
</p>
