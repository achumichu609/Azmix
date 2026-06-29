[azmix-README.md](https://github.com/user-attachments/files/29478460/azmix-README.md)
# AzMix — Creator Studio

> The production desk for content creators. Plan, shoot, edit, publish, and get paid — all in one workspace.

**Live:** https://azmix.vercel.app
**Stack:** Vanilla HTML / CSS / JavaScript · single self-contained file · deployed on Vercel

---

## What it is

AzMix is an all-in-one operating system for content creators. Instead of juggling a notes app for ideas, a spreadsheet for finances, a calendar for uploads, and a separate tool for editing status, AzMix brings the entire creator workflow into one dark, focused "studio" interface — built to feel like a professional editing suite rather than a generic dashboard.

It ships with **17 working modules** spanning the full lifecycle of a video: from capturing an idea, to planning the shoot, to tracking the edit, to scheduling the upload, to managing the brand deal that paid for it — plus an AI creative assistant that understands the user's actual studio data.

## The modules

**Workspace** — Command Center (dashboard with animated stats), AI Assistant, Daily Execution (today's focus with a completion ring)

**Planning** — Content Calendar (drag-aware monthly view, platform colors), Idea Vault (pin/favorite/filter), Goals (progress rings, categories, rewards)

**Creation** — Script Studio (Hook/Body/CTA editor with autosave, word count, speaking-time estimate), Shoot Planner (filming days with checklists), Shot List (every camera shot, checked off on set), Thumbnail Studio (concept cards with live color-palette previews and CTR ratings), Music Library (organize owned/royalty-free tracks by mood), Editing Pipeline (drag-and-drop Kanban), Upload Planner (never miss a publish date)

**Business** — Brand Deals (sponsorship pipeline from lead to paid), Creator Finance (revenue/expense tracking, profit trend, income-source breakdown)

**Growth** — Analytics (interactive SVG charts), Growth Engine (animated milestones)

## Notable engineering decisions

**Single self-contained file.** The entire app — every module, all styling, all logic — lives in one `index.html`. No build step, no dependencies, no framework. This makes it trivial to deploy (drop one file on any static host) and means the whole thing works offline. The tradeoff is a larger single file, which is acceptable for a tool of this scope.

**Per-module local storage.** All user data persists in the browser via `localStorage`, namespaced per module (`azmix:ideas`, `azmix:deals`, etc.) rather than one giant object. This keeps reads/writes targeted and the data model clean. Nothing is ever transmitted off the device — a deliberate privacy stance.

**Hand-built SVG charts.** The Analytics and Finance charts (line, bar, donut, progress rings) are drawn with raw SVG and a small set of reusable helper functions — no charting library. This keeps the single-file constraint intact and the bundle lean, while still delivering hover tooltips and smooth fill animations.

**AI assistant that reads real data.** The assistant isn't a generic chatbot. When asked "how am I doing this month?", it reads the user's actual pipeline, deals, ideas, and goals from local storage and responds with specifics. It works fully offline using a pattern-matching "coach" brain, with an optional bring-your-own-key field for users who want to upgrade to live AI generation.

**Security-conscious API design.** Rather than embedding an API key in front-end code (a common and dangerous anti-pattern), the assistant stores any user-provided key in their own browser only, and the production-correct path — a serverless proxy reading the key from a server-side environment variable — is documented as the upgrade route. The offline coach is fully functional without any key.

## Design

A deliberately distinct visual identity: a deep charcoal "cockpit" base with a single electric-mint accent (`#00E5C7`) and warm signal colors for status and platforms. Typography pairs Space Grotesk (display), Inter (UI), and JetBrains Mono (data readouts). The recurring signature motif is a mixing-fader bank — a nod to the "mix" in AzMix and the idea of blending every part of the creator workflow into one signal. Built on an 8-point spacing grid with a consistent component system, a single easing curve, and respect for reduced-motion and keyboard focus.

## Things I'd build next

- A serverless proxy so the AI assistant can do live generation securely (the key infrastructure for this is designed but not yet deployed).
- Optional cloud sync / accounts for users who want their studio on multiple devices (currently data is per-browser by design).
- A few of the future-ready hooks already scaffolded in the UI: AI thumbnail generation, invoice export from Finance.

## What I learned

- **Phased building beats trying to do everything at once.** Building the shell and a few core modules to a high standard first, then adding modules in batches, consistently produced better quality than attempting all 17 in one pass.
- **Test your own output.** I validated each batch with an automated headless render before considering it done. This caught real bugs — including a string-escaping error and a structural error in seed data — that would otherwise have shipped broken.
- **Honest scope is a feature.** Deciding that the Music Library organizes *owned/royalty-free* music (rather than pretending to host copyrighted audio) and that the AI assistant is *honestly offline by default* made the product both legally clean and more trustworthy.
