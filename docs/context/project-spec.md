# Project Spec: Full Feature Inventory

## What This App Is

Lumera is a **bespoke** patient portal and clinic management system for a single bariatric surgery practice based in Tijuana, Mexico, serving US and Mexican patients. Not a SaaS platform. Three user roles: **Patient**, **Clinic staff** (surgeons, coordinators, admin), and **Admin** (back-office operations, payment management, platform configuration).

The brand name is "Lumera" and the lead surgeon is Dr. Elena Cortes. The clinic operates out of Hospital del Valle in Tijuana's Zona Rio district, with a partner hotel (Hotel Sereno) 5 minutes away. Languages: English + Spanish.

### Confirmed Decisions

| Decision | Answer | Impact |
|----------|--------|--------|
| SaaS vs bespoke | **Bespoke** for one clinic | No multi-tenant architecture. Simpler auth, no tenant isolation, no white-labeling infrastructure. Settings/branding page is still needed for clinic self-service but not tenant-scoped. |
| Regulatory scope | **HIPAA + Mexican NOM-024** | US patients require HIPAA. Mexican law requires NOM-024. No GDPR (no EU patients). |
| Mobile strategy | **Responsive web** | No native apps. The UI must be fully responsive, especially for: daily diet check-ins, messaging, progress logging, document viewing. These are phone-first flows for patients. |
| User creation | **Simple flow, not a risk** | Standard email/password registration. Not a blocking unknown for estimation. |
| Messaging architecture | **Async** | Not real-time WebSocket chat. Email-like async messaging with notification delivery. Significantly simpler than live chat. No presence indicators, no typing indicators, no instant delivery requirement. |
| Payment processing | **Stripe** | Stripe for card payments. But the payment flow logic for different methods (full pay, deposit + balance, pay on arrival, financing) is **unspecified** and needs design. |
| Admin section | **Missing from prototype entirely** | The prototype has no admin panel. This is a significant unspecified surface area. |

---

## Complete Feature Inventory

### Critical Rule: Patient-Clinic Correspondence

**Every patient-facing action must have a corresponding clinic-side view.** Where the clinic cannot see or manage something the patient can do, it is flagged as MISSING below. These are not low-priority gaps — they are operational blockers.

---

### 1. Authentication & User Management

**Patient Portal:**
- Email + password login form
- "New here? Apply for consultation" link on login page
- Demo role picker (Patient / Clinic) for prototype purposes
- Session persistence (remember me implied)
- Profile-initiated password change (current + new password fields)
- 2FA toggle in profile settings (Twilio SMS; setup flow not shown)
- Active sessions viewer (see all devices, sign out individual or all)
- Sign-in history log in profile
- Account deletion ("Danger zone" button, with regulatory retention notice)
- Data export ("Download all my data" button in privacy settings)

**Clinic Console:**
- Email + password login form
- Session management (shared infrastructure with patient auth)
- Role-based access within clinic (permissions matrix unspecified)
- Invite team member flow (button shown on Team page)

**MISSING — Needs Design:**
- Password reset / forgot password flow
- Email verification on account creation
- 2FA setup flow (toggle exists, screen does not)
- Patient registration / account creation flow (can you apply without an account?)
- Role-based permissions matrix (what each of Dr. Elena, Dr. Mateo, Sofia, Valentina, Rafael can do)
- Admin role (confirmed as third role, zero prototype screens)
- Patient onboarding / first login experience (welcome wizard? empty dashboard?)
- Pre-login / public intake surface (prototype starts at login; nothing before it is designed)
- Clinic staff session management (shared devices, staff turnover)
- Authentication event audit log (required by HIPAA and NOM-024)

---

### 2. Dashboard

**Patient Portal:**
- Date eyebrow with personalized greeting (patient first name)
- Status banner showing current journey state with primary CTA
- 4-step journey tracker (Application, Schedule Date, Stay & Room, Payment) with done / current / locked / future states
- Pre-op checklist widget (5 items, each with status and link to relevant screen)
- Recent messages widget (2 most recent, with sender, snippet, timestamp)
- "Open inbox" button linking to Messages
- Bell notification icon with blue dot indicator in topbar

**Clinic Console:**
- Personalized greeting with date
- 3 summary stat cards (applications to review: 12, surgeries this week: 8, unread messages: 5)
- Today's surgeries list (time, patient, procedure, OR room, duration, pre-op status)
- Recent applications widget (3 clickable entries with patient, procedure, BMI, country, time, status)
- Activity feed (timestamped events: consent signed, payment received, application approved, etc.)
- Monthly progress card (surgeries completed vs target, MoM change)

**MISSING — Needs Design:**
- Clinic-side patient alerts on dashboard (overdue check-ins, missed aftercare, flagged labs)
- Admin dashboard (payment pipeline, compliance tasks, support queue)

---

### 3. Application / Intake Pipeline

**Patient Portal:**

