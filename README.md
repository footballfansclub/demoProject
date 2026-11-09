# DemoProject

A demo project by Sohel Rana. `hello.md` is the living specification. This README is the short entry point.

**Status:** application source has not been scaffolded. The folder currently holds `package.json`, `hello.md`, and this file.

## Current package

| Field | Value |
| --- | --- |
| Name | `demoproject` |
| Version | `1.0.0` |
| Author | Sohel Rana |
| License | ISC |
| Module style | CommonJS (`"type": "commonjs"`) |
| Entry | `index.js` (not created yet) |
| Dependency | `dotenv` |

```bash
npm install
```

`npm test` is a placeholder and exits with an error. There is no application script yet.

## Intended application

`hello.md` specifies the app to build next:

- Next.js with the App Router
- React and TypeScript
- Node.js and npm
- Routes under `app/`, shared UI under `components/`, shared helpers under `lib/`

Follow that document when scaffolding. After the app exists, the scripts to use are `dev`, `build`, `start`, `lint`, and `typecheck`, as listed in `hello.md`.

## Layout today

```text
DemoProject/
├── hello.md          # Living specification
├── README.md         # This file
├── package.json
├── package-lock.json
└── .cursor/rules/    # Rule that keeps hello.md in sync with the project
```
