# 6️⃣ Generics

Les **Generics** permettent de créer des composants réutilisables qui fonctionnent avec plusieurs types.

## Le problème sans Generics

```typescript
// ❌ Répétition de code
function getFirstNumber(arr: number[]): number | undefined {
  return arr[0]
}

function getFirstString(arr: string[]): string | undefined {
  return arr[0]
}

// ❌ Perte de type safety
function getFirst(arr: any[]): any {
  return arr[0]
}
```

## La solution : Generics

```typescript
// ✅ Une seule fonction, tous les types
function getFirst<T>(arr: T[]): T | undefined {
  return arr[0]
}

const firstNumber = getFirst([1, 2, 3]) // number | undefined
const firstName = getFirst(["a", "b"]) // string | undefined
```

**T** est un paramètre de type, comme une variable pour les types.

## Fonctions génériques

```typescript
function identity<T>(value: T): T {
  return value
}

// Plusieurs paramètres de type
function pair<T, U>(first: T, second: U): [T, U] {
  return [first, second]
}

const result = pair("age", 25) // [string, number]

// Avec contrainte extends
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key]
}

const user = { name: "Alice", age: 25 }
const name = getProperty(user, "name") // string
const invalid = getProperty(user, "salary") // ❌ Erreur
```

## Interfaces génériques

```typescript
interface Box<T> {
  value: T
}

const numberBox: Box<number> = { value: 42 }
const stringBox: Box<string> = { value: "hello" }

// Avec valeur par défaut
interface Response<T = any> {
  data: T
  status: number
}
```

## Classes génériques

```typescript
class GenericArray<T> {
  private items: T[] = []

  add(item: T): void {
    this.items.push(item)
  }

  get(index: number): T | undefined {
    return this.items[index]
  }

  getAll(): T[] {
    return [...this.items]
  }
}

const numberArray = new GenericArray<number>()
numberArray.add(1)
numberArray.add("2") // ❌ Erreur
```

## Contraintes sur les Generics

```typescript
// T doit avoir une propriété length
interface HasLength {
  length: number
}

function logLength<T extends HasLength>(item: T): void {
  console.log(item.length)
}

logLength("hello") // ✅ string a length
logLength([1, 2, 3]) // ✅ array a length
logLength(42) // ❌ number n'a pas length
```

## Use case réel : API Response

```typescript
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
  timestamp: Date
}

interface User {
  id: number
  name: string
}

async function fetchUser(id: number): Promise<ApiResponse<User>> {
  try {
    const data = await fetch(`/api/users/${id}`).then((r) => r.json())
    return {
      success: true,
      data,
      timestamp: new Date(),
    }
  } catch (error) {
    return {
      success: false,
      error: error.message,
      timestamp: new Date(),
    }
  }
}
```
