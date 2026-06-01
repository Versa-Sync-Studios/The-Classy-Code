# Component System

## Purpose

This file defines the reusable UI inventory so pages stay consistent. Add new reusable components here before spreading duplicate UI across features.

## Shared UI Primitives

Location:

```txt
src/shared/ui/
```

Use for business-agnostic primitives:

- `Button`
- `Input`
- `Textarea`
- `Label`
- `Select`
- `Checkbox`
- `RadioGroup`
- `Dialog`
- `Sheet`
- `DropdownMenu`
- `Tabs`
- `Accordion`
- `Badge`
- `Table`
- `Skeleton`
- `Alert`
- `Tooltip`
- `Progress`
- `Avatar`
- `Separator`

These should be based on shadcn/ui-style local components.

## App And Dashboard Components

Recommended slices:

- `widgets/public-header`
- `widgets/public-footer`
- `widgets/dashboard-shell`
- `widgets/dashboard-sidebar`
- `widgets/top-nav`
- `widgets/course-player-layout`

Components:

- `PublicHeader`
- `PublicFooter`
- `DashboardShell`
- `RoleSidebar`
- `TopNav`
- `CoursePlayerLayout`

## Course Discovery Components

Recommended slices:

- `entities/course`
- `widgets/course-grid`
- `widgets/course-filters`
- `widgets/course-hero`
- `widgets/course-curriculum-preview`

Components:

- `CourseCard`
- `CourseGrid`
- `CourseFilters`
- `CourseHero`
- `CourseMeta`
- `CourseRating`
- `CurriculumAccordion`
- `PurchaseCard`
- `InstructorMiniProfile`
- `RelatedCourses`

## Learning Components

Recommended slices:

- `widgets/course-player-layout`
- `entities/lesson`
- `entities/quiz`
- `entities/assignment`
- `features/mark-lesson-complete`
- `features/submit-assignment`

Components:

- `LessonSidebar`
- `LessonVideo`
- `LessonResources`
- `LessonNavigation`
- `ProgressBadge`
- `QuizCard`
- `AssignmentCard`
- `SubmissionForm`
- `LockedContentState`

## Dashboard Components

Recommended slices:

- `widgets/admin-metrics`
- `widgets/instructor-metrics`
- `entities/purchase`
- `entities/enrollment`

Components:

- `MetricCard`
- `DataTable`
- `StatusPill`
- `EmptyState`
- `SearchToolbar`
- `FilterBar`
- `PaginationControls`
- `RevenueChart`
- `EnrollmentChart`

## Builder Components

Recommended slices:

- `features/create-course`
- `features/update-course-curriculum`
- `entities/course`
- `entities/lesson`

Components:

- `CourseForm`
- `CourseImageUpload`
- `CourseVideoUpload`
- `TopicList`
- `LessonEditor`
- `QuizEditor`
- `AssignmentEditor`
- `UploadDropzone`
- `SortableItem`

## Rules

- Do not create page-specific copies of existing system components.
- If a component knows about LMS business data, keep it in the relevant `entities`, `features`, or `widgets` slice.
- If a component is purely visual and business-agnostic, keep it in `shared/ui`.
- Export components only through the owning slice's `index.ts`.

