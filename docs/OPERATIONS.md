# Operations And Backend Architecture Decisions

## Purpose

This file defines environment setup and backend architecture decision rules for Supabase, Razorpay, scheduled jobs, database functions, triggers, and Edge Functions.

## Required Setup Before Building

Create these before implementation starts:

- A Git repository for the project.
- A public GitHub repository for portfolio showcase.
- Two separate Supabase projects:
  - `the-classy-code-lms-dev`
  - `the-classy-code-lms-prod`
- Razorpay test credentials for development.
- Razorpay live credentials only when production checkout is ready.

Recommended workflow:

- Develop locally against the dev Supabase project or local Supabase CLI environment.
- Store schema changes as migrations in the repo.
- Apply migrations to dev first.
- Apply the same reviewed migrations to prod only after testing.
- Never make production-only schema changes manually in the Supabase dashboard.

Supabase's official deployment docs recommend managing multiple environments with migrations. Supabase also supports branching, but this project will start with two explicit projects: dev and prod.

## Environment Rules

- `.env.example` is committed.
- `.env.local`, `.env.development.local`, `.env.production.local`, and real secret files are never committed.
- Public browser keys may use `NEXT_PUBLIC_` only when they are intended for the client.
- Service role keys stay server-only.
- Razorpay secrets and webhook secrets stay server-only.
- Production data must never be copied into the public repo.

## Decision Matrix

Use the simplest correct place for each responsibility. Do not push all logic into route handlers, database triggers, or Edge Functions by default.

### Use Next.js Server Actions Or Route Handlers When

- The logic is request/response driven by the UI.
- It needs current authenticated user context.
- It coordinates validation, redirects, UI errors, or form submission.
- It calls Supabase with normal user permissions.
- It creates a Razorpay order or verifies a checkout response.

Examples:

- Create course form.
- Update lesson content.
- Enroll in a free course.
- Create Razorpay order.
- Verify Razorpay payment.

### Use Supabase RPC When

- The operation must be atomic inside Postgres.
- Multiple SQL writes must succeed or fail together.
- The operation benefits from database-side permission checks.
- The client/server needs a stable database API for a complex query.
- The logic is data-centric and does not need external network calls.

Examples:

- Atomically grant enrollment after verified payment.
- Reorder lessons and topics.
- Mark lesson complete and update aggregate course progress.
- Calculate course progress from lesson records.

Rules:

- Keep RPC functions focused.
- Avoid hiding broad application workflows inside one giant RPC.
- Document any RPC in `docs/DATABASE.md`.

### Use Database Triggers When

- A database invariant must always be maintained no matter which app path writes the row.
- The side effect is small, local to the database, and deterministic.
- The operation should happen automatically for inserts/updates/deletes.

Examples:

- Create a `profiles` row after auth user creation, if implemented database-side.
- Maintain `updated_at`.
- Maintain small aggregate counters.
- Normalize slugs or search fields if needed.

Rules:

- Do not use triggers for large business workflows.
- Do not call external services directly from ordinary triggers.
- Do not make trigger chains that are hard to reason about.
- Document triggers in `docs/DATABASE.md`.

### Use Supabase Edge Functions When

- Logic needs a secure server runtime outside the Next.js app.
- The work integrates with external services.
- The function may be invoked by a webhook, cron job, or future non-web client.
- The logic should be deployed close to Supabase and managed with Supabase tooling.

Examples:

- Razorpay webhook handling if moved out of Next.js.
- Sending transactional emails.
- Processing media metadata.
- Running scheduled cleanup or notification jobs.

Rules:

- Do not create Edge Functions for simple UI form mutations that fit better as Next.js server actions.
- Keep Edge Functions per concern, not one giant unstructured function.
- Store Edge Function secrets in Supabase secrets, not in code.

### Use Supabase Cron / `pg_cron` When

- Work must run on a schedule.
- The job is database-centric SQL or should invoke a database function.
- The job should invoke an Edge Function on a schedule.

Examples:

- Expire abandoned checkout records.
- Reconcile pending Razorpay payments.
- Send scheduled course reminders.
- Clean temporary uploads.
- Generate daily admin metrics snapshots.

Rules:

- Cron jobs must be idempotent.
- Cron jobs must log enough status to debug failures.
- Prefer SQL/database functions for database-only scheduled jobs.
- Prefer cron invoking Edge Functions for scheduled jobs that call external services.

## Architecture Decision Rule

For any non-trivial backend behavior, Codex must decide and state which layer owns it:

- Next.js server action/route handler.
- Supabase RPC.
- Database trigger.
- Supabase Edge Function.
- Supabase Cron.

The decision should optimize for correctness, security, maintainability, and operational simplicity.

## References

- Supabase deployment and branching docs: https://supabase.com/docs/guides/deployment
- Supabase local development and migrations docs: https://supabase.com/docs/guides/local-development/overview
- Supabase managing environments docs: https://supabase.com/docs/guides/deployment/managing-environments
- Supabase Cron docs: https://supabase.com/docs/guides/cron
- Supabase scheduled Edge Functions docs: https://supabase.com/docs/guides/functions/schedule-functions

