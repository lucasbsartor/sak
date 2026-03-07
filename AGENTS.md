# Development Guidelines

## Tooling

- **Prefer `bun`** over npm/yarn/pnpm for all commands.
- **Prefer Bun APIs** (`Bun.file`, `Bun.serve`, `bun:sqlite`, `Bun.$`) over Node.js equivalents.
- **Don't run dev/build commands** (`bun dev`, `bun build`) unless specifically requested.
- **Don't use `tsc`** — this project uses oxc for type checking and linting via `oxlint` (type-aware and type-check are enabled in `.oxlintrc.json`).
- **Imports and formatting** are handled by oxlint and oxfmt. After making changes, run `bun run fmt` to auto-fix.

## Commands

- Type check / lint: `bun run lint`
- Fix lint issues: `bun run lint:fix`
- Format: `bun run fmt`
- Check formatting: `bun run fmt:check`

## UI Components (shadcn/ui)

- **Check the docs**: Consult the shadcn/ui LLM index at https://ui.shadcn.com/llms.txt for available components.
- **Explore specific pages**: Follow links from the index to get accurate documentation on props and usage.
- **Install new components**: `bunx --bun shadcn@latest add <component>`

## Code Style

- **TypeScript**: Strict mode, ESNext target, bun-standard tsconfig.
- **Paradigm**: Functional over OOP. Prefer pure functions and plain objects.

## Engineering Principles

The default assumption is that **abstractions are wrong until proven necessary**.
Prioritize **KISS**, **YAGNI**, and **DRY** in that order.

### 1. KISS (Keep It Simple, Stupid)

- Prefer the simplest thing that could possibly work.
- **Functions over classes** — prefer standalone functions and data structures over classes, inheritance, or complex patterns.
- Avoid cleverness, layers, indirection, or abstractions unless they are obviously needed _right now_.

### 2. YAGNI (You Aren't Gonna Need It)

- Avoid adding features, hooks, extension points, config options, or "future-proofing" unless explicitly required by the current task.
- Avoid speculative abstractions.

### 3. DRY (Don't Repeat Yourself)

- **Duplication is cheaper than the wrong abstraction.**
- **The Rule of Three** (a guideline, not a law):
  - Consider extracting when the same logic exists in **3+ places** and a change would require editing all of them.
  - Two occurrences, or logic that changes independently, is fine to leave duplicated.

### General Guidelines

- **Colocation**: Keep logic close to where it's used.
- Write **straight-line code first**, refactor only when repetition or complexity actually appears.
- Prefer **copy-paste + edit** over creating generic helpers for one-off use.
- Prefer **explicit code over configurable code**.
- **Patterns to avoid** (common pitfalls):
  - "Manager", "Service", or "Handler" classes when a function would do.
  - Utility files or shared helpers with only 1–2 call sites.
  - Generic "Base" classes.
- If a feature fits in one screen of code, it's probably right. If it needs a diagram, it's probably over-engineered.

# UI/UX Design Direction

## Philosophy

The interface should feel like a well-engineered tool — precise, calm, and confident. Inspired by Linear, Vercel, Graphite, and Cursor: products built by engineers for engineers that respect the user's attention and never feel like they're trying too hard.

The core principle is **quiet density** — show everything that matters, hide nothing important, but never overwhelm. Every pixel should earn its place. The interface should feel inevitable — nothing missing, nothing unnecessary.

## Visual Language

### Typography as Structure

Typography does the heavy lifting instead of borders, backgrounds, or decorative elements. Establish hierarchy through font weight, size, and opacity rather than color or ornamentation.

- **Primary content**: Full opacity, medium weight — this is what the user came to see
- **Secondary/supporting content**: Reduced opacity (60-70%) — present but not competing
- **Tertiary/metadata**: Smallest size, lowest opacity — available on inspection, invisible at a glance
- **Headings**: Tight letter-spacing, semi-bold or medium weight — no need for large sizes when the weight contrast is clear
- Avoid uppercase labels unless they're tiny metadata tags — it reads as shouting

### Color with Intent

Color is a signal, not decoration. The default state of the interface is near-monochrome (grays, whites, subtle borders). Color appears only when it carries meaning:

- **Accent color**: Used sparingly for interactive elements, active states, and primary actions — one accent, used consistently
- **Semantic color**: Status indicators (success, warning, error, info) — these are functional, not aesthetic
- **Data color**: Subtle tints or pills on data elements (badges, tags, counts) — enough to differentiate without creating visual noise
- **No gratuitous gradients or colored backgrounds** — if a section needs visual separation, use spacing or a 1px border with low opacity

### Spacing and Density

