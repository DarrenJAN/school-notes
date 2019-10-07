# CS 348: Intro to Databases

- David Toman; office hours Wed 4-5pm DC 3344
- Key Parts
    - Why do we use databases?
    - How do we use a Database Management System?
    - How do we design a database?



## Lec 01: Intro to Database Management (Sept 5)

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



## Lec 02: The Relational Model (Sept 10, 12, 17)

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

  - Universe: a set of values **D** with equality (=)
  - Relation: 
    - a predicate name R, and arity k of R (the number of columns) (==schema, table declaration==)
    - Instance: a relation R contained in $D^k$
  - Database 
    - Signature: finite set $p$ of predicate names (symbols, table names)
    - Instance: **R_i** for each R_i // yes, boldness matters
  - Notation:
    - Signature $p = (R1, R2, \dots, Rn)$
    - ==Instance **DB = (D, =, R_1, ..., R_n)**==
    - ==Clarification: R1 is the TABLE R1, **R1** is the entire instance inside of R1==

- Example of relational Databases: Bibliography

- >  What is the difference between a relation name and a relation instance?
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

- ==First-Order Variables and Valuations==

  -  ==Valuation function is a function $\theta$== that maps variable names to values in the universe $\theta: \{ x1, x2 \rightarrow \textbf{D}\}$
  - $a \models  b$ means that a entails b (a being true makes b be true)
  - $\theta \models R(x1,...,xk)$ means that the tuple (x1, ..., xk) exists in some table with name R??
    - is (a, b, c) record in table R -> maybe it means a partial record, too?

- Example:

  - find pair of employees working for the same boss

- Equivalences TODO

- Relational Calculus TODO

- Free Variables TODO

  - basically just the variables used in a eqn / formula / query

### Sample Queries

<img src="./images/cs348-1.png" alt="alt text" style="zoom:30%;" />

- list all composite numbers (you have an addition and multiplication table)
  - Composite: all non-prime numbers
  - need to find two factors (none of them 1) that multiply together to create the number x
  - $composite = \{ x | \exists y,z | TIMES(y,z,x) \and \neg(x = y) \and \neg(x = z)\}$
  - can't use the constant 1 (don't have that symbol available)
- list of all prime numbers
  - $\{ x | \neg(composite(x))\}$
- list all publications (all their IDs): $\{ x | \exists y | PUBLICATION(x,y) \}$
- titles of all books: 
  - $\{ x | \exists y,z,w | PUBLICATION(y,x) and BOOK(y,z,w)\}$
  -  x is title from PUBLICATION, and book and publication both share same id y
- all publications without an author:
  - $\{ x : (\exists y : PUBLICATION(x,y)) \and \neg(\exists z,w : WROTE(z,x) \and AUTHOR(z,w))\}$
  - Where: y is the publication title, z is author that wrote the publication, w is the name of author
  - Don't actually need the AUTHOR part; if there's no WROTE record relating an author to a publication, then clearly no author write that publication
