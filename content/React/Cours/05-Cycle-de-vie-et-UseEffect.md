---
title: "05 - Cycle de vie et effets (useEffect)"
description: "Montage, mise à jour, nettoyage ; requêtes, timers et synchronisation"
---

## 5.1 Quand utiliser useEffect ? 🧐

- **Effets côté client** : requêtes réseau, timers, abonnements.
- **Synchronisation** : mettre à jour le document title, lire/écrire dans localStorage.
- **Nettoyage** : détacher listeners, annuler timers.

```tsx
import { useEffect, useState } from "react"

export function Clock() {
  const [now, setNow] = useState<Date>(new Date())

  useEffect(() => {
    const id = setInterval(() => setNow(new Date()), 1000)
    return () => clearInterval(id) // 🧹 Nettoyage
  }, [])

  return <time>🕒 {now.toLocaleTimeString()}</time>
}
```

## 5.2 Dépendances 🔁

- `[]` : exécute **au montage** uniquement.
- `[x]` : exécute au montage **et** quand `x` change.
- Omettre les dépendances = exécuter **à chaque rendu** (rarement souhaité).

⚠️ **Attention** : Toujours **déclarer** les dépendances utilisées dans l’effet.

## 5.3 Requêtes réseau 📡

```tsx
type Post = { id: number; title: string }

export function Posts() {
  const [posts, setPosts] = useState<Post[]>([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    let alive = true
    ;(async () => {
      try {
        const res = await fetch("https://jsonplaceholder.typicode.com/posts?_limit=5")
        const data: Post[] = await res.json()
        if (alive) setPosts(data)
      } finally {
        if (alive) setLoading(false)
      }
    })()
    return () => {
      alive = false
    }
  }, [])

  if (loading) return <p>⏳ Chargement…</p>
  return (
    <ul>
      {posts.map((p) => (
        <li key={p.id}>📝 {p.title}</li>
      ))}
    </ul>
  )
}
```

## 5.4 Effets et performance ⚡

- Limiter la **portée** des effets (préférer **calculer** dans le rendu quand possible).
- Utiliser **`useMemo` / `useCallback`** pour **stabiliser** des valeurs/fonctions passées en props.

```tsx
const value = React.useMemo(() => computeExpensive(data), [data])
const handleClick = React.useCallback(() => doSomething(value), [value])
```

![Lifecycle](/static/img/lifecycle.png)

---

> ✅ **À retenir**
>
> - `useEffect` synchronise la **vue** avec des **effets secondaires**.
> - Déclarer les **dépendances** correctement et nettoyer les effets.
