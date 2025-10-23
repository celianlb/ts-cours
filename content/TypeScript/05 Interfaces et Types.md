
Les **Interfaces** et **Type Aliases** permettent de définir la forme (shape) d'un objet.

## Interface : définir la structure

Une **Interface** décrit exactement quelles propriétés doivent exister.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age?: number; // Optionnel
  readonly createdAt: Date; // Immuable
}

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@email.com",
  createdAt: new Date(),
};

user.name = "Bob"; // ✅ OK
user.createdAt = new Date(); // ❌ Erreur : readonly
```

## Propriétés optionnelles et readonly

```typescript
interface Config {
  readonly apiUrl: string; // Immuable
  timeout?: number; // Optionnel
}

const config: Config = {
  apiUrl: "https://api.example.com",
};

config.apiUrl = "https://new.com"; // ❌ Erreur
config.timeout = 5000; // ✅ OK
```

## Index Signature : propriétés dynamiques

```typescript
// Dictionnaire : n'importe quelle clé string → valeur
interface Dictionary {
  [key: string]: any;
}

const dict: Dictionary = {
  name: "Alice",
  age: 25,
  anything: true,
};

// Plus typé
interface StringMap {
  [key: string]: string;
}

const translations: StringMap = {
  hello: "bonjour",
  goodbye: "au revoir",
};
```

## Extending Interfaces : héritage

```typescript
interface Person {
  name: string;
  age: number;
}

// Employee hérite de Person
interface Employee extends Person {
  companyId: string;
  position: string;
}

const employee: Employee = {
  name: "Alice",
  age: 30,
  companyId: "EMP001",
  position: "Developer",
};

// Héritage multiple
interface Timestamped {
  createdAt: Date;
  updatedAt: Date;
}

interface User extends Person, Timestamped {
  email: string;
}
```

## Type vs Interface : Le choix

### Interface : meilleur pour

```typescript
// ✅ Définir des objets
interface User {
  name: string;
}

// ✅ Declaration Merging
interface User {
  email: string; // Fusionne avec la déclaration précédente
}

// ✅ Implémentation par des classes
class UserImpl implements User {
  name: string;
  email: string;
}
```

### Type : meilleur pour

```typescript
// ✅ Unions et intersections
type ID = string | number;
type Status = "active" | "inactive";

// ✅ Tuples
type Point = [number, number];

// ✅ Mapped types
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

// ❌ Pas de merging
type User = { name: string };
type User = { email: string }; // Erreur: duplicate
```

**Recommandation :**

- **Interface** pour les objets, contrats, classes
- **Type** pour unions, intersections, types utilitaires
