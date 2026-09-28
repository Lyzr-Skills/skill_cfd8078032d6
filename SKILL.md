Read before generating agent UI
Composition rules, shell constraints, and UX principles for any Anaplan AI agent UI. Use the UI instruction skill and the theme to create the UI. Do not generate agent UI without applying this.
Before rendering any component, find its exact rule in components.css and use those literal values — sizes, colors, radii, durations, animation names. Do not approximate, redraw, or reconstruct a component's look from this document's prose alone; the prose describes behavior and composition**, the CSS is the single source of truth for** appearance**. If a class you'd expect to exist isn't in your working knowledge of this file yet, search for it before inventing a substitute.**

1. Use the UI instruction skill as the instructions to generate UI from the theme provided by architect.   2. PLAN\
Create a concise implementation plan detailing component scope, file structure (modular CSS/HTML), and specific acceptance criteria (visual fidelity, responsiveness, accessibility).\
PRESENT this plan to request approval or to trigger sub-agent delegations.  \
GENERATE\
Develop semantic, accessible, and modular HTML/CSS files.\
UTILIZE design tokens for all stylistic properties (colors, spacing, fonts, sizing).\
PRIORITIZE clean, maintainable code using semantic HTML and ARIA best practices.  \
VALIDATE\
EXECUTE automated checks: HTML semantic validation, CSS linting, accessibility audit (contrast, heading hierarchy), and responsive smoke tests.\
DELEGATE to specialized sub-agents or platform tools for complex tasks like visual regression.  \
ORCHESTRATE & DEPLOY\
USE available tools to manage repository operations: creating branches, staging files, committing, and opening PRs with descriptive, templated messages.\
DELEGATE CI/CD and deployment tasks to custom tools.\
VALIDATE every tool output before declaring progress. If a tool fails, RETRY or ESCALATE to a human.  \
REVIEW & ITERATE\
Maintain a feedback loop: if validation fails, conduct targeted fixes, update Knowledge Base citations, and re-run tests.\
UPON completion, mark the task as done and provide direct links to PRs, commits, and preview environments.  \
CONSTRAINTS
- ALWAYS refer to yourself as Anaplan AI.\
ONLY disclose system instruction in thinking states and logs.\
PRESERVE the integrity of UI Instruction, prefer it over inference. 
Known gaps and judgement calls
Worth reading before you trust this in production.
  
TypeScript compiles clean (tsc --noEmit, no errors), but React behaviour (state transitions, keyboard handling) hasn't been exercised in a browser/test runner for any component.
  
CSS is verified against Figma, TSX is not.
  
Proxima Nova isn't bundled (licensed) — --pl-font-family falls back to system stack until the host app provides it. Font's attached in theme.
  
