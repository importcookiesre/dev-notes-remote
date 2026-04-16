- Schema is the structure of a table / collection.
- By default, Mongoose adds an `_id` property to the schemas.


Schemas not only define the structure of your document and casting of properties, they also define document
- instance methods 
- static model methods
- coumpund indexes
- middlewares (document lifecycle hooks)


#### Instance methods

- Instance methods are methods that are defined on a **schema** and can be called on a **document instance**. 
- They allow you to define custom functionality that operates on specific document instances.
```ts
import mongoose, { Schema, Document } from "mongoose";

// Define an interface for TypeScript
interface IUser extends Document {
  name: string;
  email: string;
  isAdmin: boolean;
  greet(): string; // Instance method definition
}

// Define the schema
const UserSchema = new Schema<IUser>({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  isAdmin: { type: Boolean, default: false }
});

// Adding an instance method
UserSchema.methods.greet = function (): string {
  return `Hello, my name is ${this.name}`;
};

// Create the model
const UserModel = mongoose.model<IUser>("User", UserSchema);
```


#### Static Methods

| Feature    | Instance Method (`methods`)           | Static Method (`statics`)                               |
| ---------- | ------------------------------------- | ------------------------------------------------------- |
| Defined On | `userSchema.methods`                  | `userSchema.statics`                                    |
| Called On  | An **individual document** (instance) | The **model itself**                                    |
| Use Case   | Work with a **specific document**     | Perform queries or actions affecting multiple documents |
| Example    | `user.isPasswordCorrect("password")`  | `UserModel.findByEmail("test@example.com")`             |

```ts
// ✅ Add a static method
userSchema.statics.findByEmail = async function (email: string): Promise<IUser | null> {
  return this.findOne({ email });
};
```

```ts
(async () => {
  const user = await UserModel.findByEmail("test@example.com");
  if (user) {
    console.log("User found:", user);
  } else {
    console.log("User not found");
  }
})();
```

| **Use Case**             | **Solution**                        |
| ------------------------ | ----------------------------------- |
| Find a user by email     | `UserModel.findByEmail(email)`      |
| Count total users        | `UserModel.countDocuments()`        |
| Get all admins           | `UserModel.find({ role: "admin" })` |
| Custom aggregate queries | `UserModel.getUserStats()`          |
