# AGENTS.md | Take a Break (CS 177)

These instructions are mandatory for AI coding agents working in this repository. Keep the app simple, accessible, dependable, and visually faithful to `./design/`. Do not replace established project decisions with framework defaults or personal preferences.

## 1. Read first; respect the repository

Before making UI or implementation changes:

1. Read this file, `nielsen-heuristics.md`, `components.json`, and the relevant existing source files.
2. Inspect the reference screenshots in `./design/`. They are the source of truth for the intended visual direction: a near-white canvas, oversized timer, restrained controls, charcoal work mode, and muted sage break mode.
3. Inspect installed dependencies, existing shadcn components, and the available scripts before choosing an approach.
4. Work within the requested scope. Preserve existing architecture and configuration unless the task explicitly requires a change.

**Never modify `components.json`.** Its shadcn registry configuration, `base-vega` style, CSS path, and aliases are intentional. In particular, the installed UI component location is `@/shadcn/ui`, not an assumed default `@/components/ui`.

## 2. Required stack and package manager

- Next.js, React, TypeScript, Tailwind CSS, and shadcn/ui.
- **Bun exclusively** for dependency installation, scripts, tests, and CLI execution: `bun install`, `bun add`, `bun run`, `bunx`, and `bun test` when applicable. Never recommend or execute npm, npx, pnpm, Yarn, or their lockfile workflows. Keep the Bun lockfile authoritative.
- **Lucide icons from the existing `gitlab:iconify-icons/react#lucide` dependency.** Check the installed package's actual exports before importing icons. Use the Lucide icon set at https://icon-sets.iconify.design/lucide/ for names and references. Do not introduce `lucide-react`, another icon library, emoji icons, or hand-drawn replacements.
- Prefer `next/font/google` for font delivery, not Google Fonts CSS imports or externally hosted font stylesheets.

Do not install a new package if an installed dependency already solves the problem.

## 3. shadcn/ui first, without exception

Before implementing **any** UI pattern or control, first check for a suitable existing or installable shadcn component. Reuse installed components; if missing, install from the shadcn registry using `bunx shadcn@latest add <component>` and the existing `components.json` configuration.

Use shadcn primitives for dialogs, selects, switches, buttons, progress indicators, tooltips, alerts, and other applicable UI. Use the shadcn **Sonner** integration for toasts; position notifications at the **top-right** unless there is a demonstrated UX reason to change it. Use shadcn Spinner and Skeleton only when the wait genuinely benefits from that feedback.

Compose existing primitives with Tailwind utilities to form app-specific screens. A custom React composition (such as the timer view) is permitted where no shadcn component represents the feature; inventing another generic button, dialog, switch, select, or notification system is not.

Do not bypass shadcn simply because custom markup seems faster.

## 4. Typography and Perfect Fourth design scale

- **Inter**: interface text, buttons, settings, labels, notifications.
- **Manrope**: oversized timer digits. Use tabular numerals so the countdown does not shift horizontally (`tabular-nums` or equivalent font feature).
- Use a **Perfect Fourth modular ratio of 4/3 (approximately 1.333)** for authored typography, spacing, and major layout dimensions. Establish a documented, **named token scale in `src/app/globals.css`**, exposed through the Tailwind theme. Use those tokens via normal Tailwind utility classes.
- The type scale is based on 16px: 12, 16, 21.333, 28.444, 37.926, 50.568, 67.424, 89.899, 119.865, 159.82, 213.093px, etc. Map Tailwind text utilities to project tokens rather than using ad hoc font sizes. The large timer may use a higher step with a responsive cap expressed through named tokens.
- Choose spacing values from a similarly defined Perfect Fourth token scale. Avoid arbitrary spacing and bespoke per-component sizing.

**Hard prohibition:** No magic sizing values in JSX or Tailwind. Examples of forbidden authored styles: `text-[11px]`, `mt-[17px]`, `w-[287px]`, `text-[0.85rem]`, `style={{ fontSize: 11 }}`, or a one-off `calc()` used to evade the tokens. Do not use arbitrary hex colors such as `bg-[#abcdef]`.

Necessary technical dimensions are exceptions, **not permission for casual one-offs**: 1px hairlines, accessible touch-target minima, aspect ratios, full-width/viewport sizing, and runtime-computed values such as timer progress. Prefer established framework utilities or centrally defined CSS tokens for these. Selector/state variants used by shadcn (for example `data-[state=open]`) are not forbidden magic measurements. Do not rewrite vendored/shadcn internals merely to remove valid selector variants.

