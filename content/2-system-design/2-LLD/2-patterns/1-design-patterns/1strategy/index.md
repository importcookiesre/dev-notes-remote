- generally used with factory pattern.
## problem statement : 

- There is a base class (vehicle).
- multiple sub class are inheriting the base class (sports car, good vehicle etc).
- There is a method or set of methods which the sub class has it own implementation (drive()), and multiple subclasses can have the same implementation so there will be duplication of code.

![[Pasted image 20250923113457.png]]

solution : 

- Define a drive strategy.
- Implement the strategy.
- Pass the drive strategy obj to the class that uses the method. ()


![[Pasted image 20250923114620.png]]



## 1. What is the Strategy Pattern?

The **Strategy Pattern** lets you define a family of algorithms (strategies), put each of them in a separate class, and make them interchangeable at runtime.

👉 Instead of hardcoding an algorithm in a class, you can **inject the behavior** dynamically.

**In short:**
- Encapsulate different algorithms behind a common interface.
- Allow clients to switch strategies without changing their code.


## 2. Example : 

Imagine we’re building a **payment system for an e-commerce app**.  
We want to support **multiple payment methods**: `CreditCard`, `PayPal`, `Crypto`.

Without Strategy, we might have:

```ts
class PaymentProcessor {
  processPayment(method: string, amount: number) {
    if (method === "credit") {
      console.log(`Paid ${amount} using Credit Card`);
    } else if (method === "paypal") {
      console.log(`Paid ${amount} using PayPal`);
    } else if (method === "crypto") {
      console.log(`Paid ${amount} using Crypto`);
    }
  }
}
```

❌ Problems:

- Violates **Open/Closed Principle (OCP)** → adding a new method = modify class.
- Not modular → logic mixed into one big class.
- Hard to test individual payment behaviors.


### Refactoring with strategy pattern : 


**step 1:**  ==Define Strategy Interface==

```ts
interface PaymentStrategy{
	pay(amount: number) : void;
}
```

**step 2:**  ==Implement Strategies==


```ts
class CreditCardPayment implements PaymentStrategy {
  pay(amount: number): void {
    console.log(`Paid ${amount} using Credit Card`);
  }
}

class PayPalPayment implements PaymentStrategy {
  pay(amount: number): void {
    console.log(`Paid ${amount} using PayPal`);
  }
}

class CryptoPayment implements PaymentStrategy {
  pay(amount: number): void {
    console.log(`Paid ${amount} using Cryptocurrency`);
  }
}
```


**step 3:**  ==Context Class (uses a strategy)==

```ts
class PaymentProcessor {
  private strategy: PaymentStrategy;

  constructor(strategy: PaymentStrategy) {
    this.strategy = strategy;
  }

  setStrategy(strategy: PaymentStrategy) {
    this.strategy = strategy;
  }

  process(amount: number) {
    this.strategy.pay(amount);
  }
}
```

**step 4:**  ==Usage==

```ts
const processor = new PaymentProcessor(new CreditCardPayment());
processor.process(100); // Paid 100 using Credit Card

processor.setStrategy(new PayPalPayment());
processor.process(200); // Paid 200 using PayPal
```


## Go implementation : 

Step 1: Define the Strategy Interface
```go
package payment

import "context"

type PaymentStrategy interface {
	Pay(ctx context.Context, amount float64) error
}
```

step 2: concrete strategy 
```go
package payment

import (
	"context"
	"fmt"
)

type CreditCard struct {
	Name   string
	Number string
	CVV    string
}

func (c *CreditCard) Pay(ctx context.Context, amount float64) error {
	fmt.Printf("Processing credit card payment of %.2f for %s\n", amount, c.Name)
	return nil
}


// UPI 
type UPI struct {
	ID string
}

func (u *UPI) Pay(ctx context.Context, amount float64) error {
	fmt.Printf("Processing UPI payment of %.2f for %s\n", amount, u.ID)
	return nil
}

// Paypal
type PayPal struct {
	Email string
}

func (p *PayPal) Pay(ctx context.Context, amount float64) error {
	fmt.Printf("Processing PayPal payment of %.2f for %s\n", amount, p.Email)
	return nil
}
```



step3: context
```go
type PaymentProcessor struct {
	strategy PaymentStrategy
}

func NewPaymentProcessor(strategy PaymentStrategy) *PaymentProcessor {
	return &PaymentProcessor{strategy: strategy}
}

func (p *PaymentProcessor) SetStrategy(strategy PaymentStrategy) {
	p.strategy = strategy
}

func (p *PaymentProcessor) Process(ctx context.Context, amount float64) error {
	return p.strategy.Pay(ctx, amount)
}
```


step 4 : usage 
```go
package main

import (
	"context"
	"log"
	"yourapp/payment"
)

func main() {
	ctx := context.Background()

	cc := &payment.CreditCard{
		Name:   "Suraj",
		Number: "1234-5678-9876-5432",
		CVV:    "123",
	}

	processor := payment.NewPaymentProcessor(cc)

	if err := processor.Process(ctx, 500); err != nil {
		log.Fatal(err)
	}

	// Switch strategy at runtime
	upi := &payment.UPI{ID: "suraj@upi"}
	processor.SetStrategy(upi)

	if err := processor.Process(ctx, 300); err != nil {
		log.Fatal(err)
	}
}
```

## use cases

- Payment processing (CreditCard, PayPal, Crypto, etc.)
- Authentication methods (OAuth, JWT, Basic Auth, SSO)
- Compression algorithms (ZIP, RAR, GZIP)
- Sorting algorithms (QuickSort, MergeSort, HeapSort)
- Search ranking or recommendation algorithms
- Routing in maps (Shortest path, Fastest route, Scenic route)
- Discount calculation in e-commerce (Percentage, Flat, BOGO)
- Logging strategies (File, Console, Remote server)
- Serialization formats (JSON, XML, YAML)
- Caching strategies (In-memory, Redis, Disk-based)
- Retry/backoff strategies in networking
- Image processing filters (Grayscale, Sepia, Blur)
- Notification channels (Email, SMS, Push, Slack)