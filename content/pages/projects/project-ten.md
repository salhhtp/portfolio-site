---
type: ProjectLayout
title: Renaissance Trading Engine
date: '2025-03-19'
client: ''
description: >-
  A project where I aimed to closely emulate "Renaissance Technologies'" trading
  strategies under a machine learning model with using Alpaca API for chart data
  and brokerage account.
featuredImage:
  type: ImageBlock
  url: /images/Ekran Resmi 2025-10-27 23.29.36.png
  altText: Project Six Image
  caption: ''
  elementId: ''
media:
  type: ImageBlock
  url: /images/Ekran Resmi 2025-10-27 23.29.36.png
  altText: Project Six image
  caption: Renaissance Trading Engine
  elementId: ''
bottomSections: []
addTitleSuffix: true
metaTags: []
colors: colors-a
backgroundImage:
  type: BackgroundImage
  url: /images/bg2.jpg
  backgroundSize: cover
  backgroundPosition: center
  backgroundRepeat: no-repeat
  opacity: 100
---
#### *A modern React/TypeScript rebuild that upgrades the UX, performance, and reliability of an already-live AI-assisted contract review product.*

> **TL;DR**\
> I led a front-end refactor to a production legal-tech platform used for automated contract review (NDAs, consultancy agreements, DPAs, etc.).\
> The new V2 front-end delivers faster loads, cleaner navigation, stronger states & error handling, and a cohesive design system.\
> All client/company identifiers are intentionally omitted. Figma design work (tokens, components, flows) exists but is **not** committed to the repo.

***

## Background & Goals

The product was already on the market with paying users. The mandate for the V2 front-end was to:

*   **Modernize the UX** without disrupting current users’ workflows.
*   **Improve performance & reliability** in complex, long-running review sessions.
*   **Standardize the UI** with a scalable, token-driven design system.
*   **Harden states & errors** across async data flows and role-based features.
*   **Enable faster iteration** for future features (compliance reports, bulk review, dashboards).

*This page keeps the platform and client fully anonymous by design.*

***

## What I Delivered

*   A **React + TypeScript** front-end with a clean feature-folder architecture.
*   **Design system (in Figma)**: color & typography tokens, spacing scales, component library, page templates, and interaction specs.
*   **Resilient data layer** with request caching, retries, optimistic UI, and robust error boundaries.
*   **Accessibility & i18n-ready** components with semantic markup and keyboard coverage.
*   **CI/CD & environment hygiene** so sensitive keys never touch source control.

***

## UX & Design System (Figma)

> The Figma source is **not** in the repo; it’s maintained privately for handoff.

*   **Design tokens**: color (light/dark), spacing, radius, elevation, motion.
*   **Typography scale** with responsive ramps and legible line-lengths for legal text.
*   **Component library**: inputs, tables, tabs, accordions, toasts, dialogs, empty states.
*   **Patterns**: wizard flows for multi-step reviews, side-panel inspectors, diff viewers.
*   **Prototypes** for onboarding, document upload → processing → results → export.
*   **Specs & annotations** for dev handoff (states, error cases, loading skeletons).

***

## Architecture

```
src/
  app/                 # routes (auth-gated), layout shells, error boundaries
  features/
    documents/         # upload, list, detail, status polling
    review/            # reviewers, issues, highlights, diffs, export
    compliance/        # report runs, history, downloads
    admin/             # (role-gated) users, org settings, limits
  components/          # shared UI: forms, tables, modals, skeletons
  lib/                 # api client, interceptors, analytics, feature flags
  state/               # query keys, stores, selectors
  styles/              # tokens, tailwind/csass modules
  hooks/               # reusable hooks (a11y, hotkeys, clipboard)
  tests/               # unit + e2e helpers
```

**Key traits**

*   **Routing**: authenticated layouts, protected sub-routes, 404/500 fallbacks.

*   **Error isolation**: per-feature error boundaries to avoid full-app crashes.

*   **Feature flags**: gradual rollouts and A/B scaffolding.

