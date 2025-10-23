
## Comparaison directe avec un exemple réel

Prenons un scénario concret : gérer des utilisateurs dans une application.

### Version JavaScript : le chaos

```javascript
// user.js
function createUser(name, email, age) {
  return {
    name: name,
    email: email,
    age: age,
    createdAt: new Date(),
  };
}

function isAdult(user) {
  return user.age >= 18;
}

// Utilisation qui semble fonctionner
const user = createUser("Alice", "alice@email.com", 25);
console.log(isAdult(user)); // true ✅

// Mais ces erreurs passent inaperçues...
const user2 = createUser("Bob", "bob@email", "25");
// age est une string, la comparaison sera bizarre
console.log(isAdult(user2)); // "25" >= 18 → true (coercion) 😱

const user3 = createUser("Charlie");
// email et age sont undefined
console.log(user3.email.toLowerCase()); // ❌ CRASH !

const user4 = createUser("David", "david@email.com", 25, "extra", "ignored");
// Paramètres supplémentaires ignorés silencieusement 🤷
```

**Les problèmes :**

- Erreurs découvertes en production quand un utilisateur les déclenche
- Coercion automatique cache des bugs subtils
- Impossible de savoir ce qu'attend `createUser` sans lire son code
- Aucune garantie sur la structure de l'objet retourné

### Version TypeScript : Sécurité sans compromis

```typescript
// user.ts
interface User {
  name: string;
  email: string;
  age: number;
  createdAt: Date;
}

function createUser(name: string, email: string, age: number): User {
  return {
    name,
    email,
    age,
    createdAt: new Date(),
  };
}

function isAdult(user: User): boolean {
  return user.age >= 18;
}

// Utilisation correcte
const user: User = createUser("Alice", "alice@email.com", 25);
console.log(isAdult(user)); // ✅ true

// Toutes ces erreurs sont IMPOSSIBLES
const user2 = createUser("Bob", "bob@email", "25");
// ❌ Erreur de compilation : "25" n'est pas un number

const user3 = createUser("Charlie");
// ❌ Erreur : 2 arguments manquants

const user4 = createUser("David", "david@email.com", 25, "extra");
// ❌ Erreur : trop d'arguments

// L'IDE vous guide automatiquement
console.log(user.email.toLowerCase()); // ✅ Autocomplétion
console.log(user.unknownProp); // ❌ Erreur : propriété n'existe pas
```

**Les avantages :**

- Zero runtime error pour les erreurs de types
- Code auto-documenté via les types
- Autocomplétion et IntelliSense parfaits
- Refactoring sûr et facile

## Tableau comparatif

| Aspect                     | JavaScript                | TypeScript                       |
| -------------------------- | ------------------------- | -------------------------------- |
| **Types**                  | Dynamiques (runtime)      | Statiques (compile-time)         |
| **Erreurs**                | À l'exécution             | À la compilation                 |
| **IDE Support**            | Limité                    | Excellent (IntelliSense complet) |
| **Documentation**          | Externe, souvent obsolète | Intégrée via types               |
| **Refactoring**            | Manuel et risqué          | Automatisé et sûr                |
| **Courbe d'apprentissage** | Rapide pour débuter       | Modérée (concepts additionnels)  |
| **Performance**            | Native                    | Identique (compilé en JS)        |
| **Fichiers**               | .js                       | .ts → transpilé en .js           |

## Quand utiliser TypeScript ?

**TypeScript est recommandé pour :**

- Applications moyennes à grandes (>1000 lignes)
- Projets en équipe
- Code destiné à durer dans le temps
- APIs publiques (bibliothèques, frameworks)
- Applications critiques (finance, santé, e-commerce)

**JavaScript reste suffisant pour :**

- Prototypes rapides
- Scripts simples (<100 lignes)
- Projets personnels très petits
