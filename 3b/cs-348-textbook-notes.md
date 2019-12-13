# CS 348 Textbook Notes

## SQL Stuff

- Joins
  - Suppose we have tables STUDENT(id, name) and TAKES(student_id, course_id, semester, grade)
  - If we want to query all of the courses taken for each student, we might think that `SELECT * FROM STUDENT as s, TAKES as t WHERE s.id = t.student_id`
  - But what if a student has taken 0 courses? Then they wouldn't show up in the output
  - Types of joins
    - LEFT OUTER JOIN preserves tuples only in the relation named before (to left of) the left outer join operation
    - RIGHT OUTER JOIN same but for right
    - FULL OUTER JOIN preserves tuples in both relations
  - So `SELECT * FROM STUDENT LEFT OUTER JOIN TAKES ON student.ID = takes.studentID` will just have all empty take values as NULL in the output
- View
  - A virtual relation, defined by a query, that conceptually contains the result of the query (useful for showing a subset of a table, due to restrictions or access permissions)
    - Ex: a secretary might need to be able to lookup the room for any professor, but shouldn't see their salary. And, we don't want to create a new table for this subset of data, because then it might get out of date over time
  - `create view view_name as <query expression>`
  - Can store them in a materialized view (which allows for much faster querying); requires updating the content

## Embedded SQL Stuff

- CURSOR

  ```
  // declare 
  EXEC SQL DECLARE name CURSOR FOR <query>;
  
  // use it and iterate over it
  EXEC SQL OPEN name;
  for (;;) {
  	// setup host params
  	EXEC SQL FETCH name INTO <host variables>
  }
  ```

  

- Stored procedure: executes application logic directly inside the DBMS process

  - Minimize data transfer cost; have logical independence

## Dynamic SQL

- Pass parameters into SQL statements by leaving `?`s in the statement, and passing in params to them like

  - `EXEC SQL EXECUTE my_stmt USING :var1, ..., :vark`

- Get results into variables?

  ```
  EXEC SQL DECLARE cname CURSOR for stmt
  EXEC SQL OPEN cname USING :var1, ..., :vark
  EXEC SQL FETCH cname INTO :out1, ..., outn
  ```

- Unknown result types or amount? Use a DESCRIPTOR

## ODBC

- Open database connectivity; an interface built on library calls that 

## DB Design using ER Model (chapter 6)

- ER Data Model: designed to allow specification of enterprise schema that represents overall db structure
- DEFN entity: thing / object in the real world that is distinguishable; set of properties, unique key
- DEFN entity set: set of entities that share same properties (ex: instructors), usually referrign to the abstraction
- DEFN extension (of the entity set): actual values
- DEFN relationship: association among several entities (ex advisor: instructor <-> student)
- DEFN relationship instance: actual data of the relationship in the db

![Screen Shot 2019-10-29 at 10.53.44 AM](/Users/scottsandre/git/school-notes/3b/cs348-images/Screen Shot 2019-10-29 at 10.53.44 AM.png)

![Screen Shot 2019-10-29 at 10.56.04 AM](/Users/scottsandre/git/school-notes/3b/cs348-images/Screen Shot 2019-10-29 at 10.56.04 AM.png)

![Screen Shot 2019-10-29 at 10.56.47 AM](/Users/scottsandre/git/school-notes/3b/cs348-images/Screen Shot 2019-10-29 at 10.56.47 AM.png)

- cardinalities
  - One to one: `ENTITY_A <--- <relationship> ---> ENTITY_B`
  - One to many: draw an arrow TO the "one" side
    - Instructors can advise many students, students can have at most 1 advisor
    - `INNSTRUCTOR <--- <advisor> --- STUDENT`
- DEFN total participation: indicates that every entity in E must participate in at least one relationship in R; use double lines
  - Each student has at least one advisor
  - `INSTRUCTOR --- <advisor> === STUDENT`

![Screen Shot 2019-10-29 at 11.04.35 AM](/Users/scottsandre/git/school-notes/3b/cs348-images/Screen Shot 2019-10-29 at 11.04.35 AM.png)

![Screen Shot 2019-10-29 at 11.04.44 AM](/Users/scottsandre/git/school-notes/3b/cs348-images/Screen Shot 2019-10-29 at 11.04.44 AM.png)

- DEFN key: set of attributes for an entity that suffice to distinguish it from other entities

- Relationship set key:

  - If relationship $R$ involves entity sets $E_1$ to $E_k$ and R has attributes $a_1$ to $a_n$ then the individual relationship R consists of $\cup E_i \cup a_i$

- Existence Dependencies

  - If x is existence dependent on y then y is the dominant entity, x is a subordinate entity,

  - DEFN weak entity set: one whose existence is dependent on another entity set

    - Use the primary key of the identifying entity set to identify the weak entity set
    - Weak entity set is existence dependent on the identifying entity set

  - The connecting relationship is a double diamond; weak discriminator is dashed underlined

  - The weak discriminator is a set of attributes for the weak entity set; distinguish subordinate entities  of the set

  - Primary key of weak entity set? Discriminator + primary key of dominating entity

    ![Screen Shot 2019-10-30 at 2.51.12 PM](/Users/scottsandre/git/school-notes/3b/cs348-images/Screen Shot 2019-10-30 at 2.51.12 PM.png)

- Aggregation: make an entity out of a relationship; draw a box around the relationship and its related entities
- Specialization: use an inheritance triangle in the relationship arrow line

## ER to Relational Schema

- Entity set -> new table
- Attribute in entity set -> column in that table
- Relationship set -> new table column OR new table
- Strong Entity Set:
  - Strong entity set E with attributes ai gets mapped to table E with columns ai
