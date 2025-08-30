Tech Stack: ReactJs , AI module using (faiss vector database , openrouter api chatgpt-3.5-turbo , fastapi) in Python , postgres database

Diagram :https://chatgpt.com/s/m_68b32d00d4c881918615a524cc509430

Challenge: Internal DocBot for Faster, Engaging, and Measurable Food & Beverage Training
Background / Why this matters
F&B operations run on strict SOPs: food safety, equipment handling, cleaning schedules, allergen control, cash & shift procedures—yet onboarding and refreshers are slow, manual, and hard to track. A smart, internal, SOP-grounded docbot can turn static PDFs into bite-size, interactive, auditable learning—so staff learn faster, retain longer, and managers see compliance at a glance.
Problem Statement
Design and prototype an internal-facing digital training solution—centered on a Document Intelligence Bot (docbot)—that:
Converts existing SOPs/policies into just-in-time, stepwise guidance (e.g., “Deep fryer safety: step 1… step 2…”).
Delivers microlearning (short lessons + quizzes), supports spaced repetition, and tracks progress & compliance.
Gives managers dashboards for completion, competency, and adherence (audit-ready).
Works on low-friction devices used in F&B (shared tablets, kiosk mode, mobile BYOD).


Core Objectives
Doc Ingestion & Grounded Answers
Ingest SOPs in multiple formats (PDF/DOCX/Markdown/Images with OCR).
Provide retrieval-augmented answers with inline citations back to source paragraphs.
Offer step-by-step “Procedure Mode” (numbered steps; “Next”/“Repeat step” UX).


Training & Retention
Microlearning modules auto-generated (or curated) from SOPs.
Knowledge checks (quizzes) with explanations and source links.
Spaced repetition for missed/low-confidence items.


Process Adherence
Checklists (open/close, cleaning, temperature logs, equipment warm-up).
Attestation & e-sign with user/time stamps and versioned SOP references.


Measurability & Compliance


Manager dashboard: course completion, quiz scores, SOP version coverage, overdue items, exception reports.
Audit trail with SOP version IDs and timestamps.


Usability in F&B Environments


Offline-first (read-only cache + queued submissions) or graceful degradation.
Multilingual UI + content (at least 2 languages).
Kiosk/shared device UX with role-based access (Crew, Shift Lead, Manager).
Accessibility: readable fonts, large tap targets, optional TTS for lessons.


Must-Have Features (MVP)
Docbot chat that only answers from provided SOPs/policies (no hallucinations; must show citations).
Procedure Mode (step-by-step, with checkboxes and “show me how” snippets/images if available).
Quiz engine (MCQ/true-false) + spaced repetition (e.g., Leitner).
Manager dashboard with: completion %, average score, last trained date, checklist adherence.
User roles (Crew vs Manager) and basic auth (okta/email+otp/mock SSO acceptable).
Content versioning: responses display “SOP vX.Y, date”.


Nice-to-Have (Stretch)
QR codes at stations: scan → auto-open the exact SOP/procedure in Procedure Mode.
IoT tie-ins (mocked ok): temperature probe/logging and validation in checklists.
Scenario simulations (branching cases for allergen cross-contact, oil fire, etc.).
Manager nudges: Slack/WhatsApp notifications for overdue training.
xAPI/SCORM export or CSV export to plug into an LMS.
Speech interface with noise-robust STT and TTS (kitchen mode).


Constraints & Guardrails
Grounding: The bot must cite sources for every procedural or safety claim (document title + section + anchor).
Safety: If confidence is low or no source exists, reply with “I don’t have that in the SOPs” and prompt to escalate.
Privacy: No PII in logs; training data must remain local to the app/demo environment.
Offline: If using cloud LLMs, show fallback behavior (local retrieval + cached content).
Device: Optimize for tablet portrait 768×1024 and mobile 360×800.
Latency: < 1.5s for doc retrieval; < 3s for full Q&A round trip in demo.


Recommended Architecture (example)
Ingestion: File drop → OCR → text chunker → metadata (doc, section, version, language, effective date).
Index: Vector store + keyword fallback; store chunk-level provenance.
RAG Service: Query → retrieve → re-rank → answer with citations; “Procedure Mode” renders step list from SOP structure.
Learning Service: Lesson builder + quiz generator + spaced repetition scheduler.
Adherence Service: Checklists, attestations, version pinning.
Analytics: Events (view_lesson, pass_quiz, complete_checklist) → dashboard.
Clients: Web (PWA), kiosk mode, optional mobile wrapper.


Deliverables (what teams must submit)
Live demo or video (≤3 min) covering:
Ask the docbot a safety/process question and show cited answer.
Run a Procedure Mode flow (e.g., “Deep Fryer Safety – startup & shutdown”).
Take a quiz, miss a question, show spaced repetition scheduling.
Show manager dashboard with a meaningful metric shift.


