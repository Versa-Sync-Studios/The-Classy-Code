# Database

## Purpose

This file documents the planned Supabase schema, ownership rules, and RLS expectations.

## Core Tables

### `profiles`

Stores app-level user metadata.

Fields:

- `id` references `auth.users.id`
- `role`: `admin | instructor | student`
- `full_name`
- `avatar_url`
- `created_at`
- `updated_at`

### `categories`

Course categories.

Fields:

- `id`
- `name`
- `slug`
- `description`
- `created_at`

### `courses`

Course metadata.

Fields:

- `id`
- `instructor_id`
- `category_id`
- `title`
- `slug`
- `subtitle`
- `description`
- `level`: `beginner | intermediate | advanced`
- `language`
- `price_inr`
- `is_free`
- `status`: `draft | review | published | archived`
- `image_path`
- `promo_video_path`
- `created_at`
- `updated_at`
- `published_at`

### `course_topics`

Ordered course sections.

Fields:

- `id`
- `course_id`
- `title`
- `summary`
- `position`

### `lessons`

Ordered lesson content.

Fields:

- `id`
- `course_id`
- `topic_id`
- `title`
- `summary`
- `content`
- `video_path`
- `resource_paths`
- `is_preview`
- `position`
- `created_at`
- `updated_at`

### `quizzes`

Quiz metadata.

Fields:

- `id`
- `course_id`
- `lesson_id`
- `title`
- `description`
- `passing_score`
- `position`

### `quiz_questions`

Quiz question data.

Fields:

- `id`
- `quiz_id`
- `question`
- `options`
- `correct_answer`
- `position`

### `quiz_attempts`

Student quiz results.

Fields:

- `id`
- `quiz_id`
- `student_id`
- `score`
- `answers`
- `passed`
- `created_at`

### `assignments`

Assignment prompts.

Fields:

- `id`
- `course_id`
- `lesson_id`
- `title`
- `description`
- `resource_paths`
- `position`
- `created_at`

### `assignment_submissions`

Student submissions.

Fields:

- `id`
- `assignment_id`
- `student_id`
- `content`
- `file_paths`
- `status`: `submitted | reviewed | needs_revision`
- `grade`
- `feedback`
- `submitted_at`
- `reviewed_at`

### `enrollments`

Student course access.

Fields:

- `id`
- `course_id`
- `student_id`
- `source`: `free | purchase | manual`
- `status`: `active | revoked | completed`
- `created_at`
- `completed_at`

### `purchases`

Razorpay payment records.

Fields:

- `id`
- `course_id`
- `student_id`
- `razorpay_order_id`
- `razorpay_payment_id`
- `amount_inr`
- `status`: `created | paid | failed | refunded`
- `created_at`
- `paid_at`

### `lesson_progress`

Student lesson progress.

Fields:

- `id`
- `lesson_id`
- `course_id`
- `student_id`
- `status`: `not_started | in_progress | completed`
- `last_position_seconds`
- `completed_at`
- `updated_at`

## Storage Buckets

- `course-images`: private or public-read depending final policy.
- `course-videos`: private.
- `lesson-resources`: private.
- `assignment-submissions`: private.
- `avatars`: public-read or signed-read depending final policy.

## RLS Requirements

- Enable RLS on every LMS table.
- Admin can read and manage all LMS rows.
- Instructor can manage only owned courses and child records.
- Student can read published course metadata.
- Student can access protected course content only when enrolled.
- Student can manage only their own progress, attempts, submissions, and purchases.
- Webhook/service operations use server-side privileged Supabase client only in trusted route handlers.

## Migration Rules

- Every schema change must be represented as a Supabase migration.
- Never rely on dashboard-only changes without migration files.
- Update this file whenever tables, columns, policies, or storage buckets change.

## Dev And Prod Projects

Use two separate Supabase projects:

- Dev project for local development, test data, and migration validation.
- Prod project for real users, real course data, and live payments.

Rules:

- Schema changes start as migration files.
- Apply migrations to dev first.
- Apply the same reviewed migrations to prod only after testing.
- Do not make one-off production dashboard schema edits.
- Do not copy production user, payment, or private course data into the public repo.

## Database Logic Placement

Use `docs/OPERATIONS.md` before adding database functions, triggers, Edge Functions, or scheduled jobs.

Document here whenever the schema gains:

- RPC functions.
- Triggers.
- Cron jobs.
- Storage policies.
- RLS policies.
