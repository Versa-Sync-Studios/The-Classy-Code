# The Classy Code LMS - Codex Plan

## Summary

Build a full LMS MVP that replaces the Bubble-style LMS reference with a custom Next.js app for admins, instructors, and students. The MVP should include public course discovery, authentication, role-based dashboards, course creation, lessons, quizzes, assignments, media uploads, free enrollment, one-time Razorpay purchases, and student learning progress.

## Build Direction

Build a custom LMS as a Next.js application using:

- Next.js App Router
- TypeScript
- Tailwind CSS
- ESLint
- Supabase Auth, Postgres, and Storage
- Razorpay for one-time paid course purchases
- Strict Feature-Sliced Design architecture from the first implementation
- Separate Supabase projects for development and production

The local working folder is currently empty. Brand assets and Bubble LMS reference screenshots are available in:

`C:\Users\Windows\Desktop\THE CLASSY CODE`

The live site, `https://theclassycode.com/`, was blocked by bot verification during inspection, so the rebuild should use the local assets and screenshots as the primary reference.

## Roles

The MVP supports three roles:

- `admin`: full platform control, including users, instructors, courses, enrollments, purchases, categories, settings, and reporting.
- `instructor`: create and manage their own courses, topics, lessons, quizzes, assignments, uploads, students, reviews, and earnings.
- `student`: browse courses, buy or enroll, watch lessons, complete quizzes and assignments, track progress, and view purchase history.

## Route Map

### Public Routes

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

### Student Protected Routes

- `/dashboard/student`
- `/dashboard/student/my-learning`
- `/dashboard/student/courses/[slug]/learn`
- `/dashboard/student/courses/[slug]/progress`
- `/dashboard/student/courses/[slug]/assignments`
- `/dashboard/student/courses/[slug]/quizzes`
- `/dashboard/student/purchases`
- `/dashboard/student/profile`
- `/dashboard/student/settings`

### Instructor Protected Routes

- `/dashboard/instructor`
- `/dashboard/instructor/courses`
- `/dashboard/instructor/courses/new`
- `/dashboard/instructor/courses/[id]/edit`
- `/dashboard/instructor/courses/[id]/curriculum`
- `/dashboard/instructor/courses/[id]/students`
- `/dashboard/instructor/courses/[id]/reviews`
- `/dashboard/instructor/earnings`
- `/dashboard/instructor/profile`

### Admin Protected Routes

- `/dashboard/admin`
- `/dashboard/admin/users`
- `/dashboard/admin/instructors`
- `/dashboard/admin/courses`
- `/dashboard/admin/categories`
- `/dashboard/admin/enrollments`
- `/dashboard/admin/payments`
- `/dashboard/admin/reports`
- `/dashboard/admin/settings`

### Protected API Routes

- `/api/razorpay/create-order`
- `/api/razorpay/verify`
- `/api/razorpay/webhook`
- `/api/uploads/*`

## Frontend Pages And Sections

### Home Page

Purpose: introduce The Classy Code and move visitors into course discovery.

Sections:

- Hero with clear offer, brand identity, and primary CTA: `Explore Courses`
- Featured course categories
- Popular or featured courses
- Learning paths or career tracks
- Why The Classy Code
- Hands-on learning and project-based outcomes
- Instructor/expert credibility
- Student outcomes and testimonials
- How learning works: buy/enroll, learn, practice, complete
- FAQ
- Footer with contact, policies, and social links

### Course Catalog

Purpose: help learners find the right course quickly.

Sections:

- Search bar
- Category filters
- Level filters: beginner, intermediate, advanced
- Price filters: free, paid
- Sort controls: newest, popular, price, rating
- Course cards with image, title, instructor, level, duration, price, rating, and enrollment count
- Empty state for no matching courses

### Course Detail Page

Purpose: convert qualified visitors into enrolled students.

Recommended structure follows Udemy/Coursera-style course detail patterns and ecommerce product-page UX.

Sections:

- Hero with course title, subtitle, instructor, rating/social proof, level, language, and last updated metadata
- Sticky purchase/enroll card with price, preview video, and Buy Now/Enroll CTA
- What you'll learn
- Skills covered
- Requirements/prerequisites
- Course curriculum accordion with lessons, quizzes, and assignments
- Course description
- Instructor profile
- Reviews/testimonials
- Certificate/completion information, if enabled
- FAQ
- Related courses

### Student Dashboard

Purpose: help students resume learning, not browse aimlessly.

Sections:

- Continue learning / resume course
- Enrolled courses
- Progress cards
- Recent activity
- Upcoming or incomplete assignments
- Certificates/completions, if enabled
- Recommended courses
- Purchase history shortcut

### Course Player

Purpose: provide a low-distraction learning environment.

Sections:

- Video/content area
- Curriculum sidebar
- Topic and lesson progress checkmarks
- Resume from last lesson behavior
- Lesson description/resources
- Mark complete button
- Quiz and assignment entry points
- Notes/resources tab
- Previous/next lesson navigation
- Locked state for unpaid or unenrolled users

