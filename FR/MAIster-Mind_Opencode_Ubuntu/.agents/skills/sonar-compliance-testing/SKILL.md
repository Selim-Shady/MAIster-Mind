---
name: sonar-compliance-testing
description: frontend sonar rules focused on testing
---

## 🛡️ Bonnes pratiques Sonar pour éviter les issues

### 1. Tests - Règles obligatoires

```typescript
// ❌ BLOCKER - Test sans assertion (S2699)
it('should do something', () => {
  render(<Component />);
});

// ✅ CORRECT - Toujours ajouter une assertion
it('should do something', () => {
  render(<Component />);
  expect(screen.getByText('Expected')).toBeInTheDocument();
});
```

### 2. Ternaires imbriquées (S3358)

```typescript
// ❌ MAJOR - Ternaire imbriquée
const result = a ? (b ? 'x' : 'y') : 'z';

// ✅ CORRECT - Extraire dans une fonction ou utiliser if/else
const getResult = () => {
  if (!a) return 'z';
  return b ? 'x' : 'y';
};
```

### 3. Complexité cognitive (S3776)

```typescript
// ❌ CRITICAL - Complexité > 15
function complexFunction() {
  if (a) {
    if (b) {
      if (c) {
        /* ... */
      }
    }
  }
  // Encore plus de conditions...
}

// ✅ CORRECT - Extraire en sous-fonctions
function complexFunction() {
  if (!a) return handleNotA();
  if (!b) return handleNotB();
  return handleC(c);
}
```

### 4. Optional chaining (S6582)

```typescript
// ❌ MAJOR - Utilisation de && pour null check
const value = obj && obj.prop && obj.prop.value;

// ✅ CORRECT - Optional chaining
const value = obj?.prop?.value;
```

### 5. Promise dans contexte void (S6544)

```typescript
// ❌ BUG - Promise retournée où void attendu
<button onClick={async () => await fetchData()}>

// ✅ CORRECT - Wrapper la Promise
<button onClick={() => { void fetchData(); }}>

// ou
<button onClick={() => { fetchData().catch(console.error); }}>
```

### 6. Index comme clé React (S6479)

```typescript
// ❌ MAJOR - Index utilisé comme key
{items.map((item, index) => <Item key={index} />)}

// ✅ CORRECT - Utiliser un identifiant unique
{items.map(item => <Item key={item.id} />)}

// Si pas d'id unique, générer une clé stable
{items.map(item => <Item key={`${item.name}-${item.type}`} />)}
```

### 7. Props en lecture seule (S6759)

```typescript
// ❌ MAJOR - Props mutables
interface Props {
  value: string;
}

// ✅ CORRECT - Props readonly
interface Props {
  readonly value: string;
}

// ou utiliser Readonly<>
type Props = Readonly<{
  value: string;
}>;
```

### 8. Fragment redondant (S6749)

```typescript
// ❌ MAJOR - Fragment avec un seul enfant
return <>{content}</>;

// ✅ CORRECT - Retourner directement l'enfant
return content;
```

### 9. Mémoïsation du Context value (S6481)

```typescript
// ❌ MAJOR - Objet recréé à chaque render
<Context.Provider value={{ user, setUser }}>

// ✅ CORRECT - Mémoïser avec useMemo
const value = useMemo(() => ({ user, setUser }), [user, setUser]);
<Context.Provider value={value}>
```

### 10. Type alias redondant (S6564)

```typescript
// ❌ MAJOR - Alias de type primitif
type UUID = string;

// ✅ CORRECT - Utiliser directement le type
// ou créer un branded type si besoin de distinguer
type UUID = string & { readonly __brand: unique symbol };
```

---

## 📋 Checklist de validation avant commit

- [ ] Tous les tests ont au moins une assertion `expect()`
- [ ] Pas de ternaires imbriquées
- [ ] Complexité cognitive < 15 par fonction
- [ ] Optional chaining utilisé pour les null checks
- [ ] Pas d'index comme key dans les listes React
- [ ] Props marquées readonly
- [ ] Context values mémoïsées
- [ ] Pas de fragments avec un seul enfant
- [ ] Coverage des branches augmenté

---
