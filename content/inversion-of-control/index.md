+++
title = "Inversion of Control (IoC)"
date = "2024-03-11"
description = "The design principle known as Inversion of Control (IoC) refers to the reversal of the control flow."
[taxonomies]
tags = ["IoC", "inversion", "principle","oop","object","oriented","programming"]
+++

## INTRODUCTION

IoC is sometimes referred to as the **"Hollywood Principle"**

{% admonition(type="quote", title="Hollywood Principle") %}
Don't call us, we'll call you.
{% end %}

Inversion of Control shorthanded IoC is a design principle that refers to the reversal of the control flow in a software application. The goal of using the design principle is a achieve loose coupling as inverting the control over the flow of execution. In order to do this, it transfers the authority for the flow of execution to an external container or framework. 

IoC is like a real-life daily principle using. For example, imagine the scenario, a software engineer named Alice who works in a company, has a crucial task at hand while working. Despite his desire for a coffee break, he cannot afford the interruption because of the importance of the task. In this situation, he may prefer to apply the principle of IoC by delegating the coffee-making responsibility to his nearby colleagues. While their colleagues making coffee, he stays focused on his important task. This scenario is one of the daily examples of IoC.

## BACKGROUND

While learning the concept of the Inversion of Control, two fundamental terms should be known, called **"loose coupling"** and **"tight coupling"**. 

### Loose Coupling

Loose coupling means that components in the system are independent of each other. Loose coupling is achieved by components in a software application that are independent. Namely, changes in one component should not affect the other components. Moreover, every component should have minimal knowledge about each other.

### Tight Coupling

Tight coupling means components in software applications are dependent on each other and changes in one component affect the other components. Because of the tight coupling, reducing system flexibility makes maintenance more difficult. 


## IoC IMPLEMENTATION
  

The IoC principle makes it possible to invert control, enabling the delegation of control to another class. In this way, it provides a solution to the challenges associated with tight coupling.

It will be more feasible to move forward step by step so as that learn the Inversion of Control. Therefore, let's analyze an example illustrating tight coupling.  

In the first step, imagine that there are two classes, which are named **Car** and **Engine**. 

In the **Car** class, there is a private member, which is named **Engine**.  The constructor method of the **Car** class creates an instance of the **Engine** class, which is a concrete class and represents the engine of the car.

```Java
public class Car {
    private Engine engine;

    public Car() {
        this.engine = new Engine();
    }

    public void start() {
        engine.start();
        // Other car-related logic
    }
}
```

```Java
public class Engine {
    public void start() {
        System.out.println("Engine is started!");
    }
}
```
In the **Main** class, it is created an instance of the **Car** class and uses the method called **start** of this class.

```Java
public class Main {
    public static void main(String[] args) {
        Car myCar = new Car();
        myCar.start();
    }
}
```

In the previous example, it is evident that the **Car** class is dependent on the **Engine** class in order to fulfill functionality. The **Car** class cannot operate without it. Also, when a change is made to the **Engine** class, these changes affect the **Car** class. For this reason, using the Inversion of Control principle is advisable as a solution to address this issue and challenges. 

Let's refactor the previous examples to invert the control of dependency from one class to another with help using a factory design pattern.


The **Engine** class is the same as the previous.

```Java
public class Engine {
    public void start() {
        System.out.println("Engine is started!");
    }
}
```

The below now **Car** class has a private member, which is named **EngineFactory** that provides the creation of the **Engine** class instead of having a direct concrete **Engine** class. In short, the **EngineFactory** class is a concrete class to creates an instance of the **Engine** class. So, the **Car** class is not dependent on the **Engine** class.However, there is still a dependency between the **Car** class and the **EngineFactory**.



```Java
public class EngineFactory {
    public static Engine getEngine() {
        return new Engine();
    }
}
```

```Java
public class Car {
    private Engine engine;

    public Car() {
        engine = EngineFactory.getEngine();
    }

    public void start() {
        engine.start();
    }
}

```

```Java
public class Main {
    public static void main(String[] args) {
        Car myCar = new Car();
        myCar.start();
    }
}
```


The previous refactored code has not yet attained fully loosely coupled classes. To achieve fully loosely coupled, it is necessary to integrate the **Dependency Inversion Principle (DIP)**, and one type of implementation of this is **Dependency Injection (DI)**.

