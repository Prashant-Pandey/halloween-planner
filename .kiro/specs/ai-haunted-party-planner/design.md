# Design Document: AI Haunted Party Planner

## Overview

The AI Haunted Party Planner is a single-page web application built with HTML, Tailwind CSS, and vanilla JavaScript. The application uses a chat-based interface to guide users through party planning, leveraging an LLM API for natural language understanding and creative content generation. The design prioritizes simplicity, immediate usability, and a cohesive spooky aesthetic.

## Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────┐
│         Browser (Client)                │
│  ┌───────────────────────────────────┐  │
│  │   Single HTML Page                │  │
│  │  - Chat Interface Component       │  │
│  │  - Theme Selection Component      │  │
│  │  - Party Plan Display Component   │  │
│  │  - Animation Layer                │  │
│  └───────────────────────────────────┘  │
│              ↕                          │
│  ┌───────────────────────────────────┐  │
│  │   JavaScript Application Logic    │  │
│  │  - State Manager                  │  │
│  │  - LLM API Client                 │  │
│  │  - UI Controller                  │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
                ↕
    ┌───────────────────────┐
    │   LLM API Service     │
    │  (e.g., OpenAI API)   │
    └───────────────────────┘
```

### Technology Stack

- **HTML5**: Semantic markup structure
- **Tailwind CSS**: Utility-first styling with custom theme configuration
- **Vanilla JavaScript (ES6+)**: Application logic and state management
- **LLM API**: OpenAI API or compatible service for conversational AI
- **No build tools required**: Single-file deployment for simplicity

## Components and Interfaces

### 1. Chat Interface Component

**Purpose**: Primary interaction point for user-AI conversation

**Structure**:
```html
<div id="chat-container">
  <div id="chat-messages">
    <!-- Message bubbles rendered here -->
  </div>
  <div id="chat-input-area">
    <input type="text" id="user-input" />
    <button id="send-button">Send</button>
  </div>
</div>
```

**Behavior**:
- Auto-scrolls to latest message
- Displays user messages (right-aligned, distinct styling)
- Displays AI messages (left-aligned, spooky styling)
- Shows typing indicator during AI response generation
- Maintains message history in DOM

### 2. Theme Selection Component

**Purpose**: Interactive theme choice presentation

**Structure**:
```html
<div id="theme-options" class="hidden">
  <div class="theme-card" data-theme="theme-1">
    <h3 class="theme-title"></h3>
    <p class="theme-description"></p>
    <button class="select-theme-btn">Choose This Theme</button>
  </div>
  <div class="theme-card" data-theme="theme-2">
    <!-- Similar structure -->
  </div>
</div>
```

**Behavior**:
- Appears when AI presents theme options
- Animates in with spooky transition
- Handles both button clicks and typed theme names
- Disappears after selection, replaced by confirmation message

### 3. Party Plan Display Component

**Purpose**: Structured presentation of final party plan

**Structure**:
```html
<div id="party-plan" class="hidden">
  <h2>Your Haunted Party Plan</h2>
  <section id="decorations">
    <h3>🕯️ Decorations</h3>
    <ul></ul>
  </section>
  <section id="food-drinks">
    <h3>🍷 Food & Drinks</h3>
    <ul></ul>
  </section>
  <section id="music">
    <h3>🎵 Music & Atmosphere</h3>
    <p></p>
  </section>
  <section id="activities">
    <h3>🎭 Activities & Games</h3>
    <ul></ul>
  </section>
  <button id="copy-plan-btn">Copy Plan</button>
</div>
```

**Behavior**:
- Appears after theme finalization
- Positioned outside chat flow (fixed or separate section)
- Copy button uses Clipboard API
- Responsive layout adapts to screen size

### 4. State Manager

**Purpose**: Centralized application state management

**State Structure**:
```javascript
const appState = {
  conversationHistory: [],
  currentPhase: 'initial', // 'initial' | 'theme-selection' | 'refinement' | 'plan-generated'
  selectedTheme: null,
  partyPlan: null,
  isAIResponding: false
};
```

**Methods**:
- `addMessage(role, content)`: Adds message to history
- `setPhase(phase)`: Updates current interaction phase
- `setTheme(theme)`: Stores selected theme
- `setPlan(plan)`: Stores generated party plan
- `getConversationContext()`: Returns formatted history for LLM

### 5. LLM API Client

**Purpose**: Interface with LLM service

**Key Functions**:
```javascript
async function sendToLLM(userMessage) {
  // Constructs API request with conversation history
  // Includes system prompt for party planning context
  // Returns AI response
}

function buildSystemPrompt(phase) {
  // Returns phase-specific instructions for LLM
  // e.g., "Generate exactly 2 theme suggestions..."
}
```

**API Integration**:
- Uses fetch API for HTTP requests
- Handles streaming responses if supported
- Implements error handling and retry logic
- Manages API key securely (environment variable or user input)

### 6. UI Controller

**Purpose**: Orchestrates UI updates and user interactions

**Key Functions**:
```javascript
function renderMessage(role, content) {
  // Creates and appends message bubble to chat
}

function showThemeOptions(themes) {
  // Populates and displays theme selection component
}

function renderPartyPlan(plan) {
  // Populates party plan component with structured data
}

