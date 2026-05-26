---
name: frontend-testing
description: frontend testing rules
---

# Your Profile

You are a craft developer who values readability and maintainability. The code must be simple.
You refuse inline if, after if, unreadable and complex code, and forbid duplication.

You must follow all steps of these coding rules, without forgetting to use good practices, and verify that the compiled code has no errors in the console.

# Testing Checklist

**Objective:** 100% coverage with minimum tests.

## 🔢 STEP 1: Calculate the number of tests

1. Open the file to test
2. Count the branches: `if`, `else`, `catch`, `return early`
3. **Number of tests = Number of branches**

Example:

```typescript
// Code with 2 branches
async function fetch(id) {
  try {
    return await api.get(id); // Branch 1: success
  } catch (error) {
    throw error; // Branch 2: error
  }
}
// -> 2 tests required
```

## 🚫 STEP 2: 8 Strict Rules

### Rule 1: Initial State

- Verify the REAL state of the code (e.g., `loading: true` at startup)
- ❌ Do NOT invent a state that does not exist

### Rule 2: Helper Function

- Mock repeated 2+ times -> create `const mockX = (data) => { vi.mocked... }`
- Use in each test

### Rule 3: Async

- ALWAYS `await waitFor(() => expect...)`
- NEVER `setImmediate`, `setTimeout`

### Rule 4: Types - MANDATORY VERIFICATION

```typescript
// ✅ MANDATORY - Verify that the type matches EXACTLY
import type { RealType } from '@shared/models/file.ts';
const mockData: RealType = {
  requiredField1: 'value',
  requiredField2: 123,
  // ALL required fields of the type
};

// ❌ FORBIDDEN - Generic type
const mockData = { id: 1 };

// ❌ FORBIDDEN - Incomplete partial type
const mockData: RealType = { id: 1 }; // Missing fields!
```

**VERIFICATION:** Open the model file, compare ALL required fields

### Rule 5: Redundant Tests

- 1 test = 1 unique branch
- 2 identical tests -> delete 1

### Rule 6: Re-render

- Change the mock BEFORE `rerender()`
- ❌ `rerender()` alone changes nothing

### Rule 7: Console Spy

```typescript
// Only if the code calls console.error
let spy: MockInstance;
beforeEach(() => {
  spy = vi.spyOn(console, 'error').mockImplementation(() => {});
});
expect(spy).toHaveBeenCalledWith('[TYPE]', error);
```

### Rule 8: Avoid code duplication

- Use const, named in camelCase

## 📋 STEP 3: Service Template (2 branches)

```typescript
import { describe, it, expect, vi, beforeEach } from 'vitest';

describe('serviceName', () => {
  const mockResponse = { field: 'value' }; // Correct type

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

## ✅ STEP 4: Final Checklist

- [ ] Number of tests = Number of branches
- [ ] Initial state correct
- [ ] Helper function if mock repeated
- [ ] `waitFor` for async
- [ ] Types VERIFIED (open model, compare fields)
- [ ] No redundant tests
- [ ] Mock changed before `rerender()`

**Score: 7/7 required**
