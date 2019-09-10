# CS 348: Intro to Databases

- David Toman; office hours Wed 4-5pm DC 3344
- Key Parts
    - Why do we use databases?
    - How do we use a Database Management System?
    - How do we design a database?

## Intro to Database Management (Sept 5)

- persistent data: information is stored long-term, persists after power is turned off
- early data management: lots of redundancy, duplication
- file processing
    - data stored in files located on disk drives with a file system interface that supports various access methods
    - one file used by many programs
- database approach: another layer of abstraction; records of data; programmers don't need to worry about DB implementation; just program to an interface
- what is a database?
    - a large and persistent collection of data and metadata organized in a way that allows efficient retrieval and revision
    - data: John's age is 43
    - metadata: there is a concept of an employee that has a name and an age
- what is a data model?
    - determines the nature of the metadata and how retrieval and revision is expressed
    - defn: Database Management Systems (DBMS): a program (or set of programs) that implement a data model
    - ex: db = file cabinet; data model = folders and papers; dbms = system that determines how data is inserted 
- Database Management System (DBMS)
    - ==main idea==: abstract common functions and create a defined interface
    - supports an underlying data model (all data stored and manipulated in a defined way)
    - access control (data can only be accessed or changed by authorized people)
    - concurrency control (multiple concurrent applications can access data)
    - database recovery (reliability; backups done in organized way)
    - database maintenance (revising metadata)
- Schema and Instance
    - defn: Schema: collection of metadata conforming to an underlying data model
    - defn: Instance: collection of defined data (records) conforming to the schema
- History
    - Hierarchical data model: only allows 1:N parent-child relationships (tree)
    - Object-Oriented DBMSs
        - you can create an object, and it can be persisted (entire object stored on DB)
        - supports inheritence
- Three Level Schema Architecture
    - external schema: what the application programs and users see
    - conceptual schema: description of the logical structure of ALL data in the db; all the rules ==(Abstract Data Type; the interface)==
    - physical schema: description of physical aspects (how data is mapped to data-structures); storage algorithms
- Data Independence
    - idea: TODO
    - physical: TODO
    - logical: modularity; WAREHOUSE table cannot be accessed from payroll app; EMPLOYEE table cannot be accessed from inventory app
- Transactions
    - when multiple apps access same data, bad things can happen
    - idea: every app thinks it is the sole app accessing the data; DBMS should guarantee correct execution
    - defn: transaction: an application-specific atomic and durable unit of work
    - properties (ACID)
        - Atomic: TODO
        - Consistency:
        - Isolated:
        - Durable:
- Interfacing to the DBMS
    - Data Definition Language (DDL): for specifying schemas
        - may have different DDLs for external schema, conceptual schema, physical schema
        - make tables, what attributes do they have?
    - Data Manipulation Language (DML): for specifying retrieval and revision requests
        - what are the contents of the tables? what records exist? question the instances
        - navigational (procedural)
        - non-navigational (declarative)
- Types of DB Users
    - end user:
        - access to db indirectly; uses query-generating applications or generates ad-hoc queries using the DML
    - app developer:
        - designs and implements app that access the db
    - database administrators (DBA)
        - manage schema; defines internal schema; loads and re-formats db; responsible for security and reliability



## The Relational Model (Sept 10)

- How to ask the right questions?
  - Ex: Find all pairs of natural numbers that add to 5 --> {(x,y) | x >= 0 and y >= 0 and x + y = 5}
    - you can just have an edition table of x, y, and their sum; don't need to know anything about addition
  - Ex: Find all employees who work for Bob? 
    - again, have another table from employee name -> Boss
    - EMP(name, dept, boss)
    - query: {(x,y) | EMP(x,y,Bob)}
  - Ex: Find pairs of employees working for the same boss!
    - query: $\{(x1, x2) | \exists y1, y2, z | EMP(x1,y1,z) \and EMP(x2,y2,z)\}$

- The relational model

  - all information is organized in (a finite number of) relations
  - features:
    - simple and clean data model
    - powerful, declarative query languages

- Relational Databases

  - Universe: a set of values D with equality
  - Relation: 
    - a predicate name R, and arity k of R (the number of columns) (==schema, table declaration==)
    - Instance: a relation R contained in $D^k$
  - Database 
    - TODO
  - Notation:
    - Signature $p = (R1, R2, \dots, Rn)$
    - TODO

- Example of relational Databases: Bibliograph

- >  What is the diffferene between a relation name and a relation instance?
  - relation name is what do you call it (part of meta-data, part of schema)
  - Relation instance is the actual data

- Common Visualization for Relational Database Schemata

  - boxes for each schema; bold text title on top; identification fields (ex: pubid) separated from extra detail fields

  - also, these lines imply that whenever 1 pubid exists, then all of those different instances in those different tables for those different data types will exist, too

- Simple (Atomic) "Truth"

  - relationships between objects (tuples) that are present in an instance are true, relationships absent are false

- Query Conditions:

  - idea 1: use variables to generalize conditions
    - AUTHOR(x,y) will be true of any valuation where $\{x \rightarrow a, y \rightarrow b\}$ exactly when the pair (a,b) in **AUTHOR**
  - idea 2: build more complex conditions from simpler ones using 
    - conjunction (and), disjunction (or), negation
    - quantifiers: existential (there is), exists...

- Conditions in the Relational Calculus

- First-Order Variables and Valuations

  - Defn: Valuation is a function $\theta$ that maps variable names to values in the universe
  - $a \models  b$ means that a entails b (a being true makes b be true)
  - $\theta \models R(x1,...,xk)$ means that the tuple (x1, ..., xk) exists in some table with name R??
    - is (a, b, c) record in table R -> maybe it means a partial record, too?

- Example:

  - find pair of employees working for the same boss

- Relational Calculus

- Free Variables

- Sample Queries

  - list all composite numbers (you have an addition and multiplication table)
    - Composite: all non-prime numbers
    - need to find two factors (none of them 1) that multiply together to create the number x
    - $composite = \{ x | \exists y,z | TIMES(y,z,x) \and \neg(x = y) \and \neg(x = z)\}$
    - can't use the constant 1 (don't have that symbol available)

  - list of all prime numbers
    - $\{ x | \neg(composite(x))\}$
  - list all publications (all their IDs): $\{ x | \exists y | PUBLICATION(x,y) \}$
  - titles of all books: $\{ x | \exists y,z,w | PUBLICATION(y,x) and BOOK(y,z,w)\}$ // x is title from PUBLICATION, and book and publication both share same id y












