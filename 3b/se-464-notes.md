# SE 464
- Dr. Werner Ditelt (Deetle) wdietl@uwaterloo.ca
- Begin all email subjects with [SE 464]
- office hours by appointment
- tutorials: informal



## Week 1: Decomposition, Non-Functional Props, Architecture

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
  - defn: a building block tasked with effecting and regulating interactions among ocmponents
  - important because you have performance considerations and tradeoffs
  - examples
    - procedure call
    - shared memory (pointer)
    - message passing
- configurations
  - defn: also called a topology, is a set of associations between components and connectors
- deployment:
  - executable modules are physicalled placed on hardware devices on which they are to be run
  - ==TODO==

- essential difficulties (hard to overcome)
  - complexity
  - conformity: system is dependent on its environment
  - Intangibility: hard to manage or understand; not constrainted by physical laws

- attacks on complexity
  - high-level language
  - development tools and environments
  - frame the problem differently: have practical solutions to common problems



## Readings

###[What should a Software Engineering Course look like?](http://tomasp.net/blog/2019/software-engineering/)

- SE has become a course trying to cover whatever methods and tools the industry needs; not practical (keep changing)
- what about SE will still be relevant in 100 years?
- what are the mortifications that led to ideas like waterfall and agile? (agile = need for more rapid response)
- what was the context in which UML appeared? Metaphor or an architect who designs a master rplan for a system
- believes that universities should teach historically situated software engineering as it provides a framework for critical thinking about software engineering, and past examples to guide this thinking



