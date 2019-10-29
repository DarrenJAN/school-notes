# CS 348 Textbook Notes



## Chapter 6: DB Design using ER Model

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
  - Todo page 288