# Routes

## Purpose

This file documents route ownership, access rules, and redirect behavior.

## Public Routes

- `/`
- `/courses`
- `/courses/[slug]`
- `/categories/[slug]`
- `/instructors/[id]`
- `/about`
- `/contact`
- `/login`
- `/signup`
- `/forgot-password`
- `/reset-password`
- `/checkout/[courseSlug]`
- `/payment/success`
- `/payment/failed`

Public course detail pages may show marketing content and curriculum previews. Lesson/player access must require login and enrollment.

## Student Routes

- `/dashboard/student`
- `/dashboard/student/my-learning`
- `/dashboard/student/courses/[slug]/learn`
- `/dashboard/student/courses/[slug]/progress`
- `/dashboard/student/courses/[slug]/assignments`
- `/dashboard/student/courses/[slug]/quizzes`
- `/dashboard/student/purchases`
- `/dashboard/student/profile`
- `/dashboard/student/settings`

Access:

- Requires authenticated user.
- Requires role `student`, unless an `admin` impersonation/debug flow is explicitly added later.
- Course learning routes also require enrollment.

## Instructor Routes

- `/dashboard/instructor`
- `/dashboard/instructor/courses`
- `/dashboard/instructor/courses/new`
- `/dashboard/instructor/courses/[id]/edit`
- `/dashboard/instructor/courses/[id]/curriculum`
- `/dashboard/instructor/courses/[id]/students`
- `/dashboard/instructor/courses/[id]/reviews`
- `/dashboard/instructor/earnings`
- `/dashboard/instructor/profile`

Access:

- Requires authenticated user.
- Requires role `instructor`.
- Instructor can only manage courses they own.

## Admin Routes

- `/dashboard/admin`
- `/dashboard/admin/users`
- `/dashboard/admin/instructors`
- `/dashboard/admin/courses`
- `/dashboard/admin/categories`
- `/dashboard/admin/enrollments`
- `/dashboard/admin/payments`
- `/dashboard/admin/reports`
- `/dashboard/admin/settings`

Access:

- Requires authenticated user.
- Requires role `admin`.

## API Routes

- `POST /api/razorpay/create-order`
- `POST /api/razorpay/verify`
- `POST /api/razorpay/webhook`
- `/api/uploads/*`

Access:

- Create order requires authenticated student and a paid course.
- Verify requires authenticated student and server-side Razorpay signature validation.
- Webhook validates Razorpay webhook signature and must be idempotent.
- Upload routes require admin/instructor role and ownership checks.

## Redirect Rules

- Unauthenticated protected-route access redirects to `/login`.
- Authenticated users visiting `/login` or `/signup` redirect to their role dashboard.
- Wrong-role protected-route access redirects to the correct dashboard or a `403` page.
- Paid course player access without enrollment redirects to `/courses/[slug]`.
- Successful Razorpay verification redirects to `/dashboard/student/courses/[slug]/learn`.
- Failed/cancelled payment redirects to `/payment/failed`.

