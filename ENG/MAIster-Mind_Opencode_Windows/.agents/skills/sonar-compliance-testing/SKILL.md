---
name: sonar-compliance-testing
description: frontend sonar rules focused on testing
---

## 🛡️ Sonar Best Practices to Avoid Issues

### 1. Tests - Mandatory Rules

```typescript
// ❌ BLOCKER - Test without assertion (S2699)
it('should do something', () => {
  render(<Component />);
});

// ✅ CORRECT - Always add an assertion
it('should do something', () => {
  render(<Component />);
  expect(screen.getByText('Expected')).toBeInTheDocument();
});
```

### 2. Nested Ternaries (S3358)

```typescript
// ❌ MAJOR - Nested ternary
const result = a ? (b ? 'x' : 'y') : 'z';

// ✅ CORRECT - Extract to function or use if/else
const getResult = () => {
  if (!a) return 'z';
  return b ? 'x' : 'y';
};
```

### 3. Cognitive Complexity (S3776)

```typescript
// ❌ CRITICAL - Complexity > 15
function complexFunction() {
  if (a) {
    if (b) {
      if (c) {
        /* ... */
      }
    }
  }
  // More conditions...
}

// ✅ CORRECT - Extract to sub-functions
function complexFunction() {
  if (!a) return handleNotA();
  if (!b) return handleNotB();
  return handleC(c);
}
```

### 4. Optional Chaining (S6582)

```typescript
// ❌ MAJOR - Using && for null check
const value = obj && obj.prop && obj.prop.value;

// ✅ CORRECT - Optional chaining
const value = obj?.prop?.value;
```

### 5. Promise in void Context (S6544)

```typescript
// ❌ BUG - Promise returned where void expected
<button onClick={async () => await fetchData()}/>

// ✅ CORRECT - Wrap the Promise
<button onClick={() => { void fetchData(); }}>

// or
<button onClick={() => { fetchData().catch(console.error); }}>
```

### 6. Index as React Key (S6479)

```typescript
// ❌ MAJOR - Index used as key
{items.map((item, index) => <Item key={index} />)}

// ✅ CORRECT - Use a unique identifier
{items.map(item => <Item key={item.id} />)}

// If no unique id, generate a stable key
{items.map(item => <Item key={`${item.name}-${item.type}`} />)}
```

### 7. Read-Only Props (S6759)

```typescript
// ❌ MAJOR - Mutable props
interface Props {
  value: string;
}

// ✅ CORRECT - Props readonly
interface Props {
  readonly value: string;
}

// or use Readonly<> 
type Props = Readonly<{
  value: string;
}>;
```

### 8. Redundant Fragment (S6749)

```typescript
// ❌ MAJOR - Fragment with single child
return <>{content}</>;

// ✅ CORRECT - Return child directly
return content;
```

### 9. Context Value Memoization (S6481)

```typescript
// ❌ MAJOR - Object recreated on every render
<Context.Provider value={{ user, setUser }}>

// ✅ CORRECT - Memoize with useMemo
const value = useMemo(() => ({ user, setUser }), [user, setUser]);
<Context.Provider value={value}>
```

### 10. Redundant Type Alias (S6564)

```typescript
// ❌ MAJOR - Alias of primitive type
type UUID = string;

// ✅ CORRECT - Use type directly
// or create a branded type if need to distinguish
type UUID = string & { readonly __brand: unique symbol };
```

---

## 📋 Validation Checklist Before Commit

- [ ] All tests have at least one `expect()` assertion
- [ ] No nested ternaries
- [ ] Cognitive complexity < 15 per function
- [ ] Optional chaining used for null checks
- [ ] No index as key in React lists
- [ ] Props marked readonly
- [ ] Context values memoized
- [ ] No fragments with single child
- [ ] Branch coverage increased