Step 1: Procedure Selection
- 5 procedure cards (VSG $3,795, SILS $4,495, RNY $4,795, SADI-S $5,795, Bariatric Revision $4,545)
- "Not sure yet / free consult" option
- Each card: name, price, short description, "best for you if" criteria

Step 2: About You
- First name, last name, age, gender (Male / Female / Other / Prefer not to say), phone, email
- Country dropdown (~11 options + Other), state/region field

Step 3: Medical History
- Imperial / Metric unit toggle
- Height, current weight, highest adult weight, auto-calculated BMI (read-only)
- Prior bariatric surgery (yes/no), prior abdominal surgeries (textarea), known surgical conditions (yes/no)
- 8-condition diagnosis checklist (asthma/COPD, hypertension, diabetes, sleep apnea, heart disease, thyroid, GERD, fatty liver)
- HIV/hepatitis screening (yes/no) with confidentiality note
- Medications textarea (with dosage hint) and allergies field

Step 4: Lifestyle & Motivation
- Smoking (3 options), alcohol (4 options), recreational substances (3 options)
- Blood clot history (3 options), blood thinners (yes/no)
- Physical activity level (4 options)
- Previous weight loss attempts (textarea)
- Why seeking surgery now (textarea)
- Recovery support system (textarea)
- How heard about Dr. Elena (dropdown)

Step 5: Review and Submit
- 5 collapsible review sections with "Edit" links per step
- Accuracy attestation + HIPAA consent checkbox
- Click-to-sign signature area (cursive display, clear button)

Step 6: Success State
- Green checkmark, "Application submitted" heading
- 3 next-step cards (48-hour review, video consult if recommended, schedule after approval)

Every step:
- Step progress bar (clickable for navigation)
- HIPAA/encryption trust banner at bottom
- "Save draft" button (Steps 2–4)

**Clinic Console:**
- Applications list with tabs: Pending (12), Approved (45), More info requested (3), Declined (8)
- Search by patient name + filter by procedure / country / status
- Table columns: Patient, Procedure, BMI, Age, Country, Applied date, Status pill
- Pagination

Patient Detail / Review screen:
- Patient header (avatar, name, location, age, email, phone, status pills, time since applied)
- 4 action buttons (Message, Decline, Request more info, Approve application)
- Body measurements card with BMI classification
- Medical history summary card
- Lifestyle summary card
- Uploaded documents list (blood panel, ECG) with View button
- Internal notes (threaded team notes, Add note textarea)
- Suggested action card with rationale

**MISSING — Needs Design:**
- "Request more info" return flow (what does patient see: message or reopened form fields?)
- Decline flow patient experience (reason shown? appeal? reapply? cooling-off period?)
- "Not sure yet" consult scheduling (separate scheduling flow — nothing designed)
- Save draft behavior (auto-save interval? server-side or session only? draft expiry?)
- BMI eligibility validation (what happens if BMI is below threshold?)
- Application versioning (amend post-submission? switch procedures mid-process?)
- Multiple applications (reapply after decline? duplicate detection?)
- Clinic visibility of incomplete / abandoned drafts
- Application assignment to staff (random or coordinator-assigned?)
- Document review queue across all patients (no cross-patient view for newly uploaded docs)

---

### 4. Scheduling & OR Calendar

**Patient Portal:**
- Procedure confirmation card (procedure, surgeon, duration, post-op stay, price)
- Monthly calendar view (prev/next navigation, available dates blue dot, unavailable grayed, today bolded)
- Time slot picker (4 slots per day with OR room and notes)
- Booking summary (surgeon, hospital, arrival instructions)
- Confirm booking button
- Reschedule policy note ("up to 7 days before surgery")
- Times labeled as local to Tijuana (PST/PDT)

**Clinic Console:**
- OR Calendar week view (Mon–Fri, 7AM–5PM time grid)
- Color-coded by surgeon: Dr. Elena (blue), Dr. Mateo (green), Consultations (amber)
- Events show patient, procedure, OR room, estimated duration
- Prev/next week navigation, Today button, Week/Day/Month toggle
- "Add booking" button (manual creation)

**MISSING — Needs Design:**
- OR availability management (who sets bookable dates/slots? no availability editor shown)
- Patient-initiated rescheduling flow (note says "up to 7 days" but no flow designed)
- Clinic-initiated rescheduling flow (notification to patient?)
- Cancellation flow (refund trigger? slot re-opens? waitlist?)
- Clinic-side reschedule/cancellation request queue
- Surgeon unavailability / vacation blocking
- Concurrent booking prevention (optimistic locking for simultaneous selections)
- Waitlist (when all slots in a date range are full)
- Time zone conversion tool for international patients
- Booking confirmation flow (email? calendar invite? SMS reminder chain?)
- "Not sure yet" consult scheduling (separate flow, nothing designed)
- OR room assignment rules (manual or availability-based?)
- Pre-op clearance status management (no screen for updating "awaiting consent" / "pre-op cleared")

---

### 5. Diet Plan System

**Patient Portal:**

