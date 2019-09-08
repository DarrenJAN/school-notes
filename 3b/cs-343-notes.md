# CS 343: Concurrent and Parallel Programming

## Intro

- Peter Buhr
- website: https://www.student.cs.uwaterloo.ca/~cs343/people.shtml
- office: DC 2504
- instructional support coordinator: Olga Zorin (doctor notes, missing stuff, etc)
- grading question? go and see IA
- use buttons on assignment page to test / confirm compilation
- one late day for assignment 6 (Wednesday is the real due date)
- use FAQ
- see sample code-files; don't need to cite [link](https://www.student.cs.uwaterloo.ca/~cs343/codeExamples.shtml)
- c++17, gcc 9 for microC++

## Review

- routine documentation; comments to give overview AND to explain tricky portions of the code
- variables should be accompanied by a comment describing their purpose if that is not obvious
- Group of statements performing a non-trivial task should be preceded by a comment summarizing that task
- always use modularity - break down code into its functional components
- routine should perform exactly one operation
- for command line operations: must handle errors, print messages, and terminate
- ==premature optimization is the root of all evil==
- align comments at a particular column
- Global macros and types are allowed while global variables are strongly discouraged
- one situation where global variables can be used: simulation a module or package
- ==dynamic allocation must only be used when a variable's storage must outlive the block in which it is allocated OR for dynamic allocation of objects where you must use the constructor and pass in a param==
- use `const float PI = 3.14` instead of `#define PI3.14`
- closing brackets must start on a separate line, ==good habbit to leabel them== like `} // end if`
- ++, -- can only be used as statements, they cannot be used as part of expressions
- prefer += 1 to ++ etc
- only use `this` if you have to

```
// instead of this (which is illegal)
[x, y] = f(a, b)
do f(&x, &y, a, b) and set x's, y's values there
```

***

## Chapter 0: Intro
- why concurrency?
    - processor speed has stalled
    - need parallelism to increase speed
    - Moore's laws: every 18 months you can double the number of transistors
    - can't make them run faster (speed of light, heat); use more cores
    - can't keep writing sequential programs

****

## Chapter 1: Control Flow (Review)

### Advanced Control Flow
- multi-loop exit: 
    - one or more exit locations within the body of a loop; not just top or bottom
    - eliminates duplicated code
    - a loop exit should never have an else clause
    - eliminate flag variables
    - allow multiple exit conditions

```c++
// Ex 1
cin >> d
while (good) {
    cin >> d
}

vs

for (;;) {
    cin >> d
    if (!good) break
}

// Ex 2
// Bad:
for (;;) 
    S1
    if (C1) then S2
    else break
    S3

// Better
for (;;)
    S1
    if (!C1) break
    S2
    S3
```

### Static Multi-Level Exit
- exit multiple control structures where exit point is KNOWN at compile time
- label all exits (`break LX // exit LX`)
- tip, forget indentation. line up break statements with the loop or block they are breaking from!
- occasionally a flag var is useful

```c++
// works in uC++ and Java; not in C / C++
L1: {
    L2: switch () {
        L3: for () {
            break L1 // exit block
            break L2 // exit switch
            break L3 // exit loop
        }
    }
}
```

### Dynamic Memory Allocation
- use the stack, not the heap (in general)
- going to heap is expensive; must delete

```c++
// GOOD
cin >> size
int arr[size]

// BAD
cin >> size
int * arr = new int[size]
...
delete [] arr
```

- DO use heap in these cases

```c++
// variable's storage must outlive the block
Foo * getFoo() {
    Foo * f = new Foo();
    return f;
}

// amount of data read is unknown
vector<int> input // DYNAMIC sized data structure, uses heap
int temp
for (;;)
    cin >> temp
    input.push_back(temp) // implicit dynamic allocation
    
// when an array of objects must be initialized using object's constructor
// recall --> use unique_ptr's
Foo * data[size]
for (...)
    data[i] = new Foo(id) // Foo constructor needs a param
...
for (...)
    delete data[i] 

// when large local variables are allocated on a small stack
void small_routine() {
    int arr[100000] // overflow, BAD
}
void small_routine() {
    int * arr = new int[100000] // GOOD
}
```

### Dynamic Multi-Level Exit
- routine activation (call, invocation) has complex control flow
- among routines (h calls g calls f returns to g returns to h)
- cannot return from f to h directly ??????
- modularization

```c++
// modularization
// ex: labels have routine scope
B1: for ...
    B2: for ...
        if (...) break B1
        
to

int rtn() {
    B2: for ...
        if (...) break B1 // CANNOT FIND B1
}
B1: for ...
    w = rtn()
```



