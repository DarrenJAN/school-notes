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



## Let 03: Views and Non-Functional Properties

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
      - 



## Readings

###[What should a Software Engineering Course look like?](http://tomasp.net/blog/2019/software-engineering/)

- SE has become a course trying to cover whatever methods and tools the industry needs; not practical (keep changing)
- what about SE will still be relevant in 100 years?
- what are the mortifications that led to ideas like waterfall and agile? (agile = need for more rapid response)
- what was the context in which UML appeared? Metaphor or an architect who designs a master rplan for a system
- believes that universities should teach historically situated software engineering as it provides a framework for critical thinking about software engineering, and past examples to guide this thinking



