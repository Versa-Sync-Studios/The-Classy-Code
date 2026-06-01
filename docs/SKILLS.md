# Project Skills

## Purpose

This file defines repeatable workflows for Codex in this repo. `AGENTS.md` contains rules. This file contains playbooks.

## Add A New LMS Page

1. Check `docs/ROUTES.md` for route and access requirements.
2. Add or update a `src/views/*` slice for page composition.
3. Add a thin route file in `src/app`.
4. Reuse widgets, features, entities, and shared UI through public APIs.
5. Add loading, empty, and error states where needed.
6. Add tests or manual verification notes.

## Add A Protected Dashboard Route

1. Add the route to `docs/ROUTES.md`.
2. Add server-side auth and role checks in the route/layout.
3. Keep UI in `src/views` and dashboard blocks in `src/widgets`.
4. Verify wrong-role and unauthenticated redirects.

## Add A Supabase Table

1. Update `docs/DATABASE.md`.
2. Add a migration.
3. Add RLS policies.
4. Add TypeScript types or generated type references.
5. Add entity/model code only if the table is used by the app.

## Add A Course Builder Form

1. Place the user action in `src/features`.
2. Use React Hook Form and Zod.
3. Use shared form primitives.
4. Put course-specific display UI in `entities/course` or relevant entity slices.
5. Enforce instructor ownership server-side.

## Add A Razorpay Flow

1. Update `docs/API.md` if the flow changes.
2. Create or update server route handlers.
3. Verify signatures server-side.
4. Make purchase/enrollment updates idempotent.
5. Test in Razorpay test mode.

## Add A Reusable Component

1. Check `docs/COMPONENTS.md` first.
2. If business-agnostic, add to `src/shared/ui`.
3. If business-specific, add to the owning entity/feature/widget.
4. Export only through `index.ts`.
5. Add usage notes to `docs/COMPONENTS.md` when the component becomes part of the standard system.

