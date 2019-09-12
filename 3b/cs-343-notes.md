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



## Chapter 2: Dynamic Multi-Level Exit

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

- example of ==non-local transfer==
  - h calls g calls f
  - h wants to set a place that f will transfer back to (recall: labels are local scope) (we can skip g)
  - but what you can do is have a global variable label
  - these labels are constants; at compile time it knows where to go; this is why using a variable label helps you change where to go at compile time
  - sometimes we can have recursive functions. if L1 is declared in the recursive function h, then there are multiple L1 variables that have been declared! in which frame do you go? but what if you set the variable only on the 6th recursive call? this means you want to transfer to the L1 in the 6th call, and skip the first 5
  - also, when g (which is recursive) gets skipped as f skips over it to h, then f will skip over ALL recursive calls of g
  - so, we also need to store the FRAME on the STACK for the variable (location within the frame) then you know where to jump to
  - if you're in C++ and you want to go from f to h and skip over all g frames, then it might be more of a walk than a go-to because, as you go, you need to call all of the destructors for memory allocated on the heap
  - C has this capability (jmp_buf, setjmp, longjmp)

```
label L
void c(int i) {
	if (i == 1) goto L;
}
void b(int i) {
	c(i)
}
void a() {
	L = L1
	b(1)
L2:
	print("bye")
L1: // ---> f jumps to here from c; doesn't return from b
	print("hi")
}
```



### 2.1 Implementation

- compares throw/catch to variable label and goto
- TIP: see slide 11 for help on assignment 1

### 2.2 Traditional Approaches

- return code: returns a value indicating normal or exceptioonal execution
- status flag: like `errno` variable in UNIX; this is the place where errors put additional information
- Fix-up routine: global or local routine called for an exceptional event to fix-up and return a corrective result so a computation can continue

- can we force the programmer to look at these return codes?
- `optional` type is one option; `variant` is better
- varaint: you can have many things inside of it; helps to force checking, but still possible to ignore; still can only return 1 level at a time

### 2.3 Exception Handling

- is a dynamic multi-level exit (don't really know where the thrown exception will be caught)
- exception usually occurs with low frequency
- EHM execution handling mechanism

### 2.4 Execution Environment

- when you exit a {} (which is, basically, a local lambda function) and if code inside of it creates a variable, then when you exit that stack from (which it basically is) you need to call dfestructors for all of those variables
- when you have multiple stacks, the execution environment gets even more complex
- 1 stack: exception thrown, goes down stack trying to be caught. if not caught, program has error
- many stacks: get to bottom of 1 stack, should you cause error for all other stacks? you have a new dimension for you can do with exceptions

### 2.5 Terminology

- execution: language unit in which an exception can be raised, usually with its own runtime stack (any routine)

### 2.6 Static/Dynamic Call/Return

<img src="./images/cs343-1.png" alt="alt text" style="zoom:30%;" />

- Static: things that you can tell about your program when it is not running
- dynamic: program must be running to know
- dynamic return AND static call: **routine call**
  - you call foo() -> you know where you are going
  - you return from foo(): if it is called from multiple places, it must know where it must return to
- Dynamic return AND dynamic call: **routine pointer, virtual routines**
  - dynamic call: you don't know which routine is going to be called until you evaluate the pointer (think of inheritance)

### 2.7 Static Propagation (Sequel)(Static Return)

- sequel: has a static call AND a static return; you always know statically where it is going to go after
- rule: after is finishes, it goes to the just outside the END of the block in which it was DECLARED
- sequel is a nested routine
- ==If break X is going to break out of innermost for-loop, then the corresponding sequel for it must be written in that innermost for-loop==
- Disadvantage: only works for monolithic programs

```
for ... {
	sequel S1(...) { ... } // nested routine
	void M1(...) { // assume you can declare a nested function here
		...
		S1()...
	}
	... other stuff ...
	... other stuff ...
}
// S1 statically returns here!!!!
```

### 2.8 Dynamic Propogation

- Cases 3 (termination exception), 4

- Static return, dynamic call (dynamic multi level exit)

- Termination:

  ```
  void f {
  	throw E() // dynamical call, needs to find which catch clause
  }
  int main {
  	try {
  		f()
  	} catch (E) {
  		...
  	}
  	
  	try {
  		f()
  	} catch (E) {
  		...
  	}
  	
  	try {
  		// copy the code from f
  		// f
  		...
  		...
  		//throw --> instead of throw you call sequel()
  		sequel()
  	}
  }
  ```

  - Don't know statically where it will return to
  - **Throw: This is a dynamic call (a strange call)**. Throw E is supposed to find the appropriate handler (catch clause), then it executes the code in the catch clause (which is a routine). O(n) call.
  - Now we finish a catch, where do we return to? The next line. This is a static return.
  - Sequels: you pick them up, and put them in the block you want to terminate. So if you want to terminate the try block, then you make the catch a sequel, and put it inside the try block

- ==Note: microC++, don't try and check the return code of opening a file. You need to check for exceptions.==

  - Except for end of file

- **Retry**: each func has a contract it must fulfill; instead of catch, call retry, and then do try again, unless you break to finish to exit; can be simulated using a while loop

- **Resumption**: provides a limited mechanism to generate new blocks on call stack

  - Instead of throw, do resume(E). While looking for the catch in O(n), it does NOT unwind the stack
  - ==In catch, you are to FIX the problem, and after the catch it goes back to next line after the resume call==
  - Better, just use a fixup function pointer (pass it all the way up to fix the code if there is an error)

- Throws do recovery; fixups do correction

- Exceptional Example

  - unGuarded block = no catch clauses
  - When you `throw` you start on the NEXT block on the stack, don't continue in current block you are in (look at throw in C5, it does not go and check C6) 
    - Option 1: Want to do a resumption? Initial throw should have be a `resume` (and so would not have unwound the stack)
    - Option 2: retry, you go back to B2 and retry
    - Option 3: terminate, exit out of B2 and continue to next line of code in B1
  - Note: can never put anything in middle of stack, must go to top of stack
  - You can pass a pointer from the block of the exception down the stack to the catch clause



## Chapter 3: Coroutine

- Coroutine: a routine that can also be suspended at some point and resumed from that point when control returns

  - Execution location:

  - Execution state:

  - Execution status:

  - H -> G -> F -----> H' -> G' -> F (original F)

  - Does not start from the beginning of each activation; it is activated at the point of last suspension

  - In contrast, a routine always starts at the top for new activation

    ```
    call G (starts at 0)
    at G line 10, G co-calls F
    F runs from line 0 to 15, then suspends back to G
    G starts at 0 again
    at G line 10, G co-calls F (starts at line 16)
    F run from line 16 to 25, then suspends back to G
    G starts at 0 again
    at G line 10, G co-calls F
    F run from line 26 to termination
    ```

  - Coroutines execute synchronously (no concurrency)

- Recall: a variable that tells you where to transfer to is a flag variable (typically a global var)

- Semi-Coroutine:

  - It's how you use it that makes it a semi

- Full-Coroutine:

  - It's how you use it that makes it a full

- `_Coroutine`

  - Must have member routine called **main** (is the actual co-routine)
  - Main routine must be private
  - `suspend()`, `resume()` (hide these from the interface)
  - Inherits from type `uBaseCoroutine` magically
    - Can give it a name, adjust the stack size (uses that name for error messages)
    - You can ask who resumed you, which coroutine resumed me?
  - Only ever remembers the last resumer; no stack growth; just context switches