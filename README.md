# AI Job Application Agent

An autonomous job-application pipeline built on Claude Code and the Model Context Protocol (MCP). The agent reads your résumé and applies to jobs that match it — point it at a specific posting, or let it find matching roles on its own. It evaluates fit, tailors the résumé for each posting, writes a cover letter, completes the real ATS application in a browser, and follows up with recruiter outreach, with a human review checkpoint before anything is submitted.

I assembled, configured, and operate this pipeline for my own internship search, running it end-to-end against real applicant tracking systems: Workday, Greenhouse, Lever, ADP, and LinkedIn Easy Apply.

> **About this repo:** This is the public overview of a private project. The implementation — the agent's skill definitions, platform playbooks, and configuration — lives in a private repository and is not published here. This page documents what the system does and the principles it runs on, not how to reproduce it.

## What it does

A single instruction kicks off the full cycle. That can be a specific posting:

```
Apply to https://job-boards.greenhouse.io/company/jobs/1234567
```

or an open-ended search, where the agent sweeps job boards and picks roles that match my résumé:

```
Find and apply to software engineering internships that match my criteria
```

Every role then goes through the same pipeline:

- **Screens the role.** The posting is scored against my profile and criteria. Poor fits and roles with hard knockouts are flagged or skipped, not applied to.
- **Tailors the materials.** My résumé is re-emphasized for the posting, and a personalized cover letter is written from live company research.
- **Completes the actual application.** The agent operates a real browser through the employer's own application flow — account creation, multi-page forms, work history, screening questions, and file uploads.
- **Follows up.** It identifies a recruiter at the company and sends a personalized outreach email using publicly available contact information.
- **Keeps records.** Every application, every screening question with the exact answer given, and every outreach touch is logged locally.

## Where the human stays in the loop

The pipeline is deliberately not fully autonomous. It stops and hands control back for anything that has to be genuinely mine:

- Take-home assessments, coding tests, and recorded video interviews
- Essay questions it can't ground in my real work
- Personal facts I never provided
- Legal attestations, such as export-control citizenship questions
- The final review page of every application, before submission
- Login walls, CAPTCHAs, and rate limits — these pause for a human by design

When it hits one of these, it completes everything else and reports exactly what's left. A staged application I can finish in two minutes is the intended outcome, not a failure mode.

## Guardrails

- **No fabrication.** The agent never invents skills, experience, or metrics. Résumé tailoring is limited to reordering and rephrasing content that is true.
- **Skip, don't stretch.** If a role requires something I don't have, it skips the role rather than bending the truth on a screening question.
- **Consent-gated submission.** A human reviews every application before it is submitted.
- **Local-only personal data.** My profile, résumé, transcript, application history, and screenshots stay on my machine and are never committed to any repository.
- **Respectful outreach.** Recruiter emails use only publicly available contact information and are individually personalized to the role and company.

## How it's put together (high level)

Claude Code acts as the agentic orchestrator. Through MCP, one session drives a real browser for the application flows, generates the tailored documents, sends and reads email, and maintains a local ledger of every application. The pipeline's behavior — screening rules, platform handling, and refusal policies — is encoded as durable, reusable agent instructions rather than re-improvised each session, which is what makes runs reproducible.

The finer mechanics — per-platform ATS playbooks, screening logic, configuration schemas, and error-handling policy — are part of the private implementation.

## What building it involved

- Integrating browser automation and email under a single agentic session, so one agent can operate a job posting, an application form, and an inbox together
- Encoding the workflow, screening policy, and honesty rules as durable agent behavior instead of ad-hoc prompting
- Hardening against real-world ATS flows: multi-step account creation, "how did you hear about us" taxonomies, EEO and self-identification pages, and per-platform quirks across Workday, Greenhouse, Lever, and others
- Designing the human-in-the-loop boundaries — what the agent may do alone, what it must stage for review, and what it must never do at all

## Tech

Claude Code (agentic AI) · Model Context Protocol · browser automation · email integration · local JSON-based tracking

No build step and no runtime of its own — there is nothing to compile or deploy.

## Contact

The source and configuration are private. If you're a recruiter or you'd like a walkthrough or demo, reach out: **[your email or LinkedIn here]**.