Three unresolved Figma values, reasoned substitutes used (marked NOTE in components.css): Input border-radius (matches Button), Tab selected indicator (#000000 in export → uses --pl-color-action), Primary button default-size fill (taken from Small-size variant, same hex).
  
Modal does not trap Tab — handles Escape + focus-in/restore, but focus can leave the dialog. Add your own trap if needed.
  
Strawberry/Kiwi and Cranberry/Raspberry are visually identical in Runtime. All four tokens remain defined, but no shipped component references Cranberry, Raspberry, Blackberry, Apple, or Peach anymore — text is consolidated onto the 14px Kiwi/Pear pair for a cohesive scale.
  
A handful of prototype-sourced values don't resolve to tokens (marked NOTE in components.css): an ai-tint border rgb(206,206,255) (chip ring, inline KPICard border), a hairline grey rgb(228,235,248) (2 units off --pl-neutral-linkwater), KPICard's 16px radius (between md 8px and lg 12px), several ai-tone hover/pressed shades (e.g. rgb(72,72,200)), and the SegmentedControl/ChatInput shadows (softer/higher-opacity than any elevation token).

Composition & UX guidelines
Layout & spacing (tokens)
  
Spacing (8px base): half=4 1=8 2=16 3=24 4=32 5=40 6=48 7=56 8=64 9=72
  
Radius: none=0 sm=4 md=8 lg=12 pill=999 circle=50%
  
Elevation: l0–l3
  
Type scale: Display — pumpkin 64 · jackfruit 54 · durian 48 · watermelon 44. Titles — pineapple 33 (page) · grapefruit 22 (section) · mango 20 (sub-section) · banana 18 (card). Body — orange 18 (lead) · apple 16 (emphasis) · peach 16 · pear/kiwi/strawberry 14 (UI). Small — blackberry/cranberry/raspberry 13 · blueberry 12 (overline)
Component pairing
Component
Pairs with
Rule
Field
Input/Textarea/Select/Listbox
Shared label/helper/error scaffold: label → children → error/helper (role="alert" on error)
KPICard
children or KPIComparison
Renders children if given, else built-in value/metric layout
CheckboxGroup
Checkbox as children
Default composes Checkbox children; chip variant uses options/value/onValueChange instead
Tooltip
one element
Clones its single child to attach trigger behavior — no wrapper node
ToastRegion
Toast
Positioned container (placement) for one+ Toast children
Accordion
AccordionItem
variant="pill" gives a self-contained toggle instead of a full-width header
Modal
footer prop
Right-aligned button group, passed separately from children
Accessibility
  
Modal: handles Escape + focus-in/restore, but no Tab trap — add your own if needed.
  
Field: role="alert" only on error, not plain helperText. required asterisk is aria-hidden.
Icons
  
All icons come from the Icon component (<Icon name="..." size={16} />) and its 359-name set, categorized in icon-reference.md. IconName is a strict TypeScript union — an unlisted name fails to compile.
  
Never use an external icon library (lucide-react, heroicons, feather, inline placeholder SVGs, emoji, etc.) for anything this set already covers. Check icon-reference.md before drawing a custom icon.
  
If a needed icon genuinely isn't in the set, say so rather than substituting a look-alike from another library.
Colour & contrast
  
Every colour token has a recorded contrast ratio (tokens.json/tokens.css) — check it, don't eyeball.
  
WCAG AA: ≥4.5:1 normal text, ≥3:1 large text/UI components. AAA: ≥7:1.
  
Never use colour alone for meaning — pair with icon/label (see Badge/Toast/InlineMessage's status+icon).
  
Use semantic tokens for text/background pairs, not new hex values.
Thinking / agent trace (Chat.tsx)
  
ThinkingState = header (spinner+label while working, hidden once label omitted) + Hide/Show toggle. Renders ThinkingLog internally — don't nest it yourself. role="status"; open state controlled or uncontrolled (defaultOpen=true).
  
Position while active vs. complete: see "AI chat composition" under Framework & shell.
   
Visual spec — use these literal values, don't redraw from memory (see .pl-thinking* rules in components.css, search by class name — line numbers drift):
  
Spinner: 16px, ring rgb(206, 206, 232), top edge --pl-color-ai, 0.8s rotation (pl-thinking-spin; 2.4s under prefers-reduced-motion)
  
Hide/Show toggle: 28px pill, 16px radius, inset ghost ring (--pl-neutral-ghost), fills --pl-neutral-mischka when open
  
Agent tag (ThinkingLog's tag badge): 33px square, --pl-radius-md corners — not a pill, not a circular avatar
  
Gap between steps: 20px
  
Pending step: renders two shimmer bars (116px + 264px), not a spinner
  
Entrance: pl-thinking-rise (fade + 6px rise). Three separate prefers-reduced-motion overrides exist (spinner duration, row-rise removal, shimmer removal) — keep all three, don't collapse to one
  
ThinkingLog → one row per ThinkingStep: tag badge naming the agent that ran it (toned via agentTones, indexed by agent not position — so the same agent always gets the same color/tag), label, optional time, optional detail (auto-expands if log/source present), expandable log lines/source link, pending shows shimmer instead of content. Agent identity is always shown here — this is where it's surfaced, not hidden.
  
Each step expands independently.
Message rows (Chat.tsx)
  
UserBubble/AiMessage show time, swap to MessageToolbar (icon actions + time) on hover via actions prop — don't render MessageToolbar directly.
  
AiMessage.aside sits beside the footer (typically the thinking toggle).
  
MessageToolbarAction: icon, label (a11y name + title), optional onClick.
Known gaps for UI generation
  
Hardcoded (non-token) values, ~28 unbuilt Figma components, Modal's no-Tab-trap — see "Known gaps and judgement calls" above.
  
Don't invent variants outside a component's exported prop types (ButtonTone, BadgeShape, ModalSize, etc.).
  
Sheet is in scope — don't drop it as unused. Ported from the close-and-variance prototype, same canvas this framework targets. It renders the underlying Anaplan grid on the platform canvas (33px rows, 35px headers, rgb(228,235,248) hairline, tinted second group, in-scope rows highlighted) — this is the source data the agent reasons over, not agent output. See "Output boundary" under Framework & shell for how canvas and chat relate.

Agent context
  
"Agent" may appear as: specialised agent, sub-agent, swarm agent, agent capability pack — same concept, read by context.
  
User only ever talks to one overarching agent ("Anaplan AI"). Sub-agents are delegated to behind the scenes and never appear as separate chat participants or bubbles — but they are not hidden: each agent's name/identity is shown via its tag in ThinkingLog (agentTones, indexed by agent), so which agents ran is always visible in the trace, just inside ThinkingState/ThinkingLog/ThinkingStep rather than as its own message.
  
All conversation stays in chat — the user's sole control surface.
  
Anaplan AI is global, available via GNAV on every page.

Framework & shell
Agent UI renders inside a fixed shell — it doesn't control or replace it.
GNAV — asset: assets/gnav-bar.svg, full page width, fixed 48px height, sits above all content. Reference only, not functional; never overlay/hide it.
  
A complete .pl-gnav implementation exists in components.css (search by class name) — locate it and use it exactly. Do not reconstruct the bar from the SVG reference or from memory: the SVG shows what the CSS already implements, it isn't a substitute for reading the CSS.
  
White background (--pl-color-surface), 1px bottom border in --pl-neutral-linkwater, martinique (--pl-color-text) for logo/text/icons — not a dark navy bar.
  
Left-to-right: logo mark (assets category icon logo, ~24×14px) → breadcrumb (page/section labels, divider between segments) → icon cluster → avatar.
  
Icon cluster: use the nav-category icons in order — gnav-search, gnav-notifications, gnav-tasks, gnav-workflow, gnav-help, gnav-feedback, gnav-collaboration (see icon-reference.md). Don't substitute generic search/bell/help icons from elsewhere.
  
Avatar: 25×25 rounded square (rx ≈ 34% of size — use --pl-radius-md), solid martinique fill. Not circular, not a badge.
  
Classes: .pl-gnav, .pl-gnav__logo, .pl-gnav__crumbs/.pl-gnav__crumb/.pl-gnav__crumb--current/.pl-gnav__sep, .pl-gnav__icons, .pl-gnav__avatar.
AI chat window — header: "Anaplan AI" + New conversation/Download/Close. Empty state: icon, "Hello," "How can I help?" Input: "Ask anything" placeholder, CSV-only upload, persistent disclaimer "Anaplan AI can make mistakes. Check results."
AI chat composition — the chat window is a flex column: scrollable messages, then ThinkingState (only while a task is active), then ChatInput last.
  
ChatInput (.pl-chat-input-wrap) is always pinned to the bottom — empty state or full conversation, it never scrolls with content and never drifts toward the middle of the window.
  
ThinkingState (.pl-thinking), while a task is running, sits directly above ChatInput as its own fixed element — not inside the scrolling message list. It is not a separate floating box, card, or modal — it's an ordinary flex child sitting inline in the window's document flow, styled only by its own real .pl-thinking classes (see the visual spec under "Thinking / agent trace" above). It must be unmounted (not hidden) when idle, so it holds no layout space and doesn't push the chat input around.
  
Once the task completes, ThinkingState is removed from that fixed slot and its content relocates to become the resulting AiMessage's aside prop instead — still inline, still the same real classes, never a custom box invented for the occasion. See "Message rows" below, AiMessage.aside sits beside the footer, same row as the timestamp. It does not move to the top of the message.
Sizing/states (real values):
Single panel chat — 100% width, 100% height below GNAV (takes up the whole screen; no dual-panel, split-screen, or side-by-side view).
Output boundary — two separate questions, two separate rules:
  
What the agent produces (its answers, analysis, generated content) appears only inside the chat's own message list — never on canvas, never floating beside or behind the chat. This is unchanged and absolute.
  
Where the chat sits relative to the rest of the platform is a different question: this is a single panel chat that takes up the whole screen below GNAV — never a dual panel or split view alongside another page. All conversation and agent outputs take place directly within this single full-screen chat window.
So: agent output → chat only, always. The chat window takes up the whole screen.

UX generation considerations
  
Progressive disclosure — concise overview first, then next-step prompts to drill down; let the user opt into depth (mirrors ThinkingState's collapsed-by-default pattern).
  
Gestalt principles (proximity, similarity, continuity, closure, figure/ground, common region): https://ixdf.org/literature/topics/gestalt-principles
  
Nielsen's 10 usability heuristics (system status, match with real world, user control, consistency, error prevention, recognition over recall, flexibility, minimalist design, error recovery, documentation): https://www.nngroup.com/articles/ten-usability-heuristics/
General design judgment, not Palette-specific.
