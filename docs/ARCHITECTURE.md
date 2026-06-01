# Architecture

## Purpose

This project uses strict Feature-Sliced Design from the first implementation. The goal is predictable ownership, no cross-import drift, and reusable LMS features without turning `shared` into a dumping ground.

Reference: Feature-Sliced Design documents layers, slices, segments, and public APIs. Its rules state that slices and segments should expose a public API, and dependencies should flow in one direction through layers.

## Next.js + FSD Layout

Next.js reserves `src/app` for App Router and `src/pages` for the legacy Pages Router. To avoid enabling the legacy router, this repo uses `src/views` as the FSD page/view layer.

Planned structure:

```txt
src/
  app/                 # Next.js App Router only: route files, layouts, metadata, redirects
  views/               # FSD page/view modules used by route files
  widgets/             # Large page blocks composed from features/entities
  features/            # User actions and business use cases
  entities/            # Business entities and entity UI/model/api
  shared/              # Business-agnostic libraries, UI primitives, config, assets
```

Route files in `src/app` should be thin:

```tsx
export { CourseCatalogPage as default } from "@/views/course-catalog";
```

If a route needs auth, redirects, metadata, or server params, keep that logic route-level and delegate UI to `views`.

## Dependency Rule

Allowed import direction:

```txt
app -> views -> widgets -> features -> entities -> shared
```

Each layer may import only from layers below it.

Forbidden:

- `features/course-builder` importing from `features/checkout`.
- `entities/course` importing from `features/enroll-course`.
- `shared` importing from any business layer.
- Deep imports into another slice internals.
- Importing from `src/app` outside `src/app`.

## Public API Rule

Every slice exposes its public API through `index.ts`.

Allowed:

```ts
import { CourseCard } from "@/entities/course";
```

Forbidden:

```ts
import { CourseCard } from "@/entities/course/ui/course-card";
```

Internal files may deep-import within the same slice only.

## Layer Responsibilities

### `src/app`

- Next.js route files.
- Route layouts.
- Metadata.
- Server redirects.
- Route-level auth enforcement.
- Loading/error/not-found boundaries.

Do not place reusable UI or business logic here.

### `src/views`

- Route-level page composition.
- Page-specific layout decisions.
- Calls to widgets/features/entities through public APIs.

Examples:

- `views/home`
- `views/course-catalog`
- `views/course-detail`
- `views/student-dashboard`
- `views/instructor-course-builder`
- `views/admin-users`

### `src/widgets`

- Large reusable page sections.
- Composed UI blocks that combine multiple features/entities.

Examples:

- `widgets/public-header`
- `widgets/course-hero`
- `widgets/course-curriculum-preview`
- `widgets/dashboard-sidebar`
- `widgets/course-player-layout`
- `widgets/admin-metrics`

### `src/features`

- User actions and use cases.
- Forms and interactive flows.
- Server actions or client hooks for one business action.

Examples:

- `features/auth-login`
- `features/auth-sign-up`
- `features/enroll-free-course`
- `features/purchase-course`
- `features/create-course`
- `features/update-course-curriculum`
- `features/submit-assignment`
- `features/mark-lesson-complete`

### `src/entities`

- Business concepts.
- Entity-specific UI, types, API adapters, and model helpers.

Examples:

- `entities/user`
- `entities/profile`
- `entities/course`
- `entities/category`
- `entities/enrollment`
- `entities/lesson`
- `entities/quiz`
- `entities/assignment`
- `entities/purchase`

### `src/shared`

- Business-agnostic code.
- Base UI.
- Utilities.
- Supabase/Razorpay clients with no product-specific decisions.
- Constants, environment validation, assets, icons.

Examples:

- `shared/ui/button`
- `shared/ui/dialog`
- `shared/lib/cn`
- `shared/lib/supabase`
- `shared/lib/razorpay`
- `shared/config/env`

## Slice Structure

Use these segments only when needed:

```txt
slice/
  index.ts
  ui/
  model/
  api/
  lib/
  config/
```

Segment use:

- `ui`: React components.
- `model`: types, state, schemas, constants tied to the slice.
- `api`: server actions, route-client wrappers, Supabase queries tied to the slice.
- `lib`: slice-specific helpers.
- `config`: slice-specific static config.

Do not create empty segments.

## Naming

- Slices use kebab-case folders: `purchase-course`, `course-player-layout`.
- React components use PascalCase.
- Server actions use verb-first names: `createCourseAction`, `verifyRazorpayPaymentAction`.
- Types use domain names: `Course`, `CourseWithProgress`, `EnrollmentStatus`.

## Enforcement

After scaffold, add ESLint import restrictions to enforce layer direction and public API imports. Until automated rules exist, Codex must manually follow this document.

