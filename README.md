# Take a Break

Take a Break is a web-based focus and break timer developed as a team project for **CS 177 Software Engineering**. The intended interface is minimal: a large countdown, clear work/break phase and running state, restrained controls, optional progress, settings, and transition feedback. See the desktop [design references](./design/) for visual direction. **The timer is not implemented yet:** the app currently displays the Next.js starter page.

## Get started

Use **Bun only**. The repository has a Bun lockfile and no documented environment variables or `.env.example`.

```bash
bun install
bun run dev
```

Open [http://localhost:3000](http://localhost:3000). Available checks:

```bash
bun run lint
bun run build
```

These commands match the scripts in [`package.json`](./package.json). There is no test script yet.

## Stack and design direction

- **Next.js, React, and TypeScript** with **Tailwind CSS**. Shared colors and the named Perfect Fourth type/spacing scale live in [`src/app/globals.css`](./src/app/globals.css); authored sizes should follow that 4:3 scale rather than arbitrary values.
- **Inter** for interface text and **Manrope** with tabular numerals for the timer. Both fonts are loaded in [`src/app/layout.tsx`](./src/app/layout.tsx); the timer UI remains planned.
- Near-white background, charcoal work text, muted sage break text, blue-gray work progress, and restrained teal controls. Use semantic tokens rather than hardcoded colors.
- **shadcn/ui first** for generic controls. Its configured UI alias is `@/shadcn/ui`; leave [`components.json`](./components.json) unchanged. The app has no installed shadcn UI components yet.
- **Lucide icons** via the installed `@iconify-react/lucide` package, sourced from `gitlab:iconify-icons/react#lucide`. Check actual exports before importing; use the [Lucide icon reference](https://icon-sets.iconify.design/lucide/) for names.

Keep the UI sparse and faithful to [`design/`](./design/). Compose shadcn components with normal Tailwind utilities and named tokens. Do not add custom CSS classes, CSS modules, ad hoc inline styles, or arbitrary Tailwind sizing/color values. Full implementation rules live in [`AGENTS.md`](./AGENTS.md).

## Responsive design

**Every new or changed UI feature must work mobile first.** Desktop reference images do not set fixed mobile dimensions. Use unprefixed Tailwind utilities for mobile, `md:` at 768px for tablet, `lg:` at 1024px for desktop, and `sm:` at 640px only when needed. Use existing tokens and utilities, without custom breakpoints, media queries, CSS classes, or magic responsive sizes.

Keep countdown, phase and status, controls, progress, settings, and notifications usable on narrow screens when present. Touch targets must work without hover; text and dialogs must fit or scroll without accidental horizontal overflow. Inspect affected UI around **375px, 768px, and 1280px**, plus layout transitions when needed. See the [mandatory responsive rules and quality gate](./AGENTS.md).

## Accessibility and usability

Treat **WCAG 2.2 AA** as a release requirement: at least **4.5:1** contrast for normal text, **3:1** for qualifying large text, and **3:1** for meaningful non-text controls and indicators. Check rendered states, including muted text, overlays, focus, progress, and notifications, at relevant widths. Do not rely on opacity, color alone, or hover for essential meaning or actions. Preserve keyboard access, visible focus, and usable touch targets.

Follow [`nielsen-heuristics.md`](./nielsen-heuristics.md), especially **visibility of system status**: show timer phase and running/paused state, respond promptly to actions, and provide recovery from failures. Where useful, use shadcn Sonner toasts at top right for transitions; reserve Spinner/Skeleton for genuine waits, not the running countdown. [`AGENTS.md`](./AGENTS.md) is authoritative for details.

## Using an AI coding assistant

**Before asking any coding AI to edit this project, provide the repository-root [`AGENTS.md`](./AGENTS.md) and explicitly instruct the assistant to read and follow it in full.** This applies to Codex, Claude, Cursor, ChatGPT, and other assistants; do not assume automatic discovery. For UI work, also point it to [`nielsen-heuristics.md`](./nielsen-heuristics.md), [`components.json`](./components.json), and [`design/`](./design/). This README is for orientation; `AGENTS.md` is the binding source for engineering and design rules.

Example prompt:

```text
Before making changes, read and follow AGENTS.md in full. For UI work, also read nielsen-heuristics.md and components.json, and inspect ./design/. Respect the existing stack, Bun-only commands, shadcn-first approach, named design tokens, WCAG contrast rules, and mobile-first Tailwind breakpoints. Check mobile, tablet, and desktop behavior. Inspect relevant source files, then explain proposed changes before editing.
```

## Contributing

Keep changes small and scoped. Coordinate before changing established design decisions, and report how you checked your work. Run applicable Bun checks above; UI changes also need the responsive and accessibility checks in [`AGENTS.md`](./AGENTS.md). For Next.js background, see the [documentation](https://nextjs.org/docs), [interactive tutorial](https://nextjs.org/learn), and [source repository](https://github.com/vercel/next.js).
