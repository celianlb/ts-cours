---
title: "1️⃣6️⃣ Ressources"
description: "Liste de ressources, documentations officielles et outils pour aller plus loin dans ton apprentissage de TypeScript."
---

## Documentation officielle

- **TypeScript** : https://www.typescriptlang.org/docs/
- **TypeScript Playground** : https://www.typescriptlang.org/play
- **Mongoose + TypeScript** : https://mongoosejs.com/docs/typescript.html
- **Express** : https://expressjs.com/
- **DefinitelyTyped** : https://github.com/DefinitelyTyped/DefinitelyTyped

## Outils utiles pendant la piscine

- **Chercher des types** : https://www.npmjs.com → `@types/[lib]`
- **Validator.js** : https://github.com/validatorjs/validator.js
- **Bcrypt** : https://www.npmjs.com/package/bcrypt
- **JWT** : https://www.npmjs.com/package/jsonwebtoken
- **Dotenv** : https://www.npmjs.com/package/dotenv

## VSCode Extensions recommandées

- **ESLint** : Linter pour JavaScript/TypeScript
- **Prettier** : Formatage automatique
- **Error Lens** : Affiche les erreurs inline
- **Thunder Client** : Tester les APIs (comme Postman)
- **Docker** : Support Docker
- **MongoDB for VS Code** : Explorer MongoDB

## Commandes utiles

```bash
# Compiler TypeScript
npm run build

# Mode watch (recompile automatiquement)
tsc --watch

# Lancer en dev avec ts-node
npm run dev

# Vérifier les types sans compiler
tsc --noEmit

# Voir le JS généré pour un fichier
tsc --showOutput src/index.ts

# Initialiser un nouveau tsconfig
tsc --init
```

## Debugging TypeScript

1. **Utilisez source maps** : `"sourceMap": true` dans tsconfig
2. **VSCode debugger** : Ajoutez une config dans `.vscode/launch.json`
3. **Console.log avec types** : `console.log(typeof variable, variable)`
4. **Hover dans l'IDE** : Survolez une variable pour voir son type
5. **Go to Definition** : F12 sur un symbole pour voir sa définition
