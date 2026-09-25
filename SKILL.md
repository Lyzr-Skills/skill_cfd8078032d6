Lyzr Architect Theme & Design System — UI Generation Engine Specification
Target Audience: Automated UI Generation Tools, AI Coding Agents, and Frontend Builders Environment: Lyzr Architect / Lyzr Studio Agentic Workspace Status: Active / Authoritative

1. Executive Purpose & Scope
This specification defines the mandatory operational directives for the automated UI Generation Tool when consuming, parsing, and applying themes and design system assets managed within Lyzr Architect.
The core essence of this engine relies on grounding automated synthesis in foundational Gestalt Psychology Principles of visual perception and classic Usability Heuristics [1][2]. The design system and theme tokens provide the building blocks, while Gestalt laws and heuristics govern how the UI generation tool structures, groups, spaces, and reveals information.
Architectural Constraint (No Dual Panels): The agent conversational interface must live exclusively inside a responsive chat window. Dual-panel layouts, fixed split-screens, and permanent sidebar copilot columns are strictly prohibited. The main workspace canvas remains fluid and unobstructed, while the agent interacts through an adaptive, responsive chat window (floating/dockable widget on desktop, full-screen sheet on mobile).
Ad-hoc styling, hardcoded color values, arbitrary spacing units, and unstyled raw HTML elements are strictly prohibited. Every generated interface must simultaneously fulfill rigorous usability heuristics, preserve visual Gestalt cohesion, and adhere to tokenized design standards.

2. Theoretical Foundations: Gestalt Laws & Usability Heuristics
The UI generation tool must utilize Gestalt principles and usability heuristics as automated evaluation heuristics during layout and component synthesis.
2.1. Gestalt Principles of Visual Perception for UI Generation
Gestalt Principle
Engine Interpretation & Directives
Token & Component Implementation
Law of Proximity
Related items must be spaced closer together than unrelated items. Spatial gaps communicate grouping without requiring visual borders.
Use micro-spacing (var(--space-1) to var(--space-2)) for label-to-input or message-to-timestamp relationships; macro-spacing (var(--space-4) to var(--space-8)) between distinct cards and message turns.
Law of Similarity
Elements with identical visual attributes (color, shape, typography) are perceived as sharing a common function or status.
All user message bubbles share consistent surface tokens; all agent message bubbles share distinct neutral tokens; identical status badges communicate operational state.
Law of Common Region
Elements enclosed within an explicit visible boundary are perceived as a coherent unit with shared context.
Wrap the responsive chat window, analytical widgets, and metric cards inside bounded containers with --color-surface-card and --border-radius-md.
Law of Figure / Ground
The human eye separates focal visual elements (figure) from the underlying canvas (ground) through elevation and contrast.
The floating responsive chat window, dialogs, and popovers employ --elevation-flyout, surface overlays, and z-index elevation against the background --color-surface-canvas.
Law of Focal Point
High visual contrast or weight immediately directs user attention to the primary element of interest.
Apply prominent primary button tokens to the send/action trigger; secondary controls remain subtle to minimize cognitive friction.
Law of Continuity
Elements aligned along a continuous line or grid axis are perceived as belonging together, establishing natural reading paths.
Strict alignment along the 8px spatial grid, chronological thread flow in the chat window, and consistent baseline alignments across content cards.
2.2. Usability Heuristics for Automated UI Synthesis
The generation tool must enforce Nielsen's Usability Heuristics throughout generated layouts [1]:
	1	Visibility of System Status:
	◦	Every async action (e.g., agent generation, tool call execution, data save) must display an active loading indicator, token-styled spinner, or typing skeleton in the chat window. Never leave an unresponsive UI during calculation or retrieval.
	2	Match Between System and the Real World:
	◦	Follow familiar domain conventions, standard enterprise metaphors, and plain terminology instead of exposing internal agent stack traces or raw API schema identifiers [2].
	3	User Control and Freedom:
	◦	The responsive chat window must feature visible, accessible controls to minimize, expand, clear, or dismiss the conversation, backed by Esc key shortcuts [1].
	4	Consistency and Standards:
	◦	Standardized component patterns must be maintained across all synthesized pages [1][2]. Identical actions across different modules must look, behave, and position identically.
	5	Error Prevention & Recovery:
	◦	High-risk actions triggered through agent conversation (e.g., deleting models, running irreversible recalculations) require explicit affirmative confirmation prompts with distinct warning color tokens (--color-feedback-danger).
	6	Recognition Rather than Recall:
	◦	Provide contextual suggestion chips, recent prompt histories, and inline reference previews within the chat window so users do not have to recall complex prompt formats [1].
	7	Aesthetic and Minimalist Design:
	◦	Suppress gratuitous ornamentation. Every pixel, line, and color must convey semantic meaning or functional grouping, minimizing cognitive load for enterprise planners [2].

