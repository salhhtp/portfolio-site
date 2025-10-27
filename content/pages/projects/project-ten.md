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
# V2 Development for a Production Legal-Tech Platform

_A modern React/TypeScript rebuild that upgrades the UX, performance, and reliability of an already-live AI-assisted contract review product._

> **TL;DR**  
> I led a front-end refactor to a production legal-tech platform used for automated contract review (NDAs, consultancy agreements, DPAs, etc.).  
> The new V2 front-end delivers faster loads, cleaner navigation, stronger states & error handling, and a cohesive design system.  
> All client/company identifiers are intentionally omitted. Figma design work (tokens, components, flows) exists but is **not** committed to the repo.

---

## Background & Goals

The product was already on the market with paying users. The mandate for the V2 front-end was to:

- **Modernize the UX** without disrupting current users’ workflows.
- **Improve performance & reliability** in complex, long-running review sessions.
- **Standardize the UI** with a scalable, token-driven design system.
- **Harden states & errors** across async data flows and role-based features.
- **Enable faster iteration** for future features (compliance reports, bulk review, dashboards).

_This page keeps the platform and client fully anonymous by design._

---

## What I Delivered

- A **React + TypeScript** front-end with a clean feature-folder architecture.
- **Design system (in Figma)**: color & typography tokens, spacing scales, component library, page templates, and interaction specs.
- **Resilient data layer** with request caching, retries, optimistic UI, and robust error boundaries.
- **Accessibility & i18n-ready** components with semantic markup and keyboard coverage.
- **CI/CD & environment hygiene** so sensitive keys never touch source control.

---

## UX & Design System (Figma)

> The Figma source is **not** in the repo; it’s maintained privately for handoff.

- **Design tokens**: color (light/dark), spacing, radius, elevation, motion.
- **Typography scale** with responsive ramps and legible line-lengths for legal text.
- **Component library**: inputs, tables, tabs, accordions, toasts, dialogs, empty states.
- **Patterns**: wizard flows for multi-step reviews, side-panel inspectors, diff viewers.
- **Prototypes** for onboarding, document upload → processing → results → export.
- **Specs & annotations** for dev handoff (states, error cases, loading skeletons).

---

## Architecture

```text
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
