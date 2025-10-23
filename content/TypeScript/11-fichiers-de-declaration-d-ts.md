# 1️⃣1️⃣ Fichiers de déclaration (.d.ts)

Les **declaration files** (`.d.ts`) contiennent uniquement des types, pas d'implémentation.

## À quoi servent les .d.ts ?

**Problème :** Vous utilisez une bibliothèque JavaScript. Comment TypeScript connaît les types ?

**Solution :** Les fichiers `.d.ts` décrivent les types du code JavaScript.

## Générer automatiquement

```json
// tsconfig.json
{
  "compilerOptions": {
    "declaration": true,
    "declarationMap": true
  }
}
```

**Exemple :**

```typescript
// user.ts
export interface User {
  id: number
  name: string
}

export function createUser(name: string): User {
  return { id: Math.random(), name }
}
```

**Génère :**

```typescript
// user.d.ts
export interface User {
  id: number
  name: string
}

export declare function createUser(name: string): User
```

## Écrire des .d.ts manuellement

### Pour une lib JavaScript sans types

```typescript
// awesome-lib.d.ts
declare module "awesome-lib" {
  export function doSomething(value: string): number

  export interface Config {
    apiUrl: string
    timeout: number
  }
}
```

### Types globaux

```typescript
// global.d.ts
declare global {
  interface Window {
    myCustomProperty: string
  }

  var MY_GLOBAL_VAR: number
}

export {} // Fait de ce fichier un module
```

## Augmenter les types d'Express

```typescript
// types/express.d.ts
import { User } from "@models/User"

declare global {
  namespace Express {
    interface Request {
      user?: User
      token?: string
    }
  }
}
```

Maintenant :

```typescript
app.get("/profile", (req, res) => {
  console.log(req.user?.name) // ✅ TypeScript connaît user
})
```

## DefinitelyTyped et @types

Installation des types pour des libs JS :

```bash
npm install express
npm install --save-dev @types/express

npm install mongodb
npm install --save-dev @types/mongodb

npm install --save-dev @types/node
```

TypeScript cherche automatiquement dans `node_modules/@types/`.

## Publier votre lib avec types

```json
// package.json
{
  "name": "my-lib",
  "version": "1.0.0",
  "main": "dist/index.js",
  "types": "dist/index.d.ts", // ⭐ Pointer vers les types
  "files": ["dist"]
}
```
