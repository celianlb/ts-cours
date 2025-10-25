---
title: "01 - Introduction à React"
description: "Découverte de React, sa philosophie, et ses concepts clés"
---

React est une **bibliothèque** JavaScript pour construire des **interfaces utilisateur** à partir de **composants**. Elle privilégie une approche **déclarative** (décrire le “quoi” plutôt que le “comment”) et **composable** (assembler de petits composants réutilisables).

![Cycle de rendu React — Trigger → Render → Commit](/static/img/render-cycle.png)

## 1.1 Pourquoi React ? 🧠

- **Déclaratif** : le rendu dépend de l’état → moins d’effets de bord.
- **Composants** : UI découpée en briques simples et testables.
- **Écosystème** : riche (outils, bibliothèques, communauté).
- **Interopérable** : fonctionne avec TypeScript, bundlers, frameworks.

## 1.2 Virtual DOM & Reconcilation 🧬

- React maintient une **représentation en mémoire** (Virtual DOM).
- Au changement d’état, React **calcule la différence (diff)** entre l’ancienne et la nouvelle UI et applique **efficacement** les mutations dans le DOM réel (**reconciliation**).
- ⚠️ **Important** : le Virtual DOM n’est pas magique—les optimisations (memoization, clés stables) restent cruciales.

![React VDOM](/static/img/react-vdom.png)

## 1.3 JSX : du sucre syntaxique 🍯

JSX est une **extension de syntaxe** qui permet d’écrire du HTML dans du JavaScript/TypeScript. Il **compile** vers `React.createElement` (ou `jsx()` avec l’automatique runtime).

```tsx
// Exemple simple
export function Hello({ name }: { name: string }) {
  return <h1>👋 Bonjour {name} !</h1>
}
```

💡 **Astuce** : Les expressions JS/TS s’insèrent avec `{ ... }`. Les attributs suivent le **camelCase** (`className`, `onClick`).

## 1.4 Pré-requis TypeScript ✅

- Typage des props et de l’état pour **sécuriser** les composants.
- Interfaces/Types pour **documenter** les contrats.
- Utilisation de **types utilitaires** (Partial, Pick, Record…) et des **Generics**.

```tsx
type User = { id: string; username: string; admin?: boolean }

export function UserBadge({ user }: { user: User }) {
  return (
    <span>
      {user.admin ? "⭐️" : "👤"} {user.username}
    </span>
  )
}
```

## 1.5 Création de projet 🛠️

- **Vite** (recommandé) : `npm create vite@latest mon-app -- --template react-ts`
- Structure minimaliste, démarrage rapide, HMR performant.

```tsx
// src/App.tsx
export default function App() {
  return (
    <main>
      <h1>🚀 React + TS</h1>
      <p>Bienvenue dans votre première app.</p>
    </main>
  )
}
```

## 1.6 Terminologie essentielle 📘

- **Composant** : fonction qui retourne du JSX.
- **Props** : paramètres d’entrée d’un composant.
- **State** : données locales réactives d’un composant.
- **Hook** : fonction React commençant par `use` qui connecte le rendu à des fonctionnalités (state, effets…).
- **Effect (useEffect)** : effet secondaire synchronisé avec le cycle de vie.

---

> ✅ **À retenir**
>
> - React est déclaratif et composable.
> - JSX est une syntaxe, pas une obligation, mais **standard** en React.
> - TypeScript renforce la fiabilité et la maintenabilité.
