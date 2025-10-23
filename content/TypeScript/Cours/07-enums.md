---
title: "07 - Enums"
description: "Apprends à utiliser les énumérations pour gérer des ensembles de constantes lisibles et maintenables dans ton code TypeScript."
---

Les **Enums** définissent un ensemble de constantes nommées.

## Pourquoi les Enums ?

Sans enum, vous utilisez des strings "magiques" partout :

```typescript
// ❌ Sans enum
let status = "pending"
status = "pendding" // Typo, pas d'erreur ! 😱

// ✅ Avec enum
enum OrderStatus {
  Pending = "pending",
  Shipped = "shipped",
  Delivered = "delivered",
}

let status = OrderStatus.Pending
status = OrderStatus.Pendding // ❌ Erreur !
```

## Numeric Enums

```typescript
enum Direction {
  Up, // 0
  Down, // 1
  Left, // 2
  Right, // 3
}

// Avec valeurs personnalisées
enum HttpStatus {
  OK = 200,
  Created = 201,
  BadRequest = 400,
  NotFound = 404,
}
```

## String Enums

```typescript
enum LogLevel {
  Error = "ERROR",
  Warning = "WARNING",
  Info = "INFO",
  Debug = "DEBUG",
}

function log(message: string, level: LogLevel): void {
  console.log(`[${level}] ${message}`)
}

log("App started", LogLevel.Info)
```

**Avantage :** Plus lisible dans les logs.

## Const Enums : optimisation

```typescript
const enum DirectionConst {
  Up,
  Down,
}

const dir = DirectionConst.Up
// Transpilé en: const dir = 0; (inlined)
```

## Reverse Mapping (numeric enums)

```typescript
enum Status {
  Pending = 0,
  Active = 1,
}

console.log(Status.Active) // 1
console.log(Status[1]) // "Active"
```

## Enums vs Union Types

```typescript
// Enum
enum Color {
  Red = "RED",
  Green = "GREEN",
}

// Union Type équivalent
type ColorUnion = "RED" | "GREEN"
```

**Quand utiliser quoi ?**

- **Enum** : besoin d'itération, namespace, API publique
- **Union Type** : types simples, performance, flexibilité
