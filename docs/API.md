# API And Server Actions

## Purpose

This file documents backend interfaces used by the Next.js app.

For backend architecture placement, use `docs/OPERATIONS.md` before deciding between Next.js server actions, route handlers, Supabase RPC, database triggers, Edge Functions, or Cron.

## Auth

Use Supabase Auth for:

- Sign up.
- Login.
- Logout.
- Forgot password.
- Reset password.
- Session retrieval.

Server-side helpers should expose:

- Get current user.
- Get current profile.
- Require auth.
- Require role.
- Require enrollment.

## Course Actions

Actions:

- Create course.
- Update course metadata.
- Publish/unpublish course.
- Create/update/delete topic.
- Create/update/delete lesson.
- Reorder topics and lessons.
- Create/update/delete quiz.
- Create/update/delete assignment.

Rules:

- Admin can manage all courses.
- Instructor can manage only owned courses.
- Students cannot mutate course content.

## Enrollment Actions

Actions:

- Enroll in free course.
- Grant manual enrollment.
- Revoke enrollment.
- Mark course completed.

Rules:

- Free enrollment requires a published free course.
- Manual enrollment requires admin.
- Completion is based on lesson progress and required assessment rules.

## Razorpay Routes

### `POST /api/razorpay/create-order`

Creates a Razorpay order for a paid course.

Requires:

- Authenticated student.
- Published paid course.
- No existing active enrollment.

Returns:

- Razorpay order id.
- Amount.
- Currency.
- Checkout metadata.

### `POST /api/razorpay/verify`

Verifies checkout response.

Requires:

- Authenticated student.
- Razorpay order id.
- Razorpay payment id.
- Razorpay signature.

Behavior:

- Verify signature server-side.
- Mark purchase as paid.
- Create active enrollment.
- Return next learning route.

### `POST /api/razorpay/webhook`

Reconciles Razorpay payment events.

Requires:

- Valid Razorpay webhook signature.

Behavior:

- Idempotently update purchase status.
- Idempotently grant enrollment for successful payments.
- Record failed payments.

## Uploads

Upload flows should use server-side role checks and private bucket paths.

Rules:

- Instructor uploads must be scoped to owned courses.
- Admin uploads may target any course.
- Student submission uploads must be scoped to their own assignment submissions.
- Course videos and lesson resources should use signed URLs for enrolled students.

## Response Shape

Use typed return objects for server actions:

```ts
type ActionResult<T> =
  | { ok: true; data: T }
  | { ok: false; error: string; fieldErrors?: Record<string, string[]> };
```

Prefer explicit error states over thrown generic errors for expected validation failures.

## Architecture Placement Rule

Do not put every backend concern into one layer.

- Use Next.js server actions/route handlers for UI-driven request/response work.
- Use Supabase RPC for atomic database work.
- Use database triggers for small invariant-maintaining database side effects.
- Use Edge Functions for external integrations or jobs that should live with Supabase.
- Use Supabase Cron for scheduled work.

Any non-trivial API addition should state why that layer was chosen.
