# SE 464
- Dr. Werner Ditelt (Deetle) wdietl@uwaterloo.ca
- Begin all email subjects with [SE 464]
- office hours by appointment
- tutorials: informal
- Sept 16, 18 lectures: review CS 247 material
- Sept 13, 20 tutorials: review design patterns



## Lec 02: Architecture Introduction

###What is Software Architecture?

- conceptual fabric that defines a system
- role of designers: take radical problems and turn them into normal solutions
- all architecture is design, but not all design is architecture
- ==parts of a system that would be difficult to change once the system is built==
- 3 dimensions:
  - ==structure== (MVC?)
  - ==communication== (how do components talk to one another)
  - ==nonfunctional requirements== (performance, scalability; extensible, influence design choices)
- is
  - all about communication
  - what parts are there? 
  - how do they fit together?
- is not
  - about development
  - about algorithms
  - about data structures

- Web Architecture Example
  - logical architecture (relationship between pages)
  - physical architecture (servers and processing distribution)
  - UI
  - dynamic architecture (data management)

>  Architecture is the fundamental organization of a system, embodied in its components, their relationships to each other and the environment, and the principles governing its design and evolution

> Software architecture is the set of design decisions which, if made incorrectly, may cause your project to fail

- is the blueprint for a software system's construction and evolution
  - structure
  - behaviour
  - interaction
  - non-functional properties
- functional vs non-functional requirements
  - ==Function requirements==: define specific behaviour; what a system is supposed to **do**
  - ==Non-functional requirements==: specify criteria that can be used to judge the operation of a system; how a system is supposed to **be**

- stakeholders in a system's architecture
  - engineers, architects, developers, etc
  - user: uses the product
  - customer: purchases the product
  - vendors: re-sellers of software; re-sell it with additional services or configurations

### Key Definitions

- what is ==principal==?
  - principal implies a degree of importance that grants a design decision architectural status
  - definition depends on system goals
- prescriptive vs descriptive architecture
  - ==Prescriptive==: captures the design decisions made prior to the system's construction; **as-intended**
  - ==Descriptive==: describes how the system has been built; **as-implemented**
- architectural evolution
  - as a system evolves, ideally its prescriptive architecture is modified first
  - in practice, the descriptive architecture is modified directly
  - due to developer sloppiness, deadlines, lack of planning, need for optimizations
- architecture degradation
  - ==architecture drif==t: introduction of principal design decisions into a system's descriptive architecture that are not included in the prescriptive architecture but do not violate any prescriptive design decisions
  - ==architecture erosion==: into of design decisions that violate prescriptive plan
- architecture ==recovery==
  - hard to recover what the nonfunctional requirements were from source code
- architecture elements
  - processing
  - data (information; state)
  - interaction
- ==components==
  - elements that encapsulate processing and data
  - defn: a software components is an architectural entity that
    - encapsulates a subset of the systems functionality / data
    - restricts access using a defined interface
    - has explicitly-defined dependencies
    - usually are application-specific
- ==connectors==
  - interaction might become more important and challenging than functionality of individual components
  - defn: a building block tasked with effecting and regulating interactions among components
  - important because you have performance considerations and tradeoffs
  - examples
    - procedure call
    - shared memory (pointer)
    - message passing
- ==configurations==
  - defn: also called a topology, is a set of associations between components and connectors
- deployment
  - a software system cannot fulfill its purpose until it is deployed
  - executable modules are physicalled placed on hardware devices on which they are to be run
- possible assessment dimensions: available memory, power consumption, required network bandwidth
- ==essential difficulties== (hard to overcome)
  - complexity
  - conformity: system is dependent on its environment
  - Intangibility: hard to manage or understand; not constrainted by physical laws
- attacks on complexity
  - high-level language
  - development tools and environments
  - frame the problem differently: have practical solutions to common problems



## Lec 03: Views and Non-Functional Properties

- Topological goals
  - Minimize coupling; less components know about one another the better
  - Maximize cohesion; components should be responsible for a single logical service
- Abstraction
  - Abstract away unnecessary detail; focus on key issues
  - Use structured programming and data abstraction (ADT)
  - Interfaces
- Decomposition
  - ==top-down abstraction is also called decomposition==
  - Break down the problem into independent components
  - ==make typical cases simple, and exception cases possible==