*   **Skeletons & placeholders** to keep app responsive during async work.

## Tech Stack

*   **UI**: React + TypeScript

*   **State & data**: TanStack Query (server cache), lightweight local store (e.g., Zustand)

*   **Forms**: React Hook Form + schema validation (e.g., Zod)

*   **Styling**: utility-first CSS (e.g., Tailwind) + headless primitives (e.g., Radix)

*   **Tables**: virtualized large lists where needed (react-window)

*   **Build**: modern toolchain with code-splitting & lazy routes

*   **Testing**: Vitest/Jest + React Testing Library; Playwright for critical flows

*   **CI/CD**: GitHub Actions, preview builds, environment-based config

*   **Security**: JWT/OAuth session handling, role-based UI gating, no secrets in repo

> The codebase is framework-friendly: deployable as a SPA or under SSR frameworks if needed.

## Feature Highlights

*   **Document intake**: drag-and-drop upload, type detection, progress, failure recovery.

*   **Review workspace**: synchronized panes (document ↔ issue list ↔ highlights).

*   **Search & filters**: quickly locate clauses, issues, or entities across large docs.

*   **Compliance reports**: trigger runs, track status, download artifacts with lookups.

*   **Bulk operations**: multi-select actions with guarded confirmations.

*   **Notifications & toasts**: consistent feedback for async actions.

*   **Exports**: configurable templates with sensible defaults.

## State, Data & Reliability

*   **Typed API client** with interceptors (auth refresh, 429 backoff, error mapping).

*   **Query caching & invalidation** for snappy navigations.

*   **Optimistic updates** where safe, with server-reconcile on failure.

*   **Offline-tolerant UX** for transient network hiccups.

*   **Uniform empty/loading/error** components so users always know what’s happening.

## Performance

*   Route-level **code splitting** and **on-demand** component loading.

*   **Virtualized** long tables/lists.

*   **Concurrent-safe** UI patterns (suspenseful skeletons, minimal layout shift).

*   **Asset discipline**: compress, prefetch critical paths, only ship what’s used.

## Accessibility

*   WCAG 2.1 AA targets; semantic HTML across controls.

*   **Keyboard navigation** & focus ring visibility on all interactive elements.

*   **Aria labelling** for composite widgets (tabs, dialogs, toasts).

*   Reduced-motion variants for motion-sensitive users.

## Developer Experience

*   **Strict TypeScript** + ESLint + Prettier.

*   **Commit hooks** for format & type checks.

*   **Testing utilities** for mocks, fixtures, and network stubs.

*   **Story-like sandboxes** (if enabled) for rapid UI iteration.

## Deployment & Environments

*   **Environment variables** for endpoints/keys (never committed).

*   **Staging previews** from PRs for product/design review.

*   **Error tracking** & basic telemetry hooks (redacted; configurable per env).

## Security & Privacy

*   Role-based access control in the UI (admin/features gated).

*   Safe handling of document URLs & tokens (no PII in logs).

*   Hardened file-upload flows with explicit size/type checks and server-validated links.

## Roadmap

*   Inline diff viewer for clause-by-clause comparisons

*   Saved views & sharable filters

*   Session restore for long reviews

*   Full dark mode with token parity

*   Improved bulk actions (smart selection, background jobs UX)

## What I Learned

*   **Design tokens first** pays off: faster theming and fewer inconsistencies.

*   Strong **error boundaries** and empty states prevent user confusion during long async tasks.

*   A typed **data layer with caching** is the backbone of a snappy, reliable app.

*   Keeping the **Figma source canonical** (and out of the repo) avoids asset drift.

## Links

*   **Code**: Front-end repository (with sensitive assets redacted).

*   **Design**: Figma design system & prototypes are available on request.

*   **Contact**: Happy to demo the workflow (upload → process → review → export) in a staging environment.

### Notes on Anonymity

This portfolio entry intentionally avoids naming the platform, company, or client.

All domain references are generic and any identifying details have been removed.
