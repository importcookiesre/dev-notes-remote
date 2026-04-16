
## 1. What is the Singleton Pattern?

The **Singleton Pattern** is a **creational design pattern** that ensures:
1. A class has **only one instance**.
2. There is a **global point of access** to that instance.
3. Provide a consistent single access point.
👉 Think of it like: one **Database connection manager** or one **Configuration service** shared across your whole app.

- cannot be used when class requires non rigid parameters.


## 2. Example : 

### method 1:

**Implementation**
```ts
class Singleton {
  private static instance: Singleton | null = null;

  private constructor() {
    console.log("Singleton instance created");
  }

  static getInstance(): Singleton {
    if (!Singleton.instance) {
      Singleton.instance = new Singleton();
    }
    return Singleton.instance;
  }

  public log(message: string) {
    console.log(`[LOG]: ${message}`);
  }
}
```

**Usage**
```ts
const s1 = Singleton.getInstance();
s1.log("First message");

const s2 = Singleton.getInstance();
s2.log("Second message");

console.log(s1 === s2); // true ✅ same instance
```



### method 2 : ts modular syntax

- simply export the object instead of class.

implementation
```ts
// logger.ts
class Logger {
  log(message: string) {
    console.log(`[LOG]: ${message}`);
  }
}

export const logger = new Logger();
```

usage
```ts
// app.ts
import { logger } from "./logger";

logger.log("App started"); // ✅ same instance everywhere

```