### Instructor Dashboard

Purpose: let instructors create, manage, and improve their own courses.

Sections:

- Overview metrics: courses, enrollments, revenue
- Course list with draft/published status
- Course builder wizard
- Curriculum builder: topics, lessons, quizzes, assignments
- Media upload panel
- Student progress table
- Reviews/feedback
- Earnings summary

### Admin Dashboard

Purpose: operate the platform.

Sections:

- Platform overview metrics
- User management
- Instructor management/approval
- Course moderation
- Category management
- Manual enrollment controls
- Razorpay payment records
- Revenue/reporting
- Site/settings configuration

## UX Research Notes

Research references:

- Udemy course landing pages emphasize previews, what learners will learn, requirements, descriptions, curriculum, instructor details, and reviews.
- Coursera course pages emphasize outcomes, skills, tools, modules, level, duration, schedule flexibility, certificate value, and assessments.
- edX/Open edX emphasizes course outlines, progress indicators, verified/paid access states, course dashboards, and resume behavior.
- Thinkific emphasizes a learner dashboard that shows enrolled products, start/resume/replay actions, course cards, and direct access to the course player.
- Baymard online-learning ecommerce UX research frames course pages like ecommerce product pages, with attention to course-page layout, reviews, CTA placement, mobile structure, and checkout flow.

Recommended patterns:

- Treat public course pages like ecommerce product pages.
- Make the course detail page answer fit, value, trust, curriculum, price, and next action quickly.
- Use a sticky CTA purchase card, especially on mobile.
- Always show a preview video or free preview lesson before purchase.
- Show curriculum before checkout so learners know exactly what they get.
- Put trust signals near the CTA: instructor, rating, student count, certificate/completion, and secure payment.
- Keep the student dashboard focused on resume learning.
- Keep the course player free of marketing sections.
- Show progress at lesson, topic, and course level.
- Keep admin and instructor pages dense, scannable, and operational.
- Keep the existing Bubble app's broad visual direction where useful: white/light-gray workspace, blue primary actions, compact sidebar, top search/help/notification/profile shell, and cleaner responsive layouts.

Recommended MVP positioning:

- Use a Udemy + Thinkific pattern for v1.
- Avoid Coursera-level complexity in v1, including financial aid, peer grading, subscriptions, university-style tracks, or complex certificate programs.
- Use one-time course purchases through Razorpay.
- Support free courses with instant enrollment.

## Data And Backend Interfaces

Core tables:

- `profiles`: user metadata and role.
- `courses`: title, slug, description, image, promo video, level, category, price, status, and instructor owner.
- `course_topics`: ordered sections within a course.
- `lessons`: topic children with title, summary, video/file refs, rich content, and order.
- `quizzes`: quiz metadata.
- `quiz_questions`: quiz questions and answers.
- `quiz_attempts`: student quiz attempts and scores.
- `assignments`: assignment prompts and resources.
- `assignment_submissions`: student submissions, uploads, grading, and status.
- `enrollments`: student-course access with source: `free`, `purchase`, or `manual`.
- `purchases`: Razorpay order/payment IDs, amount, status, course, and student.
- `lesson_progress`: student progress per lesson.

Backend behaviors:

- Supabase Auth handles identity.
- Server-side auth/session helpers protect App Router pages, route handlers, and server actions.
- Supabase RLS protects all LMS data.
- Supabase Storage uses private buckets for course videos, images, and assignment files.
- Media upload actions use role checks and private/signed access rules.
- Razorpay server route creates orders.
- Razorpay verification route validates checkout signatures.
- Razorpay webhook reconciles payment status and grants enrollment.
- Non-trivial backend behavior must be placed deliberately in the right layer: Next.js server action/route handler, Supabase RPC, database trigger, Supabase Edge Function, or Supabase Cron.
- Use `docs/OPERATIONS.md` to decide when to use RPC, triggers, Edge Functions, and scheduled jobs.

## Architecture And Design System Direction

Architecture is locked to strict Feature-Sliced Design with a Next.js App Router adjustment:

- `src/app` is reserved for Next.js route files, layouts, metadata, route-level redirects, and route-level auth checks.
- `src/views` acts as the FSD page/view layer because `src/pages` would conflict with the legacy Next.js Pages Router.
- Dependencies flow only downward: `app -> views -> widgets -> features -> entities -> shared`.
- Slices export through public `index.ts` files only.
- No cross-importing between sibling features, entities, widgets, or views.

Design-system baseline:

- Tailwind CSS for styling primitives and tokens.
- shadcn/ui-style components for accessible, consistent UI foundations.
- Radix UI primitives where direct behavior control is needed.
- lucide-react for icons.
- TanStack Query for client-side server-state caching if the app uses interactive dashboards heavily.
- React Hook Form plus Zod for forms and validation.
- Recharts for admin/instructor reporting charts.

Design principles:

