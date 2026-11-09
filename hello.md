# DemoProject

## Project overview

DemoProject is a web application built with Next.js and TypeScript. The app uses the Next.js App Router, so routes, layouts, and pages live under the `app` directory. The UI is React. The project runs on Node.js and is managed with npm.

TypeScript is the only application language. Type checking is strict, so components, route handlers, and shared utilities are typed from the start.

**Current status:** the stack and folder layout below are the project specification. Application source has not been scaffolded yet. When the app is created, it should follow this document.

## Goals

- Serve pages through the Next.js App Router.
- Keep all application code in TypeScript.
- Use Server Components by default and Client Components only where the browser is required.
- Keep routing, UI, and shared code in separate folders.
- Run, build, and type-check the app with npm scripts.

## Tech stack

| Area | Choice | Role |
| --- | --- | --- |
| Framework | Next.js (App Router) | Routing, rendering, layouts, and the production server |
| UI library | React | Components and the page UI |
| Language | TypeScript | Types for components, routes, and shared code |
| Runtime | Node.js | Runs the Next.js dev server and production server |
| Package manager | npm | Installs dependencies and runs scripts |
| Type checking | `tsc` via `tsconfig.json` | Strict checking. Next.js compiles the app |
| Module style | ES modules | Next.js default for App Router projects |
| Styling | CSS | Global styles in `app/globals.css`. Component styles can use CSS modules |
| Lint | ESLint | Next.js recommended rules, added when the app is scaffolded |

Versions are pinned in `package.json` at scaffold time. Do not guess them in this document.

### What each part does

- **Next.js App Router** maps folders inside `app` to URLs. `layout.tsx` wraps child routes. `page.tsx` is the UI for a URL. `loading.tsx` and `error.tsx` cover loading and error states. `route.ts` is for Route Handlers (HTTP APIs).
- **React Server Components** are the default in the App Router. A file becomes a Client Component only when it starts with `"use client"`.
- **TypeScript** types props, route params, server data, and shared helpers. `strict` is enabled in `tsconfig.json`.
- **Node.js** is required to run `next dev`, `next build`, and `next start`.
- **npm** installs Next.js, React, and TypeScript and runs the scripts below.

### npm scripts (after scaffold)

| Script | Command | Purpose |
| --- | --- | --- |
| `dev` | `next dev` | Local development with hot reload |
| `build` | `next build` | Production build |
| `start` | `next start` | Serve the production build |
| `lint` | `next lint` | Run ESLint |
| `typecheck` | `tsc --noEmit` | Type-check without emitting files |

## Project structure

Intended layout once the Next.js app is created:

```text
DemoProject/
├── app/
│   ├── layout.tsx          # Root layout. Shared html, body, and metadata
│   ├── page.tsx            # Home route at /
│   ├── globals.css         # Global styles
│   ├── favicon.ico
│   ├── loading.tsx         # Optional loading UI for the root segment
│   ├── error.tsx           # Optional error UI for the root segment
│   ├── not-found.tsx       # Optional 404 UI
│   └── api/
│       └── health/
│           └── route.ts    # Example Route Handler at /api/health
├── components/
│   └── .gitkeep            # Shared React components
├── lib/
│   └── .gitkeep            # Shared helpers that are not React components
├── public/                 # Static files served from /
├── types/
│   └── .gitkeep            # Shared TypeScript types
├── hello.md                # This project specification
├── next.config.ts          # Next.js configuration
├── tsconfig.json           # TypeScript configuration (strict)
├── next-env.d.ts           # Next.js TypeScript declarations
├── eslint.config.mjs       # ESLint configuration
├── package.json
├── package-lock.json
└── .gitignore
```

### Folder rules

| Path | Put here | Do not put here |
| --- | --- | --- |
| `app/` | Routes, layouts, route-level loading and error UI, Route Handlers | Shared widgets used by many routes |
| `components/` | Reusable React components | Route files (`page.tsx`, `layout.tsx`) |
| `lib/` | Non-UI helpers: formatting, data access, constants | JSX components |
| `types/` | Types shared across routes and components | One-off types that belong next to a single file |
| `public/` | Images, icons, and other static assets | TypeScript or React source |

Route-specific components can stay next to their route, for example `app/settings/settings-form.tsx`, when only that route uses them.

### App Router conventions

- A folder is a route segment. `app/settings/page.tsx` is `/settings`.
- Parentheses groups do not affect the URL: `app/(marketing)/about/page.tsx` is `/about`.
- A segment named `[id]` is a dynamic param. Type it as a promise-aware param in the current Next.js App Router API when the scaffolded version requires that.
- `layout.tsx` persists across navigations under that segment and must render `{children}`.
- The root layout must include `<html>` and `<body>`.
- Add `"use client"` only for state, effects, browser events, or browser-only APIs.

## Configuration

### `tsconfig.json`

- `"strict": true`
- Next.js plugin enabled (`"plugins": [{ "name": "next" }]`)
- Path alias `@/*` pointing at the project root, so imports look like `@/components/...` and `@/lib/...`
- Include `next-env.d.ts`, `**/*.ts`, `**/*.tsx`, and `.next/types/**/*.ts`

### `next.config.ts`

Start with the default Next.js config object. Add options only when a feature needs them (images, redirects, headers, experimental flags).

### Environment variables

- Local secrets and machine-specific values go in `.env.local`.
- `.env.local` stays out of git.
- Variables read by browser code must start with `NEXT_PUBLIC_`.
- Server-only variables have no prefix and are read in Server Components, Route Handlers, and other server code.

No environment variables are required for the empty app.

## Request flow

1. The browser requests a URL.
2. Next.js matches that URL to a folder under `app/`.
3. The root `layout.tsx` wraps the matched `page.tsx`. Nested layouts wrap their own child segments.
4. Server Components render on the server. Client Components hydrate in the browser.
5. A request to `/api/...` runs the matching `route.ts` and returns a `Response`. It does not render a page.

## Development workflow

1. Use a current Node.js LTS release.
2. Scaffold the app in this folder with Next.js, TypeScript, the App Router, and npm.
3. Confirm `npm run dev` serves the home page.
4. Confirm `npm run typecheck` and `npm run lint` pass.
5. Add routes under `app/`, shared UI under `components/`, and shared logic under `lib/`.

## Out of scope until chosen

These are not part of the project yet:

- Database and ORM
- Authentication
- A component library or CSS framework
- Automated tests
- Deployment target

Add any of those to this document when the choice is made.

## Document maintenance

`hello.md` is the living specification. Update it in the same change as any future development or revision so the document matches the project.

Update the affected sections when any of these change:

- Overview, goals, or status
- Tech stack, dependencies, or npm scripts
- Project structure, routing, or folder rules
- Configuration, environment variables, or request flow
- Features added, removed, or moved in or out of scope

Each update also adds one row to the revision history: the date and what changed. Remove anything that is no longer true.

## Revision history

| Date | Change |
| --- | --- |
| 2026-11-09 | Initial specification: Next.js App Router, TypeScript, Node.js, React, npm |
| 2026-11-09 | Require this file to be updated on every future development and revision |
