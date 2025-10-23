---
title: "1️2 - TypeScript avec Node.js et Express"
description: "Implémente TypeScript dans un projet Node.js et Express pour sécuriser ton backend avec un typage strict et clair."
---

## Setup initial

```bash
mkdir my-api && cd my-api
npm init -y

# TypeScript et outils
npm install --save-dev typescript @types/node ts-node

# Express et types
npm install express
npm install --save-dev @types/express

# Init tsconfig
npx tsc --init
```

## Configuration package.json

```json
{
  "name": "my-api",
  "scripts": {
    "dev": "ts-node src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "@types/express": "^4.17.17",
    "@types/node": "^20.0.0",
    "ts-node": "^10.9.1",
    "typescript": "^5.0.0"
  }
}
```

## Structure recommandée

```
my-api/
├── src/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   ├── middlewares/
│   ├── services/
│   ├── utils/
│   ├── types/
│   └── index.ts
├── dist/
├── tsconfig.json
└── package.json
```

## Point d'entrée

```typescript
// src/index.ts
import express, { Express, Request, Response, NextFunction } from "express"

const app: Express = express()
const PORT = process.env.PORT || 3000

app.use(express.json())

app.get("/health", (req: Request, res: Response) => {
  res.json({
    status: "ok",
    timestamp: new Date(),
  })
})

// Error handler
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
  console.error(err.stack)
  res.status(500).json({ error: err.message })
})

app.listen(PORT, () => {
  console.log(`🚀 Server on http://localhost:${PORT}`)
})
```

## DTOs : Data Transfer Objects

```typescript
// src/types/dtos/user.dto.ts

// Pour créer
export interface CreateUserDto {
  name: string
  email: string
  password: string
}

// Pour mettre à jour
export interface UpdateUserDto {
  name?: string
  email?: string
}

// Pour répondre (sans password)
export interface UserResponseDto {
  id: string
  name: string
  email: string
  createdAt: Date
}
```

## Controller typé

```typescript
// src/controllers/userController.ts
import { Request, Response, NextFunction } from "express"
import { CreateUserDto, UpdateUserDto } from "@/types/dtos/user.dto"

export class UserController {
  async getUsers(req: Request, res: Response, next: NextFunction): Promise<void> {
    try {
      const users = await this.userService.findAll()
      res.json({ success: true, data: users })
    } catch (error) {
      next(error)
    }
  }

  async getUserById(
    req: Request<{ id: string }>,
    res: Response,
    next: NextFunction,
  ): Promise<void> {
    try {
      const { id } = req.params
      const user = await this.userService.findById(id)

      if (!user) {
        res.status(404).json({ error: "User not found" })
        return
      }

      res.json({ success: true, data: user })
    } catch (error) {
      next(error)
    }
  }

  async createUser(
    req: Request<{}, {}, CreateUserDto>,
    res: Response,
    next: NextFunction,
  ): Promise<void> {
    try {
      const userData: CreateUserDto = req.body
      const user = await this.userService.create(userData)
      res.status(201).json({ success: true, data: user })
    } catch (error) {
      next(error)
    }
  }
}
```

**Type de Request :** `Request<Params, ResBody, ReqBody, Query>`

## Middleware d'authentification

```typescript
// src/middlewares/auth.ts
import { Request, Response, NextFunction } from "express"
import { User } from "@/types/dtos/user.dto"

// Augmenter Express Request
declare global {
  namespace Express {
    interface Request {
      user?: User
    }
  }
}

export const authMiddleware = async (
  req: Request,
  res: Response,
  next: NextFunction,
): Promise<void> => {
  try {
    const token = req.headers.authorization?.split(" ")[1]

    if (!token) {
      res.status(401).json({ error: "No token" })
      return
    }

    const user = await verifyToken(token)
    req.user = user

    next()
  } catch (error) {
    res.status(401).json({ error: "Invalid token" })
  }
}
```

## Routes typées

```typescript
// src/routes/userRoutes.ts
import { Router } from "express"
import { UserController } from "@/controllers/userController"
import { authMiddleware } from "@/middlewares/auth"

const router: Router = Router()
const controller = new UserController()

router.get("/", controller.getUsers.bind(controller))
router.get("/:id", controller.getUserById.bind(controller))
router.post("/", controller.createUser.bind(controller))

// Route protégée
router.get("/profile", authMiddleware, (req, res) => {
  res.json({ data: req.user }) // req.user est typé !
})

export default router
```
