---
title: "9️⃣ Utility Types"
description: "Explore les Utility Types intégrés à TypeScript comme Partial, Pick, Omit et Record pour manipuler tes types efficacement."
---

TypeScript fournit des **Utility Types** prédéfinis pour transformer des types.

## Partial<T> : Tout optionnel

```typescript
interface User {
  id: number
  name: string
  email: string
}

type PartialUser = Partial<User>
// Équivalent à: { id?: number; name?: string; email?: string }

function updateUser(id: number, updates: Partial<User>): User {
  const user = getUser(id)
  return { ...user, ...updates }
}

updateUser(1, { name: "Alice" }) // ✅ Ne met à jour que name
```

## Required<T> : Tout obligatoire

```typescript
interface Config {
  host?: string
  port?: number
}

type RequiredConfig = Required<Config>
// Équivalent à: { host: string; port: number }
```

## Readonly<T> : Tout immuable

```typescript
interface User {
  name: string
  email: string
}

type ReadonlyUser = Readonly<User>

const user: ReadonlyUser = {
  name: "Alice",
  email: "alice@email.com",
}

user.name = "Bob" // ❌ Erreur : readonly
```

## Pick<T, K> : Sélectionner des propriétés

```typescript
interface User {
  id: number
  name: string
  email: string
  age: number
  address: string
}

type UserPreview = Pick<User, "name" | "email">
// Équivalent à: { name: string; email: string }

function displayPreview(user: UserPreview): void {
  console.log(`${user.name} (${user.email})`)
}
```

## Omit<T, K> : Exclure des propriétés

```typescript
interface User {
  id: number
  name: string
  email: string
  password: string
}

type SafeUser = Omit<User, "password">
// Équivalent à: { id: number; name: string; email: string }

function sendToClient(user: SafeUser): void {
  // Impossible d'envoyer le password accidentellement
}
```

## Record<K, T> : Dictionnaire typé

```typescript
type Role = "admin" | "user" | "guest"
type RolePermissions = Record<Role, string[]>

const permissions: RolePermissions = {
  admin: ["read", "write", "delete"],
  user: ["read", "write"],
  guest: ["read"],
}

// TypeScript vérifie que toutes les clés sont présentes
```

## Exclude<T, U> et Extract<T, U>

```typescript
type AllTypes = string | number | boolean

// Exclude : retire boolean
type StringOrNumber = Exclude<AllTypes, boolean> // string | number

// Extract : garde uniquement string
type OnlyString = Extract<AllTypes, string> // string
```

## NonNullable<T> : Retirer null et undefined

```typescript
type MaybeString = string | null | undefined
type DefiniteString = NonNullable<MaybeString> // string
```

## ReturnType<T> et Parameters<T>

```typescript
function getUser() {
  return {
    id: 1,
    name: "Alice",
    email: "alice@email.com",
  }
}

// Extraire le type de retour
type User = ReturnType<typeof getUser>

function createUser(name: string, age: number) {
  return { name, age }
}

// Extraire les types des paramètres
type CreateUserParams = Parameters<typeof createUser>
// [string, number]
```

## Awaited<T> : Unwrap les Promises

```typescript
type AsyncString = Promise<string>
type SyncString = Awaited<AsyncString> // string

// Nested promises
type DeepPromise = Promise<Promise<number>>
type Unwrapped = Awaited<DeepPromise> // number
```
