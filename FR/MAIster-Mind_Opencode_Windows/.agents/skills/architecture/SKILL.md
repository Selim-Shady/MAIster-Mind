---
name: architecture
description: force AI to follow architecture
---

# Screaming Architecture : Domain-First Structure

L'architecture doit révéler l'intention métier au premier coup d'œil. On regroupe par **Feature** et non par rôle technique (components/hooks/utils).

## 📂 Arborescence Standardisée

```text
src/
├── core/                   # Config globale (Axios, Contexts)
├── shared/                 # Composants UI atomiques, Hooks & Utils transverses
└── features/               # Dossiers par Domaine Métier (Screaming)
    └── billing-management/  # Nom explicite de la Feature
        ├── api/            # Appels services spécifiques à la feature
        ├── components/     # Composants internes (ex: InvoiceTable.tsx)
        ├── hooks/          # Logique d'état liée au domaine
        ├── types/          # Interfaces (ex: billingTypes.ts)
        ├── utils/          # Helpers métier purs
        └── __tests__/      # Tests unitaires et d'intégration
            ├── InvoiceTable.test.tsx
            └── useBilling.test.ts