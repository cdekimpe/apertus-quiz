# European Fund Structuring Quiz

A simple web-based quiz application to test knowledge about European fund structuring through Luxembourg — the core domain of **Apertus**.

## Quiz Concept

10 multiple-choice questions covering:

- **AIFMD Framework**: Alternative Investment Fund Managers Directive essentials
- **Luxembourg Fund Structures**: SICAV, SIF, RAIF, and their use cases
- **UCITS**: Understanding the retail fund framework in Europe
- **Regulatory Compliance**: Annex IV reporting, SFDR disclosures, PRIIPs
- **Cross-Border Distribution**: How North American asset managers access European capital
- **Fund Operations**: Depositary requirements, NAV calculation, share classes
- **Market Access**: The role of an AIFM in European fund distribution

## Target Audience

North American asset managers and investment professionals looking to validate their understanding of the European fund landscape before launching a Luxembourg-based fund.

## Tech Stack

Frontend-only static site built with **Vite + vanilla TypeScript** (no framework). Vite provides HMR during development and produces a `dist/` folder of pure static files. Direct DOM manipulation with template literals — no React/Vue needed for a 10-question quiz.

## Brand Design System

The design follows the **Apertus** brand as defined in BrandFolio (accessible via the `.mcp.json` MCP server configuration in this repo).

| Token | Value |
|-------|-------|
| Primary | `#d13fee` |
| Secondary | `#e368c5` |
| Accent | `#793eef` |
| Background | `#f8f7f8` |
| Text | `#2a212c` |
| CTA | `#d13fee` bg / `#FFFFFF` text |
| Headings | Space Grotesk (400, 600, 700, 800) |
| Body | DM Sans (400, 500, 600) |
| Google Fonts | `https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;600;700;800&family=DM+Sans:wght@400;500;600&display=swap` |
| Button/Card Radius | 20px |
| Card Border | 2px |
| Shadows | bold |
| Spacing | relaxed |
| Icons | Phosphor |
| Personality | Playful — friendly, clever wordplay, professional credibility |
| Logo | `https://qcgxzhqcrdoul3a9.public.blob.vercel-storage.com/logo-1767641471923-5k5nd-0475VxG5boAnQMtpSSmCWBbHBkz6wV.png` |

### Voice Guidelines

Apertus communicates with a friendly, approachable voice that brings a refreshing lightness to complex financial services. Clever wordplay and engaging metaphors make Luxembourg fund management accessible without undermining credibility.

**Do:** Use clever financial wordplay, maintain a warm conversational tone, include subtle humor that makes technical content engaging.

**Don't:** Use overly casual language, make light of compliance/regulatory topics, overuse puns.

## UX Flow

### Screen 1: Welcome
- Apertus logo + "European Fund Structuring Quiz" heading
- Subline: "10 questions. No timer. All the fun."
- Brief description of what it tests
- CTA button: "Let's Go"

### Screen 2: Question (×10, one at a time)
- Progress bar (step X of 10)
- Topic pill badge above question
- Question text + 4 answer option cards
- On click: lock answer, reveal correct/incorrect with color feedback (green/red)
- Explanation text appears below (with optional fun fact in Apertus voice)
- "Next" button (or "See Results" on Q10)
- No timer — the target audience is senior professionals

### Screen 3: Results
- Animated score ring showing X out of 10
- Tier message in Apertus voice:
  - 9–10: "Fund Structuring Virtuoso!"
  - 7–8: "Almost There!"
  - 5–6: "Getting Warmer."
  - 0–4: "Time to Brush Up!"
- Per-question breakdown (topic, your answer, correct answer, checkmark/cross)
- Two CTAs: "Try Again" (reset) + "Learn More with Apertus" (external link)

### Question Distribution

| Topic | # Questions |
|-------|-------------|
| AIFMD Framework | 1 |
| Luxembourg Fund Structures | 2 |
| UCITS | 1 |
| Regulatory Compliance | 2 |
| Cross-Border Distribution | 1 |
| Fund Operations | 2 |
| Market Access | 1 |

## Domain

`apertus.dekimpe.me` — A record pointing to the Proxmox host public IP (`37.187.226.2`), managed via OVH API on the `dekimpe.me` zone.

## Deployment

The quiz should be deployed on a **dedicated LXC container** on the Proxmox host (`10.10.10.1`), not on the dev LXC. The setup:

1. **Create a new LXC** on the Proxmox host (lightweight Ubuntu/Debian) with nginx installed
2. **Build the app** locally (`npm run build`) and copy `dist/` to the LXC, or build on the LXC directly
3. **Configure nginx** in the LXC to serve the static `dist/` files on port 80
4. **Reverse proxy** on the Proxmox host's nginx: `apertus.dekimpe.me` → `http://<lxc-ip>:80`
5. **TLS** via Certbot on the Proxmox host (`certbot --nginx -d apertus.dekimpe.me`)
6. **DNS** A record for `apertus` subdomain on `dekimpe.me` → `37.187.226.2` via OVH API
