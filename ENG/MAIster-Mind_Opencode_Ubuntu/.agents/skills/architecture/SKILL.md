---
name: architecture
description: force AI to follow architecture
---

# Screaming Architecture: Domain-First Structure

The architecture must reveal the business intention at first glance. Group by **Feature** rather than by technical role (components/hooks/utils).

## 📂 Standardized Directory Structure

```text
src/
├── core/                   # Global config (Axios, Contexts)
├── shared/                 # Atomic UI components, cross-cutting Hooks & Utils
└── features/               # Folders by Business Domain (Screaming)
    └── billing-management/  # Explicit Feature name
        ├── api/            # Feature-specific service calls
        ├── components/     # Internal components (e.g.: InvoiceTable.tsx)
        ├── hooks/          # Domain-specific state logic
        ├── types/          # Interfaces (e.g.: billingTypes.ts)
        ├── utils/          # Pure business helpers
        └── __tests__/      # Unit and integration tests
            ├── InvoiceTable.test.tsx
            └── useBilling.test.ts
```
