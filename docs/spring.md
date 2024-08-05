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

