---
name: frontend-testing
description: frontend testing rules
---

# Ton profil

Tu es un développeur crafts, qui veut de la lisibilité et de la maintenabilité. Le code doit être simple.
Tu refuses if inline, after if, code illisible et compliqué, et interdit la duplication.

Tu dois suivre toutes les étapes de ces règles de codage, sans oublier d'utiliser de bonnes pratiques, et vérifier que le code compilé ne présente aucune erreur dans la console.

# Testing Checklist

**Objectif**: 100% coverage avec minimum de tests.

## 🔢 ÉTAPE 1: Calculer le nombre de tests

1. Ouvrir le fichier à tester
2. Compter les branches: `if`, `else`, `catch`, `return early`
3. **Nombre de tests = Nombre de branches**

Exemple:

```typescript
// Code avec 2 branches
async function fetch(id) {
  try {
    return await api.get(id); // Branche 1: success
  } catch (error) {
    throw error; // Branche 2: error
  }
}
// → 2 tests nécessaires
```

## 🚫 ÉTAPE 2: 8 Règles strictes

### Règle 1: État initial

- Vérifier l'état RÉEL du code (ex: `loading: true` au démarrage)
- ❌ Ne PAS inventer un état qui n'existe pas

### Règle 2: Helper function

- Mock répété 2+ fois → créer `const mockX = (data) => { vi.mocked... }`
- Utiliser dans chaque test

### Règle 3: Async

- TOUJOURS `await waitFor(() => expect...)`
- JAMAIS `setImmediate`, `setTimeout`

### Règle 4: Types - VÉRIFICATION OBLIGATOIRE

```typescript
// ✅ OBLIGATOIRE - Vérifier que le type correspond EXACTEMENT
import type { RealType } from '@shared/models/file.ts';
const mockData: RealType = {
  requiredField1: 'value',
  requiredField2: 123,
  // TOUS les champs requis du type
};

// ❌ INTERDIT - Type générique
const mockData = { id: 1 };

// ❌ INTERDIT - Type partiel incomplet
const mockData: RealType = { id: 1 }; // Manque des champs!
```

**VÉRIFICATION**: Ouvrir le fichier du modèle, comparer TOUS les champs requis

### Règle 5: Tests redondants

- 1 test = 1 branche unique
- 2 tests identiques → supprimer 1

### Règle 6: Re-render

- Changer le mock AVANT `rerender()`
- ❌ `rerender()` seul ne change rien

### Règle 7: Console spy

```typescript
// Seulement si le code appelle console.error
let spy: MockInstance;
beforeEach(() => {
  spy = vi.spyOn(console, 'error').mockImplementation(() => {});
});
expect(spy).toHaveBeenCalledWith('[TYPE]', error);
```

### Règle 8: Eviter la duplication de code

- Utiliser des const, nommées en camelCase

## 📋 ÉTAPE 3: Template service (2 branches)

```typescript
import { describe, it, expect, vi, beforeEach } from 'vitest';

describe('serviceName', () => {
  const mockResponse = { field: 'value' }; // Type correct

  const mockApi = (data: any | Error) => {
    if (data instanceof Error) {
      return vi.spyOn(api, 'method').mockRejectedValue(data);
    }
    return vi.spyOn(api, 'method').mockResolvedValue(data);
  };

  beforeEach(() => vi.clearAllMocks());

  it('should succeed', async () => {
    mockApi(mockResponse);
    const result = await service(123);
    expect(result).toEqual(mockResponse);
  });

  it('should propagate errors', async () => {
    mockApi(new Error('Failed'));
    await expect(service(123)).rejects.toThrow('Failed');
  });
});
```

## ✅ ÉTAPE 4: Checklist finale

- [ ] Nombre de tests = Nombre de branches
- [ ] État initial correct
- [ ] Helper function si mock répété
- [ ] `waitFor` pour async
- [ ] Types VÉRIFIÉS (ouvrir le modèle, comparer champs)
- [ ] Aucun test redondant
- [ ] Mock changé avant `rerender()`

**Score: 7/7 requis**
