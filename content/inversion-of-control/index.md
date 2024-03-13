+++
title = "Inversion of Control (IoC)"
date = "2024-03-11"
description = "The design principle known as Inversion of Control (IoC) refers to the reversal of the control flow."
[taxonomies]
tags = ["IoC", "inversion", "principle","oop","object","oriented","programming"]
+++

## INTRODUCTION

IoC is sometimes referred to as the **"Hollywood Principle"**

{% admonition(type="quote", title="") %}
Don't call us, we'll call you.
{% end %}

The design principle known as Inversion of Control (IoC) refers to the reversal of the control flow in a software application. In a traditional application, the flow of control is determined by the application code, where the main program is tasked with calling and coordinating various functions. In contrast, IoC is used to invert the control over the flow of execution to achieve loose coupling. Essentially, IoC transfers the authority for the flow of execution to an external container or framework.

IoC can be likened to a real-life principle. Consider the scenario where you, as a software engineer in a company, have a crucial task at hand. Despite your desire for a coffee break, the importance of the task means you cannot afford the interruption. In this instance, you may opt to apply the principle of IoC by delegating the coffee-making responsibility to your nearby colleagues. This way, you can stay focused on your work while your friends take care of the coffee preparation. This situation exemplifies the concept of inversion of control. Similar to not personally preparing coffee and instead entrusting your friends with the task, it allows you to concentrate on your primary responsibilities without being directly involved in every detail.

## BACKGROUND

"Loose coupling" and "tight coupling" are fundamental terms in software design and architecture, used to characterize the extent of dependency between various components.

### Loose Coupling

In a loosely coupled system, components are relatively independent and have minimal knowledge of each other. Changes in one component are less likely to affect others. In the Object-Oriented Programming (OOP) approach, it is advisable to design classes with loose coupling. Loose coupling implies that modifications in one class should not necessitate changes in other classes.

### Tight Coupling

In a tightly coupled system, components are closely interconnected, and changes in one component may have a significant impact on others. A high level of coupling between components can reduce system flexibility and make maintenance more challenging.


## IoC IMPLEMENTATION
  

Fundamentally, the key concept in IoC is the inversion of the control flow. Rather than the application code directly calling and managing the execution flow of its dependencies, IoC inverses this control.  In essence, IoC simplifies the creation of objects for dependent classes.

Initially, let's analyze an example illustrating tight coupling. Imagine you have two classes, namely **"Car"** and **"Engine."**

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

```Java
public class Main {
    public static void main(String[] args) {
        Car myCar = new Car();
        myCar.start();
    }
}
```

In the provided example, the **"Car"** class is reliant on the **"Engine"** class for engine functionality. The **"Car"** cannot operate without the presence of the **"Engine"** class.

It is evident that the **"Car"** class explicitly depends on the **"Engine"** class. This is due to the fact that the **"Car"** class cannot fulfill its functions independently and requires the presence and functionality of the **"Engine"** class.

In the Object-Oriented Programming (OOP) approach, classes are designed to interact with each other to fulfill their respective functionalities such as the provided examples. In these scenarios, classes exhibit dependency on each other, resulting in a high level of coupling.

In the software development process utilizing Object-Oriented Programming (OOP), a significant challenge arises with a high level of coupling between classes. The Inversion of Control (IoC) principle is introduced as a solution to address this issue.

The IoC principle suggests inverting control, enabling the delegation of control to another class. This delegation of control is made possible through IoC, providing a solution to the challenges associated with tight coupling in software development with Object-Oriented Programming (OOP).

{% admonition(type="info", title="") %}
In the given examples, the Inversion of Control (IoC) principle incorporates patterns such as the factory design pattern to reverse the control of dependency creation. A prevalent approach to implementing Inversion of Control (IoC) is through the utilization of a Dependency Injection (DI) framework.
{% end %}

Let's refactor the preceding examples, essentially inverting the control of dependency creation from one class to another, as demonstrated below.

```Java
public class Engine {
    public void start() {
        System.out.println("Engine is started!");
    }
}
```

```Java
public class FactoryEngine {
    public static Engine getEngine() {
        return new Engine();
    }
}
```

```Java
public class Car {
    private Engine engine;

    public Car() {
        engine = FactoryEngine.getEngine();
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

In the given example, it's evident that the **"Car"** class explicitly utilizes the **"FactoryEngine"** class to obtain an object of the **"Engine"** class. This restructuring is aimed at inverting the creation of dependent objects from the **"Car"** class to the **"FactoryEngine"** class.

{% admonition(type="warning", title="warning") %}
The refactored code, focusing solely on **IoC**, has not yet attained fully loosely coupled classes. To achieve this, it is imperative to integrate **Dependency Inversion Principle (DIP)** and **Dependency Injection (DI)**.
{% end %}


Let's consider another simple example to illustrate the Inversion of Control without IoC and then show how IoC can be applied.

You assume that you delve into the project named marketplace in any company. You realized that the marketplace needed to handle various payment methods, from traditional credit cards to emerging digital currencies. Additionally, the finance department that belongs to the company requires regular updates and notifications about financial transactions.

In your initial design, the marketplace class is responsible for managing orders and payments.

```Java
public class Marketplace {
    private PaymentProcessor paymentProcessor;
    private FinancialReporter financialReporter;

