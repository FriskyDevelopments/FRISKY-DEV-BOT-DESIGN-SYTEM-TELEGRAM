# ⬡ Frisky Design System — Telegram Bot UI

**Deep Tech / Cyber** — Canonical UI tokens, components, and brand guidelines for the Frisky Developments ecosystem.

> Canonical design language powering ClipsFlow, HostCasa, MyFenrir, Frisky Mega Factory, and all Frisky Telegram bots.

---

## Overview

The Frisky Design System defines the visual language for all Frisky Developments products — Telegram bots, mini-apps, web portals, and SaaS dashboards. It provides a centralized source of truth for design tokens, reusable components, and deployment configuration.

### Ecosystem Products Using This System

| Product | Type | Surface |
|---------|------|---------|
| **ClipsFlow** | Media link resolver | Telegram bot + mini-app + web portal |
| **HostCasa** | B2B hospitality SaaS | Web portal |
| **MyFenrir** | Community access gates | Web + Telegram |
| **Frisky Mega Factory** | Job dispatch system | Telegram bot |
| **FriskyClaw / OpenClaw** | Bot framework | Telegram |

---

## Design Tokens

All tokens are defined in two formats:

| File | Format | Purpose |
|------|--------|---------|
| [`tokens.json`](tokens.json) | JSON | Machine-consumable token spec — import into any framework |
| [`public/css/tokens.css`](public/css/tokens.css) | CSS custom properties | Drop-in stylesheet for web and mini-app surfaces |

### Canonical Palette

| Token | Value | Usage |
|-------|-------|-------|
| `--fds-bg` | `#050505` | Primary background |
| `--fds-bg-glass` | `rgba(255,255,255,0.05)` | Glass/surface backgrounds |
| `--fds-bg-card` | `rgba(255,255,255,0.03)` | Card surfaces |
| `--fds-text-primary` | `#ffffff` | H1/H2 headings |
| `--fds-text-body` | `#cbd5e1` | Body text (slate-300) |
| `--fds-text-technical` | `#34d399` | Monospace technical labels (emerald-400) |
| `--fds-accent-gradient` | `#c084fc → #34d399` | Primary accent (purple-400 → emerald-400) |
| `--fds-border` | `rgba(255,255,255,0.1)` | Default borders |

### Typography

| Token | Value |
|-------|-------|
| `--fds-font-sans` | `'Inter', 'Geist', 'Space Grotesk', system-ui, sans-serif` |
| `--fds-font-mono` | `'JetBrains Mono', 'Fira Code', 'SF Mono', monospace` |

H1: 2.25rem / 700 weight / -0.02em tracking — pure white  
H2: 1.5rem / 600 weight / -0.01em tracking — pure white  
Body: 0.875rem / 400 weight — slate-300  
Technical: 0.75rem / 500 weight / 0.15em tracking / uppercase — emerald-400 monospace

### Effects

- **Glassmorphism**: `backdrop-filter: blur(24px)`, `bg-white/5` or `bg-black/40`, `border-white/10`
- **Glow**: `0 0 30px rgba(192, 132, 252, 0.15)` (premium), `0 0 15px rgba(192, 132, 252, 0.08)` (subtle)

### Spacing

Based on a 4px unit: xs=4px, sm=8px, md=16px, lg=24px, xl=32px, xxl=40px.

---

## Components

### Buttons

| Variant | Style |
|---------|-------|
| **Primary** | Accent gradient background + premium glow + white text |
| **Secondary** | Transparent background, 1px border, body text color |
| **Ghost** | Transparent, muted text, hover to body color |

### Status Badges

| Badge | Color | Indicator |
|-------|-------|-----------|
| `--success` | Emerald-400 | Online / Active / Live |
| `--warning` | Amber-400 | Degraded / Pending |
| `--error` | Red-400 | Offline / Error |
| `--info` | Purple-400 | AI Active / Info |

### Glass Card

Reusable card surface with:
- `rgba(255,255,255,0.03)` background
- 24px backdrop blur
- 1px white/8 border
- Subtle purple glow shadow

