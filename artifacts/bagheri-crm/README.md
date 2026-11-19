# Bagheri CRM

Persian, right-to-left CRM for the Bagheri pulses and dry-goods business. The React workspace now contains first-class redesigned Dashboard, Customers, Customer Detail, and Invoice Register pages. Other CRM screens and the existing customer-creation form continue to use the supplied offline CRM inside a same-origin compatibility frame while they are migrated page by page.

## Run

From the workspace root:

```sh
pnpm install
BASE_PATH=/ PORT=5173 pnpm --filter @workspace/bagheri-crm run dev
```

In Replit, use the managed `artifacts/bagheri-crm: web` workflow; it provides the app's required environment and preview routing.

## Data and compatibility

- The supplied CRM remains the source of truth for customer, product, invoice, payment, inventory, watch, and backup/restore behavior.
- Dashboard, Customers, and Customer Detail read existing calculation outputs through a read-only same-origin bridge. Customer Detail calls the existing customer action functions only when the user explicitly taps an action; it does not add or replace financial calculations or database writes.
- The original storage contract remains `baqeriDB` / `appdata` / `main`; no database or business-logic migration is included.
- The original source archive is preserved at `reference/original-source.zip`.
- Browser IndexedDB is tied to the browser origin and is not included in this source archive. The archive contains no customer or financial records.
- Invoice, payment, check, visit, watch, and other remaining screens still depend on the compatibility frame and its service worker. Physical iPhone installation and offline behavior have not been verified in this stage.

## Checks

```sh
pnpm --filter @workspace/bagheri-crm run typecheck
```

The unchanged source archive contains regression scripts under `reference/original-source.zip`. Three existing customer-summary assertions currently fail; the full audit suite also needs a separate real backup fixture that is not included.

Stage 5 adds a first-class React invoice register at `/invoices`. It reads invoice rows and payment status through the same-origin read-only bridge, using the existing `invoiceEffectivePaid` and `invoiceEffectiveRemain` functions. Search, status filtering, and sorting are presentation-only. Selecting a row still opens the existing invoice detail route; invoice creation continues to use the original invoice form through the compatibility frame. Invoice detail is not yet migrated in this stage.
