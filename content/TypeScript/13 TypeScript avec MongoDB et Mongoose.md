
## Installation

```bash
npm install mongoose
npm install --save-dev @types/mongoose
```

## Connexion

```typescript
// src/config/database.ts
import mongoose from "mongoose";

export const connectDatabase = async (): Promise<void> => {
  try {
    const uri = process.env.MONGO_URI || "mongodb://localhost:27017/mydb";
    await mongoose.connect(uri);
    console.log("✅ MongoDB connected");
  } catch (error) {
    console.error("❌ MongoDB error:", error);
    process.exit(1);
  }
};

// Events
mongoose.connection.on("disconnected", () => {
  console.log("MongoDB disconnected");
});

// Graceful shutdown
process.on("SIGINT", async () => {
  await mongoose.connection.close();
  process.exit(0);
});
```

## Modèle Mongoose complet

```typescript
// src/models/User.ts
import mongoose, { Document, Schema, Model } from "mongoose";

// 1. Interface pour le document
export interface IUser {
  name: string;
  email: string;
  password: string;
  age?: number;
  role: "user" | "admin";
  createdAt: Date;
  updatedAt: Date;
}

// 2. Méthodes d'instance
export interface IUserMethods {
  getFullInfo(): string;
  isAdmin(): boolean;
}

// 3. Type combiné
export type UserDocument = IUser & IUserMethods & Document;

// 4. Méthodes statiques
export interface IUserModel extends Model<UserDocument> {
  findByEmail(email: string): Promise<UserDocument | null>;
  findAdmins(): Promise<UserDocument[]>;
}

// 5. Schéma avec validation
const userSchema = new Schema<UserDocument, IUserModel, IUserMethods>(
  {
    name: {
      type: String,
      required: [true, "Name required"],
      trim: true,
      minlength: [2, "Name too short"],
    },
    email: {
      type: String,
      required: [true, "Email required"],
      unique: true,
      lowercase: true,
      validate: {
        validator: (v: string) => /^\S+@\S+\.\S+$/.test(v),
        message: "Invalid email",
      },
    },
    password: {
      type: String,
      required: [true, "Password required"],
      minlength: [6, "Password too short"],
      select: false,
    },
    age: {
      type: Number,
      min: [0, "Age invalid"],
    },
    role: {
      type: String,
      enum: ["user", "admin"],
      default: "user",
    },
  },
  {
    timestamps: true,
    toJSON: {
      transform: (doc, ret) => {
        delete ret.password;
        return ret;
      },
    },
  }
);

// 6. Index
userSchema.index({ email: 1 });

// 7. Méthodes d'instance
userSchema.methods.getFullInfo = function (): string {
  return `${this.name} (${this.email})`;
};

userSchema.methods.isAdmin = function (): boolean {
  return this.role === "admin";
};

// 8. Méthodes statiques
userSchema.statics.findByEmail = function (email: string) {
  return this.findOne({ email }).select("+password");
};

userSchema.statics.findAdmins = function () {
  return this.find({ role: "admin" });
};

// 9. Hooks
userSchema.pre("save", async function (next) {
  if (this.isModified("password")) {
    const bcrypt = require("bcrypt");
    this.password = await bcrypt.hash(this.password, 10);
  }
  next();
});

// 10. Export
export const User = mongoose.model<UserDocument, IUserModel>(
  "User",
  userSchema
);
```

## Service Layer

```typescript
// src/services/userService.ts
import { User, UserDocument, IUser } from "@/models/User";
import { Types } from "mongoose";

export class UserService {
  async create(data: Partial<IUser>): Promise<UserDocument> {
    const user = new User(data);
    return await user.save();
  }

  async findById(id: string): Promise<UserDocument | null> {
    if (!Types.ObjectId.isValid(id)) {
      throw new Error("Invalid ID");
    }
    return await User.findById(id);
  }

  async findAll(
    page: number = 1,
    limit: number = 10
  ): Promise<{ users: UserDocument[]; total: number }> {
    const skip = (page - 1) * limit;

    const [users, total] = await Promise.all([
      User.find().skip(skip).limit(limit).sort({ createdAt: -1 }),
      User.countDocuments(),
    ]);

    return { users, total };
  }

  async update(
    id: string,
    updates: Partial<IUser>
  ): Promise<UserDocument | null> {
    return await User.findByIdAndUpdate(
      id,
      { $set: updates },
      { new: true, runValidators: true }
    );
  }

  async delete(id: string): Promise<boolean> {
    const result = await User.findByIdAndDelete(id);
    return result !== null;
  }
}
```

## Relations entre modèles

```typescript
// src/models/Tournament.ts
import mongoose, { Document, Schema, Types } from "mongoose";

export interface ITournament {
  name: string;
  participants: Types.ObjectId[]; // Références User
  winner?: Types.ObjectId;
  status: "upcoming" | "ongoing" | "finished";
}

export type TournamentDocument = ITournament & Document;

const tournamentSchema = new Schema<TournamentDocument>(
  {
    name: { type: String, required: true },
    participants: [
      {
        type: Schema.Types.ObjectId,
        ref: "User", // ⭐ Référence
      },
    ],
    winner: {
      type: Schema.Types.ObjectId,
      ref: "User",
    },
    status: {
      type: String,
      enum: ["upcoming", "ongoing", "finished"],
      default: "upcoming",
    },
  },
  { timestamps: true }
);

export const Tournament = mongoose.model<TournamentDocument>(
  "Tournament",
  tournamentSchema
);
```

## Populate : charger les relations

```typescript
// src/services/tournamentService.ts
import { Tournament } from "@/models/Tournament";
import { UserDocument } from "@/models/User";

export class TournamentService {
  async getWithParticipants(id: string) {
    return await Tournament.findById(id)
      .populate<{ participants: UserDocument[] }>("participants", "name email")
      .populate<{ winner: UserDocument }>("winner", "name")
      .exec();
  }
}
```
