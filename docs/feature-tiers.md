# Feature Tiers: Launch vs Enhancement

## Launch (Required for the app to function)

### Authentication
- Email/password login (patient + clinic)
- Password reset flow
- 3 roles: Patient, Coordinator, Surgeon (scoped views per role)
- Session management (logout, active sessions)

### Patient Application
- 5-step application wizard (procedure, about you, medical history, lifestyle, review + signature)
- Save draft
- Application submitted confirmation

### Clinic Application Review
- Application queue with tabs (pending, approved, declined, more info requested)
- Patient detail view (medical history, lifestyle, body measurements, BMI)
- Approve / decline / request more info actions
- Internal notes per patient

### Scheduling
- Patient: calendar view, pick available date + time slot, confirm booking
- Clinic: OR calendar (week/day/month views, color-coded by surgeon)
- Manual booking by clinic staff
- Reschedule (up to 7 days before)

### Patient Dashboard
- Journey tracker (application → schedule → stay → payment)
- Pre-op checklist with links to relevant screens
- Status banner (approved, scheduled, etc.)

### Clinic Dashboard
- Today's surgeries list
- Applications to review count
- Unread messages count
- Recent activity feed

### Documents
- Patient: upload lab results (PDF/image)
- Patient: sign informed consent (digital signature)
- Patient: download clinic-provided PDFs (pre-op guide, diet plan, recovery guide, travel pack)
- Clinic: view uploaded documents per patient
- Clinic: consent form templates (view/edit)

### Payment
- Pay in full via Stripe (card payment)
- Deposit ($500) via Stripe + record balance on arrival
- Order summary (procedure, add-ons, discount, total)
- Clinic: record on-arrival payment (manual entry)
- Clinic: payment history per patient
- Refund processing (via Stripe)

### Messaging
- Async patient ↔ clinic messaging (threaded, per-patient)
- Unread indicators / badge counts
- File attachments
- "Online now" status indicator
- Clinic: quick reply templates

### Diet Plan
- 5-phase plan view assigned to patient (meals, targets, avoid lists per phase)
- Phase timeline with current phase indicator
- Day picker within each phase
- Supplements list

### Progress Tracking
- Weight logging (manual entry)
- Weight chart (actual vs projected)
- Daily check-in (weight, mood, water, walking, notes)
- Milestones timeline
- Progress photos (upload, 3-column grid, privacy note)
- Body measurements (7 measurements with deltas, update button)

### Travel & Stay
- Patient: enter travel dates, flight numbers
- Patient: select airport pickup option (SAN, TIJ, self-arrange)
- Patient: select hotel room type (standard, premium, family)
- Patient: companion info

### Aftercare
- Follow-up timeline (week 1, month 1, 3, 6, year 1, year 2+)
- Confirm / reschedule actions per appointment
- "Add to calendar" download (.ics file)

### Profile & Settings
- Patient: personal info, contact, emergency contact
- Patient: notification preferences (toggles)
- Patient: language, timezone, measurement system
- Patient: care team display (lead surgeon, assisting surgeon, coordinator)
- Patient: sign-in history log
- Patient: account deletion (soft delete + confirmation)
- Clinic: practice info, procedures + pricing

### Help Center
- Static FAQ page with accordion sections

### Notifications
- Email notifications (transactional: application status, new messages, appointment reminders)

### Clinic Patient Roster
- Patient list with stage tabs (pre-op, recovery, follow-up, discharged)
- Search + filter
- Weight loss column, next milestone column
- Overdue check-in flagging

### HIPAA Compliance
- Encryption (TLS + at rest)
- Audit logs (who accessed what, when)
- Access rules (patient isolation + 3 roles with scoped views)
- Secure hosting (HIPAA-ready infrastructure)
- MFA for clinic staff

---

## Enhancement (Can wait, app works without it)

### Video Consultations
- Use Google Meet or Zoom links manually for now
- Built-in video integration (Zoom Healthcare) can come later

### Integrations
- Google Calendar sync (staff can manually check OR calendar)
- Slack notifications (they have the in-app inbox)
- QuickBooks / Xero sync (export CSV manually for now)

### Advanced Analytics
- Revenue dashboards and charts
- Revenue by procedure breakdown
- Patient outcome analytics (avg weight loss, complication rates)
- Application funnel metrics
- Diet compliance analytics
- Aftercare adherence rates
- Coordinator performance metrics
- Revenue forecasting

### Earnings Page
- Monthly/YTD revenue stats
- Revenue bar chart with period toggles
- CFDI invoice generation (use accounting software for now)

### AI Features
- Risk scoring
- Application summaries
- Suggested approval actions
- AI messaging suggestions

### Advanced Diet Features
- Meal completion checkboxes / compliance tracking
- Shopping guides with store-specific brand recommendations
- Grocery list generation
- Recipe ideas
- International store localization (non-US patients)
- Diet plan CMS editor (assign from templates at launch, edit manually)

### Progress Enhancements
- Clinic-side progress detail view (weight chart, photos, measurements per patient)
- Anomaly detection / clinical alerts

### Advanced Scheduling
- Waitlist when slots are full
- Cancellation flow with automated refund trigger
- Concurrent booking prevention (optimistic locking)
- Surgeon vacation / unavailability blocking

### Travel Management (Clinic Side)
- Clinic-side travel dashboard (all arrivals, pickups, hotel bookings)
- Airport pickup scheduling / driver manifest
- Hotel reservation coordination
- Travel pack auto-generation
- Arrival confirmation flow

### Aftercare Management (Clinic Side)
- Clinic aftercare calendar across all patients
- Auto-generated follow-up appointments from surgery date
- Aftercare compliance tracking (who attended, who missed)
- Lab order generation for follow-up labs

### Team Management
- On-call schedule editing
- Staff invitation flow with role assignment
- Staff performance metrics
- Surgeon availability management

### Template System
- Full template editor UI (diet plans, consent forms, documents)
- Template versioning
- Merge field / PDF generation pipeline
- Automated trigger configuration

### Advanced Notifications
- SMS notifications via Twilio
- Web push notifications
- Notification center (bell dropdown with notification list)
- Notification batching / digest

### Advanced Compliance
- NOM-024 specific requirements
- Audit log viewer UI
- Data retention automation (HIPAA 6yr, NOM-024 7yr)
- Breach incident management workflow

### Spanish Language Support
- i18n infrastructure (English + Spanish)
- Spanish consent forms
- Patient portal language toggle

### Admin Panel
- Document review queue (cross-patient)
- Patient stage management rules
- Bulk communication / broadcast messaging

### Other
- Global search (patients, messages, documents)
- Public-facing landing page / intake
- Export all patient data (GDPR portability)
- Advanced payment methods (financing partner integration, "pay on arrival" trust flow)
