# Java Spring FrameWork

Course Link - [Chad Darby](https://gale.udemy.com/course/spring-hibernate-tutorial/learn/lecture/36828980#overview)

## Spring Core

### Inversion of Control (IoC)

- In general approach whenever a components needs to use another component, it creates an instance of the compoments or requests for the component from a service provider, this creates a "tight coupling" between the components i.e whenever one component changes the other would be affected by the change
- To solve this Inversion of control reverses the traditional approach, in this approach whenever a component requires an object, it is passed as a prop rather than instatiating the object in the class.
- Different Types of IoC

1. Dependency Injection: receive the dependencies through constructor, settor or an interface
2. service locator: receive the dependencies by making a request to a central repository.
3. Factory Pattern: Have a interface class deliver the required dependencies

- Spring framework uses spring container, to handle the Ioc functionalities

## Annotations

- In spring application, the Application context contains the bean factory, by default spring does not create any class objects but by using the @component annotation, we can notify the bean factory.
- If we have multiple class dependencies, the spring framework is notified to fetch required beans using the @AutoWired annotation.
- scopes for objects, singleton and prototype (creates multiple objects each type getBean() is called)

### @Configuration

- Annotation used to define class, which would be used to create the bean factory

### @Bean

- This tag is used to declare beans inside the config file, all beans would be declared and made available during the init process

### @Value

- sets the value in a bean, when its created

## Layers

### Controller

- responsible for handling all the incoming connections, use @RestController annotation, swagger can be integrated into the controller layer

### Service

- responsible for handling all the bussiness logic, use @Service annotation

### Respository

- responsible for connecting to the database, use @Repository annotation

## JPA

- Java Persistence API, its a form of contract used to connect and talk with different databases
- ORM (object relation mapper) is the tool which translates java objects into data base schemas and data rows

## Spring AOP

This is usefull for performance metrics, input validation and security and providing rbac, rollback transactions on exceptions, custom exception handling, caching, auditing, API rate limiting


| **Use Case**                 | **Description**                               | **Example Annotation** |
| ---------------------------------- | --------------------------------------------------- | ---------------------------- |
| **Logging & Monitoring**     | Logs method execution time and method calls.        | `@Around`                  |
| **Security & Authorization** | Restricts access based on user roles.               | `@Before`                  |
| **Transaction Management**   | Ensures rollback on failures.                       | `@Transactional`           |
| **Exception Handling**       | Centralized error handling across services.         | `@AfterThrowing`           |
| **Caching**                  | Stores frequently used data to improve performance. | `@Cacheable`               |
| **Auditing**                 | Tracks user activity for compliance.                | `@Before`                  |
| **API Rate Limiting**        | Prevents abuse by limiting API calls.               | `@Before`                  |
| **Feature Toggles**          | Enables/disables features dynamically.              | `@Before`                  |

Aspect Oriented Programming Concepts in a Nutshell

1. Join Point (When): Think of it as a scene in the movie of your program's execution.
   It's where the action happens!
2. Advice (What): This is the action! It's what happens at a particular scene (Join Point). It's like a plot twist in your program's execution.
3. Aspect (Where-Conceptual): It's the script of your movie. It defines what plot twists (Advice) happen and where (Pointcut).
4. Pointcut (Where-Operational): It's the specific scenes (Join Points) where the plot twists (Advice) occur. It's like a bookmark in your script (Aspect).
5. Target Object (Whom): This is the main character. It's the object that experiences the plot twists (Advice).
6. Weaving (How): This is the director's job. It's how the script (Aspect) is turned into a movie. In Spring AOP, this happens at runtime.
7. Proxy (The Double): This is the stunt double in your movie. In Spring AOP, it's the object that is created after applying advice to the target object. It's the one that takes the hits, performs the stunts, and makes the main character (Target Object) look good.
