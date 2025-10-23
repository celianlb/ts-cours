---
title: "🔟 Configuration tsconfig.json"
description: "Découvre le rôle du fichier tsconfig.json et comment le configurer pour compiler, optimiser et structurer ton projet TypeScript."
---

Le fichier `tsconfig.json` est le **cœur** de votre projet TypeScript. Une mauvaise configuration = faux sentiment de sécurité.

## Pourquoi tsconfig.json est crucial

`tsconfig.json` contrôle :

- La sensibilité du type checking (strictness)
- Quels fichiers compiler (include/exclude)
- Comment compiler (target, module)
- Où mettre les fichiers compilés (outDir)

## Structure de base

```json
{
  "compilerOptions": {
    /* Options de compilation */
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

## Target et Module

```json
{
  "compilerOptions": {
    "target": "ES2022", // Version JS générée
    "module": "commonjs", // Système de modules
    "lib": ["ES2022"], // APIs disponibles
    "moduleResolution": "node" // Résolution Node.js
  }
}
```

**Valeurs courantes :**

- **target** : `ES5`, `ES2015`, `ES2020`, `ES2022`, `ESNext`
- **module** : `commonjs` (Node.js), `esnext` (bundlers)

## Strictness : LA partie cruciale ⭐

```json
{
  "compilerOptions": {
    "strict": true // ⭐ TOUJOURS activer !
  }
}
```

`"strict": true` active automatiquement :

1. **noImplicitAny** : Interdit les `any` implicites
2. **strictNullChecks** : null/undefined doivent être gérés explicitement
3. **strictFunctionTypes** : Vérifications strictes des fonctions
4. **strictBindCallApply** : Vérifie bind/call/apply
5. **strictPropertyInitialization** : Propriétés de classe doivent être initialisées
6. **noImplicitThis** : Interdit `this` sans type
7. **alwaysStrict** : Émet "use strict"

### Comprendre chaque option

**noImplicitAny :**

```typescript
// ❌ Sans noImplicitAny
function greet(name) {
  // name est any implicite 😱
  return "Hello " + name
}

// ✅ Avec noImplicitAny
function greet(name: string) {
  // Type explicite obligatoire
  return "Hello " + name
}
```

**strictNullChecks :**

```typescript
// ❌ Sans strictNullChecks
let name: string = null // Pas d'erreur 😱

// ✅ Avec strictNullChecks
let name: string = null // ❌ Erreur
let nullableName: string | null = null // ✅ Explicite
```

**⚠️ RÈGLE D'OR : Toujours `"strict": true` dans un nouveau projet !**

## Type Checking additionnel

```json
{
  "compilerOptions": {
    "strict": true,

    // Code mort
    "allowUnreachableCode": false,

    // Switch incomplet
    "noFallthroughCasesInSwitch": true,

    // Variables/paramètres inutilisés
    "noUnusedLocals": true,
    "noUnusedParameters": true,

    // Fonction qui ne retourne pas toujours
    "noImplicitReturns": true,

    // Override sans mot-clé
    "noImplicitOverride": true
  }
}
```

## Paths : Imports élégants

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@models/*": ["src/models/*"],
      "@utils/*": ["src/utils/*"],
      "@controllers/*": ["src/controllers/*"],
      "@/*": ["src/*"]
    }
  }
}
```

**Avant :**

```typescript
import { User } from "../../../models/User"
```

**Après :**

```typescript
import { User } from "@models/User"
```

## Output et Source Maps

```json
{
  "compilerOptions": {
    "outDir": "./dist", // Dossier de sortie
    "rootDir": "./src", // Dossier source
    "sourceMap": true, // Pour debugging
    "declaration": true, // Génère .d.ts
    "declarationMap": true, // Source maps .d.ts
    "removeComments": true // Allège le code
  }
}
```

## Interop et Compatibility

```json
{
  "compilerOptions": {
    "esModuleInterop": true, // Interop ESM/CJS
    "allowSyntheticDefaultImports": true, // Import default CJS
    "forceConsistentCasingInFileNames": true, // Casse cohérente
    "resolveJsonModule": true, // Import JSON
    "skipLibCheck": true // Performance
  }
}
```

## Configuration complète pour Node.js + Express

```json
{
  "compilerOptions": {
    /* LANGUAGE */
    "target": "ES2022",
    "lib": ["ES2022"],

    /* MODULES */
    "module": "commonjs",
    "rootDir": "./src",
    "moduleResolution": "node",
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@models/*": ["src/models/*"],
      "@controllers/*": ["src/controllers/*"],
      "@utils/*": ["src/utils/*"]
    },
    "resolveJsonModule": true,

    /* OUTPUT */
    "outDir": "./dist",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "removeComments": true,

    /* INTEROP */
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "allowSyntheticDefaultImports": true,

    /* TYPE CHECKING ⭐ */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "**/*.test.ts"]
}
```

## Include et Exclude

```json
{
  "include": [
    "src/**/*" // Tous les fichiers dans src
  ],
  "exclude": [
    "node_modules", // Toujours exclure
    "dist", // Dossier de build
    "**/*.test.ts", // Tests
    "**/*.spec.ts"
  ]
}
```