{% admonition(type="info", title="Dependency Injection (DI)") %}
In the given examples, the factory design pattern is preferred to reverse the control of dependency creation. A more suitable and common approach to implementing IoC is to use the Dependency Injection (DI) framework. 
{% end %}


In the second example, let's illustrate the Inversion of Control in a real scenario.

In this example, imagine that it is needed to develop a project named marketplace in any company. The marketplace should be run with several payment services such as PayPal, Stripe, and Bank. 

According to the scenario, the **Transaction** class refers to payment transactions as a model class in this design.

```Java
public class Transaction {
    private String transactionId;
    private BigDecimal amount;
    private String currency;
    private String recipientAccount;
    private String senderAccount;

    public Transaction(String transactionId, BigDecimal amount, String currency, String recipientAccount, String senderAccount) {
        this.transactionId = transactionId;
        this.amount = amount;
        this.currency = currency;
        this.recipientAccount = recipientAccount;
        this.senderAccount = senderAccount;
    }

    // getter, setter
}
```


There is an interface named **PaymentGateway** and three gateway classes to represent the payment processing logic, which are **PayPalPaymentGateway** class, **StripePaymentGateway** class, and **BankTransferGateway** class. The three classes inherit the interface named **PaymentGateway**.

```Java
public interface PaymentGateway {
    void processPayment(Transaction transaction);
}
```

```Java
public class PayPalPaymentGateway implements PaymentGateway {
    @Override
    public void processPayment(Transaction transaction) {
        System.out.println("Payment processing with PayPal: " + transaction.getCurrency() + transaction.getAmount());
        // Other PayPal payment processing
    }
}
```

```Java
public class StripePaymentGateway implements PaymentGateway  {
    @Override
    public void processPayment(Transaction transaction) {
        System.out.println("Payment processing with Stripe: " + transaction.getCurrency() + transaction.getAmount());
        // Other Stripe payment processing
    }
}
```

```Java
public class BankTransferGateway implements PaymentGateway {
    @Override
    public void processPayment(Transaction transaction) {
        System.out.println("Payment processing with Bank: " + transaction.getCurrency() + transaction.getAmount());
        // Other Bank payment processing
    }
}
```

**TransactionProcessor** class is responsible for processing interactions using different payment methods.It is considered a bridge between three classes (**PayPalPaymentGateway**, **StripePaymentGateway**, and **BankTransferGateway**) and the **Main** class.

```Java
public class TransactionProcessor {
    public void processTransaction(double amount, String paymentMethod) {
        if ("PayPal".equals(paymentMethod)) {
            PayPalPaymentGateway payPalGateway = new PayPalPaymentGateway();
            payPalGateway.processPayment(amount);
        } else if ("Stripe".equals(paymentMethod)) {
            StripePaymentGateway stripeGateway = new StripePaymentGateway();
            stripeGateway.processPayment(amount);
        } else if ("BankTransfer".equals(paymentMethod)) {
            BankTransferGateway bankTransferGateway = new BankTransferGateway();
            bankTransferGateway.processPayment(amount);
        } else {
            throw new IllegalArgumentException("Invalid payment method: " + paymentMethod);
        }
    }
}
```

The **Main** class represents the flow of how to use the class design. In the **Main** class, it is created an instance of the **TransactionProcessor** class and uses the method called process of this class.

```Java
public class Main {
    public static void main(String[] args) {
        TransactionProcessor processor = new TransactionProcessor();
        Transaction transaction = new Transaction("2398129", BigDecimal.TEN, "$", "12345", "12346");
        processor.process(transaction, "PayPal");
    }
}
```

As can be seen in the above example, when it is wanted to integrate different payment processors or add financial reporting services, it is noticed easily that the design becomes tightly coupled and difficult to extend and change. Because of this reason, it is wise to integrate the IoC and so decrease dependence between components. 

In this step, let's refactor the previous example by using a factory design pattern in order that achieve the goal of loosely coupled.

The **PaymentGateway** interface and its concrete implementations **PaypalPaymentGateway**, **StripePaymentGateway**, and **BankTransferGateway** are the same as the previous examples without IoC. 