## 5. No custom CSS classes

**Never create author-defined CSS utility or component classes.** No `.timer`, `.panel`, `.my-button`, `.text-special`, CSS Modules, Sass modules, CSS-in-JS styling libraries, or custom class selectors in stylesheets. Do not add `@apply` wrappers to disguise custom components.

Use **Tailwind utilities directly in `className`**, and reusable, semantic theme variables defined centrally in `src/app/globals.css`. Global CSS is for Tailwind imports/theme directives, tokens, and essential element-level global styles, not a parallel styling system. Framework-generated styles and necessary styles supplied by shadcn are permitted; do not author a competing CSS class system.

When a required effect cannot be expressed cleanly with the approved utilities/tokens, propose the smallest token or composition change instead of silently adding custom CSS.

## 6. Palette and semantic color tokens

Preserve the design reference's light, restrained character. Define the following **starting palette** in `src/app/globals.css` as semantic theme tokens; no hardcoded component-level hex values:

| Role | Starting value | Usage |
| --- | --- | --- |
| Background | `#F7FAFB` | Near-white app canvas |
| Surface | `#FFFFFF` | Modal and toast surfaces |
| Foreground / work timer | `#2A2A2A` | Primary text and work digits |
| Secondary text | `#52646F` | Settings descriptions and labels |
| Break timer / label | `#5C7565` | Accessible, muted sage |
| Work progress | `#5B7786` | Accessible blue-gray foreground |
| Break progress | `#5C7565` | Accessible sage foreground |
| Accent controls | `#008F70` | Switches, interactive indicators, focus accents where suitable |
| Accent text | `#005E4A` | Accent-colored text on light backgrounds |
| Progress track | `#DEE5E7` | Nonessential, low-emphasis track |

These values intentionally darken some colors seen in the screenshots. The earlier pale work progress, pale break progress, and bright turquoise accents did **not** all meet meaningful non-text contrast targets. The work/break state colors must not silently weaken readability.

Preserve shadcn's semantic tokens (`background`, `foreground`, `muted`, `muted-foreground`, `border`, `primary`, `ring`, etc.), mapping them to the accessible design palette. Add dedicated timer/work/break tokens instead of overloading `primary`. If a proposed token fails its actual usage context, **change the token**. These samples are not an exemption from contrast testing.

Do not add gradients, decorative cards, strong shadows, or saturated branding colors not supported by the designs.

## 7. Contrast and accessibility are release blockers

**Enforce WCAG 2.2 AA contrast at every rendered state.** This is a hard acceptance requirement, not a subjective visual preference:

- Normal text, secondary text, helper text, input values, placeholders, and labels: **at least 4.5:1** contrast against their actual background.
- Large text (at least 24px regular, or approximately 18.67px bold): **at least 3:1**. Prefer 4.5:1 where practical.
- Meaningful non-text graphics and UI states (progress fill against its track, interactive control boundaries, selected indicators, meaningful icons, visible focus states): **at least 3:1** against adjacent relevant colors.
- Evaluate **actual composited backgrounds**, not only hex tokens in isolation. Recheck after opacity, translucency, blur, overlays, hover effects, active states, and modal backdrops.
- Do not lower opacity on readable labels or use low-contrast text to suggest visual hierarchy. Hierarchy comes from scale, weight, placement, and spacing.
- Do not communicate timer mode, pause state, completion, errors, or availability **by color alone**. Use meaningful text and/or accessible labels.
- Disabled controls are exempt from some WCAG contrast success criteria, but should still be recognizable as controls; never make critical information look disabled or disappear.
- Keyboard operation, visible focus, semantic HTML, accessible control names, screen-reader status updates, and reduced-motion accommodations must be considered as part of implementation.

**Verification is mandatory:** inspect the rendered UI (not just source code) and check contrast ratios with a contrast checker or equivalent reliable tooling across all affected component states. If rendered inspection or contrast measurement is unavailable, explicitly say it was **not verified**; do not claim accessibility compliance. Correct failures before considering the work complete. An attractive screenshot is not proof of accessible contrast.

## 8. Nielsen heuristics: design before coding

