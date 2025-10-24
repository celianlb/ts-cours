---
title: "02 - Les bases de React"
description: "JSX, composants, rendu et clés de l’architecture réactive"
---

## 2.1 Composants fonctionnels 🧩

Un composant est **une fonction** qui retourne du JSX.

```tsx
type ButtonProps = { label: string; onClick?: () => void }

export function Button({ label, onClick }: ButtonProps) {
  return <button onClick={onClick}>{label}</button>
}
```

💡 **Convention** : Noms en **PascalCase** pour les composants.

## 2.2 JSX en profondeur 🔍

- Expressions via `{}`.
- Children : contenu entre balises.
- Fragments pour **éviter des wrappers inutiles**.

```tsx
export function Card({ title, children }: { title: string; children: React.ReactNode }) {
  return (
    <>
      <h3>🗂 {title}</h3>
      <div className="card">{children}</div>
    </>
  )
}
```

⚠️ **Attention** : `class` devient `className`, `for` devient `htmlFor`.

## 2.3 Rendu conditionnel 🎭

```tsx
export function Status({ online }: { online: boolean }) {
  return online ? <span>🟢 En ligne</span> : <span>⚪️ Hors ligne</span>
}
```

## 2.4 Listes & clés 🔑

Les **clés** aident React à identifier les éléments.

```tsx
export function TodoList({ items }: { items: { id: string; title: string }[] }) {
  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>✅ {item.title}</li>
      ))}
    </ul>
  )
}
```

⚠️ **Jamais** d’index comme clé pour des listes **mutables**.

## 2.5 Styles 🎨

- **Inline** : objet JS `{ color: "red" }`
- **CSS Modules / Tailwind** : recommandés pour projets réels.

```tsx
export function Badge({ tone = "info" }: { tone?: "info" | "error" | "success" }) {
  const color = tone === "error" ? "tomato" : tone === "success" ? "seagreen" : "royalblue"
  return <span style={{ color, fontWeight: 700 }}>🏷 Badge</span>
}
```

## 2.6 Import/Export et composition 🧩

```tsx
import { Button } from "./Button"

export default function Toolbar() {
  return (
    <div>
      <Button label="💾 Sauvegarder" />
      <Button label="🗑 Supprimer" />
    </div>
  )
}
```

---

> ✅ **À retenir**
>
> - Composants = fonctions + JSX.
> - Clés stables pour les listes.
> - Composition > héritage en React.