```Java
public class Transaction {
    private String transactionId;
    private BigDecimal amount;
    private String currency;
    private String recipientAccount;
    private String senderAccount;

    public Transaction(String transactionId, BigDecimal amount, String currency, String recipientAccount, String senderAccount) {
        this.transactionId = transactionId;
        this.amount = amount;
        this.currency = currency;
        this.recipientAccount = recipientAccount;
        this.senderAccount = senderAccount;
    }

    // getter, setter
}
```

```Java
public interface PaymentGateway {
    void processPayment(BigDecimal amount);
}
```

```Java
public class PayPalPaymentGateway implements PaymentGateway {
    @Override
    public void processPayment(Transaction transaction) {
        System.out.println("Payment processing with PayPal: " + transaction.getCurrency() + transaction.getAmount());
        // Other PayPal payment processing
    }
}
```

```Java
public class StripePaymentGateway implements PaymentGateway  {
    @Override
    public void processPayment(Transaction transaction) {
        System.out.println("Payment processing with Stripe: " + transaction.getCurrency() + transaction.getAmount());
        // Other Stripe payment processing
    }
}
```

```Java
public class BankTransferGateway implements PaymentGateway {
    @Override
    public void processPayment(Transaction transaction) {
        System.out.println("Payment processing with Bank: " + transaction.getCurrency() + transaction.getAmount());
        // Other Bank payment processing
    }
}
```

The **PaymentGatewayFactory** class has a static factory method that returns a specific **PaymentGateway**. It is used to decide which payment services to use. It is a kind of implementation of a factory design pattern.

```Java
public class PaymentGatewayFactory {
    public static PaymentGateway createPaymentGateway(String paymentMethod) {
        switch (paymentMethod) {
            case "PayPal":
                return new PayPalPaymentGateway();
            case "Stripe":
                return new StripePaymentGateway();
            case "BankTransfer":
                return new BankTransferGateway();
            default:
                throw new IllegalArgumentException("Invalid payment method: " + paymentMethod);
        }
    }
}
```

The **TransactionProcessor** class uses IoC by accepting a **PaymentGatewayFactory** instance in its constructor and delegates the creation of specific payment gateways to the factory.

```Java
public class TransactionProcessor {
    public void processTransaction(Transaction transaction, String paymentMethod) {
        PaymentGateway paymentGateway = PaymentGatewayFactory.createPaymentGateway(paymentMethod);
        paymentGateway.processPayment(transaction);
    }
}
```


In this **Main** class, it is created an instance of **TransactionProcessor** and used to process a payment transaction and uses a method named processTransaction to process a payment transaction.

```Java

public class Main {
    public static void main(String[] args) {
        TransactionProcessor transactionProcessor = new TransactionProcessor();
        Transaction transaction = new Transaction("2398129", BigDecimal.TEN, "$", "12345", "12346");
        transactionProcessor.processTransaction(transaction, "PayPal");
    }
}
```

The previous examples developed step by step are examples of the implementation of IoC. However, this step is only just the beginning to have a fully loosely coupled design.  In order to achieve it completely, it is not necessary to use a factory design pattern.  it is essential to incorporate DIP (Dependency Inversion Principle) and DI (Dependency Injection) alongside IoC.

{% admonition(type="info", title="Note") %}
It is important to note that IoC is only a principle, not a specific pattern. Not giving implementation details, it provides high-level guidelines for the design of maintainable and flexible software systems. For this reason, there are options to implement IoC using different methods or approaches such as factory design patterns that used previous examples. 
{% end %}

The next article will be about Dependency Injection (DI) helping to achieve loosely coupled.


## REFERENCES

[1] <a href="https://www.tutorialsteacher.com/ioc/inversion-of-control" target="_blank">https://www.tutorialsteacher.com/ioc/inversion-of-control</a>

[2] <a href="https://martinfowler.com/bliki/InversionOfControl.html" target="_blank">https://martinfowler.com/bliki/InversionOfControl.html</a>

[3] <a href="https://www.cesarsotovalero.net/blog/inversion-of-control-and-dependency-injection-in-java.html" target="_blank">https://www.cesarsotovalero.net/blog/inversion-of-control-and-dependency-injection-in-java.html</a>

[4] <a href="https://www.yegor256.com/2017/05/10/inversion-of-control.html" target="_blank">https://www.yegor256.com/2017/05/10/inversion-of-control.html</a>