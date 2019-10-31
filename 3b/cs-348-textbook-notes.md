# CS 348 Textbook Notes



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

  