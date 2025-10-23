
## Les 3 règles d'or

1. **`"strict": true`** toujours

2. **Jamais de `any`** (sauf cas exceptionnel justifié)

3. **Validation** de toutes les données externes

## 1. Éviter `any`

```typescript
// ❌ MAUVAIS
function process(data: any): any {
  return data.value;
}

// ✅ BON
interface Data {
  value: string;
}

function process(data: Data): string {
  return data.value;
}
```

## 2. Utiliser `unknown` pour l'inconnu

```typescript
function parseJSON<T>(
  json: string,
  validator: (data: unknown) => data is T
): T {
  const parsed: unknown = JSON.parse(json);

  if (!validator(parsed)) {
    throw new Error("Invalid data");
  }

  return parsed;
}
```

## 3. Type Guards pour validation

```typescript
function isUser(data: unknown): data is User {
  return (
    typeof data === "object" &&
    data !== null &&
    "name" in data &&
    "email" in data
  );
}
```

## 4. Classes d'erreur typées

```typescript
class AppError extends Error {
  constructor(message: string, public statusCode: number) {
    super(message);
    this.name = this.constructor.name;
  }
}

class NotFoundError extends AppError {
  constructor(resource: string) {
    super(`${resource} not found`, 404);
  }
}

// Middleware
function errorHandler(
  err: Error,
  req: Request,
  res: Response,
  next: NextFunction
) {
  if (err instanceof AppError) {
    res.status(err.statusCode).json({ error: err.message });
  } else {
    res.status(500).json({ error: "Internal error" });
  }
}
```

## 5. Readonly pour l'immutabilité

```typescript
// ❌ MAUVAIS : mutations
interface User {
  name: string;
}

function updateName(user: User, name: string) {
  user.name = name; // Mutation !
}

// ✅ BON : immutabilité
function updateName(user: Readonly<User>, name: string): User {
  return { ...user, name };
}
```

## 6. Optional Chaining et Nullish Coalescing

```typescript
// ❌ VERBEUX
function getCity(user: User | null): string {
  if (user !== null) {
    if (user.address !== undefined) {
      if (user.address.city !== undefined) {
        return user.address.city;
      }
    }
  }
  return "Unknown";
}

// ✅ ÉLÉGANT
function getCity(user: User | null): string {
  return user?.address?.city ?? "Unknown";
}
```

## 7. Const Assertions

```typescript
// Sans const assertion
const colors = ["red", "green", "blue"]; // string[]

// Avec const assertion
const colors = ["red", "green", "blue"] as const;
// readonly ["red", "green", "blue"]

// Créer des types
type Color = (typeof colors)[number]; // "red" | "green" | "blue"
```

## 8. Builder Pattern type-safe

```typescript
class QueryBuilder<T> {
  private filters: Partial<T> = {};
  private sortField?: keyof T;
  private limitValue?: number;

  where(field: keyof T, value: T[keyof T]): this {
    this.filters[field] = value;
    return this;
  }

  sort(field: keyof T): this {
    this.sortField = field;
    return this;
  }

  limit(value: number): this {
    this.limitValue = value;
    return this;
  }

  build() {
    return {
      filters: this.filters,
      sort: this.sortField,
      limit: this.limitValue,
    };
  }
}

// Utilisation
interface User {
  name: string;
  age: number;
}

const query = new QueryBuilder<User>()
  .where("name", "Alice")
  .sort("age")
  .limit(10)
  .build();

// ❌ Erreur : 'invalid' n'existe pas
const invalid = new QueryBuilder<User>().where("invalid", "value");
```

## 9. API Client générique

```typescript
class ApiClient {
  constructor(private baseUrl: string) {}

  private async request<T>(
    endpoint: string,
    options: RequestInit = {}
  ): Promise<T> {
    const response = await fetch(`${this.baseUrl}${endpoint}`, {
      ...options,
      headers: {
        "Content-Type": "application/json",
        ...options.headers,
      },
    });

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }

    return response.json();
  }

  async get<T>(endpoint: string): Promise<T> {
    return this.request<T>(endpoint, { method: "GET" });
  }

  async post<T, B>(endpoint: string, body: B): Promise<T> {
    return this.request<T>(endpoint, {
      method: "POST",
      body: JSON.stringify(body),
    });
  }

  async put<T, B>(endpoint: string, body: B): Promise<T> {
    return this.request<T>(endpoint, {
      method: "PUT",
      body: JSON.stringify(body),
    });
  }

  async delete<T>(endpoint: string): Promise<T> {
    return this.request<T>(endpoint, { method: "DELETE" });
  }
}

// Utilisation type-safe
const api = new ApiClient("https://api.example.com");

interface User {
  id: string;
  name: string;
}

const user = await api.get<User>("/users/123");
console.log(user.name); // ✅ TypeScript connaît le type
```

## 10. Factory Pattern

```typescript
interface Product {
  id: string;
  name: string;
  price: number;
}

class ProductFactory {
  private static idCounter = 0;

  static create(name: string, price: number): Product {
    return {
      id: `PROD-${++this.idCounter}`,
      name,
      price,
    };
  }

  static createMultiple(items: Array<Omit<Product, "id">>): Product[] {
    return items.map((item) => this.create(item.name, item.price));
  }
}

const products = ProductFactory.createMultiple([
  { name: "Laptop", price: 999 },
  { name: "Mouse", price: 25 },
]);
```
