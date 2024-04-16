+++
title = "Dependency Inversion Principle (DIP)"
date = "2024-04-16"
description = "The Dependency Inversion Principle named DIP is one of the SOLID principles created."
[taxonomies]
tags = ["dependency", "inversion", "principle","dip","IoC"]
+++

## INTRODUCTION

The Dependency Inversion Principle named DIP is one of the SOLID principles created by Robert Cecil Martin. The previous article belongs to title stand "Inversion of Control (IoC)", which is the first step so that achieving a loose couple design. It can be achieved more loose couple design by applying the Dependency Inversion Principle compared to the previous state.

The goal of this principle is to achieve loose coupling. With this principle, high-level modules, which are responsible for complex logic, remain unaffected by changes in low-level modules. In order to achieve this goal, it is important to utilize an abstraction that separates high-level and low-level modules. In short, the principle relies on interfaces or abstract classes as opposed to depending on concrete classes.

## DEPENDENCY INVERSION PRINCIPLE (DIP)

{% admonition(type="quote", title="Definition") %}
Robert C. Martin's definition of the Dependency Inversion Principle consists of two parts:
1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
2. Abstractions should not depend on details. Details should depend on abstractions.
{% end %}

According to this principle, if any system has several components, it is not preferred the approach of injecting one direct component into another. In this principle, it's advisable to create an abstraction layer between components so that have a low level of coupling.

Firstly, in an effort to understand the Dependency Inversion Principle (DIP), let's create a simple example without using the Dependency Inversion Principle. 

In this example, imagine that it is needed to develop a car model. Naturally, a car consists of a lot of components such as an engine, brake, steering wheel, and clutch. As these components can be replaced, each component of a car can be created from different brands. For example, any car can structured from a Ford engine and GM brake. In summary, a car can be structured in different combinations. However, in this scenario, all components depend on each other. If there is a need to replace any components, it affects the model due to this dependency. 

The **Car** class represents the model containing concrete classes such as **Engine**, **SteeringWheel**, and **Clutch**. Without these components, any car in this model would be not functional. In the constructor method of the **Car** class, it creates the instance of the **Engine**, **SteeringWheel**, and **Cluctch** classes.

```Java
public class Car {
    private Engine engine;
    private SteeringWheel steeringWheel;
    private Clutch clutch;

    public Car() {
        this.engine = new Engine();
        this.steeringWheel = new SteeringWheel();
        this.clutch = new Clutch();
    }

    public void start() {
        engine.start();
        // other car-related logic
    }
}
```

**Engine** class represents the engine of a car. It has methods that are related to fulfill the functionality of the engine. 

```Java
public class Engine {
    public void start() {
        System.out.println("Engine is started!");
    }

    public void stop(){
        System.out.println("Engine is stopped!");

    }
}
```

This **SteeringWhel** class represents the steering wheel of a car.

```Java
public class SteeringWheel {
    public void turnLeft() {
        System.out.println("Steering wheel is turned left!");
    }

    public void turnRight() {
        System.out.println("Steering wheel is turned right!");
    }
}
```

This class represents the clutch of a car.

```Java
public class Clutch {
    public void engage() {
        System.out.println("Clutch is engaged!");
    }

    public void disengage() {
        System.out.println("Clutch is disengaged!");
    }
}
```

As it can be seen easily in the provided example, the **Car** class is dependent on the **Engine**, **SteeringWheel**, and **Clutch** concrete classes for functionality. 

For example, if there is a need to change the engine, you might have to replace the corresponding adjustment to the clutch or add new features because the current clutch might not be compatible with the new engine. In the context of this scenario, does the model seem correct? Probably, it does not seem correct. In the case of this situation, It may be considered restructuring the model so that it is loosely coupled.

Let's refactor the previous examples, especially with help from the Dependency Inversion Principle.

Initially, It should be defined interfaces for each car component. With these interfaces, it will be able to create concrete implementations for special components.

```Java
public interface Engine {
    void start();
    void stop();
}
```

```Java
public interface Brake {
    void apply();
}
```

```Java
public interface Clutch {
    void engage();
    void disengage();
}
```

```Java
public interface SteeringWheel {
    void turnLeft();
    void turnRight();
}
```

Next, it should be created a concrete implementation for each component. In this context, each implementation can belong to a specific brand. 

```Java
public class FordEngine implements Engine {
    @Override
    public void start() {
        System.out.println("Ford engine is started!");
    }

    @Override
    public void stop() {
        System.out.println("Ford engine is stopped!");
    }
}
```

