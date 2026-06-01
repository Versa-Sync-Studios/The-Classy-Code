# AGENTS.md

## Purpose

This file is the root instruction file for Codex and any future agents working in this repo. Before making code changes, read this file and the linked docs.

## Required Reading Order

1. `CODEX_PLAN.md`
2. `docs/ARCHITECTURE.md`
3. `docs/DESIGN_SYSTEM.md`
4. `docs/COMPONENTS.md`
5. `docs/ROUTES.md`
6. `docs/DATABASE.md`
7. `docs/API.md`
8. `docs/OPERATIONS.md`
9. `docs/REPO_HYGIENE.md`
10. `docs/GIT_WORKFLOW.md`
11. `docs/TESTING.md`

Use `docs/SKILLS.md` for repeatable project workflows.

## Project Direction

Build The Classy Code LMS as a Next.js App Router application with TypeScript, Tailwind CSS, Supabase, and Razorpay.

The app must support:

- Public course discovery and course sales pages.
- Auth flows.
- Role-protected admin, instructor, and student dashboards.
- Course builder, lessons, quizzes, assignments, media uploads, and progress tracking.
- Free enrollment and one-time Razorpay paid course purchases.

## Architecture Rules

- Use strict Feature-Sliced Design as documented in `docs/ARCHITECTURE.md`.
- Do not cross-import between sibling features, entities, widgets, or pages.
- Import from a slice only through its public API barrel: `index.ts`.
- Keep Next.js route files thin. Route files should load page modules and handle route-level metadata/redirects only.
- Shared code must be business-agnostic. If code knows about courses, users, enrollments, payments, roles, lessons, quizzes, or assignments, it does not belong in `shared`.

## Design System Rules

- Use the stack in `docs/DESIGN_SYSTEM.md`: Tailwind CSS, shadcn/ui-style local components, Radix primitives, lucide-react, React Hook Form, Zod, TanStack Query where useful, and Recharts for reporting.
- Do not introduce another UI library without updating `docs/DESIGN_SYSTEM.md`.
- Reuse components listed in `docs/COMPONENTS.md` before creating new UI.
- Public pages should be branded and conversion-focused.
- Dashboards should be compact, scannable, and operational.
- Course player pages should be low-distraction and learning-first.

## Access And Security Rules

- All protected routes must enforce role checks server-side.
- Client-side hiding is not security.
- Supabase RLS must be enabled for all LMS tables.
- Private media must never be served by public bucket URLs.
- Razorpay payment completion must require server-side signature verification or webhook reconciliation before enrollment is granted.

## Editing Rules

- Keep changes scoped to the task.
- Do not rewrite unrelated files.
- Do not change architecture, design-system, route, database, or API conventions without updating the matching doc.
- Prefer small, reusable, tested modules.
- Add or update tests when behavior changes.
- This repository is public portfolio code. Do not commit secrets, credentials, private keys, production data, local logs, screenshots containing sensitive data, or generated junk.
- Do not add AI-flavored filler copy, decorative nonsense, or generic placeholder prose to UI, docs, comments, commits, or code.
- Do not use em dashes in app copy, docs, comments, or generated code. Prefer clear short sentences.
- Keep comments crisp. Use `//` for short inline or leading comments in TypeScript when a comment is genuinely useful.
- Do not write long explanatory comments for self-explanatory code.
- Keep lines reasonably short and readable. Break long expressions instead of hiding complexity in a single line.
- Use the Git workflow in `docs/GIT_WORKFLOW.md`: `dev` is active development, `main` is production/stable.

## Commands

Document final commands after project scaffold exists in `docs/TESTING.md`.

Expected command categories:

- Install dependencies.
- Run development server.
- Typecheck.
- Lint.
- Test.
- Build.
