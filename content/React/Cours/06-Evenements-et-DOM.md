---
title: "06 - Événements et gestion du DOM"
description: "Système d’événements, refs, contrôles d’entrée et accès DOM"
---

## 6.1 Événements synthétiques 🎛️

React normalise les événements pour un comportement **cohérent** entre navigateurs.

```tsx
export function Clicker() {
  function handleClick(e: React.MouseEvent<HTMLButtonElement>) {
    console.log("Position:", e.clientX, e.clientY)
  }
  return <button onClick={handleClick}>👆 Clique-moi</button>
}
```

## 6.2 Transmettre des paramètres 🔧

```tsx
export function Remover({ onRemove, id }: { onRemove: (id: string) => void; id: string }) {
  return <button onClick={() => onRemove(id)}>🗑 Supprimer</button>
}
```

## 6.3 Refs & accès DOM 🔎

`useRef` permet de conserver des **références mutables** (non réactives).

```tsx
import { useRef } from "react"

export function FocusInput() {
  const ref = useRef<HTMLInputElement>(null)
  return (
    <div>
      <input ref={ref} placeholder="Tape quelque chose…" />
      <button onClick={() => ref.current?.focus()}>🎯 Focus</button>
    </div>
  )
}
```

## 6.4 Composants contrôlés vs non contrôlés 🧭

- **Contrôlés** : la **valeur** vient du **state** (source de vérité React).
- **Non contrôlés** : la valeur est **dans le DOM** (`defaultValue`, `ref`).

```tsx
export function UncontrolledEmail() {
  const ref = React.useRef<HTMLInputElement>(null)
  return (
    <form
      onSubmit={(e) => {
        e.preventDefault()
        alert(ref.current?.value)
      }}
    >
      <input ref={ref} type="email" defaultValue="test@example.com" />
      <button>Envoyer</button>
    </form>
  )
}
```

## 6.5 Empêcher les comportements par défaut 🚫

```tsx
export function LinkLike({ onNavigate }: { onNavigate: () => void }) {
  return (
    <a
      href="#"
      onClick={(e) => {
        e.preventDefault()
        onNavigate()
      }}
    >
      🧭 Naviguer
    </a>
  )
}
```

![Flux React — Événement → Handler → État → Rendu](/static/img/flux.png)

---

> ✅ **À retenir**
>
> - Les événements sont **syntétiques** et typés avec React.
> - `useRef` pour lire/écrire dans le DOM sans re-render.