3. Design System Directory Architecture
The UI generation tool must inspect the workspace repository and align with the following directory structure:
├── .lyzr/
│   ├── architect.config.json          # Lyzr Architect runtime & agent bindings
│   └── theme-manifest.json            # Active theme metadata, tokens, and asset map
├── src/
│   ├── theme/
│   │   ├── tokens.json                # Master design tokens (W3C Design Token format)
│   │   ├── colors.css                 # CSS custom properties for palette & semantic states
│   │   ├── typography.css             # Font declarations, scales, and line heights
│   │   ├── spacing.css                # Spatial scale (4px/8px grid system based on Proximity)
│   │   └── elevation.css              # Box shadows & z-index (Figure/Ground separation)
│   ├── components/
│   │   ├── core/                      # Atomic primitives (Button, Input, Badge, Icon)
│   │   ├── composite/                 # Molecules (Card, Modal, FormField, Tooltip)
│   │   └── enterprise/                # Organisms (DataGrid, MetricTile, AgentChatWindow, NavHeader)
│   └── styles/
│       └── globals.css                # Global resets and CSS variable bindings
└── README.md                          # This instruction specification

4. Core Directives for the UI Generation Tool
4.1. Zero-Hardcoding Rule (Tokens Only)
	•	Never emit raw hex, RGB, HSL, or named colors. All color references must use established CSS variables (e.g., var(--color-primary-base), var(--color-surface-card)) or corresponding framework utility tokens (e.g., bg-primary, text-neutral-700).
	•	Never emit arbitrary dimension values. Margin, padding, gap, and dimension properties must map strictly to the spacing scale (var(--space-1) to var(--space-16) or p-2, gap-4).
	•	Typography constraints: Font families, font sizes, font weights, and line heights must use defined typographic tokens. Do not introduce arbitrary font-size: 17px or custom @font-face declarations.
