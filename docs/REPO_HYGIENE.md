# Public Repo Hygiene

## Purpose

This repository is intended to be public portfolio code. Keep it clean, professional, and safe to share.

## Never Commit

- `.env*` files with real values.
- Supabase service role keys.
- Razorpay secrets or webhook secrets.
- Database URLs with passwords.
- Production exports or customer/student data.
- Private course videos or paid lesson files.
- Local logs.
- Build output.
- `.next`, `node_modules`, coverage output, caches, and temporary files.
- Screenshots containing credentials, user data, payments, or private dashboard data.
- AI-generated junk, filler copy, or unexplained placeholder content.

## Code Style

- Write clear, direct code.
- Use short meaningful names.
- Prefer explicit types at boundaries.
- Keep functions focused.
- Avoid clever abstractions until duplication or complexity justifies them.
- Do not add decorative comments.
- Do not add long comments explaining obvious code.
- Use `//` for short TypeScript comments only when they clarify non-obvious intent.
- No em dashes in comments, docs, UI copy, or generated text.
- Avoid AI-sounding phrases such as "seamlessly", "unlock potential", "elevate", "revolutionize", or generic marketing filler.

## UI Copy Rules

- Copy should be plain, specific, and useful.
- Public pages should sound like a real learning platform, not a template.
- Dashboard copy should be short and operational.
- Empty states must tell the user what happened and what to do next.
- Error states must be specific enough to act on without exposing secrets.

## Assets

- Use brand assets from the local `THE CLASSY CODE` folder only if they are intended for this project.
- Optimize images before committing when possible.
- Do not commit large raw videos.
- Do not commit paid course content unless it is demo/sample content approved for public display.

## Git Hygiene

- Keep commits focused.
- Do not commit generated artifacts unless they are required source files.
- Review diffs before commit.
- Keep docs updated when architecture, routes, database, API, or design-system rules change.

