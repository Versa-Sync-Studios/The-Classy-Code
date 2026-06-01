# Design System

## Recommendation

Use a custom LMS design system built on:

- Tailwind CSS for design tokens and utilities.
- shadcn/ui-style local components for fast, accessible UI foundations.
- Radix UI primitives for accessibility and behavior where direct control is needed.
- lucide-react for icons.
- React Hook Form plus Zod for forms and validation.
- TanStack Query for interactive dashboard server state when client-side caching is useful.
- Recharts for admin and instructor analytics.

This is better for this LMS than using a closed visual library like MUI, Ant Design, or Mantine because the app needs both public marketing/course-sales pages and dense dashboards. shadcn/ui-style components give a strong starting point while keeping component source local and fully customizable.

## Component Source Rule

Do not build a component from scratch when an exact or close shadcn/ui component exists.

Default order:

1. Use an existing local `src/shared/ui` component.
2. If it does not exist locally, add the matching shadcn/ui component and adapt it to the project tokens.
3. If shadcn/ui has a close match, compose or lightly adapt it instead of creating a custom primitive.
4. Build from scratch only when shadcn/ui and Radix do not cover the behavior or the LMS needs a product-specific composite component.

Examples:

- Use shadcn/ui for buttons, inputs, dialogs, sheets, dropdowns, tabs, accordions, tables, badges, cards, skeletons, alerts, forms, selects, checkboxes, tooltips, progress, and avatars.
- Build product-specific composites such as `CourseCard`, `PurchaseCard`, `CurriculumAccordion`, `CoursePlayerLayout`, and `DashboardShell` on top of shared primitives.
- Do not create custom modal, dropdown, select, tabs, tooltip, or accordion behavior when Radix/shadcn already covers it.

## Why This Stack

- Tailwind CSS gives a direct API for design tokens such as colors, spacing, radius, shadows, and typography.
- shadcn/ui has first-class Next.js setup guidance and works well with Tailwind.
- Radix UI provides accessible unstyled primitives for dialogs, dropdowns, tabs, accordions, selects, tooltips, and similar UI.
- Local component ownership matters because course cards, purchase cards, curriculum accordions, dashboards, and course players need product-specific behavior.

## Visual Direction

Use the existing Bubble app as a loose foundation:

- White and light-gray surfaces.
- Blue primary actions.
- Compact left dashboard sidebar.
- Top nav with search, help, notifications, and profile.
- Clean cards and tables.
- Spacious public course pages.
- Dense operational dashboards.

Avoid generic AI-looking UI:

- No decorative glowing blobs or abstract orbs.
- No oversized gradient cards for dashboards.
- No nested cards.
- No hero-scale typography inside dashboards.
- No one-off component styles when a system component exists.

## Typography

Recommended font stack:

- Primary UI font: `Inter`
- Fallback stack: `system-ui`, `Segoe UI`, `Roboto`, `Arial`, `sans-serif`
- Monospace/code font: `Geist Mono` or `ui-monospace`, `SFMono-Regular`, `Menlo`, `Consolas`, `monospace`

Inter is the recommended default because it is highly readable, neutral, widely used in SaaS/product interfaces, and works well across public pages, dashboards, dense tables, forms, and course player UI.

Usage:

- Use `Inter` for all public pages, dashboards, forms, tables, course cards, and course player UI.
- Use the monospace font only for code snippets, technical lesson content, IDs, logs, or developer-facing values.
- Do not use multiple decorative fonts.
- Do not scale font sizes with viewport width.
- Letter spacing stays `0` unless a small uppercase label needs subtle tracking.

Recommended type scale:

- `text-xs`: metadata, table helpers, badge text.
- `text-sm`: secondary dashboard text, form help text.
- `text-base`: default body and form text.
- `text-lg`: card titles and section intros.
- `text-xl` / `text-2xl`: dashboard page titles and major section titles.
- `text-3xl` / `text-4xl`: public page and course detail heroes.
- Avoid larger text unless it is a true public marketing hero.

## Tokens

Define semantic tokens, not one-off colors:

