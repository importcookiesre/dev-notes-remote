
- Repository design pattern is a structural pattern that abstracts data access, providing a centralized way to manage data operations.
- It tell us to use a repository system to access the database. Our service layer does not know from where our data is coming.
- It is generally used with : 
	- Factory pattern
	- dependency injection
	- singleton pattern
	- 

Real-world Analogy
	Imagine a **library**:
	- You don’t deal with the warehouse where books are stored.
	- You ask the librarian (Repository) for a book.
	- The librarian fetches it for you, no matter whether it’s stored in shelves, basement, or digital archives.



## Example :

1. User repository Pattern: 
	1. create user interface.
	2. create User repository interface.
	3. create implementations of user repository 
	4. business logic (service layer)
	5. Use it in controllers.

### 1. User interface : 


```ts
// domain/entities/User.ts
export interface User {
  id: string;
  name: string;
  email: string;
  createdAt: Date;
}
```


### 2. respository interface : 



```ts
// domain/repositories/IUserRepository.ts
import { User } from "../entities/User";

export interface IUserRepository {
  findById(id: string): Promise<User | null>;
  findByEmail(email: string): Promise<User | null>;
  save(user: User): Promise<void>;
  delete(id: string): Promise<void>;
}
```



### 3. Implementation  



```ts
// infra/repositories/PostgresUserRepository.ts
import { IUserRepository } from "../../domain/repositories/IUserRepository";
import { User } from "../../domain/entities/User";

export class PostgresUserRepository implements IUserRepository {
  async findById(id: string): Promise<User | null> {
    // Pretend DB call
    console.log(`Querying Postgres for user id: ${id}`);
    return { id, name: "Alice", email: "alice@mail.com", createdAt: new Date() };
  }

  async findByEmail(email: string): Promise<User | null> {
    console.log(`Querying Postgres for email: ${email}`);
    return { id: "123", name: "Alice", email, createdAt: new Date() };
  }

  async save(user: User): Promise<void> {
    console.log(`Saving user ${user.email} into Postgres`);
  }

  async delete(id: string): Promise<void> {
    console.log(`Deleting user ${id} from Postgres`);
  }
}
```



### 4. Business logic (service layer)


```ts
// application/services/UserService.ts
import { IUserRepository } from "../../domain/repositories/IUserRepository";
import { User } from "../../domain/entities/User";

export class UserService {
  constructor(private userRepository: IUserRepository) {}

  async registerUser(name: string, email: string): Promise<User> {
    const existing = await this.userRepository.findByEmail(email);
    if (existing) throw new Error("User already exists");

    const newUser: User = {
      id: Math.random().toString(36).substring(2),
      name,
      email,
      createdAt: new Date(),
    };

    await this.userRepository.save(newUser);
    return newUser;
  }
}
```


### 5. Usage : 


```ts
import { UserService } from "./application/services/UserService";
import { PostgresUserRepository } from "./infra/repositories/PostgresUserRepository";

const repo = new PostgresUserRepository();
const service = new UserService(repo);

(async () => {
  const user = await service.registerUser("Bob", "bob@mail.com");
  console.log("Registered:", user);
})();
```