    public Marketplace() {
        paymentProcessor = new PaymentProcessor();
        financialReporter = new FinancialReporter();
    }

    public void processOrder(Order order) {
        // Business logic for processing the order
        // ...

        // Call the payment processor
        paymentProcessor.processPayment(order);

        // Update financial reports
        financialReporter.generateReport(order.getTransaction());
    }
}
```

```Java
public class PaymentProcessor {
    public void processPayment(Order order) {
        // Payment processing logic
        // ...
        System.out.println("Payment is started!");
    }
}
```

```Java
public class FinancialReporter {
    public void generateReport(Order order){
        // Business logic for processing the order
        // ...
        System.out.println("Report is started!");
    }
}
```

```Java
public class Order {
    private int id;
    private String nameSurname;
    private BigDecimal price;

    public Order(int id, String nameSurname, BigDecimal price) {
        this.id = id;
        this.nameSurname = nameSurname;
        this.price = price;
    }

    // getter, setter
}
```

```Java
public class Main {
    public static void main(String[] args) {
        Marketplace marketplace = new Marketplace();
        Order order = new Order(1, "Bill Fox", new BigDecimal(150));
        marketplace.processOrder(order);
    }
}
```

As you can see in the above example, when you start integrating different payment processors and financial reporting modules, you will notice that the code becomes tightly coupled and difficult to extend.

In this situation, IoC could help break the tight coupling between components by inverting the control flow. Therefore, let's refactor the previous examples, essentially transferring the control of dependency creation from one class to another.

```Java
public class PaymentProcessor {
    public void processPayment(Order order) {
        // Payment processing logic
        // ...
        System.out.println("Payment is started!");
    }
}
```

```Java
public class FinancialReporter {
    public void generateReport(Order order){
        // Business logic for processing the order
        // ...
        System.out.println("Report is started!");
    }
}
```

```Java
public class FactoryPaymentProcessor {
    public static PaymentProcessor getPaymentProcessor() {
        return new PaymentProcessor();
    }
}
```

```Java
public class FactoryFinancialReporter {
    public static FinancialReporter getFinancialReporter() {
        return new FinancialReporter();
    }
}
```

```Java
public class Marketplace {
    private PaymentProcessor paymentProcessor;
    private FinancialReporter financialReporter;

    public Marketplace() {
        paymentProcessor = FactoryPaymentProcessor.getPaymentProcessor();
        financialReporter = FactoryFinancialReporter.getFinancialReporter();
    }

    public void processOrder(Order order) {
        // Business logic for processing the order
        // ...

        // Call the payment processor
        paymentProcessor.processPayment(order);

        // Update financial reports
        financialReporter.generateReport(order);
    }
}
```

```Java
public class Main {
    public static void main(String[] args) {
        Marketplace marketplace = new Marketplace();
        Order order = new Order(1, "Bill Fox", new BigDecimal(150));
        marketplace.processOrder(order);
    }
}
```

The previously refactored code represents a basic implementation of IoC. However, this initial step is just the beginning of moving towards a fully loosely coupled design. **This refactored code has not achieved complete loosely coupled classes by only using IoC. To attain this, it is essential to incorporate DIP (Dependency Inversion Principle) and DI (Dependency Injection) alongside IoC.**

It's important to note that IoC is a principle, not a specific pattern. It provides high-level design guidelines without giving implementation details. You have the flexibility to implement the IoC principle according to your preferences, as seen in the previous examples that utilized the factory design pattern.

{% admonition(type="info", title="info") %}
The next article, incorporating **DIP (Dependency Inversion Principle)** and **Dependency Injection (DI)**, will steer the design towards achieving complete loose coupling.
{% end %}



## REFERENCES

[1] <a href="https://www.tutorialsteacher.com/ioc/inversion-of-control" target="_blank">https://www.tutorialsteacher.com/ioc/inversion-of-control</a>

[2] <a href="https://martinfowler.com/bliki/InversionOfControl.html" target="_blank">https://martinfowler.com/bliki/InversionOfControl.html</a>

[3] <a href="https://www.cesarsotovalero.net/blog/inversion-of-control-and-dependency-injection-in-java.html" target="_blank">https://www.cesarsotovalero.net/blog/inversion-of-control-and-dependency-injection-in-java.html</a>

[4] <a href="https://www.yegor256.com/2017/05/10/inversion-of-control.html" target="_blank">https://www.yegor256.com/2017/05/10/inversion-of-control.html</a>