- Phase timeline strip (5 clickable cards: phase name, duration, date range, progress bar)
- Day picker (horizontal scrollable pills with done / today / selected / future states)

Phase 1: Liver Shrinking Diet (14 days)
- 4 meals/day (breakfast, lunch, snack, dinner) with time, name, calories, protein, ingredients
- Daily nutrition targets (1,400 cal, 100g protein min, 80g carbs max, 64oz water min)
- "Avoid" list (7 items)
- US shopping guide with brand recommendations and estimated weekly cost ($85–$110)

Phase 2: Clear Liquids Only (1 day pre-surgery)
- Allowed items list (water, sugar-free electrolytes, clear broth, black coffee/tea, sugar-free Jello)
- Midnight cutoff rule and 2-hour pre-surgery NPO rule
- "Strict no" list (7 items including colored beverages)

Phase 3: Hospital Recovery (3 days)
- Day-by-day schedule (surgery day: ice chips then 1oz/hr; Day 1: clear liquid progression; Day 2: discharge)
- Protein targets per day
- Hospital no-list (no straws, no gulping, no lying flat, no sugar, no carbonation)

Phase 4: Full Liquid Recovery (14 days)
- 6 meals/day (protein shakes, strained soups, yogurt drinks)
- Rotating shake flavors to prevent fatigue
- Daily targets (120g protein, 64oz fluid, ~600 cal)
- Sip rules card
- Supplements list with brands and dosages
- Shopping guide ($60–$80/week)

Phase 5: Pureed Foods (14 days)
- 5 small meals/day, max 1/4 cup per meal
- Daily targets (82g protein, 64oz fluid, ~570 cal)
- "Not yet" list (chunky/stringy foods, bread/rice/pasta, raw veggies, tough meat, spicy/acidic, drinking with meals)
- Supplements card, shopping guide ($55–$75/week)

Sidebar (all phases):
- Upcoming phases preview strip
- Quick links: grocery list, recipe ideas, ask Sofia, FAQ
- "Why this phase matters" card
- Shopping guide link

**Clinic Console:**
- Diet plan templates page (5 templates by procedure, last updated date, Edit button)
- Create new template button

**MISSING — Needs Design:**
- Clinic-side diet compliance view (which phase, following it?, compliance metrics)
- Meal completion / compliance tracking (no checkboxes on meal cards; streak metric is uncalculable)
- Diet plan assignment to individual patient (auto from procedure template? clinic customizes? when triggered?)
- Plan start date calculation from actual surgery date (currently hardcoded example dates)
- Diet plan editor CMS (what does "Edit" open? structured form? WYSIWYG? developer-only?)
- Allergy/dietary restriction adaptation (no substitution logic for patient-provided allergies)
- International shopping guides (US store references only; Mexican patients may need local store options)
- Phases 6+ beyond pureed foods (soft foods, regular foods, long-term eating — nothing after Phase 5)
- Grocery list generation ("Phase X grocery list" quick link exists but no feature behind it)
- Recipe ideas ("Recipe ideas for Phase X" quick link exists but no content system)
- Diet compliance alerts (who gets alerted when patient is behind, and how?)

---

### 6. Travel & Stay

**Patient Portal:**
- Travel dates form (arrival date/time, departure date/time, inbound/outbound flight numbers)
- Info banner ("We'll confirm with our coordinator")
- Airport pickup options (3 radio choices):
  - San Diego International (SAN): cross-border van, ~90 min
  - Tijuana International (TIJ): direct hotel transfer, ~15 min
  - Self-arrange (no pickup needed)
- Hotel room type options (3 radio choices):
  - Standard room (included in package)
  - Premium suite (+$420)
  - Family suite (+$890)
- Travel companion form (name, relationship, phone, dietary preferences)

**Clinic Console:**
- (No corresponding screen in prototype — see MISSING below)

