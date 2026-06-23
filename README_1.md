# 🎯 job-apply

> A Claude Skill that tailors your resume to a job description, finds the right people to reach out to, and drafts a cover letter — all from one pasted JD.

![Claude Skill](https://img.shields.io/badge/Claude-Skill-6A5ACD)
![Status](https://img.shields.io/badge/status-active-brightgreen)
![License](https://img.shields.io/badge/license-add%20one-lightgrey)

No more rewriting your resume from scratch for every application. Paste a JD, point it at your base resume, and the skill takes it from there.

---

## What it does

| Mode | What you get |
|---|---|
| 🧩 **Resume Tailoring** | A JD keyword breakdown, a rewritten summary + bullets + reordered skills section, ATS-safe formatting, and a `[STRENGTHEN]` flag on every bullet missing a metric |
| 📇 **Cold Outreach** | 3–5 real contacts (recruiters, hiring managers, senior ICs) with LinkedIn links, a confidence-rated email guess, and ready-to-send LinkedIn + email templates |
| ✍️ **Cover Letter** | A tight, 3-paragraph, sub-300-word letter, reusing the JD analysis from Mode 1 |

The design principle running through all three: **never invent — flag instead.** Missing metrics get tagged, not fabricated. Email formats get labeled confirmed / guessed / unknown, never stated as fact off one data point. Contacts with no real name get left out, not filled with placeholders.

## Example

You: *(paste a job description)* "tailor my resume to this"

Claude:
```
JD BREAKDOWN
Role level:    Mid-senior
Top keywords:  "high-throughput", "own features end-to-end", "distributed systems"
Gaps to flag:  Kafka, fintech experience
```
...followed by the rewritten resume, a changelog of what was edited and why, and an offer to draft a cover letter next.

## Install

Skills work a little differently depending on where you use Claude. Pick the one that matches your friend group.

### Option A — claude.ai (web or desktop app)
1. Zip the `job-apply/` folder (it needs to contain `SKILL.md` and `references/` at the top level)
2. **Settings → Capabilities** → confirm *Code execution and file creation* is on
3. **Customize → Skills → “+” → Create skill → Upload a skill**
4. Upload the zip — make sure the folder name inside the zip matches the skill name (`job-apply`), that's the #1 cause of upload errors

### Option B — Claude Code (CLI)
Personal install (available in every project):
```bash
git clone https://github.com/<you>/job-apply.git
cp -r job-apply ~/.claude/skills/job-apply
```
Project-only install (lives in a repo, ships with it):
```bash
cp -r job-apply .claude/skills/job-apply
```
Start a new session — Claude picks it up automatically, no slash command needed. Double-check the file actually landed at `~/.claude/skills/job-apply/SKILL.md` and not flattened loose inside `~/.claude/skills/` — that one nesting mistake is the most common reason a skill silently fails to load.

## Usage

Just talk to it normally — no special syntax:
- "tailor my resume to this JD: *[paste]*"
- "find contacts at Figma for a backend eng role"
- "write me a cover letter for this one"

## File structure

```
job-apply/
├── SKILL.md              # the skill itself — all 3 modes, all the logic
└── references/
    └── ats-rules.md       # ATS formatting rules, loaded on demand
```

## Keep your base resume current

The skill is only as good as what you feed it. Keep one master resume at a fixed path (e.g. `Desktop/resume-master.docx`) and update it the moment you ship a new project — that's what gets tailored every time.

## Limitations — read before trusting it blindly

- **Contact search uses public web search, not scraping.** Small companies or low-visibility employees will come back sparse — that's intentional (no fake placeholder names), not a bug.
- **Email guesses are still guesses.** Confidence-labeled, but verify before sending anything that matters.
- **DOCX and PDF are best-effort.** Guaranteed output is clean plain text you can paste anywhere; DOCX generates if `npm`/the `docx` package are available, PDF generates on top of that if LibreOffice (`soffice`) is installed. No LibreOffice → you still get text + DOCX, just no PDF.
- **ATS guidance reflects general best practice as of 2026.** Specific platforms (Workday, Greenhouse, Lever) vary and their PDF parsing has been improving — the docx-over-pdf default is a safe bet, not a law of physics.

## Roadmap ideas

- [x] ~~Generate the PDF output directly, not just DOCX-if-available~~ — done in v1.1
- [x] ~~Bake a "personalize before sending, don't mass-blast" reminder into the outreach templates~~ — done in v1.1
- [ ] Multi-language resume/JD support
- [ ] Pull the base resume straight from a Google Doc / Drive link
- [ ] Starter resume template in `references/` for friends who don't have a base resume yet
- [ ] Re-run the eval suite (you already built one) against every SKILL.md change before calling it stable

## Contributing

PRs welcome — especially fixes to the contact-search prompts, new ATS edge cases, or region-specific resume norms.

## Changelog

- **v1.1** — Added real PDF generation (LibreOffice, with an honest fallback if it's not installed), added a personalization/pacing reminder to outreach so it doesn't enable mass-blasting identical messages, fixed Mode 3 (cover letter) to run its own JD analysis instead of assuming Mode 1 already happened.
- **v1.0** — Initial release: resume tailoring, cold outreach finder, cover letters.

## License

Pick one before sharing this publicly — MIT is the easiest default for something like this. *(no LICENSE file included yet)*

---
Built with Claude.