```Java
public class GMSteeringWheel implements SteeringWheel {
    @Override
    public void turnLeft() {
        System.out.println("GM steering wheel is turned left!");
    }

    @Override
    public void turnRight() {
        System.out.println("GM steering wheel is turned right!'");
    }
}
```

```Java
public class ToyotaBrake implements Brake {
    @Override
    public void apply() {
        System.out.println("Toyota brake is applied!");
    }
}
```

```Java
public class GMClutch implements Clutch {
    @Override
    public void engage() {
        System.out.println("GM clutch is engaged!");
    }

    @Override
    public void disengage() {
        System.out.println("GM clutch is disengaged!");
    }
}
```

Finally, it can be created a **Car** class that depends on the interfaces rather than concrete implementations. It is important to note that using interfaces achieves loose coupling. 

```Java
public class Car {
    private Engine engine;
    private Brake brake;
    private Clutch clutch;
    private SteeringWheel steeringWheel;

    public Car(Engine engine, Brake brake, Clutch clutch, SteeringWheel steeringWheel) {
        this.engine = engine;
        this.brake = brake;
        this.clutch = clutch;
        this.steeringWheel = steeringWheel;
    }

    public void startCar() {
        engine.start();
        // Other operations
    }

    public void stopCar() {
        engine.stop();
        // Other operations
    }

    public void turnLeft() {
        steeringWheel.turnLeft();
    }

    public void turnRight() {
        steeringWheel.turnRight();
    }

    public void applyBrake() {
        brake.apply();
    }

    public void engageClutch() {
        clutch.engage();
    }

    public void disengageClutch() {
        clutch.disengage();
    }
}
```

The below code represents how you might use the **Car** class as having a loose coupling. As reviewed carefully the **Main** class, it can be seen easily that each component belongs to a special brand. Namely, they are a kind of implementation of a special brand.

```Java
public class Main {
    public static void main(String[] args) {
        Engine fordEngine = new FordEngine();
        SteeringWheel gmSteeringWheel = new GMSteeringWheel();
        Brake toyotaBrake = new ToyotaBrake();
        Clutch fordClutch = new GMClutch();

        Car car = new Car(fordEngine, toyotaBrake, fordClutch, gmSteeringWheel);

        car.startCar();
        car.turnLeft();
        car.applyBrake();
        car.engageClutch();

        car.disengageClutch();
        car.stopCar();
    }
}
```

Thus, it has received a loose coupling. There is a high-level module named **Car** class and low-level module named **FordEngine**, **GMSteeringWheel**, **ToyotaBrake** and **GMClutch** classes are dependent on an abstraction named **Brake**, **Engine**, **Brake**, and **Clutch** interfaces.

Now, let's consider another example to illustrate the Dependency Inversion Principle in a real scenario.

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

Each payment gateway class (**PayPalPaymentGateway**, **StripePaymentGateway**, **BankTransferGateway**) directly represents its payment processing logic. They inherit an interface, which is **PaymentGateway**. 

This class represents a PayPal payment gateway.

```Java
public class PayPalPaymentGateway {
    public boolean processPayment(Transaction transaction) {
        System.out.println("Payment processing with PayPal: " + transaction.getCurrency() + transaction.getAmount());
        return true;
    }
}
```

The **StripePaymentGateway** class represents a Stripe payment gateway inheritance of the **PaymentGateway**.

```Java
public class StripePaymentGateway  {
    public boolean processPayment(Transaction transaction) {
        System.out.println("Payment processing with Stripe: " + transaction.getCurrency() + transaction.getAmount());
        return true;
    }
}
```

This class represents a bank transfer payment gateway.

```Java
public class BankTransferGateway {
    public boolean processPayment(Transaction transaction) {
        System.out.println("Payment processing with Bank: " + transaction.getCurrency() + transaction.getAmount());
        return true;
    }
}
```

This class is responsible for processing transactions using different payment methods. It is considered a bridge between three classes (**PayPalPaymentGateway**, **StripePaymentGateway**, and **BankTransferGateway**) and the **Main** class. When reviewing the code carefully, it can be seen easily that there are three private members. These private members, which are **PayPalPaymentGateway**, **StripePaymentGateway**, and **BankTransferGateway**, are created in the constructor method. Thus, **TransactionProcessor** is dependent on these members. 

