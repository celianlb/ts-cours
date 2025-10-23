---
title: "8️⃣ Type Guards et Narrowing"
description: "Apprends à affiner les types à l’exécution grâce aux Type Guards et au narrowing pour éviter les erreurs de typage dynamiques."
---

Le **Narrowing** est le processus par lequel TypeScript affine un type général vers un type plus spécifique.

## typeof : Type Guard basique

```typescript
function padLeft(value: string, padding: string | number): string {
  if (typeof padding === "number") {
    return " ".repeat(padding) + value
  }
  return padding + value
}
```

TypeScript comprend que dans le `if`, `padding` est forcément `number`.

## instanceof : pour les classes

```typescript
class Dog {
  bark(): void {
    console.log("Woof!")
  }
}

class Cat {
  meow(): void {
    console.log("Meow!")
  }
}

function makeSound(animal: Dog | Cat): void {
  if (animal instanceof Dog) {
    animal.bark()
  } else {
    animal.meow()
  }
}
```

## in : vérifier l'existence d'une propriété

```typescript
interface Bird {
  fly(): void
  layEggs(): void
}

interface Fish {
  swim(): void
  layEggs(): void
}

function move(animal: Bird | Fish): void {
  if ("fly" in animal) {
    animal.fly() // TypeScript sait que c'est Bird
  } else {
    animal.swim() // TypeScript sait que c'est Fish
  }
}
```

## Type Predicates : Custom Type Guards

```typescript
interface User {
  id: number
  name: string
  email: string
}

interface AdminUser extends User {
  role: "admin"
  permissions: string[]
}

// Custom type guard avec "is"
function isAdmin(user: User): user is AdminUser {
  return "role" in user && (user as any).role === "admin"
}

function handleUser(user: User | AdminUser): void {
  if (isAdmin(user)) {
    console.log(user.permissions) // TypeScript sait que c'est AdminUser
  } else {
    console.log(user.name) // User
  }
}
```

**La syntaxe `user is AdminUser`** dit à TypeScript : "Si cette fonction retourne true, alors user est AdminUser".

## Discriminated Unions : Le pattern optimal

```typescript
interface SuccessResponse {
  status: "success"
  data: any
}

interface ErrorResponse {
  status: "error"
  error: string
}

type ApiResponse = SuccessResponse | ErrorResponse

function handleResponse(response: ApiResponse): void {
  switch (response.status) {
    case "success":
      console.log(response.data) // ✅ data existe
      break
    case "error":
      console.log(response.error) // ✅ error existe
      break
  }
}
```

## Exhaustiveness Checking

```typescript
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; size: number }
  | { kind: "rectangle"; width: number; height: number }

function getArea(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2
    case "square":
      return shape.size ** 2
    case "rectangle":
      return shape.width * shape.height
    default:
      const _exhaustiveCheck: never = shape
      return _exhaustiveCheck
  }
}
```

Si vous ajoutez un nouveau type à Shape, TypeScript détectera que le switch n'est plus exhaustif.