- `background`
- `foreground`
- `surface`
- `surface-muted`
- `border`
- `primary`
- `primary-foreground`
- `secondary`
- `accent`
- `success`
- `warning`
- `danger`
- `muted`
- `muted-foreground`

Use Tailwind theme variables/CSS variables as the source of truth.

## Color System

Use semantic color roles so the brand can evolve without rewriting components.

Recommended starting palette:

- `background`: white / near-white.
- `foreground`: deep navy for primary text.
- `surface`: white cards, panels, dialogs.
- `surface-muted`: light cool gray for app backgrounds and dashboard bands.
- `border`: soft gray-blue border.
- `primary`: The Classy Code blue for primary actions.
- `primary-foreground`: white.
- `secondary`: navy/indigo for secondary emphasis.
- `accent`: brand yellow for small highlights only.
- `success`: green for completed/enrolled/paid states.
- `warning`: amber for pending/review states.
- `danger`: red for destructive actions, failed payments, revoked access.
- `muted`: pale neutral backgrounds.
- `muted-foreground`: slate/gray secondary text.

Rules:

- Do not hardcode raw hex values in feature/view components.
- Use semantic Tailwind classes or CSS variables from the token system.
- Keep blue as the primary action color across public pages and dashboards.
- Use yellow sparingly for brand accents, not full backgrounds.
- Use status colors only for status meaning.
- Keep dashboard backgrounds light and neutral for readability.

## Layout Rules

Public pages:

- Use full-width section bands or constrained unframed layouts.
- Use a clear first viewport with course/brand value and CTA.
- Put course imagery or preview media where users can inspect it.

Dashboards:

- Use a persistent sidebar on desktop.
- Use a drawer/sheet navigation on mobile.
- Keep tables dense and scannable.
- Use cards for repeated items and metrics only.
- Avoid large marketing sections.

Course player:

- Prioritize video/content, curriculum, progress, and next action.
- Keep marketing and discovery out of the player.
- Use stable responsive constraints so the video, sidebar, and lesson content do not overlap.

## Responsive Design Rule

The web app must be fully responsive across mobile, tablet, laptop, and desktop.

Do not destroy a strong desktop section just to force it into a mobile layout. If a section works well on desktop but becomes awkward, cramped, or confusing on mobile, create a mobile-native version of that section using the same content, hierarchy, and design tokens.

Rules:

- Design desktop and mobile intentionally, not as accidental squished versions of each other.
- Preserve desktop density for dashboards where it helps productivity.
- Use mobile-native patterns when needed: sheets, drawers, stacked cards, horizontal scrollers, condensed summaries, accordions, sticky bottom CTAs, and simplified toolbars.
- Course detail pages should keep purchase/enroll actions visible on mobile, usually through a sticky bottom CTA or compact sticky purchase panel.
- Dashboard sidebars become mobile sheets/drawers.
- Tables become responsive cards, scrollable tables, or priority-column layouts when needed.
- Course player layout may switch from side-by-side video/curriculum to stacked video plus collapsible curriculum.
- Do not hide essential actions on mobile.
- Do not let mobile fixes regress desktop spacing, hierarchy, or usability.

## Component Rules

- Start with shadcn/ui-style primitives in `src/shared/ui`.
- Product-specific components live in FSD slices, not `shared`.
- Components must support loading, empty, and error states where relevant.
- Icon-only buttons need accessible labels and tooltips when meaning is not obvious.
- Form errors should appear near fields and be screen-reader friendly.

## Accessibility

- Use semantic HTML first.
- Use Radix primitives for complex keyboard/focus behavior.
- Maintain visible focus states.
- Do not rely on color alone for status.
- Keep contrast sufficient for text, buttons, badges, and charts.

## Recommended Initial shadcn/ui Components

Install only what is used:

- button
- input
- textarea
- label
- form
- select
- checkbox
- radio-group
- dialog
- sheet
- dropdown-menu
- tabs
- accordion
- card
- badge
- table
- skeleton
- alert
- tooltip
- progress
- separator
- avatar