4.2. Theme Token Resolution Reference Table
Category
Token Variable
Permitted Values / Mapping
Usage Context & Gestalt Mapping
Primary Brand
--color-primary-base<br>--color-primary-hover<br>--color-primary-subtle
#0073E6<br>#005BB5<br>#EBF4FE
Primary CTA buttons, send buttons, active tabs, link highlights (Focal Point)
Neutral Surface
--color-surface-canvas<br>--color-surface-card<br>--color-surface-overlay
#F8F9FA / #121212<br>#FFFFFF / #1E1E1E<br>#FFFFFF / #252525
Background canvas, chat window container, cards, modal dialogs (Common Region)
Typography
--font-family-sans<br>--font-size-sm<br>--font-size-base<br>--font-size-lg<br>--font-size-xl
Inter, system-ui<br>0.875rem (14px)<br>1.000rem (16px)<br>1.125rem (18px)<br>1.500rem (24px)
Data labels, chat bubbles, card headings, section titles (Visual Hierarchy)
Semantic Feedback
--color-feedback-success<br>--color-feedback-warning<br>--color-feedback-danger<br>--color-feedback-info
Green (#107C41)<br>Amber (#D83B01)<br>Red (#A80000)<br>Blue (#0073E6)
Badges, agent status dots, validation alerts, toast banners (Similarity & Feedback)
Elevation & Border
--border-radius-sm<br>--border-radius-md<br>--elevation-card<br>--elevation-flyout
4px<br>8px<br>0 1px 3px rgba(0,0,0,0.1)<br>0 8px 24px rgba(0,0,0,0.15)
Inputs, cards, buttons, chat window container, dropdown menus (Figure/Ground)

5. UI Generation Pipeline (Execution Steps)
The UI generation tool must execute generation requests through five sequential phases:
[Phase 1: Ingestion & Token Parsing]
               │
               ▼
[Phase 2: Intent Analysis & Component Matching]
               │
               ▼
[Phase 3: Structural Layout & Responsive Chat Architecture]
               │
               ▼
[Phase 4: State Machine & Interactive Styling]
               │
               ▼
[Phase 5: Automated Heuristic Audit & Emission]
Phase 1: Ingestion & Token Parsing
	1	Read .lyzr/theme-manifest.json and src/theme/tokens.json.
	2	Determine active theme modes (Light mode default, Dark mode via [data-theme="dark"] attribute).
	3	Cache the component registry from src/components/.
Phase 2: Intent Analysis & Component Matching
	1	Analyze user prompt or architectural spec to identify required UI primitives.
	2	Match requested widgets to available design system components:
	◦	Agent Conversational Interface / Assistant → src/components/enterprise/AgentChatWindow.tsx
	◦	KPI metrics / Variance card → src/components/enterprise/MetricTile.tsx
	◦	Interactive tables / Grids → src/components/enterprise/DataGrid.tsx
	◦	Input controls → src/components/core/InputField.tsx
	3	If an exact component does not exist, synthesize a composite following Section 6 (Extensibility & Fallbacks).
Phase 3: Structural Layout & Responsive Chat Architecture
	1	Single Canvas Workspace (No Dual Panels): The main application viewport is a unified, full-width workspace (e.g. dashboards, grids, scenario planners). The agent does not occupy a permanent vertical column or split pane.
	2	Responsive Chat Window Layout Rules:
	◦	Desktop (> 1200px): Floating or dockable overlay window anchored to bottom-right (width: 400px, max-height: 620px), with elevation shadow and collapse/minimize toggle. The main canvas remains completely visible and interactive.
	◦	Tablet (768px - 1199px): Floating adaptive popover card (width: 380px - 440px, max-height: 560px) accessible via a persistent floating action trigger.
	◦	Mobile (< 768px): Full-viewport overlay sheet (width: 100%, height: 100dvh) with prominent header back/close button and mobile-optimized virtual keyboard handling.
	3	Apply Proximity & Common Region: Group message threads chronologically within common card bubbles; separate user prompts and agent responses using similarity-based surface tinting.
Phase 4: State Machine & Interactive Styling
Every generated interactive element must support complete state definitions adhering to Heuristic #1 (Visibility of System Status):
	•	Default: Clean baseline token styling.
	•	Hover: Subtle tint/shade shift using --color-primary-hover or --color-surface-hover.
	•	Focus-Visible: Unambiguous 2px outline with --color-focus-ring and 2px offset.
	•	Active / Pressed: Physical depression or deeper shade accent.
	•	Disabled: 40% opacity, cursor: not-allowed, pointer-events suppressed.
	•	Loading / Streaming (Agent): Typing pulse indicator, streaming text chunks, or skeleton card during LLM token generation.
Phase 5: Automated Heuristic Audit & Emission
Before emitting code, the generation tool must execute a self-lint check:
	•	[ ] No Dual Panel Violation: Does the agent interface live strictly within a responsive chat window rather than a fixed dual panel or split view?
	•	[ ] Gestalt Check: Are related controls grouped by proximity and enclosed by common region cards?
	•	[ ] Heuristic Check: Is system status visible during transitions? Are destructive actions protected?
	•	[ ] Token Hygiene: No raw HEX colors present (# checks) and no inline pixel margins/paddings.
	•	[ ] Accessibility: Semantic HTML tags utilized (<header>, <main>, <section>, <form>), ARIA attributes present, and contrast ratio ≥ 4.5:1.

6. Extensibility & Fallback Guidelines
When user requests require UI elements not present in the pre-built component catalog:
	1	Compose, Do Not Invent: Build new complex widgets by assembling existing atomic primitives (Button, Card, InputField, Badge).
	2	Inherit Base Foundations: Use --color-surface-card for background, --border-radius-md for corners, and --elevation-card for surface depth.
	3	Typography Rhythm: Use --font-size-base for standard text and --font-size-sm with --color-text-secondary for metadata and timestamps.
	4	Export New Composites: Place newly generated reusable components into src/components/composite/ with complete TypeScript interfaces and prop typings.

7. Implementation Code Templates
7.1. Compliant Component Synthesis Example (Enterprise Card)
import React from 'react';

interface MetricVarianceCardProps {
  title: string;
  currentValue: string;
  targetValue: string;
  variancePct: number;
  isPositive: boolean;
  onDrilldown?: () => void;
}

export const MetricVarianceCard: React.FC<MetricVarianceCardProps> = ({
  title,
  currentValue,
  targetValue,
  variancePct,
  isPositive,
  onDrilldown,
}) => {
  return (
    <div className="lyzr-card rounded-md border border-[var(--color-border-subtle)] bg-[var(--color-surface-card)] p-4 shadow-[var(--elevation-card)] transition-shadow hover:shadow-[var(--elevation-flyout)]">
      {/* Law of Proximity: Header and Status chip closely paired */}
      <div className="flex items-center justify-between gap-2">
        <h4 className="text-sm font-medium text-[var(--color-text-secondary)]">
          {title}
        </h4>
        <span
          className={`inline-flex items-center px-2 py-0.5 rounded text-xs font-semibold ${
            isPositive
              ? 'bg-[var(--color-feedback-success-subtle)] text-[var(--color-feedback-success)]'
              : 'bg-[var(--color-feedback-danger-subtle)] text-[var(--color-feedback-danger)]'
          }`}
        >
          {isPositive ? '+' : ''}{variancePct}%
        </span>
      </div>

      {/* Law of Focal Point: Metric figure dominates primary visual weight */}
      <div className="mt-3 flex items-baseline justify-between">
        <div className="text-2xl font-bold tracking-tight text-[var(--color-text-primary)]">
          {currentValue}
        </div>
        <div className="text-xs text-[var(--color-text-tertiary)]">
          Target: {targetValue}
        </div>
      </div>

      {/* Heuristic 3 & 7: Minimalist actionable trigger with clear affordance */}
      {onDrilldown && (
        <button
          onClick={onDrilldown}
          className="mt-4 w-full rounded-sm bg-[var(--color-surface-subtle)] py-1.5 text-xs font-medium text-[var(--color-primary-base)] hover:bg-[var(--color-primary-subtle)] focus-visible:outline focus-visible:outline-2 focus-visible:outline-[var(--color-focus-ring)] transition-colors"
        >
          Analyze Variance Breakdown →
        </button>
      )}
    </div>
  );
};
7.2. Responsive Agent Chat Window Pattern (Floating / Mobile Adaptive)
import React, { useState } from 'react';

interface ChatMessage {
  id: string;
  sender: 'user' | 'agent';
  text: string;
  timestamp: string;
}

interface ResponsiveAgentChatWindowProps {
  agentName: string;
  status: 'idle' | 'thinking' | 'streaming' | 'error';
  messages: ChatMessage[];
  onSendMessage: (msg: string) => void;
  isOpen: boolean;
  onClose: () => void;
}

export const ResponsiveAgentChatWindow: React.FC<ResponsiveAgentChatWindowProps> = ({
  agentName,
  status,
  messages,
  onSendMessage,
  isOpen,
  onClose,
}) => {
  const [inputText, setInputText] = useState('');

  if (!isOpen) return null;

  const handleSend = (e: React.FormEvent) => {
    e.preventDefault();
    if (!inputText.trim()) return;
    onSendMessage(inputText);
    setInputText('');
  };

  return (
    <div 
      className="fixed z-50 flex flex-col bg-[var(--color-surface-card)] border border-[var(--color-border-subtle)] shadow-[var(--elevation-flyout)] transition-all duration-200 
        /* Mobile: full viewport takeover */
        bottom-0 right-0 w-full h-[100dvh] rounded-none
        /* Tablet & Desktop: floating bottom-right dockable chat window */
        sm:bottom-6 sm:right-6 sm:w-[420px] sm:h-[600px] sm:max-h-[85vh] sm:rounded-lg"
      role="dialog"
      aria-labelledby="agent-chat-title"
    >
      {/* Header: Visibility of System Status (Heuristic 1) & User Control (Heuristic 3) */}
      <header className="flex items-center justify-between border-b border-[var(--color-border-subtle)] px-4 py-3 bg-[var(--color-surface-subtle)] sm:rounded-t-lg">
        <div className="flex items-center gap-2">
          <span 
            className={`h-2.5 w-2.5 rounded-full ${
              status === 'thinking' || status === 'streaming'
                ? 'bg-[var(--color-feedback-warning)] animate-pulse'
                : status === 'error'
                ? 'bg-[var(--color-feedback-danger)]'
                : 'bg-[var(--color-feedback-success)]'
            }`} 
            aria-hidden="true"
          />
          <span id="agent-chat-title" className="text-sm font-semibold text-[var(--color-text-primary)]">
            {agentName}
          </span>
          <span className="text-xs text-[var(--color-text-tertiary)] font-normal capitalize">
            ({status})
          </span>
        </div>
        <div className="flex items-center gap-1">
          <button
            onClick={onClose}
            aria-label="Close chat window"
            className="rounded p-1 text-[var(--color-text-tertiary)] hover:bg-[var(--color-surface-hover)] hover:text-[var(--color-text-primary)] focus-visible:outline focus-visible:outline-2 focus-visible:outline-[var(--color-focus-ring)]"
          >
            ✕
          </button>
        </div>
      </header>

      {/* Message Thread (Law of Common Region & Similarity) */}
      <main className="flex-1 overflow-y-auto p-4 space-y-3 bg-[var(--color-surface-canvas)]">
        {messages.map((msg) => (
          <div 
            key={msg.id} 
            className={`flex flex-col ${msg.sender === 'user' ? 'items-end' : 'items-start'}`}
          >
            <div 
              className={`max-w-[85%] rounded-md px-3 py-2 text-sm leading-relaxed ${
                msg.sender === 'user'
                  ? 'bg-[var(--color-primary-base)] text-white shadow-sm'
                  : 'bg-[var(--color-surface-card)] text-[var(--color-text-primary)] border border-[var(--color-border-subtle)] shadow-sm'
              }`}
            >
              {msg.text}
            </div>
            <span className="mt-1 text-[10px] text-[var(--color-text-tertiary)] px-1">
              {msg.timestamp}
            </span>
          </div>
        ))}
        {status === 'thinking' && (
          <div className="flex items-center gap-2 text-xs text-[var(--color-text-tertiary)] p-2">
            <span className="animate-spin inline-block h-3 w-3 border-2 border-current border-t-transparent rounded-full" />
            Agent is analyzing your plan...
          </div>
        )}
      </main>

      {/* Input Composer: Focal Point for Action */}
      <footer className="p-3 border-t border-[var(--color-border-subtle)] bg-[var(--color-surface-card)] sm:rounded-b-lg">
        <form onSubmit={handleSend} className="flex items-center gap-2">
          <input
            type="text"
            value={inputText}
            onChange={(e) => setInputText(e.target.value)}
            placeholder={`Ask ${agentName}...`}
            className="flex-1 rounded-md border border-[var(--color-border-subtle)] bg-[var(--color-surface-canvas)] px-3 py-2 text-sm text-[var(--color-text-primary)] focus-visible:outline focus-visible:outline-2 focus-visible:outline-[var(--color-focus-ring)] placeholder:text-[var(--color-text-tertiary)]"
          />
          <button
            type="submit"
            disabled={!inputText.trim()}
            className="rounded-md bg-[var(--color-primary-base)] px-3 py-2 text-sm font-medium text-white hover:bg-[var(--color-primary-hover)] disabled:opacity-40 disabled:cursor-not-allowed focus-visible:outline focus-visible:outline-2 focus-visible:outline-[var(--color-focus-ring)] transition-colors"
          >
            Send
          </button>
        </form>
      </footer>
    </div>
  );
};

8. Quality Assurance Checklist for the Generator
Prior to finalizing any UI file emission, verify:
	1	Responsive Chat Window Architecture: Does the agent interface live strictly within a responsive chat window (floating/docked on desktop, full-viewport sheet on mobile) with zero dual-panel or fixed split-screen dependencies?
	2	Gestalt Alignment: Are components grouped by Proximity, bounded by Common Region, and differentiated by Figure/Ground elevation?
	3	Heuristic Compliance: Does every interactive flow satisfy system status visibility, provide user control/escape hatches, and prevent catastrophic errors?
	4	Token Hygiene: Are all colors, shadows, borders, and type tokens referenced through CSS variables or theme configuration?
	5	Dark Mode Resilience: Does switching data-theme="dark" on the root container dynamically re-theme all background surfaces, text colors, and borders without contrast degradation?
	6	Accessibility Standards: Are color contrasts ≥ 4.5:1? Do all buttons and inputs have accessible names and focus rings?

Sources
[1] Nielson's Usability Heuristics
[2] Design Principles and Values v1
