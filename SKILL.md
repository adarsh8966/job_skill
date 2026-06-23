---
name: job-apply
description: >
  Job application assistant — use this whenever the user wants to apply for a job,
  tailor their resume to a job description, optimize a resume for ATS, make a resume
  ATS-friendly, write a cover letter for a job, find people to cold email or connect
  with on LinkedIn, or prepare any part of a job application. Trigger on: "tailor my
  resume", "make my resume ATS friendly", "write a cover letter for this job",
  "find contacts at [company]", "help me apply to", "I want to apply for", "who should
  I reach out to at", "customize my resume for", or any combination of a pasted job
  description and a request for help. When in doubt, invoke this skill — a job
  application is always the right context.
---

# Job Application Assistant

You help users land interviews. Three modes — figure out which from context, or ask once
if it's genuinely unclear. If the user pastes a JD and mentions their resume, go straight
into Mode 1 without asking.

---

## Mode 1: Resume Tailoring

The goal: a resume that (a) clears ATS keyword filters and (b) makes a human reviewer
immediately see the fit.

### Step 1: Gather inputs

You need:
- **Base resume** — file path (.docx or .pdf), or pasted text content
- **Job description** — full text

If either is missing, ask before proceeding. Don't guess.

Tip to share: keep your master resume at a fixed path (e.g., `Desktop/resume-master.docx`)
and update it there whenever you finish a new project. You'll always know where to find it.

### Step 2: Analyze the JD — do this first, show it briefly

Extract and display a quick breakdown before touching the resume:

```
JD BREAKDOWN
Role level:    [junior / mid / senior / staff]
Top keywords:  [exact phrases — "React.js" not "React", "CI/CD pipelines" not "CI/CD"]
Top 3 responsibilities: [what they'll actually do, in order of JD emphasis]
Culture signals: [words like "fast-paced", "ownership", "data-driven"]
Gaps to flag:  [JD requirements not in the base resume]
```

Showing this upfront signals to the user that you're being systematic, not just
rewriting randomly.

### Step 3: Tailor the resume

**Professional summary (2-3 sentences)**
Write one that mirrors the JD's top 3 priorities directly. Use their framing — if they
say "engineers who own outcomes end-to-end", open there. Include 2-3 top keywords naturally.

**Experience bullets**
Rewrite to:
- Use exact JD keyword phrases where genuinely applicable (never invent experience)
- Lead with impact first: "Reduced API latency 40%" > "Worked on API performance"
- Match the seniority language of the JD ("drove", "owned", "led" for senior roles;
  "built", "contributed", "implemented" for mid/junior)

For every bullet that's missing a metric, add this tag inline:
`[STRENGTHEN: add a number here — e.g., "by X%", "for N users", "from Xms to Yms"]`

These tags are important — they show the user exactly where a quantified result would
make the biggest difference. Don't skip them.

**Section order**
- For new grads or <3 years experience: Projects above Education
- Reorder projects so the most JD-relevant one is first

**Skills section**
- Reorder: JD-matching skills first
- Add skills the JD requires that the user clearly has (mentioned in bullets) but
  forgot to list explicitly

**ATS formatting — read `references/ats-rules.md` for the full list. Core rules:**
- Single column only — no sidebars, no two-column layouts
- Standard headers: Summary, Experience, Education, Projects, Skills
- No text boxes, tables for layout, images, or graphics
- Standard fonts: Calibri, Arial, Times New Roman, Garamond
- Consistent date format throughout

### Step 4: Output the resume

**Primary output (always):** Write the full tailored resume as clean plain text. Format it
clearly with section headers so it's easy to copy into a Word doc or Google Doc. This
always works regardless of what tools are installed.

**Bonus — DOCX file (if `npm` and the `docx` package are available):** Use the docx-js
approach from the installed `docx` skill to also generate a properly formatted .docx.
Name it `resume-[Company]-[Role].docx`. Only attempt this if npm is confirmed available
— don't fail the whole task trying.

### Step 5: Summarize what changed + what's missing

After the resume, give a short section:

```
CHANGES MADE
- [keyword 1]: added to [where]
- [keyword 2]: rewrote [which bullet] to include it
...

GAPS (be honest)
- [requirement]: not in your background. [brief honest note — e.g., "Ruby: listed Python/Go
  as alternatives; should be fine", or "Kafka: your task queue project covers the concept
  but not the tool — name it in a cover letter"]
- [STRENGTHEN] count: N bullets could be stronger with metrics
```

### Step 6: Offer a cover letter

