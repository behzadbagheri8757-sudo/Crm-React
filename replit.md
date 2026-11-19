# Bagheri CRM

Persian, right-to-left CRM for a family-run pulses and dry-goods business, wrapped in an iPhone-first workspace while retaining its offline-first customer, sales, inventory, and accounting workflows.

## Run & Operate

- `pnpm --filter @workspace/bagheri-crm run dev` — run the web app through its managed workflow
- `pnpm --filter @workspace/bagheri-crm run typecheck` — check the app's TypeScript
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages

The CRM itself does not use the workspace PostgreSQL database. Its existing data remains in browser IndexedDB.

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Web app: React + Vite
- CRM data: existing offline-first IndexedDB engine and original business logic
- Locale: Persian (fa), RTL, Jalali calendar, Vazirmatn
- PWA: installable shell with service-worker caching for the app and CRM runtime

## Where things live

- `artifacts/bagheri-crm/src/` — React shell, route synchronization, theme, and PWA registration
- `artifacts/bagheri-crm/public/legacy/` — supplied CRM runtime, original page views, and shared iOS-inspired presentation layer
- `artifacts/bagheri-crm/reference/original-source.zip` — unchanged source archive
- `artifacts/bagheri-crm/public/legacy/js/models.js` and `js/db.js` — existing IndexedDB contract and storage behavior

## Architecture decisions

- Keep original CRM workflows and calculations in the same-origin embedded app; the React shell owns navigation, theme, and global chrome only.
- Preserve the existing `baqeriDB` database, `appdata` object store, and `main` record. Do not migrate, reset, or replace them.
- Keep offline caching separate for the root workspace shell and the legacy `/legacy/` PWA scope.

## Product

- The five main destinations are Dashboard, Customers, Reports, Invoices, and More.
- More retains the original modules, including products, inventory, suppliers, payments, checks, visits, prospects, watches, locations, settings, and backup/restore.
- Customer, invoice, inventory, and accounting actions continue through the supplied CRM's existing code.

## User preferences

Preserve the supplied CRM's business logic, IndexedDB data structure, backup compatibility, and offline-first behavior while redesigning its interface.

## Gotchas

IndexedDB is scoped to the browser origin. The source archive contains application code, not a user's browser database. The Vite artifact workflow supplies `PORT` and `BASE_PATH`; use the managed workflow rather than starting it without those values.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
