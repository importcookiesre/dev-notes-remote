
https://medium.com/@raafvargas/dependency-injection-in-go-35293ef7b6


**Dependency Injection** is a design pattern where an object receives other objects that it depends on (its "dependencies") from an external source, rather than creating them itself.

The main principle behind DI is **Inversion of Control (IoC)**. The control of providing dependencies is inverted—it's moved from the object itself to an external entity.

Think of it like building a car. 🚗

- **Without DI:** The car factory would have to build the engine, the transmission, and the wheels from scratch _inside_ the car's assembly line. The `Car` object would be responsible for creating its own `Engine` object. This makes the `Car` tightly coupled to a specific type of engine.
    
- **With DI:** The car factory receives a pre-built engine, a transmission, and wheels from other specialized factories. It then assembles them. The `Car` object is _given_ (or "injected with") an `Engine` object when it's created.


- The application should decide dependencies at the top level and not scattered across domains (90% of time).
	- Eg. db are injected at the system level and not across all repositories. 




## 1. Types of DI :

There are three common ways to inject a dependency into an object:

1. **Constructor Injection:** Dependencies are provided through the class constructor. This is the **most common and generally recommended** approach. It makes it very clear what an object needs to function, and it ensures the object is in a valid state as soon as it's created.

```ts
interface PaymentGateway {
  pay(amount: number): void;
}

class StripeGateway implements PaymentGateway {
  pay(amount: number) { console.log(`Paying ${amount} via Stripe`); }
}

class PaypalGateway implements PaymentGateway {
  pay(amount: number) { console.log(`Paying ${amount} via PayPal`); }
}

class PaymentService {
  constructor(private gateway: PaymentGateway) {}

  checkout(amount: number) {
    this.gateway.pay(amount);
  }
}

// Swap implementations without changing PaymentService
const stripePayment = new PaymentService(new StripeGateway());
stripePayment.checkout(100);

const paypalPayment = new PaymentService(new PaypalGateway());
paypalPayment.checkout(200);

```

2. **Setter (or Property) Injection:** Dependencies are passed through a public setter method or property _after_ the object has been created. This is useful for optional dependencies that are not critical to the object's initial state.

```ts
class NotificationService {
  private emailProvider!: EmailProvider;

  setProvider(provider: EmailProvider) {
    this.emailProvider = provider;
  }

  send(msg: string) {
    this.emailProvider.sendEmail(msg);
  }
}

// Usage
const service = new NotificationService();
service.setProvider(new GmailProvider());
service.send("Hello");
```


3. **Method (or Interface) Injection:** The dependency is passed directly to a method that needs to use it. This is useful when only one specific method needs the dependency, not the entire object.


