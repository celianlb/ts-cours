---
title: "07 - Structuration d’une application React"
description: "Organisation des fichiers, conventions, communication et patterns réutilisables"
---

## 7.1 Organisation des dossiers 📁

```
src/
  components/     # Composants UI (purs, réutilisables)
  features/       # Domaines fonctionnels (ex: auth, cart)
  hooks/          # Hooks personnalisés
  pages/          # Entrées routing (si react-router)
  lib/            # Utilitaires, API clients
  styles/         # Styles globaux / thèmes
  assets/         # Images, icônes
```

## 7.2 Conventions 🔤

- Noms de fichiers **PascalCase** pour composants (`UserCard.tsx`).
- Exporte **par défaut** pour pages, **nommé** pour composants réutilisables.
- Préfère **fonctions pures** et **props explicites**.

## 7.3 Communication inter-composants 🔌

- Parent → enfant via **props**.
- Fratrie via **état levé** dans le parent.
- Profondeur via **contexte** ou **store**.

## 7.4 Hooks personnalisés 🪝

```tsx
function useLocalStorage<T>(key: string, initial: T) {
  const [value, setValue] = React.useState<T>(() => {
    const raw = localStorage.getItem(key)
    return raw ? (JSON.parse(raw) as T) : initial
  })
  React.useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value))
  }, [key, value])
  return [value, setValue] as const
}
```

## 7.5 Gestion d’erreurs & frontières 🧯

- Composants **Error Boundary** (avec `react-error-boundary`) pour capturer les erreurs runtime du rendu.
- États d’erreur **explicites** pour les requêtes.

```tsx
type Data = { id: string; name: string }

function DataView({ data }: { data: Data[] }) {
  if (!data.length) return <p>ℹ️ Aucune donnée</p>
  return (
    <ul>
      {data.map((d) => (
        <li key={d.id}>{d.name}</li>
      ))}
    </ul>
  )
}
```

## 7.6 Tests (aperçu) 🧪

- **Unitaires** : logique pure, hooks, utils.
- **Composants** : avec React Testing Library (rendu, interactions).
- **E2E** : Playwright/Cypress pour parcours critique.

![Folder structure](/img/folder-structure.png)

---

> ✅ **À retenir**
>
> - Structure claire par **features** + **réutilisables**.
> - Hooks personnalisés = logique partagée et testable.