After the resume, always ask:
"Want me to draft a cover letter too? I'll use the same JD analysis — takes 30 seconds."

If yes, go to Mode 3.

---

## Mode 2: Cold Outreach Finder

Find the 3-5 highest-leverage people at a company for the user to contact.

### Step 1: Get the target

Ask for:
- Company name
- Role or team they're applying to

### Step 2: Find the email domain FIRST — before anything else

This is critical. Get the domain wrong and every email bounces.

Search in this order until you have 2+ confirming sources:

1. `"@[company.com]" email` — looks for real employee emails in public sources
2. `[company] email format site:hunter.io`
3. GitHub commits from company employees (often show real email addresses)
4. Press releases, blog post bylines, or LinkedIn contact sections
5. `[company.com] email format`

Once you have 2+ consistent results, label it **"Likely confirmed: first.last@company.com"**.
If you only have one source, label it **"Guessed from one source — verify before sending"**.
If you find nothing, say so honestly: **"Format unknown — use Hunter.io or Apollo.io"**.

Never state an email format as fact without at least two matching data points.

### Step 3: Find contacts

Run these searches:

**Recruiters / Talent Acquisition:**
- `"[Company]" "technical recruiter" OR "talent acquisition" site:linkedin.com`
- `"[Company]" "[role area] recruiter"` — e.g., "Stripe engineering recruiter"
- Check the company's LinkedIn People page → filter by "Human Resources"

**Hiring managers:**
- `"[Company]" "engineering manager" OR "head of [team]" OR "director of [team]" site:linkedin.com`
- Check the company's careers page — some list the recruiter or team name
- Search `[company] [team] team site:[company.com]` — team pages often name leads
- Check the company's engineering blog for authors on the team

**Senior ICs / peers (underrated path):**
- `"[Company]" "senior [role]" OR "staff [role]" site:linkedin.com`
- Someone hired in the last 6-18 months is often willing to help

Do 4-6 searches. Sparse results are fine — 3 real contacts beats 10 placeholders.

### Step 4: Present contacts

| Name | Title | LinkedIn | Email | Priority |
|------|-------|----------|-------|----------|
| ... | ... | linkedin.com/in/... | first.last@co.com | High |

Priority:
- **High** = direct recruiter for this role, or manager of the hiring team
- **Med** = adjacent manager, senior team member, general recruiter

Be honest: say "guessed" for emails you inferred, "confirmed" for ones you found directly.
If you couldn't find a real name for a slot, say so — don't fill in placeholders.

### Step 5: Outreach templates

Provide both. Make them company-specific, not generic.

**LinkedIn connection request (≤300 chars):**
```
Hi [Name], I'm applying for [Role] at [Company] and was genuinely drawn to [one
specific real thing about the company/team/product]. Would love to connect!
```

**Cold email:**
```
Subject: [Role] Application — Quick Question

Hi [Name],

I'm [your name], applying for [Role] at [Company]. I was drawn to [one specific,
non-generic thing — a product decision, a blog post, something real].

[One sentence on your most relevant experience for this role, with a number if possible.]

Would you be open to a 10-minute chat? No pressure — appreciate your time either way.

[Name]
[LinkedIn URL]
```

Remind them: connect on LinkedIn first, wait 2-3 days, then message. It's warmer.

---

## Mode 3: Cover Letter

Write a tight 3-paragraph cover letter. Reuse the JD analysis from Mode 1 if it's
already been done.

**Paragraph 1 — Why this company (not generic)**
Reference something specific and real: a product decision, a recent launch, a blog post,
a mission statement that actually resonates. One sentence of genuine interest is worth
more than three sentences of flattery.

**Paragraph 2 — Why you (with evidence)**
Pick the single strongest match between the user's experience and the JD's top priority.
One tight paragraph with a concrete example and a number if they have one. Don't
summarize the whole resume — pick the best thing.

**Paragraph 3 — Close**
Express enthusiasm, note that you've attached the resume, and keep it brief. No "I
believe I would be a great fit" — show, don't tell.

Format: plain text, under 300 words total. Short cover letters get read; long ones don't.

---

## Quick tips (share after any mode)

- **ATS check:** Run the tailored resume through Jobscan or Resume Worded (both free tier)
  before submitting — they catch keyword gaps fast
- **Timing:** Apply and reach out the same day — most companies screen within 48-72 hours
  of a posting going live
- **Outreach goal:** A reply, not a referral. Ask for advice or a quick chat, not a favor
- **Follow up:** Once, after 5-7 days. Not more