```Java
public class TransactionProcessor {
    private final PayPalPaymentGateway payPalPaymentGateway;
    private final StripePaymentGateway stripePaymentGateway;
    private final BankTransferGateway bankTransferGateway;

    public TransactionProcessor() {
        this.payPalPaymentGateway = new PayPalPaymentGateway();
        this.stripePaymentGateway = new StripePaymentGateway();
        this.bankTransferGateway = new BankTransferGateway();
    }

    public boolean process(Transaction transaction, String paymentMethod) {
        if (paymentMethod.equals("PayPal"))
            return payPalPaymentGateway.processPayment(transaction);
        else if (paymentMethod.equals("Stripe"))
            return stripePaymentGateway.processPayment(transaction);
        else if (paymentMethod.equals("BankTransfer"))
            return bankTransferGateway.processPayment(transaction);

        return false;
    }
}
```

The below code represents how you might use the **TransactionProcessor**. In the **Main** class, it is created an instance of the **TransactionProcessor** class and uses the method called process of this class.

```Java
public class Main {
    public static void main(String[] args) {
        TransactionProcessor processor = new TransactionProcessor();
        Transaction transaction = new Transaction("2398129", BigDecimal.TEN, "$", "12345", "12346");
        processor.process(transaction, "PayPal");
    }
}
```

As in the first example without the Dependency Inversion Principle, it can be seen easily that **TransactionProcessor** is dependent on the **PayPalPaymentGateway**, **StripePaymentGateway**, and **BankTransferGateway** concrete classes. In this design, if it is needed to add a new payment gateway class, the **TransactionProcessor** class will have to be changed, too. So, restructuring the design is necessary.

Let's refactor the given example by using the Dependency Inversion Principle in order that achieve the goal of loosely coupled.

Initially, it should be defined as an interface. Due to this interface, the design will be able to be loosely coupled.

The **Transaction** class, **PaymentGateway** interface, and its concrete implementations **PaypalPaymentGateway**, **StripePaymentGateway**, and **BankTransferGateway** are the same as the previous examples without IoC.

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
    boolean processPayment(Transaction transaction);
}
```

```Java
public class PayPalPaymentGateway implements PaymentGateway {
    @Override
    public boolean processPayment(Transaction transaction) {
        System.out.println("Payment processing with PayPal: " + transaction.getCurrency() + transaction.getAmount());
        return true;
    }
}
```

```Java
public class StripePaymentGateway implements PaymentGateway{
    @Override
    public boolean processPayment(Transaction transaction) {
        System.out.println("Payment processing with Stripe: " + transaction.getCurrency() + transaction.getAmount());
        return true;
    }
}
```

```Java
public class BankTransferGateway implements PaymentGateway {
    @Override
    public boolean processPayment(Transaction transaction) {
        System.out.println("Payment processing with Bank: " + transaction.getCurrency() + transaction.getAmount());
        return true;
    }
}
```

With the above code, it is created separate classes (**PayPalPaymentGateway**, **StripePaymentGateway**, **BankTransferGateway**) that implement the **PaymentGateway** interface. Each of these classes provides its specific implementation of the processPayment method. 

Finally, **TransactionProcessor** class is created, which is responsible for processing transactions using different payment methods and depends on the interfaces rather than concrete implementations. 

```Java
public class TransactionProcessor {
    private final PaymentGateway paymentGateway;

    public TransactionProcessor(PaymentGateway paymentGateway) {
        this.paymentGateway = paymentGateway;
    }

    public boolean process(Transaction transaction) {
        return paymentGateway.processPayment(transaction);
    }
}
```

In the Main class, it is demonstrated how the system can easily switch between different payment gateways without modifying the **TransactionProcessor** class. 

```Java
public class Main {
    public static void main(String[] args) {
        PaymentGateway paymentGateway = new BankTransferGateway();
        TransactionProcessor processor = new TransactionProcessor(paymentGateway);
        Transaction transaction = new Transaction("2398129", BigDecimal.TEN, "$", "12345", "12346");
        processor.process(transaction);
    }
}
```

## CONCLUSION

As the above examples, the Dependency Inversion Principle provides the development of interchangeable components. In line with this principle, a design is provided adaptable, extensible, and flexible. Abstractions have an important role in order that revert the inversion of control mechanisms. 

## REFERENCES

[1] <a href="https://stackify.com/dependency-inversion-principle/" target="_blank">https://stackify.com/dependency-inversion-principle/</a>

[2] <a href="https://www.tutorialsteacher.com/ioc/dependency-inversion-principle" target="_blank">https://www.tutorialsteacher.com/ioc/dependency-inversion-principle</a>