- Public pages should be clear, conversion-oriented, and visually branded.
- Dashboards should be compact, scannable, and task-first.
- Cards should be used for repeated entities such as courses, metrics, assignments, and reviews.
- Avoid nested cards and oversized marketing UI inside operational dashboards.
- Mobile layouts must preserve CTA visibility and avoid sidebar/content overlap.

## Governance Files

The repo now uses root `AGENTS.md` plus focused docs under `docs/`.

### Root Files

- `AGENTS.md`: Codex/agent operating rules, required reading order, architecture rules, security rules, and editing rules.
- `CODEX_PLAN.md`: product scope, roles, routes, page sections, UX research, data direction, and acceptance criteria.
- `.env.example`: required environment variables for local and deployed setup.

### `docs/ARCHITECTURE.md`

Defines strict Feature-Sliced Design usage:

- Next.js route ownership.
- FSD layers.
- Dependency direction.
- Public API rule.
- Slice structure.
- Naming rules.

### `docs/DESIGN_SYSTEM.md`

Defines the frontend design stack and visual rules:

- Tailwind CSS.
- shadcn/ui-style local components.
- Radix UI primitives.
- lucide-react.
- React Hook Form plus Zod.
- TanStack Query where useful.
- Recharts.
- LMS-specific layout and accessibility rules.

### `docs/COMPONENTS.md`

Defines reusable UI inventory:

- Shared UI primitives.
- App shells.
- Course discovery components.
- Course player components.
- Dashboard components.
- Course builder components.

### `docs/ROUTES.md`

Defines public routes, protected role routes, API routes, and redirect behavior.

### `docs/DATABASE.md`

Defines Supabase tables, storage buckets, RLS expectations, and migration rules.

### `docs/API.md`

Defines server actions, Razorpay routes, upload flows, and standard action response shape.

### `docs/OPERATIONS.md`

Defines dev/prod Supabase setup, environment rules, and backend architecture decision rules for Next.js server logic, Supabase RPC, triggers, Edge Functions, and Cron.

### `docs/REPO_HYGIENE.md`

Defines public repository hygiene, secret handling, clean-code rules, UI copy rules, asset rules, and Git hygiene.

### `docs/TESTING.md`

Defines verification expectations for auth, roles, course discovery, enrollment, payment, learning, media, and UI.

### `docs/SKILLS.md`

Defines project-specific Codex playbooks:

- Add a new LMS page.
- Add a protected dashboard route.
- Add a Supabase table.
- Add a course builder form.
- Add a Razorpay flow.
- Add a reusable component.

### Pattern Library / Component System

Recommended goal:

- Keep UI consistent by centralizing reusable components before the app grows.

Candidate components:

- `AppShell`
- `DashboardShell`
- `RoleSidebar`
- `TopNav`
- `CourseCard`
- `CourseGrid`
- `CourseFilters`
- `CourseHero`
- `PurchaseCard`
- `CurriculumAccordion`
- `CoursePlayer`
- `ProgressBadge`
- `MetricCard`
- `DataTable`
- `EmptyState`
- `UploadDropzone`
- `FormField`
- `StatusPill`

## Test Plan

- Typecheck and lint the Next.js app.
- Verify auth redirects for public, authenticated, and role-protected routes.
- Verify admin cannot be accessed by instructors/students.
- Verify instructor cannot edit another instructor's course.
- Verify student cannot access paid course player before enrollment.
- Verify free enrollment works instantly.
- Verify Razorpay test payment creates an order, verifies payment, records purchase, and grants enrollment.
- Verify private videos/files are denied to unenrolled users.
- Verify responsive layouts for public pages, dashboards, and course player.
- Verify checkout success/failure states.
- Verify production readiness: Supabase RLS enabled on all LMS tables, Razorpay webhook secret configured, `.env.example` documented, and `npm run build` passes.

## References

- [Next.js App Router docs](https://nextjs.org/docs/app/getting-started)
- [Supabase Next.js Auth docs](https://supabase.com/docs/guides/auth/quickstarts/nextjs)
- [Supabase Row Level Security docs](https://supabase.com/docs/guides/database/postgres/row-level-security)
- [Razorpay Standard Checkout docs](https://razorpay.com/docs/payments/payment-gateway/web-integration/standard/integration-steps/)
- [Udemy course preview and comparison guidance](https://support.udemy.com/hc/en-us/articles/229231027-How-to-Preview-And-Compare-Courses)
- [Udemy course landing page rules](https://support.udemy.com/hc/en-us/articles/229233007-Course-Landing-Page-Rules-and-Guidelines)
- [Open edX course outline and progress docs](https://docs.openedx.org/en/latest/educators/concepts/open_edx_platform/about_course_outline.html)
- [edX dashboard docs](https://edxsupport.zendesk.com/hc/en-us/articles/360000340307-What-is-my-dashboard)
- [Thinkific student dashboard docs](https://support.thinkific.com/hc/en-us/articles/1500001538961-The-Student-Dashboard)
- [Baymard online-learning ecommerce UX audit scope](https://baymard.com/audits/online-learning)
