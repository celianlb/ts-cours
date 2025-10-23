
## Union Types : "Ceci OU cela"

Un **Union Type** représente une valeur qui peut être l'un parmi plusieurs types.

```typescript
// id peut être string OU number
let id: string | number;
id = "abc123"; // ✅ OK
id = 123; // ✅ OK
id = true; // ❌ Erreur

// Use case : gestion flexible
function formatId(id: string | number): string {
  if (typeof id === "string") {
    return id.toUpperCase();
  } else {
    return `ID-${id}`;
  }
}
```

**Le pouvoir des unions :** TypeScript vous force à gérer tous les cas possibles.

### Union avec literal types

```typescript
// Status ne peut être QUE l'une de ces valeurs
type Status = "pending" | "success" | "error";

let orderStatus: Status = "pending"; // ✅
orderStatus = "shipped"; // ❌ Erreur : valeur non autorisée
```

## Intersection Types : "Ceci ET cela"

Un **Intersection Type** combine plusieurs types. L'objet doit satisfaire TOUS les types.

```typescript
interface Person {
  name: string;
  age: number;
}

interface Employee {
  companyId: string;
  position: string;
}

// EmployeePerson doit avoir TOUTES les propriétés
type EmployeePerson = Person & Employee;

const employee: EmployeePerson = {
  name: "Alice",
  age: 30,
  companyId: "EMP001",
  position: "Developer",
};
```

## Literal Types : valeurs exactes comme types

Les **Literal Types** utilisent une valeur spécifique comme type.

```typescript
// direction ne peut être que ces 4 valeurs exactes
let direction: "north" | "south" | "east" | "west";
direction = "north"; // ✅
direction = "northwest"; // ❌ Erreur

// Avec des numbers
let diceRoll: 1 | 2 | 3 | 4 | 5 | 6;
```

### Discriminated Unions : Le pattern élégant

En combinant literal types et unions, vous créez des types auto-documentés.

```typescript
interface SuccessResponse {
  status: "success"; // Literal type unique
  data: any;
}

interface ErrorResponse {
  status: "error"; // Literal type différent
  error: string;
}

interface LoadingResponse {
  status: "loading";
}

type ApiResponse = SuccessResponse | ErrorResponse | LoadingResponse;

function handleResponse(response: ApiResponse): void {
  // TypeScript narrow automatiquement sur 'status'
  switch (response.status) {
    case "success":
      console.log(response.data); // ✅ data existe
      break;
    case "error":
      console.log(response.error); // ✅ error existe
      break;
    case "loading":
      console.log("Loading..."); // ✅ pas d'autres propriétés
      break;
  }
}
```

## Type Aliases : nommer des types complexes

```typescript
// Au lieu de répéter ce type partout
function getPoint(): { x: number; y: number } { ... }

// Créez un alias
type Point = {
  x: number;
  y: number;
};

function getPoint(): Point { ... }

// Alias pour unions
type ID = string | number;
type Callback = (data: string) => void;
```

## Null et Undefined : gérer l'absence

Avec `strictNullChecks: true`, `null` et `undefined` doivent être gérés explicitement.

```typescript
// ❌ Avec strictNullChecks
let name: string = null; // Erreur

// ✅ Autoriser null explicitement
let nullableName: string | null = null;

// Optional chaining
interface User {
  name: string;
  address?: {
    city: string;
  };
}

const user: User = { name: "Alice" };
console.log(user.address?.city); // undefined (pas d'erreur)

// Nullish coalescing
const city = user.address?.city ?? "Unknown";
```

## Types spéciaux

### void : fonction sans retour

```typescript
function logMessage(message: string): void {
  console.log(message);
  // Pas de return
}
```

### never : ne retourne jamais

```typescript
function throwError(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}
```

### object : n'importe quel objet

```typescript
function acceptObject(obj: object): void {
  console.log(obj);
}

acceptObject({ name: "Alice" }); // ✅
acceptObject([1, 2, 3]); // ✅
acceptObject(42); // ❌ number est primitif
```
