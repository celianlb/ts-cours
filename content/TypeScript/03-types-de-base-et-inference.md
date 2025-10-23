# 3️⃣ Types de base et inférence

## Comprendre le système de types

Un **type** est comme une étiquette qui décrit ce qu'une variable peut contenir. C'est un contrat : "cette variable contiendra toujours un nombre" ou "cette fonction retournera toujours une chaîne".

En JavaScript, les variables n'ont pas de types fixes. En TypeScript, vous déclarez le type d'une variable, et TypeScript vérifie que vous respectez ce contrat.

## Les types primitifs

TypeScript reprend les types primitifs de JavaScript et ajoute des vérifications strictes.

```typescript
// Nombres (integers et floats sont tous "number")
let age: number = 25
let price: number = 19.99
let temperature: number = -10

age = "vingt-cinq" // ❌ Erreur : incompatible

// Chaînes de caractères
let name: string = "Alice"
let greeting: string = `Bonjour ${name}` // Template strings

name = 42 // ❌ Erreur : incompatible

// Booléens
let isActive: boolean = true
let hasPermission: boolean = false

isActive = "yes" // ❌ Erreur : "yes" n'est pas un booléen

// null et undefined : représentent l'absence de valeur
let nothing: null = null
let notDefined: undefined = undefined

// BigInt : pour les très grands nombres
let bigNumber: bigint = 9007199254740991n

// Symbol : identifiants uniques
let uniqueId: symbol = Symbol("id")
```

## Arrays : collections typées

En JavaScript, un tableau peut contenir n'importe quoi. En TypeScript, vous déclarez le type des éléments.

```typescript
// Tableau de nombres uniquement
let numbers: number[] = [1, 2, 3, 4, 5]
numbers.push(6) // ✅ OK
numbers.push("7") // ❌ Erreur

// Syntaxe alternative
let names: Array<string> = ["Alice", "Bob", "Charlie"]

// Avantage : autocomplétion
numbers.forEach((num) => {
  console.log(num.toFixed(2)) // ✅ TypeScript sait que num est number
})
```

## Tuples : tableaux à structure fixe

Un **tuple** est un tableau avec un nombre fixe d'éléments et des types précis pour chaque position.

```typescript
// Tuple : [string, number]
let person: [string, number] = ["Alice", 25]

// L'ordre compte !
person = ["Bob", 30] // ✅ OK
person = [25, "Alice"] // ❌ Erreur : ordre incorrect
person = ["Charlie"] // ❌ Erreur : manque le number

// Use case : retourner plusieurs valeurs
function getUserInfo(): [string, number, boolean] {
  return ["Alice", 25, true] // [nom, âge, actif]
}

const [name, age, active] = getUserInfo()
// TypeScript connaît le type de chaque variable
```

## Type `any` : la porte de sortie (à éviter !)

`any` désactive complètement le système de types.

```typescript
let anything: any = "hello"
anything = 42 // ✅ Pas d'erreur
anything = true // ✅ Pas d'erreur
anything.nonExistentMethod() // ⚠️ Pas d'erreur de compilation !
```

**Le problème :** `any` est contagieux et annule tous les bénéfices de TypeScript.

```typescript
function processData(data: any) {
  return data.value // Retourne any
}

const result = processData({ value: 42 })
result.toUpperCase() // ⚠️ Pas d'erreur, mais crash à l'exécution !
```

**Règle d'or :** Évitez `any` autant que possible !

## Type `unknown` : l'alternative sûre

`unknown` est comme `any`, mais vous DEVEZ vérifier le type avant utilisation.

```typescript
let userInput: unknown = getUserInput()

// ❌ Impossible d'utiliser directement
console.log(userInput.toUpperCase()) // Erreur

// ✅ Vérification obligatoire
if (typeof userInput === "string") {
  console.log(userInput.toUpperCase()) // OK
}
```

## L'inférence de type : TypeScript devine

L'**inférence** signifie que TypeScript peut deviner le type d'une variable automatiquement.

```typescript
// Pas besoin d'écrire le type explicitement
let age = 25 // TypeScript infère : number
let name = "Alice" // TypeScript infère : string
let isActive = true // TypeScript infère : boolean

// Essayer de changer le type génère une erreur
age = "vingt-cinq" // ❌ Erreur

// Inférence avec fonctions
function add(a: number, b: number) {
  return a + b // TypeScript infère le retour : number
}

const result = add(5, 3) // result est automatiquement number

// Inférence avec objets
const user = {
  name: "Alice",
  age: 25,
  email: "alice@email.com",
}
// Type inféré : { name: string; age: number; email: string }
```

**Bonne pratique :** Laissez TypeScript inférer quand c'est évident. N'écrivez le type explicitement que pour :

- Documenter l'intention
- Les paramètres de fonction
- Les APIs publiques

## Type assertions : "Je sais mieux que toi"

Parfois, vous en savez plus sur un type que TypeScript. Les **assertions** vous permettent de forcer un type.

```typescript
let someValue: unknown = "hello world"

// Syntaxe "as"
let length: number = (someValue as string).length

// Syntaxe angle-bracket (évitez en React/JSX)
let length2: number = (<string>someValue).length
```

**⚠️ ATTENTION :** Les assertions ne changent PAS le type à runtime !

```typescript
let value: unknown = 42
let str = value as string // Vous affirmez que c'est une string
console.log(str.toUpperCase()) // ❌ CRASH ! 42 n'a pas toUpperCase
```

**Règle :** N'utilisez les assertions que si vous êtes ABSOLUMENT sûr.
