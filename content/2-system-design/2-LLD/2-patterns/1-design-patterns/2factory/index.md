
- goal : hide the complexity of object creation

## 1. What is the Factory Pattern?

The **Factory Pattern** is a **creational design pattern**.  
Instead of creating objects directly with `new`, you use a **factory method or class** to encapsulate object creation.

- Often combined with **Strategy** (Factory decides _which Strategy_ to return).


## 2. Example : Notification system 


**Step 1:** Strategy Interface

```ts
interface NotificationStrategy {
  send(to: string, message: string): void;
}
```

**Step 2:** Strategy Implementation.

```ts
class EmailNotification implements NotificationStrategy {
  send(to: string, message: string): void {
    console.log(`📧 Email sent to ${to}: ${message}`);
  }
}

class SMSNotification implements NotificationStrategy {
  send(to: string, message: string): void {
    console.log(`📱 SMS sent to ${to}: ${message}`);
  }
}

class PushNotification implements NotificationStrategy {
  send(to: string, message: string): void {
    console.log(`🔔 Push notification to ${to}: ${message}`);
  }
}
```

**Step 3**: Create a Factory

- here Notification Factory Class contains only one creation method, it can contain more factory methods.
```ts
class NotificationFactory {
  static create(type: "email" | "sms" | "push"): NotificationStrategy {
    switch (type) {
      case "email":
        return new EmailNotification();
      case "sms":
        return new SMSNotification();
      case "push":
        return new PushNotification();
      default:
        throw new Error(`Unknown notification type: ${type}`);
    }
  }
}

```

Step 4: Usage client :
- client does not have any idea about how services are created.

```ts
const emailNotifier = NotificationFactory.create("email");
emailNotifier.send("user@example.com", "Welcome!");

const smsNotifier = NotificationFactory.create("sms");
smsNotifier.send("+1234567890", "Your OTP is 1234");

```

## 3. pros and cons

- pros: 
	- Guarantees abstractions.
	- Code is flexible and adaptable.
	- Very useful for frameworks and libraries.
- cons : 
	- complex code
	- Takes time to set the base.
	- Existing code cannot be migrated, always implemented from stratch.
	- Extra abstraction (a simple `new` might be enough sometimes).
	- If Factory grows too big → can become a “God class” (bad smell).

## 4. real world use cases : 

- everywhere where you can use strategy pattern.

- **Payment gateways** (choose PayPal, Stripe, Razorpay).
- **Database connections** (create MySQL, Postgres, Mongo clients).
- **Document parsers** (JSON parser, XML parser, CSV parser).
- **UI elements** (Button factory → creates MaterialButton, BootstrapButton).
- **Cloud storage providers** (AWS S3, Google Cloud Storage, Azure Blob).