> The structure of a software system reflects the structure of the organization that built it (Conway's Law)

- Architectural Representations
  - Fundamentally about facilitating technical communication between project stakeholders (let them talk about their viewpoints, their strengths)
  - properties
    - (Min) Ambiguity: open to more than one interpretation?
    - (Max) Accuracy: correct within tolerances (how close are you to the true value, the goal)
    - (Max) Precision: consistent but not necessarily correct (how spread out your measurements are)

- 4 + 1 View Model
  - Logical View: static decomposition
  - Development View: how do you decompose into individual files; package naming; which developers work on what; aspects about the development process
  - Process View: sequence diagrams; how typical interactions between components works; sequence of events that system must support
  - Physical View: deployment view, servers, network topology, power consumption
  - +1: all the possible scenarios; ==want to be grounded in your understanding of what the end-user actually wants==
- Architectural Models, Views, and Visualizations
  - Model: high-level documentation of the design
  - Visualization: 
  - View: subset of related design decisions (deployment viewpoint)
- Component Diagram
  - Capture components and relationships
  - Expose an input interface and output interface
- Sequence Diagram
  - Inter-component collaboration
  - Specific runtime scenarios
  - Arrows running left and right between components, with time increasing as you move down
- Deployment Diagram: How to map components to physical devices
- Statechart Diagram: state machine; might design first to figure out how your system will actually function

### F and NF Properties

- FP: system shall do X
- NFP: system shall be Y
- NFP:

  - Constrain on the manner in which the system implements and delivers its functionality
  - High-level types: Product requirements, process requirements, external requirements
  - Examples: Efficiency; complexity; scalability
  - Lots of conflicts between stakeholder NFP desires
- Design Guidelines for different NFPs

#### Efficiency 

- Components 
  - small (**high cohesion**)
  - simple and concise interfaces
  - separate data from processing components
  - separate data from meta-data (keep movie names list separate from GB's of movies video data)
  - **know which components are bottlenecks**
- Connectors 
  - select connectors carefully (live in same memory? Communicate over server?)
  - encourage async interactions
  - careful of broadcast connectors (all other components will need to listen and be able to handle that info)
  - Location transparency (scalability: don't want to care where that other component (DB) is; efficiency: DO want to know where it is)
- Topology
  - keep frequent collaborators close; ex: don't pass GBs of data through multiple layers

#### Complexity

- Proportional to size of system; internal structure
  - Components
    - Separate concerns
    - Isolate functionality from interaction
    - Ensure high cohesiveness
  - connectors
    - Isolate interaction from functionality
    - Network connector: bitstream in, bitstream out
    - Keep a simple network topology
    - Restrict interaction; **good API design**
  - topology
    - Eliminate unnecessary dependencies
    - If you have many components, the **compose** groups of related components into larger components; **zoom out**

#### Scalability / Heterogeneity:

- Scalability: capability of a system to be adapted to meet new size or scope requirements
- Heterogeneity: a system's ability to be composed of, or execute within, disparate parts
  - Portability: ability of a system to properly execute on multiple platforms
  - Components
    - Keep components focused (==so you can find bottlenecks==)
    - Distribute data sources (different data centres around the world)
    - Replicate data: don'y only have 1 DB that stores the information; have different data centres around the world, each replicate the data; migrate changes over time (issue with concurrent modifications)

  - Connectors
  - Be explicit: don't use shared memory as a communication platform (doesn't scale)
    - Direct vs indirect: easier to scale if indirect (didn't hard-code the connection)

  - Topology
  - Place data close to customer
    - Think about who owns the data, about what moves around and where it stays

#### Evolvability

- Ability to change to satisfy new requirements and environments
  - Components
  - 
  - Connectors
    - 
  - Topology

#### Dependability

- examples
  - reliability: system will perform without failure
  - availability: probability system is available at a moment in time
  - robustness:
  - fault-tolerance:
  - survivability:
  - safety:
- components
  - support exception handling
  - control external component dependencies
- connectors
  - provide interaction guarantees
- topology:
  - avoid single points of failure
  - enable backups



## Lec 04/5: Design Patterns

- composition vs aggregation
  - composition: 
    - both are dependent on one another
    - **(part of)** object A can't exist without object B (a carbonator can't exist without a car)
  - aggregation: 
    - unidirectional (one-way): a department has a student, but a student does not have a department
    - **(has a)** class A and class B can exist without one another
- Liskov Substitution Principle: subtypes should behave (and be able to replace) their parents
- Design Pattern General Stuff
  - 4 main parts
    - name
    - problem
    - solution
    - consequences
  - still need to manually apply the design patterns using names that make sense

### Creational Patterns

- what is bad about `Foo foo = new Foo()` ?
- use `Foo foo = indirection.makeFoo()` instead
- defer decisions about class instantiation until runtime. **Why is this better?**

#### Singleton Pattern

- is basically a global variable; affects other parts of the system that maybe you don't want to
- concurrency issues

```java
class Singleton {
	private static Singleton instance;
	private Singleton(){}
	static Singleton create() { // method on the class instead of the object
															// don't need an instance to use it
		if (instance == null) instance = new Singleton()
		return instance
	}
}
```

- why not just make all fields and functions static? because in the create method you could still have some logic to decide what kind of singleton to create (if there are sub-classes). You could want to use a RedSingleton or a BlueSingleton etc`

#### Factory Method Pattern

- call factory method to give me a new object instead of directly calling the constructor
- need to pass in params to factory method if you want to get a new concrete class

```java
public abstract class Shape {
  public abstract void draw();
}
public class Square extends Shape {
  @Override
  public void draw(print("square"))
}
public class Circle extends Shape {
  @Override
  public void draw(print("circle"))
}
public class ShapeFactory {
  public Shape getShape(String shape) {
    if shape is null
      return null
    if shape is "square"
      return new Square()
    if shape is "circle"
      return new Circle()
  }
}
```



#### Abstract Factory Method Pattern

- an interface (super factory) is responsible for creating a factory of related objects without explicitly specifying their classes
- isolates client code from concrete implementation factory classes

```java
interface Shape {
	void draw();
}
class RoundedSquare implements Shape {
	void draw() { print("ROUNDED SQUARE") }
}
class RoundedRectangle implements Shape {
	void draw() { print("ROUNDED RECTANGLE") }
}
class Rectangle implements Shape {
	void draw() { print("RECTANGLE") }
}
class Square implements Shape {
	void draw() { print("SQUARE") }
}
abstract class Factory {
	abstract Shape getShape(String type)
}
class NormalShapeFactory extends Factory {
	Shape getShape(String type) {
		if type is SQAURE
			return new Square()
		if type is RECTANGLE
			return new Rectangle()
	}
}
class RoundedShapeFactory extends Factory {
	Shape getShape(String type) {
		if type is SQAURE
			return new RoundedSquare()
		if type is RECTANGLE
			return new RoundedRectangle()
	}
}
class FactoryProducer { // this is what you use to get your factory
	static Factory getFactory(boolean isRounded) {
		if is rounded return RountedShapeFactory()
		else return NormalShapeFactory()
	}
}

void main() {
  	Factory f = FactoryProducer.getFactory(isRounded=false);
    Shape normalSquare = f.getShape("square");
  
  	Factory f2 = FactoryProducer.getFactory(isRounded=true);
  	Shape roundedRect = f2.getShape("rectangle")
}
```



#### Builder Pattern

```java
class Car {
	int numSeats
	String engineType
	String colour
	// setters and getters
}

// Generic Car Builder
interface Builder {
	void reset()
	void setSeats(int x)
	void setEngineType(String e)
	void setColour(c)
}
class CarBuilder implements Builder {
	private Car car
	public CarBuilder() {
		reset()
	}
	void reset() { this.car = new Car() }
	void setSeats(int x) {
		car.setSeats(x)
	}
	void setEngineType(String e) {
		car.setEngineType(e)
	}
	void setColour(String c) {
		car.setColour(c);
	}
	Car getProduct() {
		return this.car
	}
}

class Director {
	void constructSportsCar(Builder b) {
		builder.reset()
		builder.setSeats(2)
		builder.setEngine("FAST")
		builder.setColour("red")
	}
	
	void constructSUV(Builder b) {
		...
	}
}

main {
	CarBuilder builder = new CarBuilder()
	director.constructSportscar(builder)
	Car c = builder.getProduct()
}
```

#### Prototype Builder

- instead of having a class and saying give me this class, you add fields and add methods to a data structure, and at some point you say "this is my object, it has everything I want" and from then on you just clone that prototype



### Structural Patterns

- ==make the relationships/structure of the code more flexible==

#### Class Adapter Pattern

- have an existing impl that provides some functionality
- adapter provides impl that new interface can use but still have access to origin impl
- what's bad of using class-based: because it inherits, then client can see ALL of the methods of the adaptee's
- **use case: old interface to new interface; want to abstract away old implementation details and use a new interface**

```java
interface Bird {
	void makeSound()
}
class Sparrow implements Bird {
  @Override makeSound() { print("Chirp Chirp") }
}
interface ToyDuck {
  void squeak();
}
class PlasticToyDick implements ToyDuck {
  @Override squeak() { print("Squeak")}
}
class SparrowAdapter extends Sparrow implements ToyDuck {
  @Override squeak() { this.makeSound() } // ADAPTED
  // this.makeSound() is STILL PUBLIC for this static class
}
main {
  ToyDuck toyDuck = new PlasticToyDuck()
  ToyDuck toySparrow = new BirdAdapter(new Sparrow())
	toyDuck.squeak() // --> "squeak"
  toySparrow.squeak() // --> "chirp chirp"
  
  SparrowAdapter gg = new SparrowAdapter();
  gg.squeak();
  gg.makeSound(); // --> we don't want this to be public
}
```



#### Object Adapter Pattern

- Adaptor doesn't inherit from Adaptee, ==instead it just HAS the Adaptee it wants to use==
- interface to client is not cluttered with Adaptee functions, only sees the Adaptor method
- **use case: old interface to new interface; want to abstract away old implementation details and use a new interface**

```java
interface Bird {
	void makeSound()
}
class Sparrow implements Bird {
  @Override makeSound() { print("Chirp Chirp") }
}
interface ToyDuck {
  void squeak();
}
class PlasticToyDick implements ToyDuck {
  @Override squeak() { print("Squeak")}
}
class BirdAdapter implements ToyDuck {
  // **** OBJECT ADAPTER, adapter holds instance of adaptee ****
  Bird bird;
  public BirdAdapter(Bird b) { this.bird = b}
  @Override squeak() { bird.makeSound() } // ADAPTED
}
main {
  ToyDuck toyDuck = new PlasticToyDuck()
  ToyDuck toySparrow = new BirdAdapter(new Sparrow())
	toyDuck.squeak() // --> "squeak"
  toySparrow.squeak() // --> "chirp chirp"
}
```



#### Bridge Pattern

- ==separate the abstraction from the implementation==
- client codes to the abstraction (interface) without being concerned about the implementation
- abstraction has a reference to the implementor
- Note: adapter helps things work AFTER they've been created; Bridge designs classes such that they work up front and let abs/impl vary independently

```java
// Implementation:
// Abstract Implementation
abstract class MoveLogic {
  abstract void move()
}
// Concrete Implementations
class Walk extends MoveLogic {
  void move() { print("move my legs") }
}
class Fly extends MoveLogic {
  void move() { print("flap my wings") }
}

// Abstraction:
// Abstract Abstraction
abstract class Animal {
  abstract void howDoIMove()
}
// Concrete Implementations
class Person extends Animal {
	private MoveLogic moveLogic;
  public Person(MoveLogic m) { this.moveLogic = m }
  public void howDoIMove() {
    this.moveLogic.move()
  }
}
class Bird extends Animal {
	private MoveLogic moveLogic;
  public Bird(MoveLogic m) { this.moveLogic = m }
  public void howDoIMove() {
    this.moveLogic.move()
  }
}

main {
  MoveLogic walk = new Walk()
  MoveLogic fly = new Fly()
  Animal person = new Person(walk)
  Animal bird = new Bird(fly)
  person.howDoIMove() // --> move my legs
  bird.howDoIMove() // --> flap my wings
}

```



#### Decorator Design Pattern

- lets users add new functionality to an **existing** object without altering its structure
- create a decorator class which wraps the original class and provides additional functionality 

```java
abstract class Vehicle {
  abstract String description();
}
class Car extends Vehicle {
  @Override String description() { return "I am a car" }
}
abstract class Decorator extends Vehicle {
  protected Vehicle component;
  public Decorator(Vehicle v) { this.component = v}
  // does NOT override description()
}
class CoolWheels extends Decorator {
  public CoolWheels(Vehicle v) {
    super(v)
  }
  @Override String description() {
    return component.description() + " with cool wheels"
  }
}
class CoolPaint extends Decorator {
  public CoolPaint(Vehicle v) {
    super(v)
  }
  @Override String description() {
    return component.description() + " with cool paint"
  }
}
main {
  Vehicle car = new Car()
  car = new CoolWheels(car)
  car = new CoolPaint(car)
  print(car.description())
  // i am a car with cool wheels with cool paint
}
```



#### Facade Pattern

- Be like an API or system / library interface to make using lots of their components easier

```java
class Oven {
  void startOven
}
class PizzaMaker {
  void preparePizza
}
class PizzaBoxer {
  void boxPizza
}
class PizzaFacade {
  private Oven oven
  private PizzaMaker pizzaMaker
  private PizzaBoxer pizzaBoxer
  void getAndServicePizza() {
    oven.startOven
    pizzaMaker.preparePizza
    pizzaBoxer.boxPizza
  }
}
```



#### Proxy Pattern

- control and manage access to object they are protecting

```java
interface Internet {
	void connectTo(String serverHost)
}
class RealInternet implements Internet {
  void connectTo(String serverHost) { 
    print("connected to real internet")
  }
}
class ProxyInternet implements Internet {
  private Internet internet = RealInternet()
  List<String> bannedSites
  void connectTo(String serverHost) {
    if serverHost in bannedSites
      throw error
    else internet.connectTo(serverHost)
  }
}
```



#### Composite

- partitioning design pattern that describes a group of objects that is treated the same way whether it is a single instance of a collection of instances of that object type

```java
abstract class StoryCollection {
  abstract void printDescription()
  abstract void addToCollection(StoryCollection c)
}
class Book extends StoryCollection {
  String name
 	public Book(String n) { this.name = n }
  void printDescription() {
    print("I'm a book: " + name)
  }
}
class Series extends StoryCollection {
  List<StoryCollection> stories
  void addToCollection(StoryCollection c) {
    this.stories.add(c)
  }
  void printDescription() {
    stories.forEach(story -> story.printDescription())
  }
}
```



#### Flyweight Pattern

- provides ways to decrease object count
- used when we need to create a large amount of similar objects ($> 10^5$)
- objects must be **immutable**
- use a hashTable to have keys point to instances of objects
- intrinsic states: states that can be shared for the different instances of the same object (ex: text editor, each letter is (char, font, size). 'B' is no different from any other 'B')
- extrinsic state: can't be shared. Now, each letter is (char, font, size, row, col). row and col can't be shared for 2 different 'B's
  - ==so, you have 26 char objects (or, more because of font and size, but still not too many). you get the object, then you assign the row and col, and then you print.==
  - ==don't need to create thousands and thousands of objects which need to be stored in memory==

```java
interface Player {
	void assignWeapon(String w)
	void mission()
}
class Terrorist implements Player {
	private final String TASK = "PLANT A BOMB"; // intrinsic
	private final String weapon; // extrensic
	void assignWeapon(String w) { this.weapon = w}
	void mission(print("Terrorist with weapon " + weapon))
}
class CounterTerrorist implements Player {
	private final String TASK = "DIFFUSE BOMB"; // intrinsic
	private final String weapon; // extrensic
	void assignWeapon(String w) { this.weapon = w}
	void mission(print("CounterTerrorist with weapon " + weapon))
}
class PlayerFactory {
  private static HashMap<String, Player> playerMap
  public static Player getPlayer(String type) {
    if type in playerMap.keys
      return playerMap.get(type)
    else
      if type is "Terrorist"
        Player p = new Terrorist()
        playerMap.put("Terrorist", p)
     else
        Player p = new CounterTerrorist()
        playerMap.put("CounterTerrorist", p)
  }
}
main {
  for i from 0 to n
    // always returns one of 2 objects only, not n
    Player p = PlayerFactory.getPlayer(randomType())
    p.assignWeapon(randomWeapon())
    p.mission() // send them on their mission
}
```



### Behavioural Patterns

- Identify common communication patterns between objects; increase flexibility in carrying out this communication
- Interpreter, Template, chain of responsibility, command, iterator, mediator, memento, observer, state, strategy, visitor

#### Visitor

- Separate an algorithm from an object structure on which it operates
- Can add new operations to an existing object without modifying those structures

```java
abstract class Weapon {
  void strike(Soldier s); // visit
  void strike(Troll t);
}

class Sword extends Weapon {
  void strike(Solider s) {
    s.health -= 1;
  }
  
  void strike(Troll t) {
    t.health -= 2;
  }
}

abstract class Enemy {
  void beStruckBy(Weapon w); // accept
}

class Soldier extends Enemy {
  void beeStruckBy(Weapon w) {
    w.strike(this);
  }
}

class Troll extends Enemy {
  void beeStruckBy(Weapon w) {
    w.strike(this);
  }
}
```

- ==Double dispatch==: dispatches a function call to different concrete functions based off of the runtime type of the two objects involved
- ==Single dispatch==: for most calls in an OOP language, the concrete function that is called depends only on the runtime type of one object



#### Subject / Observer Pattern

```java
abstract class Observer {
  void getNotified()
}

abstract class Subject {
  List<Observer> observers;
  void attach(Observer o); // add to observers
  void detach(Observer o); // remove
  void notifyAll(); // call o.getNotified() for all observers
  State getState();
}

class Election extends Subject {
  void electionIsOver() {
    notifyAll()
  }
}

class NewsStation extends Observer {
  void getNotified() {
    ...
  }
}
```



#### Template method pattern

- Uses a skeleton 

```java
abstract class PhoneBill {
  abstract double getPlanBill();
  double getTotalBill()  {
    int bill = 0;
    bill += 10; // base rate
    bill += getPlanBill(); // abstract away to a conrete type
    bill *= 1.13; // tax
    return bill
  }
}

class CheapPhoneBill extends PhoneBill {
  double getPlanBill() {
    return 10;
  }
}

class ExpensivePhoneBill extends PhoneBill {
  double getPlanBill() {
    return 20;
  }
}
```



## Lec 06: Architectural Styles

- Architectural Pattern: set of architectural design decisions that are applicable to a recurring design problem, and parameterized to account for different software development contexts in which that problem appears

  - ex: Three-Tiered Pattern
    - Front
    - Middle
    - Back
  - ex: MVC
  - ex: Sense-Compute-Control

- Architectural Styles: 

  - certain design choices regularly result in solutions with superior properties
  - a named collection of architectural design decisions that
    - are applicable to a given development context
    - constrain architectural design decisions that are specific to a particular system within that context
    - elicit beneficial qualities in each resulting system
  - good properties
    - structure system such that it makes it easy to evolve in the future
  - rarely see "pure" architectural styles used
  - Style Analysis Dimensions
    - design vocabulary 
    - allowable structural patterns?
    - underlying computation model: focus more on communication or more on how data gets exchanged
    - essential invariants: what properties should always hold?
    - common use examples
    - pros and cons

- Architectural Styles types

  - Dataflow: stages of processing; focus on how the data flows through that system

    - how it gets manipulated along the way

  - shared memory: shared repository through which you share information

  - interpreter: need flexibility of having a configuration language; some interpreter that configures your system; code gets interpreted at point in system that makes most sense

  - implicit invocation: event systems; don't care which component responds to that event

  - Layered Style

    - each layer exposes an API to be used by above layers
    - each layer provides services to higher layers, and interacts with lower layers
    - do not expose implementation details
    - example: operating system
    - advantages:
      - increasing abstraction levels
      - can have different implementations of layers (evolvability)
    - disdvantages
      - not universally applicable
      - performance

  - Client-Server 

    - servers do not know number or identities of clients
    - clients know server's identity




## Lec 07: Architecture Styles 2

- Virtual Machine: provides abstraction that lets other services use it without having to worry about implementation

- Dataflow: some graph network that represents how data flows throughout system

  - Not just a layered architecture flipped sideways
  - One focuses on APIs (layered)
  - Datflow focuses on data and how it moves

- Pipe and Filter: many tiny programs that communicate through a common API

- Shared memory: 

  - Sure, small programs use shared memory, but that isn't shared memory architecture

  - Blackboard:

    - Central data structure: the blackboard

    - Components operating on the blackboard

    - System is entirely driven by the blackboard state

    - It triggers interaction between the components

    - Typically used for AI systems, compiler architecture

    - Don't setup any communication between individual components

    - Only indirect communication

    - Ex: compiler

      - parser
      - Lexical analysis
      - optimization
      - assembler

      - Code generation
      - Sounds like a pipe and filter? Different phases which pass along information
        - But then you have a symbol table
        - And an Abstract Symbol Tree
        - Cyclic dependencies between components

  - Rule-Based

    - Inference engine parses user input and determines whether it is a fact/rule or a query
    - If it is a fact or rule, it adds this entry to the knowledge base
    - Otherwise, it queries the knowledge base for the applicable rules and attempt to resolve the query
    - Becomes very difficult when large number of rules
    - Ex: datalog (type of first order analysis)
      - High level description language?
      - Rule-based system?
      - Individual data rules describe some aspect of your static analysis

  - More examples:

    - Calculator 
      - Shared representation: larger mathematical expression
      - Each operator looks at the total expression and sees if it can solve and modify any smaller components
    - Database as backend?
      - Depends; is the interaction of all these micro services always through the database? Probably not

  - disadvantages

    - Concurrency is an issue, locks, waiting

- Interpreter style

  - Interpreter parses and executes input commands, update the state maintained by the interpreter
  - components
    - Command interpreter
    - Program state
    - User interface
  - Ex:
    - Command line interface (terminal)
    - Web browser (displays visuals by interpreting Javascript)
    - Pdf viewers
  - Mobile code
    - Javascript vs pdf viewers?
      - Usually pdf is local data
      - Javascript is mobile code, download it and execute it as part of a website

- Implicit invocation

  - Event announcement instead of method invocation
  - Listeners register interest in and associate methods with events
  - x happened, whomever wants to listen to x gets notified
  - Don't hardcode dependency between event and component
  - Publish and subscribe
    - Game server publishes events
  - Event-based stytle
    - Shared message bus
    - Each component can put a message on the bus
    - Queueing system
    - Each component and look at the event, take it off, handle it, put it back on
    - Just care on the event, not on where that event came from

- Peer to peer

  - Distributed database? maybe
  - Peers acts as either clients or servers
  - Peers:
  - Connectors:
  - Data elements:

- Distributed objects:

  - COBRA
  - REST API / Microservices
    - Components in any programming language, exchange information over standard HTTP protocols to affect state

- ==Design Recovery==: examine existing code base, determine overall topology; uses 2 types of clustering
  - ==Syntactic Clustering==: static relationships; coupling and cohesion
  - ==Semantic clustering==: behavioural information; difficult to automate; executing system on different inputs

- Greenfield Design

  - Product statement, come up with a solution
  - Blank slate
  - Literature searching: research to learn about technologies
  - Morphological charts: draw trees and how they evolve
  - Removing mental blocks:



## Let 08: Architecture Modelling

- What is an ==architecture model==?
  - Recall: all architecture is design, but not all design is architecture
  - ==Artifact that captures some or all of the design decisions that comprise a system's architecture==
- How do we choose what to model?
  - Which decisions and concepts
  - Which level of detail
  - How much right / formality
- Examples of stuff to model
  - Static and dynamic aspects
  - Function and non-functional aspects
- Important Characteristics of Models
  - Ambiguity: open to more than one interpretation
  - Accurate: correct, conforms to fact
  - Precise: sharply exact
- Views and Viewpoints:
  - Model could be too large, complex, confusing to put into a single model / document
  - So, create several coordinated models
  - Viewpoint: user interaction
  - Viewpoint: system administrator
- Consistency Among Views
  - Views often contain overlapping and related design decisions
  - Want views to be consistent -> all contain design decisions that are compatible (one truth, one true total system model design)
- Common Types of Inconsistencies
  - Direct (model A says 3 x, model B says 2x)
  - Refinement (high level view and low-level view conflict)
  - Static vs dynamic
  - Functional vs non-functional
- Evaluating Modelling Approaches
  - Scope and purpose; what does it help you model?
  - Basic elements
  - Style
  - Dynamic modelling
  - Non-functional aspects
  - Concise ? Time it takes to understand ?
- Examples
  - Natural Language; not concise; can't understand quickly;
  - UML (Unified Modelling Language)
    - Common vocabulary
    - Disadvantages: need to ensure consistency among views 
  - Diagrams to Review
    - Static structure
      - Component, deployment, class diagrams
    - Dynamic behaviour
      - Sequence, use case, state machine diagrams



## Lec 08: Security

- NFP: ==Security==
  - The protection afforded to an automated information system in order to attain the applicable objectives of preserving the integrity, availability, and confidentiality of information system resources (hardware, software, firmware, data)
  - ==Confidentiality==: prevent unauthorized parties from accessing information
  - ==Integrity==: only authorized parties can manipulate data
  - ==Availability==: accessible by authorized parties on all appropriate occasions
- Design Principles for Security (9)
  - Least Privilege: 
    - Only the minimum necessary rights should be assigned to a subject that requests access to a resource and should be in effect for the shortest duration necessary (remember to relinquish privileges)
  - Fail Safe Defaults
  - Economy of Mechanism
  - Complete Mediation
    - ==Mediation == intervention / arbitration between user and object, middle step for validation==
    - Requires access checks to an object each time a subject requests access
    - Decreases chances of mistakenly giving elevated permissions
    - If you only check permissions once, then attackers can exploit the system (what if the access control rights are decreased after user first accesses data)
    - Caching permissions can improve performance, but at the cost of security
  - Open Design
  - Separation of Privilege
  - Least Common Mechanism
  - Psychological Acceptability:
    - Make sure security algorithm is accepted by its users (what's the point if they just use a sticky note with the password)
  - Defense in Depth 



## Lec 10: Design

- ==Analysis==
  - Domain-level modelling of real world objects
- ==Design==
  - Modelling of implementation objects
- Design process: measure --> learn --> build --> repeat
  - alAre you just trying to understand the problem domain?
  - Or are you now trying to focus on the implementation?
- Motivation
  - We want specific notations that let us represent these design ideas in code
- Concepts
  - Design patterns are for high-level re-use
  - How do we know if a solution is elegant?
  - abstraction
    - Remove details while retaining essential properties of structure
    - Designer can focus on key issues without being distracted by implementation
    - Example of too much abstraction
      - Debugging a visitor
      - Double dispatch is hard to debug
  - Static vs dynamic
    - Increased abstraction == decreased control
    - Composition instead of inheritance 
- Design Principles
  - ==STUPID Principles==
    - Singleton (shared global state is bad)
    - Tight Coupling
    - Untestable (large functions, can't mock)
    - Premature Optimization (start to optimize A, but maybe A isn't even the bottle neck)
    - Indescriptive Naming
      - Do use nouns for class names
      - Do use verbs for function names
    - Duplication
  - ==SOLID Principles==: https://itnext.io/solid-principles-explanation-and-examples-715b975dcad4
    - ==Single Responsibility Principle==
      - Classes should only have one major task
    - ==Open/Closed Principle==
      - Open for extensions closed for modifications
      - Template method in super class, calls other methods, maybe some are implemented or some are abstract
      - Then you have subclasses that can adapt the behaviour
      - Allow for its behaviour to be extended WITHOUT modifying its source code
      - Essentially; just use interfaces (or abstract base classes)
    - ==Liskov Substitution Principle==
      - You want subtypes to behave consistently with super types
      - If you have a method in the super class that accepts all integers, then you DONT want to have a child class that implements the same function but throws if you pass in a negative integer
    - ==Interface Segregation Principle==
      - Don't add extra methods to an interface if it doesn't relate to that interface
      - Just make a new interface instead
      - Pick the smallest implementation that provides what you need
    - ==Dependency Inversion Principle==
      - Have dependencies between high level abstractions (have dependencies between Observer and Subject, not on concrete implementations)
  - Lower-Level Principles
    - Program to abstractions (interfaces), not to implementations (classes)
    - Favour composition over inheritance
  - Quality Attributes: Coupling
  - Quality Attributes: Cohesion
    - Coincidental
    - Logical
    - Temporal: all these operations occur at time x, put them in module X; all these other operations occur at time y, put them in module Y
    - Communication
    - Sequential
    - Functional: every module has 1 functionality only
- Spotting Incoherency
  - An operation's description is full of 'and' clauses (do this and do that and then do this)
  - An operation's description has many "if...then...else"
- Cognitive Dimensions
  - Premature commitment (decisions made with insufficient data)
  - Hidden dependencies
    - Who needs to update what elements of a data structure and in what sequence 
  - Secondary notation
    - UML diagrams; state machines; sequence diagrams
  - Viscosity
    - Resistance to change
- Why do designs matter?
  - Flexibility, reusability, shared vocabulary



## Lec 11: Code Smells

- SOLID Principles

- Liskov Substitution Principle

  - Subtypes should behave same as their parent types

  - Conform to the same contract, but have different implementations

  - Allow us to program against an interface, and rely that any concrete implementation will work (won't crash, throw unexpected errors, etc)

  - Overloading: different method that has the same name, but does different stuff

  - Overriding: this method might get selected at runtime based on dynamic types

    - In java: param types must be the same (pretty much the same, child classes are allowed)

  - ```java
    class A {
      RA m(PA p)..
    }
    class B extends A {
      RB m(PB p)...
    }
    
    RB extends RA
    PA extends PB
    
    // in Java, param types must be the same
    
    int main() {
    	A a = new B();
    	RA foo = a.m(...)
    }
    ```

  - Params: You can only go same or MORE general (PA in A, B extends A, and its function takes a PB, which is the parent of PA, and it is MORE GENERAL)

  - Return type: you can only go MORE SPECIFIC

    - Specify a return type of List
    - In implementation, you return an ArrayList

  - **Subclass**:

    - **Less strict pre-conditions** (accept more general)
    - **More-strict post-conditions** (return more specific)

- Visitor

  - Data structure; want to apply operations on to it
  - At some point, you make it available to different people, and each of them want to be able to affect it in different ways
  - Want to allow external extensions; be able to add different interactions
  - Element: accept(Visitor)
  - Each visitor then implements visit
  - What is it called double dispatch?
    - Only have the accept methods once for each data structure
    - That dispatches to the visitor
    - Only need to implement functionality once in the visitor (for each different elemnt)
    - 1 dispatch on the data structure to accept a visitor
    - Another dispatch in the accept method to dispatch to the correct visitor
  - Similar to the COMPOSITE DESIGN PATTERN
    - Needs to provide visit functionality for each of them
  - downsides
    - Hard to debug
    - Lots of overhead

- Single, Double, Multiple Dispatch

  - Single dispatch: only matters what runtime type of receiver 
  - Finds what the most concrete method is for a given call

- Code Smells
  - Large classes and methods 
  - Deep nesting of control structures (too much indentation; weird logic, breaks)
  - Duplicate code
  - Static state (state that is shared between all instances of a class; need to think of how all instances interact)
  - No information hiding, bad encapsulation
    - Difference? Information hiding; static visibility of names in your program (public protected private etc)
    - Encapsulation is the runtime concept; its the public API that you expose and how those functions interact with the protected and private information
      - Need to ensure public API still protects information
      - ==Solution: defensive copying in a getter; return a copy; not a reference==
      - ==Another solution: make a copy of input variables; don't want outside world to still have a reference to that data==
  - Many params
  - Casts to implementation classes

- Refactoring
  - Restructure existing code without changing its behaviour
  - Renaming methods; moving methods
  - Having good test coverage gives incidence in changes
  - When you re-name a method, maybe you accidentally override a method from the super class

- MVC / MVP / MVVM

  - Why is MVC bad?
    - Data structures shared?
    - View just gets a pointer to a data structure from the model
    - Tight coupling between the data structures between these components
    - Bad re-use
    - Tight coupling between view and controller
  - Model View Presenter MVP
    - Presenter only takes primate common data structures
      - Glues model and view together; breaks down domain objects into primitives
    - View:
      - Only interacts with primitives
      - Does not interact with domain logic
      - Better separation
  - Model View ViewModel
    - Have another view model...
    - Good for event-driven programming
  - MVC vs MVP
    - MVP: easier to test; each view has its own presenter; interaction with view is through an interface; view is in charge
    - MVC: harder to test; one controller; controller determines which view to show; all views use the same controller; view is more tightly coupled to the model; controller is in charge

- Midterm Review
  - Stakeholder Questions:
    - What do they want?
    - Do they all have a common understanding? No
  - Views and NonFunc Proeprties
    - 4+1; why do we need them?
    - What's wrong with having different views? Inconsistencies 
    - Non func props: tradeoffs?
  - Design patterns
    - Composition lets you change it at runtime (you have a reference to it)
  - Architectural Styles
  - Modelling
  - Security
    - Read chrome paper?
  - Design Intro
    - SOLID
    - Coupling / Cohesion, different levels
  - Code smells
    - Liskov substitution principle
    - Examples of bad codex
    - Dispatch: when you call a function, which function is it actually at runtime?
    - Double dispatch: visitor example; end function depends on runtime type of visitor AND acceptor



## ... oct 28

- inversion control
  - abstract factory
  - plug in the concrete class that should be instantiated
  - service locator
  - template method; abstract giving the detailed implementations
- dependency injection
  - an example of inversion control
  - allows us to achieve this dependency inversion
  - Eliminate initialization statements (Foo f = new ConcreteFoo())
  - bootstraps object creation with a configured injector
- dependency inversion principle



## Design patterns we didn't know

- State: finite state machine
  - State ABC, and then different concrete implementations
  - Repeatedly call state.handle(action).handle(action).handle(action) etc
  - Handle returns the new state; always takes in some action
- Memento design pattern
  - Stacks with getState setState
- Mediator
  - Bunch of colleagues implement from college ABC
  - Mediator HAS a bunch of colleagues
  - Mediator acts as a broker
  - C1 calls mediate(c1) and then it gets state from c1, and then refers to c2 to do stuff
- Command
  - Dynamically call methods (ex: from a string)
  - Client, command, receiver

## More on Arch Styles

- LLIDIPS
- For each:
  - Component and connectors
  - Allowed structural patterns
  - Computational model
  - Essential invariants
  - Common examples
  - Pors and cons
  - Specialization

#### Language-Based

####Layered

- Programs that can be decomposed into groups (layers) of subtasks; each layer provides services to next higher layer
- EX: desktop applications; e commerce web apps
- Simplifies design; different role players can work at different levels

####Interpreter

- Interprets programs written in a dedicated language (CFG, SQL)
- Context <---- Client ----> Abstract Expression
  - TerminalExpression
  - NonTerminal Expression

####Dataflow

> Focused on the flow of data; data is transformed by a series of steps into output data

- Batch sequential:
  - Sequential programs are executed in order and the data is passed from one program to the next
  - Con: no concurrency
- Pipe and Filter:
  - Multiple pipes and filters, with loops, more complex logic; data is passed through pipes; processing steps are enclosed in filters
  - Ex: compilers; different filters perform lexical analysis, parsing, semantic analysis, code generation

#### Implicit Invocation

- Event based (shared event bus)
  - Sources, event bus, and listeners
- Publisher and subscriber

#### Peer to Peer

- Individual components called peers which function both as clients (request services) and servers (provide services)
- Usage: file-sharing networks
- Issues with data backup and recovery

#### Shared Memory

- Blackboard:
  - Computational framework for systems that have large and diverse specialized modules and complex control strategies
  - Different modules need to be combined to solve a problem
  - Components: blackboard (global memory), knowledge sources (modules with their own representation), control component (selects and configures and executes modules)
  - Ex: speech recognition, vehicle identification and tracking, sonar signal interpretation
- Rule Based
  - Store and manipulate knowledge to interpret information in a useful way
  - Ex: expert system that uses rule to make deductions or choices (help a doctor make the right diagnosis)



## Acronyms

- Non Func Properties: ESCHPED (ess-sch-ped)
  - Evolvability scalability complexity hetero_something portability efficiency dependability
- Architectural Styles: LLIDIPS (lee-dips)
  - Language based, layered, interpreter, data flow, implicit invocation, peer to peer, shared memory
- Security: CIA
  - ? ? ?

## Lec 14: Dependency Injection

- SOLID. D stands for dependency inversion

  - High level modules should depend on abstractions, not low-level concrete implementations
  - Minimizes direct coupling between concrete classes
  - Program to interfaces not to implementations
  - Details should depend on abstractions
  - Often manifests itself in object creation
  
- IoC: inversion of control

  - Custom-written (concrete, specific) parts of a program receive the flow of control from a generic framework

- Dependency inversion (more)

  - Want to eliminate coupling between concrete classes
  - Want to be able to substitute different implementations without recompiling
  - Solution: separate objects from their assemblers

- Eliminate initialization statements

  - `Foo f = new ConcreteFoo()`
  - Takes a set of components (classes, interfaces)
  - Adds a set of configuration metadata
  - Provides the metadata to a dependency injection framework
  - Bootstraps object creation with a configured injector

- Googl Guice

  ```
  public class BillingModule extends AbstractModule {
  	@Override
  	protected void configure() {
  		bind(Foo.class).to(SimpleFoo.class);
  		bind(Bar.class).to(ExpensiveBar.class);
  		bind(BillingService.class).to(RealBillingService.class);
  	}
  }
  
  public class RealBillingService implements BillingService {
  	// Guice automatically instantiates the correct objects
  	// determines which concrete implementations get injected into constructor arguments
  	@Inject
  	public RealBillingService(Foo foo, Bar bar) {
  		this.foo = foo;
  		this.bar = bar;
  	}
  }
  
  public static void main(String[] args) {
  	Injector inj = Guice.createInjector(new BillingModule());
  	BillingService b = injector.getInstance(BillingService.class);
  }
  ```

  

## Lec 15: Cloud Web Services

- Cloud/REST architectures

  - Pre-cursors
    - Grid computing: super-computers; lots of computers put together; work on parallel tasks

  - Model for enable convenient, on-demand network access to a shared pool of configurable computing resources (networks, servers, storage, applications, services) that can be rapidly provisioned and released with minimal management effort or service provider interaction

  - Promotes availability

  - 5 essential characteristics (OBRRM)

    - On-demand self service (clients can provision computing capabilities without human interaction)
    - Broad network access (capabilities available over network)
    - Resource pooling (computing resources are pooled to serve multiple consumers); location independence

    - Rapid elasticity (resourced can be added/removed easily)
    - Measured service (metering; storage; bandwidth)
    - Benefits?
      - Agility; scalability; cost; reliability; security

  - 3 service models (___ as a service)
    - Infrastructure (IaaS) vendor provides resources
    - Platform (PaaS) vendor controls the environment
    - Software (Saas) vendor controls the application
  - 4 deployment models



## Lec 17: Code Quality

```
class A only has an add(T t) function
A<Object> a
A<String> b

a = b; // not valid, you have to pointers to the same object using two different types. so they both can access and get and add to it. but b things its just using strings, whereas that is not the case

class B only has a T get() function

// PRODUCER
foo(B <? <Number> b) // can pass in Number or anything that is a sub-typew of Number
// can pass in Integer
// cannot pass in Ojbect

// CONSUMER
bar(A <? super Number> a) // can pass in another that is Number of higher than number
// cannot pass in Integer
// can pass in Object
```

- Liksov Substitution Principle
  - Subtypes should behave as their parent types
  - If B inherits from A, and A has a method foo, then be must have foo but can change things a bit
  - B can RETURN more specific, and B's params can ACCEPT more general

## Lecture 18: Optional Type Systems & Checker Framework

- Lots of errors that we miss
  - Not sanitizing user input
  - Insert object into hash map; then change that object; then say map.get(my object). Object has changed so its hash value has changed, too. So this is an error
- Never directly use user input to perform some type of important action or system call -> always check it and sanitize it first to make sure it's good
  - Ex: SQL injection attack
- But given code that uses tainted input, we can't directly tell where the defect/error is; it depends on our specification
- So we use annotations like `@Untainted | @Tainted` to help with the specification
- `@Immutable`
- Benefits:
  - Improves documentation; helps you find bugs
  - Aids compilers
- Negatives:
  - False Positive: an error message that points to something that isn't actually an error
  - Checker itself might contain an error
    - Theoretical: this is how the type system works
    - Implementation problem
- `@Regex(1) foo = "..."` typechecks that foo has at least ? Or only ? 1 group

## Lecture 19: Building a Type System

- Java 8: `Optional<T>` type
  - `bool isPresent()`
  - `T get()`

- Defining a type system

  1. Type hierarchy (sub-typing)

     - Type = type_qualifier + java basetype
     - `@NonNull String foo;`
     - `@Nullable <-- @NonNull`
     - @Nullable is the DEFAULT

  2. Type rules (what operations are illegal)

     ```
     // @MaybePresent <--- @Present
     // for optionals
     @Present Optional<String> foo;
     <------> 									type qualifier
     				<---------------> java basetype
     <---------------------> 	type
     class Optional<T> {
     	T get(@Present Optional<T> this)
     }
     ```

  3. Type introduction (what types for literals)

  4. Dataflow (run time tests)

  ```
  if (x != null) {
  	// x is @NonNull here
  	...
  }
  // x is @Nullable again here
  
  @Nullable Foo f;
  f = new Foo()
  // f is @NonNull here
  f = unknownVal
  // f is @Nullable here
  ```


## Lecture 21:

- @NonNull: will never be null

- @Nullable: might be null

- @SideEffectFree: does not modify externally visible state // not checked, this is trusted

- @Deterministic: if called with args, then it returns the same args (according to == comparator)

- @Pure: both side-effect-free and deterministic

- @MonotonicNonNull: might be null or might be non-null. May only be (re-)assigned a non-null value. So you can avoid re-checking. Once non-null, always non-null

- @RequiresNonNull: lets you state non-null-requirements for values inside an object

  ```
  class MyClass {
  	String other
  }
  @RequiresNonNull({ "other.foo" })
  void method(@NonNull MyClass other) {
  	...
  	other.foo.toString()
  	...
  }
  ```

- @EnsuresNonNull

- @EnsuresNonNullIf

- @PolyNull: creates two output instances for every 1 usage instance. One with it replaced by @Null and another with it replaced by @NonNUll

  ```
  @PolyNull String foo(@PolyNull String a) ...
  	if a is null return a
  	else return a.foo
  creates
  
  @Null String foo(@Null String a)
  and
  @NonNull String foo(@NonNull String a)
  ```

  

## Chapter 23: Symbolic Execution

- Fuzzing
  - Feed random inputs in and check for abnormal behaviour 
  - American Fuzzy Lop (AFL)
  - libFuzzer
- Symbolic Execution:
  - Automatically explore program paths
  - Execute program on symbolic input values
  - Fork execution at each branch
  - Generates test cases
  - Used to reason about all inputs that take the same path through a program; uses a symbolic value for the input variable; then collects symbolic path conditions to help you determine what are good input values that will test all parts of the code
  - Symbolic State is a pair S = (Env, PC) where
    - Env: L->E is a mapping / environment, from program variables to symbolic expressions
    - PC is a first-order-logic formula called a path condition
  - difficulties
    - Loops, complex flow, procedures
    - Environment: pointers, data structures, files, databases, threads, etc
- EXE ALgorithm
  - Same as symbolic execution but now for a given path condition you give the symbolic state of each input variable S(x) and the concrete state of all local and input variables C(x); that way by the end you know exactly what concrete states work (need to be tested)













***

## Readings

#### 4 + 1 View Model Architecture

- Logical view (classes)
  - Functional requirements; UML diagrams; ER diagrams
- Process view (concurrency and synchronization)
  - Groups and networks of different processes
  - Some NFPs (performance, availability)
- Physical view (software -> hardware)
  - NFPs: reliability; scalability; performance 
  - Map needs onto software nodes (servers, processors); development vs production 
  - 1
- Development view 
  - Decomposed into different libraries and modules; those are decomposed into different layers
  - Team organization; cost planning; portability; imports / exports
- Use cases / scenarios view



#### Design patterns vs arch patterns

- How the major parts of the system fit together; how messages and data flow through it;
- Scaffolding; framework upon which everything rests