- All pairs of coauthor names
  - $\{ (x,y) : (\exists w,z : AUTH(w,x) \and AUTH(z,y) \and \neg(w = z)) \and (\exists v,t : PUBL(v,t) \and WROTE(w,v) \and WROTE(z,v)\}$
  - W is the authID of name x
  - Y is the authID of name y
- Titles of publications written by a single author
  - $\{ x : (\exists y : PUBL(y,x)) \and (\exists z : WROTE(z,y)) \and (\forall z' : WROTE(z',y) \implies z'=z)\}$
  - Recall: can't use $\forall$, need to convert it to an $\exists$ 
  - $\neg(\exists z' : \neg(WROTE(z',y) \implies z'=z))$
  - $\neg(\exists z' : (WROTE(z',y) \and \neg(z'=z))$ // should convert implication to logical equivalent instead
  - ==Recall: $\forall x : \beta$ is the same as $\neg (\exists x : \neg \Beta)$==
  - x is publication name
  - Y is publication ID
  - Skip the author table because if z' exists in WROTE then

- Asking Questions and Understanding Answers
  - Find the neutral element of addition: just do PLUS(x,x,x) don't need to do exists y such that PLUS(y,x,x) and PLUS(x,y,x) because addition is COMMUTATIVE and those two are the same

- Laws (aka Integrity Constraints)
  - Addition is commutative
  - Addition is a total function

### Integrity Constraints

- In addition to having the names of our tables, and structure of our tuples, we also have some restrictions on what combinations of value can appear in some instances (because they have to obey our laws)
- For example, in PLUS if you find 0,1,1 then this guarantees that you will find 1,0,1
- Yes/no conditions that must be true in every valid database instance
- Examples
  - Every boss is an employee
    - $\{ \forall x,y,z : EMP(x,y,z) \implies \exists u,w : EMP(z,u,w)\}$
    - For all bosses z this implies there exists other roles (u,w) such that boss z is also an employee
  - Every boss manages a unique department
- Relational signature captures only the structure of relations
  - Valid database instances satisfy additional integrity constraints
    - For all x,y then PUBLICATION(y,x) implies STRING(x)
  - Values of attributes are unique among tuples in a relation (**keys**)
  - Values appearing in one relation must also appear in another relation (ex: bosses must be employees) (**referential integrity**)
  - Values cannot appear simultaneously in certain relations (**disjointness**) (ex: author IDs do not look like publication IDs, so if you see an authorID x then you know that x is not an ID in the publication table)
  - Values in a certain relation must appear in at least one of another set of relations (**coverage**)
- More examples to do (exercise)
  - TODO...
- Views and Integrity Constraints
  - Idea: answers to queries can be used to define derived relations (views); an extension of a DB schema
  - **[UNDERSTAND THIS BETTER]**
- Database Instances and Integrity Constraints
  - Relational database schema:
  - Relational database instance **DB**:

### Safety and Finiteness

- Important: database instances must be **finite**
- Unsafe Queries
  - All y that are not the names of authors (so all strings in your database that aren't names of authors)
  - All x,y,z such that BOOK(x,y,z) or PROCEEDINGS(x,y)
    - Image you have no books and 1 proceedings, then z can be ANY value (infinite results)
  - All x,y such that x = y (returns everything)
  - Domain-Independent Query:
    - The above unsafe queries will end up using all or most of the domain
    - ==If you fix the instance (contents) of the DB, but you fiddle with the domain (currently just English strings, next you put all Chinese strings into it too) then the query better return the SAME answer==
    - TODO write equations
    - Recall: Domain **D1** all of the information (**single values**) (SUE, BOB, ..., ...)
    - All the contents of **R1** must also be in **D1** and **D2**...
    - Unary relations (just 1 value) are a subset of a domain
    - Can only query the intersection of those two domains
  - Domain-Independent Theorem: answers to domain-independent queries contain only values that exist in **R1, R2,...,Rk** (the active domain)
  - active domain: the domain you construct just from the DATA (finite file) I've told you about
  - ==Domain-Independent and finite database implies safe==
- Safety and Query Satisfiability
  - theorem: satisfiability of first-order formulas is undecidable
    - co-r.e. in general
    - r.e. for finite databases
  - theorem: domain-independence of first-order queries is undecidable
    - you cannot write a program to answer "is this db domain-independent?"
- Range-Restricted Queries
  - see defn slide 38
  - want to ensure finite input ==> finite output
  - conjunction: answer to p1 is finite, p2 is finite, so intersection of them is finite
  - equality with conjunction: if one of the equality variables is in p1 then the output is finite
  - existential:
  - disjunction: we require that the free variables (FV) or p1 = the FV(p2); solves the problem of having ANY value fill one of the variable values
  - negations must be stuck in a conjunction: p1 and not(p2); FV(p2) must be a subset of the FV(p1)
  - NOTE: 3 types of conjunction (regular, with equality, with negation)
  - ==Theorem: range-restricted ==> domain-independent==
- What have we lost?
  - (P(x,y) AND Q1(x)) OR (P(x,y) and Q2(y)) (GOOD)
  - <==>
  - P(x,y) AND (Q1(x) OR Q2(y)) // BAD, FV(Q1(x)) = x is not contained in FV(Q2(y)) = y
  - **Look at Q1(x) OR Q2(y)**
    - one way to solve this is (Q1(x) AND ActiveDomain(y)) or (Q2(y) and ActiveDomain(y))
    - ANDing x with the activeDomain(y) (all the possible values of y) restrict the values of x

- computational properties
  - evaluation of every query terminates
  - data complexity:
    - in the size of the database and for a fixed query
    - runs in polynomial time
    - using log space (for every query, you don't need to make copies of the database every, just needs a bunch of pointers to the db)
    - AC_0 (constant time if CPUs running in parallel)
  - combined complexity
    - polynomial space
- Query Evaluation vs Theorem Proving
  - theorem proving: proving for all queries
  - query evaluation: easier, just for this one query
- Query Equivalence and DB Schema
  - TODO
- What Queries cannot be expressed in RC?
  - built-in operations (ordering, arithmetic, string stuff)
  - counting
  - reachability/connectivity
    - much harder



## Lec 03: SQL (Sept 17, 19, 24)

- good: conjunctive queries, set operations

- bad: multiset semantics; null values

- ugly: committee design standards 

- major parts
  - DML (Data Manipulation Language): query and update
    - also, embedded SQL (JDBC); useful for application development
    - help reduce injection attacks (SQL statements compiled ahead of time, can't send strings to attack)
  - DDL (Data Definition Language): define schema for relations; create objects
  - DCL (Data Control Language): access control
  
- Data Types
  
  - varchar(n) variable length string (at most n)
  
- Example schema: `AUTHOR(aud integer, name char(20))`

- `SELET DISTINCT <results> FROM <tables> WHERE <condition>`

- Variables vs Attributes

  - usually have many columns; in relational calculus you would need to use lots of variable names
  - corelations in SQL: for every table you only need to invent variable names for what you want, SQL will generate rest of variables for you
  - `{ (p.AID, .NAME : AUTHOR(p.AID, p.NAME\}​`

- Example: List all publications with at least two authors

  ```sql
  select distinct r1.publication
  from wrote [as] rl, wrote r2 // table name and correlation variable
  where r1.publication = r2.publication
  and r1.author != r2.author
  ```

- Example: List titles of all books

  ```sql
  select distinct title
  from publication, book // did not declare correlation variable
  											 // used table name as corr. var. itself
  where publication.pubid = book.pubid
  ```

- **FROM Clause (summary)**

  - `FROM R1 [[as] n1], ..., Rk[[as] nk]`
  - ==where `Ri` is the table, [] means optional, so you don't need `ni` unless you have duplicate table names, and the [as] is optional, too==
  - ==clause represents the CONJUNCTION of R1 AND ... AND Rk==
  - all ni need to be distinct

- **SELECT Clause (summary)**

  - `SELECT DISTINCT e1[[AS] n1], ..., ek [[AS] nk]`
  - you can give names ni to the attributes in the answer
  - e1 is the variable name, n1 is the new name we have given it to use in our output

- Example: for every article list the number of pages

  ```
  select distinct pubid as id, 
  			 endpage-startpage+1 as numpages
  	from article
  ```

- **WHERE Clause**

  - additional conditions on tuples that qualify for the answer
    - standard atomic conditions =, !=, <. <=, ...
    - **conditionals may involve expressions**
      - similar conditions as in the SELECT clause

- Example: find all journals printed since 1997

  ```
  select * from journal where year >= 1997
  ```

- Boolean Connectives: AND, OR, NOT

- Complex Queries in SQL:

  - so far we can write only EXISTS, AND queries
  - remaining:
    - OR, NOT (expressed using SET operations)
    - forall: re-write using negation and EXISTS; same for `==>` and `<==>`

- **Set Operations**

  - note: answers to select blocks are relations (sets of tuples); we an apply set operations on them
  - union: Q1 UNION Q1
  - difference: Q1 EXCEPT Q2 (everything in q1 that's not in q2)
    - ==p1 and not(p2) ==> we said earlier that the FV(p1) must be a subset of FV(p2)==
    - so, we just change it to p1 and not(p1 and p2)
    - now it meets the requirement above
  - intersection: Q1 INTERSECT Q2
  - ==Q1, Q2 must have union-compatible signatures; same number and types of attributes==
    - this means that the free variables are the same on both sides
    - but it's flexible; can do 1 column of smallint with 1 column of largeint

- Example: Union >> List of all publication ids for books or journals

  ```sql
  (select distinct pubid from book)
  union
  (select distinct public from journal);
  ```

- Nesting of Queries

  - we can use select blocks as arguments of set operations
  - but what if we need to use a set operation inside of a select block?

- Solution: Naming Sub-Queries

  - queries denote relations
  - we provide a naming mechanism that allows us to assign names to results of queries; can be used later in place of base relations

  ```
  with foo1 [opt schema 1] AS <query 1 goes here>
  	...
  	foon [opt schema n] AS <query n goes here>
  	...
  	query that uses foo1 and foon as table names
  ```

- example: list of all publication titles for books or journals

  ```
  with bookorjournal(publid) as
  	(
  		(select distinct pubid from book)
  		UNION
  		(select distinct pubid from journal)
  	)
  select distinct title
  from publication, bookorjournal
  where publication.pubid = bookorjournal.pubid
  ```

- can also do it inline!

  ```
  select distinct title
  from publication,
  	(
  		(select distinct pubid from book)
  		UNION
  		(select distinct pubid from journal)
  	) as bj
  where publication.pubid = bj.pubid
  ```

- can't we just use OR instead of UNION?

  ```
  select distinct title
  from publication, book, journal
  where pub.id = book.id
  or pub.id = journal.id // what if no journals?
  ```

  - if journals is empty, then FROM PUBL, BOOK, JOURNAL will return empty set, because FROM is a CONJUNCTION
  
- Summary of First-Order SQL

  - captures all of relational calculus (RC)
  - shortcomings
    - some queries are had to write
    - no counting
    - no path in graph (recursion)

### Syntactic Sugar

- WHERE subqueries

  - so far, WHERE clause cannot introduce new variables (must use variables from FROM tables)
  - what kind of yes/no questions can you ask about sets?
    - is it empty or not empty?
    - does it contain some element?
  - EX: select distinct title from publication where pubid in (select pubid from article)

- Parametric Subqueries

  - before: all variables in WHERE clauses come from the FROM tables
  - you can communicate variables as params into those where clauses

- Examples

  ```
  select *
  from wrote r
  where exists (
  	select * from wrote as s
  	where r.publication = s.publication
  	and r.author <> s.author // <> == != 
  // same publication, diff author, therefore it was at least 2 authors
  )
  ```

- more levels of nesting

  - recall: these subqueries in the where clause are just yes, no queries with params; we cannot use those values as output in our outer SELECT statement; we can just use them to compare

- Ex: all authors who always publish with someone else (the same person)
- Summary of WHERE clause
  
  - slide 50

### Updates

- tables are large but updates are small ==> incremental updates
- commands: INSERT, DELETE, UPDATE
- INSERT
  - `INSERT INTO table_name[attr1, ..., attrk] VALUES (vl,...,vk)`
  - recall that [...] means its optional
  - `INSERT into table_name(Q)` where Q is some query
- DELETE
  - `DELETE FROM r WHERE condition`; deletes all tuples that match condition
  - deletion using cursors (later) ==> only way to delete one out of two duplicate tuples
- UPDATE
  - `UPDATE r SET <update statement> WHERE <condition>`



> Recall: no values from WHERE clause can "escape" and be used in output



- Support for Transactions
  - transactions starts with first access of the database until it sees **COMMIT** (make changes permanent) or **ROLLBACK** (discard changes)
  - read SQL specification: there are certain commands that start a transaction (select, etc) but there are some that do NOT start a transaction
  - NOTE: DB2 has an AUTO COMMIT feature turned on automatically

### Aggregates

- Aggregate (column) functions are introduced
  - Find number of tuples, find min or max values of an attribute, add values of an attribute (over the whole relation)
  - canNOT be expressed in relational calculus
  - Can apply to pieces of the tables (groups of tuples)
  - ==Syntax looks similar, but it is totally different==
  - ==All non-aggregate variables (x1,..., xk) that appear in the SELECT clause must also appear in the GROUP BY clause==

```
SELECT x1,...,xk, agg1, ..., aggn
FROM Q
GROUP BY x1,..., xk
```

- example

```
for each publication coun the number of authors
select publication, count(author)
from wrote
group by publication

// here, publication is NOT part of the aggregation so it must appear in the group by

// what about publications with 0 authors?

SOLUTION:

another select clause (unioned) to get all publications that don't have authors
```

```
SELECT author, sum(e - s + 1) as pages
FROM (
	SELECT w.author, startpage as s, endpage as e
	FROM wrote as w, article as a
	WHERE pubid = publication // ==== a.pubid = w.publication
)
GROUP BY author
// HAVING pages >= 70

what if we want only pages >= 70?

do this

SELECT * FROM (
  SELECT author, sum(e - s + 1) as pages
  FROM (
    SELECT w.author, startpage as s, endpage as e
    FROM wrote as w, article as a
    WHERE pubid = publication // ==== a.pubid = w.publication
  ) as t1
  GROUP BY author
) as t2
WHERE pages >= 70

but this is stupid and silly.

use HABING
```



- HAVING clause
  - WHERE can't impose conditions on values of aggregates

```
all publications with 1 author
select publication, count(author)
from wrote
group by publication
having count(author) = 1
```





## Lec 04: SQL 2 (Ordering, Duplicates, NULL) (Sept 26)

### Ordering

- Ordering results

  - Can't assume any order of rows in tables
  - Can't assume any order of intermediate result in query
  - But you can use ORDER BY
  - `ORDER BY e1 [dir1], ..., ek [dirk]`, `diri = ASC | DESC, ASC default`

  - ==ORDER BY, needs to be at end==

- Aside

  - LIMIT 1 is bad -> allowed to return an arbitrary row each time
  - TOP 1 is bad -> if there are multiple tops, allowed to return arbitrary one each time

### Duplicate Semantics

- Multisets and Duplicates

  - SQL lets you use a MULTISET/BAG semantics instead of SET semantics
  - Bolt, bolt, bolt, bolt, nut, nut =====> bolt (count 4), nut (count 2)

- Range-Restricted Queries for Multisets

  - Using DISTINCT
  - A finite valuation (tuple) can appear k times (k > 0) as a query answer
  - **DB**, theta, k entails phi reads "finite evaluation theta appears k times in phi's answer"
  - Slide 9 ???

  ```
  T(x,y)
  1 2
  1 2
  1 3
  1 4
  ====
  T(x,y)
  1 2 (2)
  1 3 (1)
  1 4 (1)
  
  All x such that there exists a y such that T(x,y)
  returns 1, 4 times
  ```

  - ==IMPORTANT m - n entails phi - v; m entails phi, n entails v AND m > n==
  - ==What is going on?==

- Ex: One problem with duplicates

  - (A(x) and B(x)) or C(x) === (A(x) or C(x)) AND (B(x) or C(x))
  - Assume all A,B,C contains a (but in different quantities) (n, m, l respectively)
  - LHS Result is (n m) + 1 // conjunction is product, disjunction is plus
  - RHS Result is (n + l)(m + l) THESE ARE NOT EQUIVALENT

- SELECT DISTINCT 

  - Duplicate elimination operator

- Bag Operations

  - UNION ALL
  - EXCEPT ALL
  - INTERSECT ALL

- EX

  ```
  select r.g
  from s
  where r.a in ( select b from s)
  
  different than
  select r.b
  from r, s
  where r.a = s.b
  
  // if s contains duplicate b values, then for every r.a values equal to that b value then it will return ALL those duplicates from s
  ```



### NULL values

- What is a null value?

  ```
  PHONE
  NAME OFFICE HOME
  john 123    ?
  ```

  - Does John have a phone? Or we do we just not know John's phone number?
  - "Value inapplicable" --> this is what most NULLs mean
    - Bad schema design; should instead separate into 2 tables for Name->OFFICE  and Name -> HOME, and then John would not have an entry in the HOME
    - Why don't we do this? Efficiency
  - "Value unknown" 
    - John does have some home number, but we don't know what it is
    - Does John have a home phone? Yes
    - Can you give me John's home phone? No
    - Many possibilities / possible worlds
    - Solution: use certain answers such that the answer is the same in all worlds

- What to do with NULLs in SQL?

  - Nodes, edges. Is there an edge such that those 2 nodes have same colours?
  - If node colours are all ?, then it gets crazy hard to figure out does there exist a colour evaluation for each ?1, ?2, ?3, ... that satisfies the predicate?
  - table without nulls; is this graph 3 colour? just search for an edge where the edge point colours are the same
  - but what if we allow null? then it has to guess how to assign the colours to the nodes. if for all possible cases you find a bad edge, then return NO. else, return YES. (this is all for null unknown). If we can solve this, then we can solve an intractable problem, best algorithms are exponential. ==so NPH. so no SQL implements value unknown for nulls==
  
- More to do with NULL in SQL

  - general rule: NULL as a parameter to an operation makes the result NULL
  - set operations
    - unique special value for duplicates, just ONE NULL value (not different from each other)
  - aggregate operations
    - doesn't count;
  - predicates / comparisons
    - **Three-valued logic**

- Comparison Revisited 

  - 1 == NULL ? Uknown ? UNKNOWN is the 3rd truth value (TRUE, FALSE, UNKNOWN)

<img src="./images/cs348-2.png" alt="alt text" style="zoom:100%;" />

- UNKNOWN in WHERE clause

  - WHERE `<cond>` IS TRUE | IS FALSE | IS UNKNOWN

- Counting

  - if some table has a row with a URL as null, then count(*) vs count (URL) can return 3, 2

- ex:

  ```
  select aid, publication
  from author left join wrote
  									on aid=author
  									
  // lenient to authors
  
  vs
  
  UNION ALL
  	SELECT aid, 0 (the number zero)
  	FROM author WHERE aid not in (select author from wrote)
  ```

- OUTER JOIN
  - allow null-padded answers that fail to satisfy a conjuct in a conjunction
  - `FROM R <j-type> JOIN S ON C`
  - j-type is FULL, LEFT, RIGHT, or INNER



## Lec 05: Application Programming

- don't want to write SQL; you want some better programming langauge to interact and connect for you
- write SQL in a string? only find out errors at run time
  - watch out for sql injection attacks

### Embedded SQL (in C)

- considerations
  - how can SQL be parameterized?
    - how to pass params, how to get results, errors?

- declarations

  - `EXEC SQL INCLUDE SQLCA`;
  - `EXEC SQL <sql statement>`;

- HOST variable

  - how do we communicate values between C and SQL?
  - identifiers (in C, variables; in SQL, params)

  ```
  EXEC SQL BEGIN DECLARE SECTION;
  declarations of variables to be used
  in SQL statements go here
  EXEC SQL END DECLARE SECTION;
  ```

  - put a : in front of a host variable to use it inside of a query

- ERRORS

  - check sqlcode, or use exception handling with go to's

- Prepare you application

  - pre process compile ..
  - use the makefile from the website

- Real SQL Statements

- NULLS ?

  - use INDICATOR variable

- Dealing with multiple return results (Impedance mismatch)

  - basically like reading one by one from a file (**CURSOR**)
  - CURSOR holds the result of a query as if it was a file
  - `EXEC SQL FETCH AUTHOR INTO :name, title, ...;`
    - Supposed to be more :'s there according to the standard, but there is a bug in the preprocessor and so that will not compile
    - Just takes one colon; everything after that is a host variable. Against the standard, but that's life

### Stored Procedures

- A stored procedure executes application logic directly inside the DBMS process
- Don't want to be transferring large amount of data back and forth (between application and the server)
- Part of your application that runs ON the server
- Advantages of stored procedures
  - Minimize data transfer costs
  - Centralize application code
  - Logical independence



## Lec 06: Dynamic SQL

- Overview:

  - Execute a string as a SQL statement
  - How do we know if a string is a valid statement? 
  - How do we execute?

- PREPARE

  - `EXEC SQL PREPARE stmt from :string`
  - Stmt is NOT a host variable; it's an identified the statement used by the preprocessor

- Using Parameters?

  - Use the parameter marker "?" In the string
  - then

  ```
  EXEC SQL EXECUTE stmt
  	USING :var1 [, ..., :vark];
  	// colons before the param names
  ```

- Unknown number / types of variables?
  
  - Use a dynamic descriptor area
- Putting it all together: `adhoc.sqc`
  - Start up and prepare the statement
  - init_da sets the `i` value; if it is greater than 0 then it is a query
  - Else, if it is 0 then it's just a normal statement (not a query) and you can execute it (since it has no return values, not a special case)



## Lec 07: ODBC

- Want to talk to other databases? Tell your favourite database about the other ones, let it handle talking to the other ones
- That's the old way
- ODBC let's you talk to multiple databases
- 3 fundamental objects
  - environments
  - connections
  - Statements 
- Have to set up a lot more stuff yourself





## Assignment Talk

### A1

- "course with department D"
  - convoluted rule through PROFs ? (TERRIBLE)
  - missing "dept" in course? if cnum is CS247 then dept is CS; just use SUBSTR --> state your assumptions (YUP)
    - use SUBSTR(X,Y,"CS") pretend there's a substring table
  - put 2 attributes as the key for course, the course number and the depart; lots of repercussions  (need to apply to rest of diagram) (NOT GREAT)
- q3: "among" means for ALL the students that have achieved the highest grade (maybe multiple)
- q6: the max and min for all classes for a given term where that class is in a course taught by a CS or CO professor; all courses in terms
- q10: take all pairs of professors

### A2

- `db2 -f name_of_file`