**MISSING — Needs Design:**
- Clinic travel management view (all patients' arrival dates, airports, room types, companions in one place)
- Airport pickup scheduling and driver manifest workflow
- Hotel reservation coordination with Hotel Sereno (no admin interface shown)
- Companion dietary preferences visible to clinic (needed for hotel/hospital coordination)
- Travel document requirements screen (passport, visa, other docs)
- Travel pack generation trigger and flow ("Travel itinerary pack" template exists but no generation UI)
- Arrival confirmation flow (how does clinic mark a patient as "arrived"?)
- Border crossing logistics (SAN van service: liability, documentation, operator coordination)
- Last-minute travel change flow (patient updates flight; clinic notification?)

---

### 7. Documents & Consent

**Patient Portal:**
- Action required section with status pills per document:
  - Informed consent (Awaiting signature → Sign now)
  - Lab results (Not uploaded → Upload)
  - Medical history form (Complete)
- Status pills: Awaiting signature, Not uploaded, Complete, Signed, Uploaded
- Resources section (clinic-provided PDFs with Download buttons):
  - Pre-op guide (18 pages)
  - Liquid diet plan (6 pages)
  - Recovery & aftercare guide (24 pages)
  - Travel pack (8 pages)
- File upload interaction (Upload button; specific UI not shown in detail)
- In-app click-to-sign flow (from Step 5 of application)

**Clinic Console:**
- Uploaded documents in patient detail view (blood panel, ECG, View button per doc)
- Templates page:
  - Consent forms: bariatric surgery consent, anesthesia consent, HIPAA/data processing consent (English/Spanish)
  - Document templates: discharge summary (auto from surgery notes), lab order request, travel itinerary pack
  - Edit button per template, Create new template button

**MISSING — Needs Design:**
- Document review queue across all patients (no cross-patient view for newly uploaded docs)
- Clinic document approval/rejection workflow (review labs, flag issues, request re-upload)
- Consent countersignature by physician (patient signs but no surgeon co-sign flow)
- E-signature technology decision (custom canvas vs DocuSign/HelloSign; legal validity under HIPAA and NOM-024)
- Document generation from merge-field templates (what populates fields? what generates the PDF?)
- Consent versioning (which version did patient sign? re-sign required when form updates?)
- Multi-language document support (Spanish-language docs for Mexican patients; translation workflow undefined)
- File upload UX specifics (drag-and-drop? progress bar? file type/size limits? virus scanning?)
- Post-op lab result uploads (distinct from pre-op labs; no upload or view flow exists)
- Lab order generation and delivery to patient's local clinic

---

### 8. Payment & Billing

**Patient Portal:**
- 4 payment method options (radio):
  1. Pay in full now (5% discount, card or bank transfer, refundable up to 14 days before)
  2. Deposit ($500) + balance on arrival ($500 Stripe now, remainder at clinic)
  3. Pay on arrival (no online payment at booking)
  4. Financing through clinic partner ("from $159/month," soft credit check)
- Card payment form (cardholder name, card number, expiry, CVC)
- "Processed by Stripe. We don't store card details." notice
- Order summary sidebar (procedure, surgeon, surgery date, room/stay, line items, discount, total)
- Refund deadline note
- Step labeled "Step 4 of 4" (payment is final booking step)

**Clinic Console:**
- Earnings page:
  - 4 stat cards (this month: $198.7k, YTD: $718.2k, outstanding: $12.4k, refunds: $5.2k)
  - 12-month revenue bar chart with period toggle (Week / Month / 12mo / YTD)
  - Summary (12-month total: $1.73M, avg/month: $144k, avg/surgery: $4,720)
  - Recent payments table (patient, procedure, amount, method, date)
  - Revenue by procedure bar chart + percentages
  - Outstanding balances list with "Send all reminders" button
  - Accountant integrations (QuickBooks Online auto-sync, Xero not connected, CFDI/SAT Mexico, CSV/Excel export)

**MISSING — Needs Design:**
- Payment flow state machine per method:
  - "Pay in full": charge Stripe, mark paid, issue receipt
  - "Deposit + balance": $500 Stripe now; how is in-person balance recorded and linked?
  - "Pay on arrival": who records it? POS? manual entry? no-show prevention?
  - "Financing": partner name (Affirm? CareCredit? United Medical Credit?), integration type, v1 scope?
- Staff payment recording form (manually record cash or on-arrival payment — critical operational gap)
- Outstanding payments action workflow (call patient, record payment, mark paid, generate receipt)
- Deposit-to-balance reconciliation workflow
- Refund processing workflow (who approves? Stripe API trigger? partial refunds?)
- Invoice and receipt generation (CFDI via SAT: no workflow for triggering or sending)
- "Pay on arrival" no-show prevention mechanism (authorization hold? binding agreement?)
- Failed payment handling (declined card: retry flow? clinic notification?)
- Currency display for international patients (USD shown; local currency equivalent?)
- Financing partner integration (entirely unspecified)
- Price variation rules beyond suite add-ons (complexity surcharge, companion meals, cosmetic add-ons?)
- Payment method change after initial selection

---

### 9. Messaging & Communication

**Patient Portal:**
- Split-pane layout (conversation list left, thread right)
- 3 conversation threads (Dr. Elena Cortes, Sofia, Dr. Mateo Aguilar)
- Thread view (day dividers, outgoing blue bubbles, incoming white bubbles, timestamps)
- Compose area (textarea, attachment button, send button)
- Video call button in thread header (Zoom for Healthcare)
- "Online now" status indicator in thread header
- SLA notice ("Replies within 4 working hours")
- Badge count on Messages nav item

**Clinic Console:**
- Same chat shell from clinic perspective
- Quick reply strip (3 template buttons: Schedule a call, Send pre-op guide, Refer to coordinator)
- "Open profile" button in thread header (jumps to patient detail)
- Multiple patient threads in conversation list
- Messages stat card on Overview (5 unread)
- Badge count on Messages nav item (5)

**MISSING — Needs Design:**
- Async notification delivery for new messages (email? SMS via Twilio? web push? no spec exists)
- Notification center (bell icon with blue dot leads nowhere; no dropdown or page)
- Automated message triggers (welcome on approve, 7-day pre-op check-in listed but full trigger list undefined)
- File attachment specification (allowed types? size limits? inline image preview? virus scanning?)
- Video call implementation (embedded Zoom room vs Zoom link? HIPAA-compliant Zoom config?)
- Escalation handling (patient not replying, clinical message unread — no monitoring shown)
- Mass/broadcast messaging (no way to notify all pre-op patients of a schedule change)
- Message search within threads or across conversations
- Read receipts (decision point for async model)
- Translation support for non-English patients
- Conversation assignment (which staff handles which patient thread?)

---

### 10. Progress Tracking

**Patient Portal:**
- 3 stat cards (current weight with delta, % toward goal with target, diet streak in days)
- Weight log chart (SVG line: actual solid vs projected dashed, today marker, surgery day marker)
- "Log today's weight" button
- Daily check-in form (weight, mood emoji selector, water intake, walking minutes, optional notes)
- Progress photos section (3-column grid, privacy note: "Only shared with your care team")
- Body measurements section (7 measurements: waist, hips, chest, left/right arm, left/right thigh, each with delta)
- "Update measurements" button
- Milestones timeline (6 milestones with date and done/future status)
- "Auto-sharing with care team" note on page

**Clinic Console:**
- Weight loss column in patients table (aggregate, e.g., "−12.3 kg")
- Next milestone column in patients table
- Overdue check-in highlighting (3 overdue in yellow in patient list)

**MISSING — Needs Design:**
- Clinic-side patient progress detail view (weight chart, check-in history, mood, water, walking, notes — none shown)
- Clinic-side progress photos view (shared with care team but no clinic screen exists)
- Clinic-side body measurements view (no screen for 7-measurement history or trends)
- Overdue check-in management workflow (who contacts patient? how is contact logged? how is status cleared?)
- Anomaly detection and clinical alerts (thresholds for rapid weight gain, plateau, consecutive missed check-ins)
- Weight logging method (manual input shown; smart scale integration? CSV? Apple Health / Fitbit?)
- Progress photo storage spec (S3/encrypted, file size/format limits, compression)
- Post-op lab results integration (Month 3/6/Year 1 labs required but no upload or view flow)
- Body composition data capture (Year 1 body composition scan — manual entry? DEXA upload?)
- Diet compliance linkage to streak metric (streak cannot be calculated without meal completion input)

---

### 11. Aftercare & Follow-up

**Patient Portal:**
- Status banner ("No extra cost, ever")
- Vertical timeline with done / next / future states:
  - Surgery day
  - Week 1: coordinator phone call (15 min)
  - Month 1: video consult with Dr. Elena (30 min)
  - Month 3: video consult + lab work required
  - Month 6: video consult + labs + body composition scan
  - Year 1: annual review (45 min)
  - Year 2+: annual check-ins, described as lifelong
- Each item: timing label, title, scheduled date/time (or "TBD"), format, agenda
- Actions per item: Confirm attendance, Reschedule, Add to calendar
- 24-hour care line phone number displayed prominently

**Clinic Console:**
- No dedicated aftercare management screen in prototype
- Patients table shows "Next milestone" column and stage tabs (Follow-up 87, overdue 3)
- Patient stages: Follow-up 3mo / 6mo / 12mo, Overdue, Discharged

**MISSING — Needs Design:**
- Clinic-side aftercare schedule management (upcoming appointments across all patients; confirm/decline status)
- Aftercare appointment scheduling automation (auto-generated from surgery date vs coordinator creates manually?)
- Aftercare compliance tracking (who attended Month 1? who is overdue for Month 3 labs?)
- "Reschedule" flow for aftercare items (calendar picker? through coordinator? message?)
- Lab order generation for follow-up labs (who sends it to patient's local clinic? how?)
- Clinical notes from follow-up consults (where do surgeons document Month 1/3/6 outcomes?)
- Discharge workflow (what triggers it? who triggers it? what does patient see post-discharge?)
- Emergency / complication escalation (24-hour line shown but no in-portal urgent-flag flow)
- Lifelong follow-up management at scale (hundreds of patients at varying aftercare stages)

---

### 12. Profile & Settings

**Patient Portal:**

Personal information:
- First name, last name, date of birth, gender, preferred name, profile photo upload

Contact information:
- Email, phone, mailing address, emergency contact (name + phone)

Care team display:
- Lead surgeon, assisting surgeon, care coordinator (read-only)

Notification preferences:
- 7 toggles (appointment reminders, new messages, daily diet reminders, weekly progress summary, pre-op milestone alerts, post-op check-in reminders, marketing/newsletter)
- Notification channel preference selector

Language & region:
- Language, timezone, date format, measurement system (Imperial / Metric)

Account & security:
- Change password (current + new)
- 2FA toggle (SMS via Twilio)
- Active sessions list (Desktop Chrome, iPhone Safari; "Sign out all" button)
- Sign-in history log

Privacy:
- Share progress photos with research/marketing toggle
- Anonymized data for aggregate research toggle
- Download all my data button

Danger zone:
- Delete account button with regulatory retention notice

**Clinic Console:**
- Practice info (name, lead surgeon, CONACEM license, RFC tax ID, address, phone, email, website)
- Procedures and pricing (5 procedures with editable prices)
- Hospital partner (Hospital del Valle, 2 dedicated OR rooms)
- Hotel partner (Hotel Sereno, room rates)
- Patient portal branding (name, logo, primary color, portal URL, custom email sender)
- Integrations:
  - Google Calendar (connected): OR schedule sync
  - Stripe (connected): card payments
  - Twilio SMS (connected): reminders and 2FA
  - Slack (not connected): critical alerts
  - Zoom for Healthcare (connected): HIPAA-compliant video consults
- Compliance section:
  - HIPAA BAA status (signed and current)
  - CONACEM accreditation (renewed Jan 2026, valid through 2029)
  - Data hosting (US + Mexico)
  - Audit log retention (7 years per Mexican health regulations)

**MISSING — Needs Design:**
- Profile change notifications to clinic (emergency contact, phone, address updates)
- 2FA setup flow (toggle exists; no enrollment screen)
- Clinic staff individual profiles (personal notification prefs, password change, own 2FA)
- Audit log viewer (retention confirmed in settings but no search/view screen)
- Data export specification (what's included? format? generation time? email link?)
- Account deletion compliance flow (steps, cooling-off period, what's retained vs deleted, HIPAA/NOM-024 retention requirements)
- Clinic integration setup flows (OAuth flows for Google Calendar, Stripe, Twilio, Zoom, Slack)

---

### 13. Team Management

*(Clinic console only — no patient-facing equivalent)*

**Clinic Console:**
- 3 stat cards (total team: 5, on-call now: 2, surgeries this week: 11)
- Surgeon profiles (photo, name, online/offline status, role, email, experience, lifetime surgeries, active patients, on-call badge)
- Staff profiles (Sofia: coordinator, Valentina: patient liaison, Rafael: admin)
- On-call weekly schedule grid (Surgeon row + Coordinator row × 7 days)
- "Invite team member" button

**MISSING — Needs Design:**
- Invite team member flow (no invitation screen; how is role/permissions assigned during onboarding?)
- Role and permissions management screen (roles shown read-only; no editor)
- On-call schedule editing (grid is read-only; who edits it?)
- Individual staff member detail view (patient assignments, messages, upcoming surgeries)
- Surgeon unavailability / vacation blocking (no mechanism; related to Scheduling gap)
- Staff deactivation without losing historical records
- Performance metrics per staff member (response times, cases handled)

---

### 14. Templates

*(Clinic console only — no patient-facing equivalent)*

**Clinic Console:**
- Diet plan templates (5 by procedure, last updated date, Edit button)
- Consent form templates (bariatric surgery consent, anesthesia consent, HIPAA/data processing consent; English/Spanish; Edit button)
- Quick reply templates (welcome auto-sent on approval, 7-day pre-op check-in auto, schedule a call manual, refer to coordinator manual; usage count; Edit button)
- Document templates (discharge summary auto from surgery notes, lab order request, travel itinerary pack; Edit button)
- "Create new template" button

**MISSING — Needs Design:**
- Template editor UI per type:
  - Diet plan: structured day-by-day meal editor (nutrition targets, shopping guide, avoid lists)
  - Consent forms: rich text / legal document editor
  - Quick replies: text editor with merge field support
  - Document templates: merge-field editor with PDF preview
- Template versioning (which version did each patient sign? version history?)
- Automated trigger configuration (conditions, timing, patient segment for "auto" sends)
- Template assignment to individual patients (no assignment workflow UI)
- Merge field specification (full list of available fields; rendering pipeline undefined)
- Multi-language template management (English/Spanish; linking translated versions)

---

### 15. Analytics & Earnings

*(Clinic console only — no patient-facing equivalent)*

**Clinic Console:**

Earnings page:
- 4 stat cards (this month: $198.7k, YTD: $718.2k, outstanding: $12.4k, refunds: $5.2k)
- 12-month revenue bar chart with period toggle (Week / Month / 12mo / YTD)
- Summary (12-month total: $1.73M, avg/month: $144k, avg/surgery: $4,720)
- Recent payments table (patient, procedure, amount, method, date)
- Revenue by procedure bar chart + percentages
- Outstanding balances list with "Send all reminders"
- Accountant integrations (QuickBooks Online auto-sync, Xero not connected, CFDI/SAT 42 this month, CSV/Excel export)

**MISSING — Needs Design:**
- Patient outcomes analytics (avg weight loss by procedure, complication rates, success rate by BMI band)
- Application funnel analytics (apply → approved → scheduled → paid → surgery → aftercare completion rates)
- Diet compliance analytics (phase completion rates, phase-level dropout, avg compliance score)
- Aftercare adherence analytics (follow-up attendance rate, drop-off by milestone)
- Coordinator performance metrics (avg response time, patients managed, overdue resolution rate)
- Revenue forecast (projected revenue for 30/60/90 days based on scheduled surgeries and payment methods)
- Refund detail log (reason, amount, initiator, date, patient — only summary shown currently)
- CFDI invoice management (generate, review, correct individual invoices — no workflow shown)

---

### 16. Admin Operations

*(Entirely missing from prototype — three confirmed roles: Patient, Clinic staff, Admin)*

**Payment Administration:**
- Outstanding payments dashboard (who owes what, payment method, surgery date, balance due)
- Payment recording form (manually record cash or on-arrival payment: patient, amount, method, date, staff)
- Deposit-to-balance reconciliation (link $500 Stripe deposit to in-person balance; handle partial payments)
- Refund processing (initiate, reason, full/partial, Stripe API trigger, outcome recorded)
- Financing application tracking (status per patient: pending, approved, denied, funded)
- Payment dispute and chargeback handling workflow
- Manual invoice and receipt generation (trigger CFDI per patient, send receipt)
- Payment reminder management (individual and bulk for outstanding balances)
- Financial ledger export (per-patient payment history, full transaction log)

**Patient Journey Operations:**
- Diet plan assignment (correct template to newly approved patient, start date from surgery date)
- Aftercare schedule generation (auto-generate or manually create all follow-ups at surgery completion)
- Travel coordination (arriving patients by date, airport pickups, hotel confirmations, travel packs)
- Day-of-surgery checklist (pre-op clearance, OR assignment, anesthesia clearance, hospital notification)
- Hospital intake form (post-op: fluids, medications, pain levels)
- Discharge processing (criteria checklist, discharge summary from template, hand off to aftercare)
- Patient stage management (transitions between Pre-op, Recovery, Follow-up, Discharged)

**Document Management Operations:**
- Document review queue (newly uploaded docs across all patients requiring clinic review)
- Lab result review and approval workflow (view, flag issues, request re-upload with instructions)
- Consent countersignature workflow (surgeon reviews and countersigns patient consent forms)
- Lab order generation (merge patient data, track whether results received)
- Bulk document distribution (send pre-op guide or travel pack to a patient stage/procedure group)

**Compliance Administration:**
- Audit log viewer (searchable: who accessed which patient record, when, what action; required by HIPAA and NOM-024)
- Patient data export and deletion processing (HIPAA right of access; NOM-024 patient record requirements; generate data package)
- Consent registry (per-patient: consents given, when, version, revocations)
- Breach incident management (document breach, track notification deadlines, affected patients, mitigation)

---

### 17. Compliance & Audit

*(Cross-cutting concern — applies to both portals and admin layer)*

**Confirmed regulatory scope: HIPAA + NOM-024**

HIPAA requirements:
- Encrypted data at rest and in transit (AES-256 / TLS 1.2+)
- Minimum necessary access principle (drives role permissions matrix)
- BAA signed (confirmed in Settings)
- PHI access logging (user, timestamp, action per patient record access)
- Breach notification within 60 days
- 6-year record retention minimum

NOM-024 requirements (Mexican electronic health records standard):
- Specific audit trail format requirements
- Data residency in Mexico for clinical records
- Clinical record retention: 5 years minimum; audit logs: 7 years

Cross-jurisdiction conflicts:
- HIPAA 6-year retention vs NOM-024 5-year clinical / 7-year audit log retention (apply the stricter requirement per record type; document the rationale)
- Breach notification: HIPAA 60 days + NOM-024 requirements (confirm NOM-024 timeline with legal counsel)

**MISSING — Needs Design:**
- Audit log viewer UI (7-year retention confirmed but no search/view/export screen)
- Patient data export and deletion workflow (HIPAA right of access; account deletion with retention hold on required records)
- Consent registry (per-patient consent record with version and revocation tracking)
- Privacy policy and terms of service (prototype shows HIPAA notice only; two-jurisdiction policy needed: HIPAA + NOM-024)
- Data retention automation (records auto-flagged for review or deletion per retention schedules)

---

### 18. Notifications

**Patient Portal:**
- Bell icon in topbar with blue dot indicator
- Badge count on Messages nav item
- 7 notification preference toggles in profile settings
- Notification channel preference selector in profile settings

**Clinic Console:**
- Bell icon in topbar with notification indicator
- Badge count on Messages nav item (5)
- Badge count on Applications nav item (12)
- Activity feed on Overview dashboard (timestamped events)

**MISSING — Needs Design:**
- Notification center (bell icon leads nowhere in prototype; dropdown or page needed)
- Notification types and triggers specification:
  - Patient: application status change, new message, surgery reminder (7 days, 24 hours), diet phase starting, check-in reminder, aftercare appointment reminder
  - Clinic: new application submitted, document uploaded, aftercare confirmed, payment received, payment failed
- Notification delivery channels (email service: SendGrid? Postmark?, SMS via Twilio, web push, in-app)
- Web push implementation (service worker, browser permission prompt UX)
- Email template design (transactional emails matching Lumera brand)
- Automated trigger configuration (full rule set undefined beyond two examples in Templates)
- Notification batching (digest for multiple rapid events vs individual sends)
- Notification read/dismiss state (what marks a notification as read?)

---

### 19. Platform (Search, Help, Navigation)

**Patient Portal:**
- Sidebar navigation grouped as "Care journey": Dashboard, Application, Schedule, Diet plan, Progress, Travel & stay, Documents, Payment, Aftercare
- Sidebar navigation grouped as "Support": Messages, Profile, Help center
- Badge count on Messages sidebar item
- Bell notification icon with blue dot in topbar
- Search icon in topbar
- Breadcrumb in topbar + page title
- "Switch demo view" button in topbar (prototype only)
- User chip at sidebar bottom (avatar, name, role label)
- Help Center: placeholder card only (searchable knowledge base, FAQs, direct contact described but not built)

**Clinic Console:**
- Sidebar navigation labeled "Clinic console" with Lumera brand logo
- Sections: Practice (Overview, Applications badge 12, Patients, OR Calendar, Messages badge 5) and Practice admin (Earnings, Team, Templates, Settings)
- User chip at sidebar bottom (Dr. Elena Cortes, Bariatric surgeon)
- Bell notification icon in topbar
- Search icon in topbar
- Breadcrumb in topbar + page title
- "Switch demo view" button in topbar (prototype only)

**MISSING — Needs Design:**
- Search results page (icon exists but leads nowhere; what is searchable in each context?)
- Help center content system (CMS-backed articles? static FAQs? AI chatbot? live chat handoff?)
- "Ask Sofia" feature (quick link on diet plan: AI chatbot? direct message to coordinator Sofia? significant scope if AI)
- Mobile navigation (sidebar is desktop-oriented; hamburger or bottom nav treatment needed)
- Landing page / public intake (pre-login surface entirely unspecified)
- Patient onboarding / first login experience (empty dashboard state; guided setup wizard?)

---

## Decisions Resolved

| # | Question | Answer | Date |
|---|----------|--------|------|
| 1 | SaaS vs bespoke | **Bespoke** for one clinic | 2026-05-11 |
| 2 | Account creation flow | **Simple flow**, not a risk | 2026-05-11 |
| 3 | Real-time vs async messaging | **Async** messaging | 2026-05-11 |
| 4 | Mobile strategy | **Responsive web** (no native apps) | 2026-05-11 |
| 5 | Regulatory scope | **HIPAA + NOM-024** | 2026-05-11 |
| 6 | Payment processor | **Stripe** | 2026-05-11 |

---

## Open Questions

Sorted by impact on scope and estimation.

### Must answer before estimating

**1. Admin section scope**
- Which items in Section 16 are in scope for v1? (payment administration only? full operations?)

**2. Payment flow state machines**
- "Pay on arrival": who records it? POS terminal? manual admin panel entry?
- Deposit reconciliation: how does $500 Stripe deposit link to in-person balance payment?
- Financing: which partner (Affirm? CareCredit? United Medical Credit?)? In scope for v1?

**3. Diet plan authoring**
- Full CMS editor (Dr. Elena creates/edits meals, macros, shopping lists), or developer-maintained templates?

**4. Stripe configuration**
- Stripe US or Stripe Mexico? (affects currency, payout timing, CFDI generation)

**5. "Ask Sofia" scope**
- AI chatbot, direct message to coordinator Sofia, or something else?

**6. Clinic-side progress and compliance views**
- Patient-level progress dashboard tab, or compliance monitoring dashboard across all patients?

### Should answer before estimating

**7. Languages at launch**
- English only at launch? English + Spanish?

**8. Existing system and data migration**
- Greenfield, or existing patient data to migrate?

**9. Compliance implementation depth for v1**
- Full HIPAA + NOM-024 from day one, or baseline security first with compliance features added incrementally? Which NOM-024 requirements (audit trail format, data residency) are enforced at launch vs deferred?

**10. Aftercare scheduling automation**
- Auto-generated from surgery date, or coordinator creates each appointment manually?

**11. Template editing scope**
- Rich-text document editor needed, or templates are developer-managed and "Edit" is scoped out of v1?

**12. Notification channels**
- Email only? Email + SMS (Twilio connected)? Web push?

**13. "Request more info" return flow**
- Message with instructions, or specific form fields that reopen?

**14. "Not sure yet" consultation flow**
- Separate scheduling system? Zoom calls? How does it convert to a surgery application?

**15. HIPAA vs NOM-024 retention conflict resolution**
- Legal documentation needed before implementing account deletion and data export (HIPAA 6-year vs NOM-024 5-year/7-year; which record types are governed by which standard?)