Read and apply **`./nielsen-heuristics.md`** on every nontrivial user-facing change, especially **"1. Visibility of system status"**. Translate relevant rules into actual interaction behavior and tests, rather than mentioning Nielsen in a checklist after the work.

For this app, in particular:

- Show the current timer phase (work/break), countdown, and running/paused state clearly. Active, paused, completed, and transitioning states must not be ambiguous.
- Respond promptly to pause, resume, skip, reset, and settings changes. Provide feedback proportional to the action; do not toast every trivial click.
- On work/break transitions, change the visible state immediately and use an appropriate top-right Sonner toast, with an accessible announcement where useful. Handle browser notification permissions and unavailable capabilities truthfully; do not claim that a notification was delivered unless it was.
- For genuine asynchronous operations, define applicable idle, pending, success, unavailable/empty, and error states. Use Spinner for activity and Skeleton for layout-preserving loading where appropriate; always provide an exit or recovery path.
- Avoid a spinner for the normal running timer; the live countdown already communicates activity.
- Do not steal focus with notifications. Ensure dialogs support expected keyboard behavior and return focus correctly.
- Test responsive layouts and ensure critical actions never depend exclusively on hover.

Favor the screenshot's minimalism while preserving state visibility and accessibility. Do not remove important feedback to make the interface look cleaner.

## 9. Responsive design (mandatory)

- Design every new or changed page, component, modal, and interaction **mobile first**. It must remain usable at mobile, tablet, and desktop sizes. Treat `./design/` as desktop visual direction, not fixed dimensions to impose on small screens; a layout that already works needs no forced breakpoint change.
- Use Tailwind's default breakpoints only: unprefixed utilities for mobile, `md:` at 768px and up for tablet, `lg:` at 1024px and up for desktop, and `sm:` at 640px and up only when a real intermediate layout needs it. Do not define custom breakpoints, author `@media` styling or custom CSS classes, or use viewport-specific magic numbers or arbitrary sizing utilities in JSX.
- Adapt the layout instead of merely shrinking it. Keep timer digits fully visible, controls separate and reachable, progress within available width, and spacing and text readable. Fit dialogs and settings within the viewport with scrolling when needed; wrap or accessibly truncate long labels. Prevent accidental horizontal page overflow.
- Keep essential timer actions (pause, resume, skip, reset, and settings) keyboard and touch operable at narrow widths. No essential hover-only actions. Preserve clear phase and running/paused labels, visible focus, WCAG-compliant contrast, usable touch targets, and readable transition feedback or toasts at every width.
- Use the existing Perfect Fourth typography/spacing tokens, normal Tailwind utilities, and shadcn components. Add centrally named responsive or fluid tokens in `src/app/globals.css` only if a later feature truly needs them; do not introduce tokens or restyle the app in a documentation-only task.
- For affected UI, inspect rendered behavior around **375px mobile, 768px tablet, and 1280px desktop**, plus intermediate widths near layout transitions when relevant. Check overflow, countdown, controls, dialogs/settings, toasts, keyboard/touch access, focus, and actual rendered contrast. These widths are testing targets, not CSS tokens. Report what was inspected; if rendering or contrast measurement was unavailable, say so rather than claiming verification.

## 10. Quality gate before declaring a task done

1. Confirm the solution uses shadcn where available, approved Lucide icons, and Bun only.
2. Confirm `components.json` is unchanged.
3. Check that new code introduces **no custom CSS classes, arbitrary sizing/color utilities, or undocumented design values**.
4. Confirm `globals.css` contains any new shared tokens and that typography/spacing follow the agreed scale, with explained technical exceptions.
5. Test interaction states, timer phase changes, keyboard and focus behavior, and meaningful failure/recovery paths.
6. For any UI change, inspect rendered mobile (~375px), tablet (~768px), and desktop (~1280px) layouts, plus relevant transition widths. Check responsive behavior and accessibility across affected states. **Measure contrast in real rendered states**; fix failures. If rendered inspection or contrast measurement was unavailable, report that limit and do not claim responsive or accessibility verification.
7. Run the relevant project checks with Bun (for example `bun run lint`, `bun run build`, `bun test` **only when those scripts/tests exist**). Do not claim to have run checks that were not run.

When reporting completed work, state what changed, what was tested, and any unverified limitations concisely.