Generous but purposeful. The interface should breathe without feeling empty.

- **Consistent rhythm**: Use a base unit (4px or 8px) and stick to it religiously — spacing should feel mathematical, not arbitrary
- **Group by proximity**: Related items sit close together, unrelated groups have clear gaps — this replaces the need for dividers and boxes
- **Density where it matters**: Data tables and lists can be tighter; forms and actions need room — match density to task intensity
- **Whitespace is not waste**: Empty space around key actions and data points draws the eye naturally

### Surfaces and Depth

Flat with minimal layering. Avoid stacked cards, heavy shadows, or deeply nested containers.

- **Subtle borders over shadows**: 1px borders at 5-10% opacity separate sections without creating visual weight
- **Minimal nesting**: If a card is inside a card inside a page, the hierarchy has failed — flatten it
- **Background differentiation**: Use slight background shifts (e.g., from white to gray-50) sparingly, for distinct zones like sidebars or code blocks
- **Modals and popovers**: Light shadow, crisp border — they float just enough to indicate layering without drama

### Motion and Interaction

Transitions should be fast and functional, never theatrical.

- **Duration**: 100-200ms for micro-interactions, 200-300ms for layout shifts — anything longer feels sluggish
- **Easing**: Ease-out for entrances, ease-in for exits — elements arrive confidently and leave quickly
- **Hover states**: Subtle background shift or opacity change — not a color explosion
- **Loading states**: Skeleton screens or subtle shimmer over spinners — maintain layout stability
- **Layout stability**: Avoid layout shifts. Reserve space for content that loads asynchronously. The interface should feel stable even while data is in flight.

## Interaction Philosophy

### Progressive Disclosure

Show the common path. Reveal complexity on demand.

- **Default view**: The actions and state the user needs 90% of the time — visible immediately
- **On interaction**: Advanced configuration, bulk operations, secondary metadata — available via expand, hover, or menu
- **Destructive actions**: Never prominent. Tucked behind confirmation or placed in overflow menus — accessible but never accidental

### Keyboard-First

Every core action should be reachable without a mouse. This is non-negotiable for engineering tools.

- Support a command palette (Cmd+K / Ctrl+K) for global navigation and actions
- Common actions get dedicated shortcuts — document them inline (visible in tooltips or beside menu items)
- Focus management should be predictable: tab order follows visual order, modals trap focus, escape closes things

### Discoverability Without Clutter

The interface should be learnable without a tutorial, but shouldn't plaster every option on screen.

- Tooltips on icon-only actions — always
- Contextual actions appear on hover or selection, not permanently
- Empty states explain what goes here and how to fill it — brief, helpful, not cute

### Icons

Icons are earned, not default.

- **Use icons for**: Compact action affordances (toolbar buttons), universally recognized metaphors (search, filter, close, settings), visual scanning aids in dense tables or lists
- **Don't use icons when**: A text label already communicates the action clearly, or the icon requires a tooltip to be understood — at that point, use the text instead
- **Never pair redundant icon + label** unless the context is a navigation rail or sidebar where both aid scanning

## Patterns to Follow

- **Tables and lists**: Clean rows, no zebra striping — use hover highlight instead. Align data types consistently (numbers right, text left). Subtle header with reduced opacity or smaller size.
- **Forms**: Labels above inputs, not beside. Inputs have minimal borders that strengthen on focus. Validation appears inline, not in alert banners.
- **Navigation**: Current state indicated by weight or subtle background, not bright color bars. Sidebar items are compact, scannable.
- **Empty states**: Brief, helpful, not cute. Tell the user what goes here and how to fill it — no mascot illustrations.
- **Actions**: Primary action is visually distinct (filled button, accent color). Secondary actions are ghost/outline. Destructive actions are muted until confirmed. Don't put 5 buttons where 2 will do.

## Patterns to Avoid

- Rounded-3xl on everything — subtle rounding (4-8px) is enough
- Color-coded everything — if every status has a unique background color, the page becomes a rainbow
- Excessive iconography — if a label reads "Settings," it doesn't also need a gear icon
- Decorative dividers and section borders — use spacing and typography instead
- Oversized padding that makes a simple list feel like a slideshow
- Skeleton UIs that are more complex than the actual content
- Toast notifications for every action — confirm destructive or async actions, not routine saves
- Layout shifts during loading — reserve space, use stable skeletons, never let content jump

## Implementation Notes

When building components, default to the restrained option. It's easier to add visual weight later than to strip it out. If a component feels too plain, first try adjusting spacing and typography before reaching for color or borders. Visual decoration should be the last resort.

The goal is an interface where the user's data and actions are the visual focus — the chrome disappears and the content speaks.