function showTypingIndicator() {
  // Displays animated "AI is typing..." indicator
}
```

## Data Models

### Message Object
```javascript
{
  role: 'user' | 'assistant' | 'system',
  content: string,
  timestamp: Date
}
```

### Theme Object
```javascript
{
  name: string,
  description: string
}
```

### Party Plan Object
```javascript
{
  theme: string,
  decorations: string[],      // 3-5 items
  food: string[],             // 2-3 items
  drinks: string[],           // 1-2 items
  music: string,              // Single suggestion
  activities: string[]        // 2 items
}
```

## Visual Design System

### Color Palette
```css
:root {
  --color-bg-primary: #0a0a0a;        /* Deep black */
  --color-bg-secondary: #1a0f2e;      /* Deep purple */
  --color-accent-orange: #ff6b35;     /* Glowing orange */
  --color-accent-green: #39ff14;      /* Toxic green */
  --color-accent-red: #8b0000;        /* Dark red */
  --color-text-primary: #e0e0e0;      /* Light gray */
  --color-text-secondary: #a0a0a0;    /* Medium gray */
}
```

### Typography
- **Primary Font**: 'Creepster' or 'Nosifer' (Google Fonts) for headers
- **Body Font**: 'Roboto' or system font for readability
- **Font Sizes**: Responsive scale using Tailwind's default sizing

### Animations

**Flickering Candle Effect** (Chat Container):
```css
@keyframes flicker {
  0%, 100% { box-shadow: 0 0 20px rgba(255, 107, 53, 0.5); }
  50% { box-shadow: 0 0 30px rgba(255, 107, 53, 0.8); }
}
```

**Bubbling Cauldron** (Typing Indicator):
```css
@keyframes bubble {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-5px); }
}
```

**Button Hover** (Interactive Elements):
```css
.spooky-button:hover {
  transform: scale(1.05);
  box-shadow: 0 0 15px var(--color-accent-green);
  transition: all 0.3s ease;
}
```

## Conversation Flow

### Phase 1: Initial Greeting
1. App loads → Display welcome message
2. User provides initial input
3. LLM processes input → Generates 2 themes

### Phase 2: Theme Selection
1. Display theme options with buttons
2. User selects theme (button or typed)
3. Acknowledge selection
4. Allow refinements or proceed to planning

### Phase 3: Refinement (Optional Loop)
1. User provides refinement request
2. LLM adapts theme based on refinement
3. Confirm changes
4. Return to refinement or proceed to planning

### Phase 4: Plan Generation
1. User confirms theme is finalized
2. LLM generates detailed party plan
3. Parse response into structured data
4. Display in party plan component

### LLM Prompt Strategy

**System Prompt Template**:
```
You are a spooky AI party planner helping users plan Halloween-themed parties.
Current Phase: {phase}
Instructions: {phase-specific-instructions}
Maintain a fun, slightly spooky tone. Be creative and specific.
```

**Phase-Specific Instructions**:
- **Initial**: "Ask about guest count, age group, and desired vibe. Then suggest exactly 2 distinct themes."
- **Theme Selection**: "Present themes with names and 2-3 sentence descriptions."
- **Refinement**: "Acknowledge the refinement and explain how you'll adjust the theme."
- **Planning**: "Generate a complete party plan with: 3-5 decorations, 2-3 foods, 1-2 drinks (non-alcoholic), music suggestion, 2 activities."

## Error Handling

### API Errors
- **Network Failure**: Display "The spirits are restless... connection lost" message
- **Rate Limiting**: Queue requests and show "The cauldron is bubbling... please wait"
- **Invalid Response**: Retry with simplified prompt or show fallback message

### User Input Validation
- **Empty Messages**: Disable send button when input is empty
- **API Key Missing**: Show configuration prompt on first load

### Graceful Degradation
- If animations cause performance issues, provide reduced-motion mode
- If LLM unavailable, show error state with retry option

## Testing Strategy

### Manual Testing Checklist
1. **Chat Flow**: Complete full conversation from greeting to plan generation
2. **Theme Selection**: Test both button clicks and typed theme names
3. **Refinement**: Test multiple refinement requests in sequence
4. **Responsive Design**: Test on mobile (375px), tablet (768px), desktop (1440px)
5. **Copy Functionality**: Verify clipboard API works across browsers
6. **Animations**: Verify animations don't interfere with usability

### Browser Compatibility
- Target: Modern browsers (Chrome, Firefox, Safari, Edge)
- Minimum: ES6 support, Fetch API, Clipboard API

### Performance Considerations
- Limit conversation history sent to LLM (last 10 messages)
- Debounce user input to prevent excessive API calls
- Lazy-load animations to reduce initial page weight

## Deployment

### Single-File Structure
```
index.html (contains inline CSS and JavaScript)
```

### Configuration
- API key: Prompt user on first load or use environment variable
- API endpoint: Configurable constant in JavaScript

### Hosting
- Static hosting (GitHub Pages, Netlify, Vercel)
- No server-side requirements
- HTTPS required for Clipboard API

## Future Enhancements (Out of Scope)

- Save/load party plans to localStorage
- Share plans via URL
- Multiple theme selection and comparison
- Image generation for decorations
- Shopping list generation with price estimates
