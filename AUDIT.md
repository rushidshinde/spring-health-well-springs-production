# Webflow Component & Asset Dependency Audit

This document provides a comprehensive audit of all UI components in `components/`, detailing their source JavaScript import dependencies and the required asset embed files from `dist/` needed for Webflow deployment in **Spring Health Well Springs**.

---

## 📌 Shared Asset Embeds Reference

All shared asset embed scripts are compiled into `dist/assets/`. When a component depends on an asset embed, that asset embed script must be included on the Webflow page (or globally in Site Settings) prior to running the component snippet.

| Asset Embed File | Exposed Global Properties on `window.SH` | Purpose & Description |
|---|---|---|
| **`dist/core/main-core.html`** | `window.SH.registry`, `window.SH.register`, `window.SH.insertStyles`, `window.SH._initialized` | **Core Bundle**: Global stylesheet (`main.css`), animation controller, style inserter helper, and component lifecycle runner. **Required by all components.** |
| **`dist/assets/asset-classes.html`** | `window.SH.Classes` | Mappings for standard CSS state/utility classes (`ACTIVE`, `DISABLED`, `OPEN`, `CLOSED`, `LOCKED`, `CLONE`). |
| **`dist/assets/asset-breakpoints.html`** | `window.SH.Breakpoints` | Media query breakpoint constants matching CSS responsive design rules. |
| **`dist/assets/asset-scroll-lock.html`** | `window.SH.lockScroll`, `window.SH.unlockScroll`, `window.SH.toggleScroll` | Utilities for locking and unlocking body scroll (used by header menu/dialogs). |
| **`dist/assets/asset-accordion.html`** | `window.SH.Accordion` | Accordion widget constructor and event handler logic (`index-a83eb4d7.js`). |
| **`dist/assets/asset-tabs.html`** | `window.SH.Tabs` | Accessible tab navigation widget constructor class (`index-20230311.js`). |
| **`dist/assets/asset-pagination.html`** | `window.SH.Pagination` | Dynamic client-side pagination controller class (`index-dcd7da8e.js`). |

---

## 📋 Component Dependency Matrix

Below is the complete audit matrix for all active components:

| Component Name | Source Import Chunks | Required Asset Embeds in `dist/assets/` | Webflow Embed Status |
|---|---|---|---|
| **`header`** | `insert-styles`, `scroll-lock`, `breakpoints`, `classes` | `asset-scroll-lock.html`, `asset-breakpoints.html`, `asset-classes.html` | ✅ OK (15.2 KB) |
| **`footer`** | `index-a83eb4d7` (Accordion), `insert-styles` *(side-effect `index-20230311`)* | `asset-accordion.html` | ✅ OK (0.9 KB) |
| **`faq`** | `index-a83eb4d7` (Accordion), `insert-styles` *(side-effect `index-20230311`)* | `asset-accordion.html` | ✅ OK (0.8 KB) |
| **`skip-navigation`** | `insert-styles` | *(None - Core only)* | ✅ OK (0.5 KB) |
| **`filter-replays`** | `classes`, `index-dcd7da8e` (Pagination), `insert-styles` *(side-effect `index-20230311`)* | `asset-classes.html`, `asset-pagination.html` | ✅ OK (2.2 KB) |
| **`tabs-review`** | `index-20230311` (Tabs), `insert-styles` | `asset-tabs.html` | ✅ OK (1.6 KB) |

---

## 🛠 Maintenance & Re-auditing

To refresh this audit table automatically when component imports or source files change, run:
```bash
node scripts/audit-imports.js
```
