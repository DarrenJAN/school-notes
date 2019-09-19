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
- parts of a system that would be difficult to change once the system is built
- 3 dimensions:
  - structure (MVC?)
  - communication (how do components talk to one another)
  - nonfunctional requirements (performance, scalability; extensible, influence design choices)
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
  - Function requirements: define specific behaviour; what a system is supposed to **do**
  - Non-functional requirements: specify criteria that can be used to judge the operation of a system; how a system is supposed to **be**

- stakeholders in a system's architecture
  - engineers, architects, developers, etc
  - user: uses the product
  - customer: purchases the product
  - vendors: re-sellers of software; re-sell it with additional services or configurations

### Key Definitions

- what is principal?
  - principal implies a degree of importance that grants a design decision architectural status
  - definition depends on system goals
- prescriptive vs descriptive architecture
  - Prescriptive: captures the design decisions made prior to the system's construction; **as-intended**
  - Descriptive: describes how the sytem has been built; **as-implemented**
- architectural evolution
  - as a system evolves, ideally its prescriptive architecture is modified first
  - in practice, the descriptive architecture is modified directly
  - due to developer sloppiness, deadlines, lack of planning, need for optimizations
- architecture degradation
  - architecture drift: introduction of principal design designs into a system's descriptive architecture that are not included in the prescriptive architecture but do not violate any prescriptive design decisions
  - architecture erosion: into of design decisions that violate prescriptive plan
- architecture recovery
  - hard to recover what the nonfunctional requirements were from source code
- architecture elements
  - processing
  - data (information; state)
  - interaction
- components
  - elements that encapsulate processing and data
  - defn: a software components is an architectural entity that
    - encapsulates a subset of the systems functionality / data
    - restricts access using a defined interface
    - has explicitly-defined dependencies
    - usually are application-specific
- connectors
  - interaction might become more important and challenging than functionality of individual components
  - defn: a building block tasked with effecting and regulating interactions among components
  - important because you have performance considerations and tradeoffs
  - examples
    - procedure call
    - shared memory (pointer)
    - message passing
- configurations
  - defn: also called a topology, is a set of associations between components and connectors
- deployment
  - a software system cannot fulfill its purpose until it is deployed
  - executable modules are physicalled placed on hardware devices on which they are to be run
- possible assessment dimensions: available memory, power consumption, required network bandwidth
- essential difficulties (hard to overcome)
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

  - Efficiency 

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

  - Complexity

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

  - Scalability / Heterogeneity:

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

  - Evolvability

    - Ability to change to satisfy new requirements and environments
    - Components
      - 
    - Connectors
      - 
    - Topology



## Lec 04: Design Patterns

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
- 

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
abstract class AbstractFactory {
	abstract Shape getShape(String type)
}
class NormalShapeFactory extends AbstractFactory {
	Shape getShape(String type) {
		if type is SQAURE
			return new Square()
		if type is RECTANGLE
			return new Rectangle()
	}
}
class RoundedShapeFactory extends AbstractFactory {
	Shape getShape(String type) {
		if type is SQAURE
			return new RoundedSquare()
		if type is RECTANGLE
			return new RoundedRectangle()
	}
}
class FactoryProducer { // this is what you use to get your factory
	static AbstractFactory getFactory(boolean isRounded) {
		if is rounded return RountedShapeFactory()
		else return NormalShapeFactory()
	}
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

- instead of having a class and saying give me this class, you add fields and add methods to a data structure, and at some point you say "this is my object, it has everything I want" and from then on you just close that prototype



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

- separate the abstraction from the implementation
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
class Decorator extends Vehicle {
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





























***

## Readings

###[What should a Software Engineering Course look like?](http://tomasp.net/blog/2019/software-engineering/)

- SE has become a course trying to cover whatever methods and tools the industry needs; not practical (keep changing)
- what about SE will still be relevant in 100 years?
- what are the mortifications that led to ideas like waterfall and agile? (agile = need for more rapid response)
- what was the context in which UML appeared? Metaphor or an architect who designs a master rplan for a system
- believes that universities should teach historically situated software engineering as it provides a framework for critical thinking about software engineering, and past examples to guide this thinking