Repo with README: Architecture diagram, setup steps, model choices, limits, and next steps.
Data binding: A visible configuration showing which SOP versions are loaded.
Demo scripts (provided below) executed successfully.


Evaluation Criteria
Real-world usefulness (30%): Does this reduce time-to-competency and improve adherence?
Grounding & safety (20%): High-quality citations; graceful “don’t know” behavior.
Learning & retention (20%): Microlearning quality, quiz design, spaced repetition.
Analytics & compliance (15%): Clarity of dashboards, audit trails, exports.
UX in F&B context (15%): Multilingual, kiosk UX, offline resilience.
Demo Scripts (Judging Will Use These)
Ask & Cite
 “What are the steps to safely drain and filter the deep fryer oil?”
 Expected: Numbered steps + citations to the exact SOP sections.
Procedure Mode
 “Start-up checklist for espresso machine” → run the checklist; mark steps complete; require attestation.
Quiz & Retention
 Start “Food Allergen Basics” lesson → take 5-question quiz → miss 2 → show how/when items recur via spaced repetition.
Manager View
 Show completion by role, SOP coverage by version, and an exception report (e.g., missed “End-of-day cleaning”).
Versioning
 Update an SOP (v1.2 → v1.3). Show that answers now cite v1.3 and the dashboard logs the update.


What We Need From the Organizer (Data Pack + Constraints)
Please provide a small, anonymized Data Pack so all teams build against the same materials:
Required (minimum)
SOP—Deep Fryer Safety & Maintenance (v1.2, effective date, 2–4 pages)
Startup, operation, draining/filtering, shutdown, PPE, spill response.
SOP—Espresso Machine Daily Procedure (v1.0)=
Warm-up, backflush cleaning, gasket care, descaling schedule.
Food Allergen Basics (Training Doc) (v1.1)
8 common allergens, cross-contact prevention, signage, customer disclosure script.
Cleaning & Sanitation Checklist (daily/weekly CSV)
Task name, frequency, tools/chemicals, target time, verification step.
Roles & Permissions (CSV)
user_id, name, role (Crew/Manager), location.
Optional (nice)
Allergen Matrix for 10 menu items (CSV).
Opening/Closing Checklists (CSV).
Incident Escalation Policy (PDF).


Formatting & Constraints
Accepted formats: PDF/DOCX/Markdown for SOPs; CSV/JSON for checklists and matrices; PNG/JPG for images (label clearly).
Include document metadata in filenames or a manifest: title, version, effective_date, language.
No PII. Replace names with roles.
Keep each SOP ≤ 4 pages so teams can index quickly.
Provide an English set; if possible, include a second language (e.g., Spanish/Hindi) of the same fryer SOP to test multilingual.
Seed Sample (you can share this verbatim if you like)
Deep Fryer Safety (Excerpt, v1.2):
PPE: Heat-resistant gloves, apron, closed shoes.
Startup Steps:
Verify oil level is between MIN/MAX.
Ensure the drain valve is closed.
Set the thermostat to 180°C; wait for ready light.


Draining & Filtering:
Turn off heat; cool to 60–70°C.
Place filter container; open drain slowly.
Filter per manufacturer guide; remove crumbs.
Close valve; refill to MAX.


Emergency: If oil ignites, do not use water; use Class K extinguisher; cut power; alert manager.


Cleaning Checklist (Excerpt, daily):
task,frequency,area,tool,verify_by,notes
Sanitize prep surfaces,daily,Prep bench,Food-safe sanitizer,Shift Lead,Before opening & after rush
Filter fryer oil,daily,Hot line,Filter kit,Manager,Per SOP v1.2 section 3
Backflush espresso machine,daily,Bar,Backflush disc,Shift Lead,End of day

Quiz Items (Allergens, sample):
Q1: “Which extinguisher class is used for oil fires?” → Class K (explain why)
Q2: “Name two ways to prevent cross-contact.” → Separate utensils; dedicated prep area.
Acceptance Criteria (to pass judging)
Answers are traceable to documents (citations visible).
Procedure Mode works end-to-end, with at least one fryer or espresso flow.
Quiz + spaced repetition demonstrably resurface weak areas.
Dashboard shows at least: completion %, average score, last trained date.
Versioning visible in UI and logs.
Tech Notes (teams may choose their stack)
Ingestion: OCR (e.g., Tesseract), text chunking with headings preserved.
Indexing: Vector DB (e.g., FAISS, Chroma) + keyword fallback.
LLM: Any (open-source or API), but must constrain outputs to retrieved chunks; show “no answer” when ungrounded.
Frontend: Web app/PWA; support kiosk mode; multilingual i18n.
Analytics: Simple events → SQLite/Postgres; charts in dashboard.
Offline: Cache docs/index; queue checklist/quiz events until online.