### Terminal Frame

Monospace terminal emulation widget with:
- Traffic light dots header
- Purple prompt (`❯`)
- Emerald accent values
- Animated blinking cursor

### Telegram Bot UI

| Component | Description |
|-----------|-------------|
| **Bot Message Card** | Resolved stream card with avatar, bot badge, metadata rows, and action buttons |
| **Inline Keyboard** | Row of keyboard buttons matching Telegram's InlineKeyboardMarkup spec |
| **Mini-App Surface** | Safe-area-aware full-height container for Telegram Mini Apps |

---

## Framework Evaluation

### Chosen Stack: Cloudflare Workers + Pages

**Cloudflare Workers** was evaluated against the following criteria and selected as the optimal deployment target for the Frisky Design System:

| Criterion | Cloudflare Workers + Pages | Alternative (Vercel) | Alternative (GitHub Pages) |
|-----------|---------------------------|---------------------|---------------------------|
| Cold start | ✅ < 1ms | ✅ ~50ms | ✅ Static |
| Global edge | ✅ 330+ cities | ✅ 14 regions | ❌ Single region |
| Wrangler CLI | ✅ Built-in | ❌ No | ❌ No |
| Cost | ✅ Free tier generous | ✅ Free tier | ✅ Free |
| Telegram webhook | ✅ Native Workers | ✅ Serverless fns | ❌ Static only |
| Git CI/CD | ✅ Auto-deploy | ✅ Auto-deploy | ✅ GitHub Actions |

**Decision**: Cloudflare Pages for static design system site + Workers for Telegram webhook endpoints. Wrangler CLI is pre-installed at `/opt/homebrew/bin/wrangler` v4.106.0.

---

## Deployment

### Prerequisites

- [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/) (v4+)
- Cloudflare account with Workers/Pages enabled

### Local Development

```bash
npm run dev
```

This starts a local dev server with live reload at `localhost:8788`.

### Deploy to Cloudflare Pages

```bash
npm run deploy
```

Or directly:

```bash
npx wrangler pages deploy public --branch main
```

### Project Structure

```
FRISKY-DEV-BOT-DESIGN-SYTEM-TELEGRAM/
├── public/
│   ├── index.html          # Design system showcase page
│   ├── css/
│   │   ├── tokens.css      # CSS custom properties + utility classes
│   │   └── styles.css      # Page layout and showcase styles
├── tokens.json             # Machine-readable design token spec
├── wrangler.toml           # Cloudflare Pages configuration
├── package.json            # Scripts: dev, deploy, format
├── .gitignore
└── README.md               # This file
```

---

## Usage

### Import Tokens in Your Project

**CSS:**

```css
@import url('https://friskydev.com/bot-design-system/css/tokens.css');
```

**JavaScript (from tokens.json):**

```js
import tokens from './tokens.json' assert { type: 'json' };
// tokens.colors.background → '#050505'
```

### Telegram Bot Formatting

The design system maps directly to Telegram's bot API surfaces:

- **Inline keyboards**: `InlineKeyboardMarkup` with keyboard buttons styled via design tokens
- **Bot messages**: `ParseMode.MarkdownV2` for formatted bot message cards
- **Mini-apps**: Full-screen web app surfaces with safe-area-aware padding

---

## Brand Guidelines

- **Logo**: Official wolf mark (⬡) — never replace with generic icons
- **Gradient**: Purple (`#c084fc`) → Emerald (`#34d399`) — primary accent, never inverted
- **Background**: `#050505` — pure black with depth
- **Glass**: Use `backdrop-filter: blur(24px)` for depth without opacity stacking
- **Typography**: Space Grotesk for headings, Inter for body, JetBrains Mono for code
- **Technical labels**: Always uppercase, monospace, emerald-400 — denotes system/technical content

---

## License

MIT — Frisky Developments

---

*Part of the [Frisky Developments](https://github.com/FriskyDevelopments) ecosystem.*
