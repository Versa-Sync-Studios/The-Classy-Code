# Testing

## Purpose

This file defines how changes should be verified.

## Command Placeholders

After scaffold, fill in exact commands:

```txt
npm run dev
npm run lint
npm run typecheck
npm run test
npm run build
```

## Required Test Coverage

### Auth

- Sign up creates profile.
- Login redirects by role.
- Logout clears session.
- Forgot/reset password flow works.
- Authenticated users cannot access login/signup unnecessarily.

### Role Protection

- Admin routes reject students and instructors.
- Instructor routes reject students.
- Student routes reject unauthenticated users.
- Instructor cannot mutate another instructor's course.

### Course Discovery

- Catalog search and filters work.
- Course detail page shows curriculum and purchase/enroll CTA.
- Preview lessons are visible when configured.

### Enrollment And Payment

- Free course enrollment creates active enrollment.
- Paid course checkout creates Razorpay order.
- Verified Razorpay payment creates purchase and enrollment.
- Failed payment does not create enrollment.
- Webhook handling is idempotent.

### Learning

- Enrolled student can access course player.
- Unenrolled student cannot access paid course player.
- Lesson progress updates.
- Quiz attempts are recorded.
- Assignment submissions are recorded.

### Media

- Private videos/files deny unenrolled users.
- Enrolled users receive access through signed or protected URLs.
- Upload permissions follow role and ownership.

### UI

- Public pages work on desktop and mobile.
- Dashboard sidebar does not overlap content.
- Course player layout remains usable on mobile.
- Loading, empty, and error states render for major screens.

## Acceptance Before Major Milestones

Before declaring a milestone complete:

- Typecheck passes.
- Lint passes.
- Build passes.
- Role/access checks have been manually or automatically verified.
- Payment path has been tested in Razorpay test mode.
- No real secrets or sensitive data are present in the git diff.
- Public repo hygiene from `docs/REPO_HYGIENE.md` has been checked.
- Dev Supabase migrations have been validated before applying to prod.