- Weak Entity Set
  - Weak entity set E with parent set P with key p_key (could be multiple attributes) gets mapped to
    - table E with all attributes of E as column
    - Also all attributes from relationship
    - with p_key as columns too
- Aggregation: just make a table for the aggregated relationship/entities (primary key is the union of primary keys of all related entities)
- Specialization: treat the child as a weak set
- Generalization:



## Normal Forms

- DEFN: X -> Y means that X functionally determines Y, where X, Y are both subsets of attributes of a relation schema R

- DEFN: let $F$ be a set of functional dependencies. Then the closure of $F$, which is $F^+$ (but I might call it F' in the notes to keep it simple) is the set of all function dependencies that are logically implied by F
  
  - F = {A->B, B->C} then $F^+$ = {A->B, B->C, A->C}
  
- Rules

  - Reflectivity: $Y \subseteq X \implies X \rightarrow Y$
  - Augmentation: $X \rightarrow Y \implies XZ \rightarrow YZ$
  - Transitivity: $X \rightarrow Y, Y \rightarrow Z \implies X \rightarrow Z$
  - Union: $X \rightarrow Y, X \rightarrow Z \implies X \rightarrow YZ$
  - Decomposition: $X \rightarrow YZ \implies X \rightarrow Y$

- DEFN superkey: $K \subseteq R$ is a super key of R if K -> R holds // but, R is a super key, so we want to find the minimal super key

- DEFN candidate key: $K \subseteq R$ is a candidate key of R if K -> R holds AND K is minimal 
  
  - We choose this as our primary key
  
- Computing $X^+$ algorithm:

  ```
  // @param: F is a set of FDs
  // @param: X is a FD. We want to calculate X+
  computeX+(X,F)
  	X+ = X
  	while true
  		if there exists a Y -> Z in F where
  			Y is contained in X+
  			and
  			Z not contained in X+
  			then add Z to X+
  		return X+
  ```

  ```
  example:
  F =
  	1. sin, pnum -> hours
  	2. sin -> ename
  	3. pnum -> pname, ploc
  	4. ploc, hours -> allowance
  
  does sin, pnum -> allowance?
  X is..
  sin, pnum
  sin, pnum, hours (1)
  sin, pnum, hours, ename (2)
  sin, pnum, hours, ename, pname, ploc (3)
  sin, pnum, hours, ename, pname, ploc, allowance (4)
  ```


- BCNF decomposition

  - Rules:
    - A schema `R` and a list of function dependencies `F` is in BCNF iff whenever `X->Y` is in` F+` AND `XY` contained in `R` then
      - `X->Y` is trivial (so `Y` contained in `X`), OR
      - `X` is a super key of `R`
  - algorithm

  ```
  function ComputeBCNF (R, F):
  	result = {R}
  	while not done:
  		if there is a schema Ri in result that is not in BCNF:
  			let A -> B be a nontrivial FD that holds on Ri
  			// this means that AB must be contained in Ri
  			such that A+ does not contain Ri and A INTERSDECT B = {}
  			result = (result - Ri) UNION (Ri - B) UNION (A,B)
  	return result
  ```

  - example

```
relation:
class(course_id, title, dept_name, credits, sec_id, semester, year, building, room_number, capacity, time_slot_id)

FDs:
1. course_id -> title, dept_name, credits
2. building, room_number -> capacity
3. course_id, sec_id, semester, year -> building, room_number, time_slot_id

candidate key: course_id, sec_id, semester, year
- it entails all of class

******************************************************
iteration 1:
- class is not in BCNF
- course_id -> title, dept_name, credits
	- is NOT a super key and
	- is NOT trivial
- so we create
// REMOVE title, dept_nam, credits FROM CLASS
class1(course_id, sec_id, semester, year, building, room_number, capacity, time_slot_id)
and
course(course_id, title, dept_name, credits)

******************************************************
iteration 2:
- class 1 is not in BCNF
- building, room_number -> capacity
	- not NOT a super key and
	- is NOT trivial
- so we create
// REMOVE capacity from CLASS1
class2(course_id, sec_id, semester, year, building, room_number, time_slot_id)
and
classroom(building, room_number, capacity)

******************************************************
iteration 3:
- class 2 is in BCNF, so is classroom and course

we are done

we are left with
class(course_id, sec_id, semester, year, building, room_number, time_slot_id)
classroom(building, room_number, capacity)
course(course_id, title, dept_name, credits)
```

- Serialization / precedence graph
  - Given a list of histories of reads, writes performed by a set of transactions, we will construct a serialization graph
  - We will add $T_i \rightarrow T_j$ if and only if one of the following holds
    - $T_i$ Writes Q before $T_j$ reads Q
    - $T_i$ Reads Q before $T_j$ writes Q
    - $T_i$ Writes Q before $T_j$ writes Q
  - If the precedence graph has a cycle then schedule S is not conflict serializable
  - Else it is
- Transaction Isolation Levels
  - Serialization allows programmers to ignore concurrency issues when they code transactions
  - But the protocols needed to allow serialization might not allow enough concurrency (parallel execution) to take place, thus hindering performance
  - Different levels of isolation help longer transactions that do not need to be precise
  - Levels
    - Serializable. Highest level
      - Use when you need exclusive access; touching all rows; doing a count all; figuring out min or max over a range
    - Repeatable read. Only committed data can be read. Data item can't be updated between reads.
    - Read committed: only committed data can be read.
      - Writing to one row
    - Read uncommitted: lowest isolation level allowed by SQL
      - When you do not need a lock (reading 1 row)