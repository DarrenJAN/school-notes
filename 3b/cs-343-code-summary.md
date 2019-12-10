# CS 343 Code Summary

- optional

  ```c++
  optional <int> rtn() {
  	if (random() % 2) return random();
  	return nullopt;
  }
  
  int main() {
  	optional<int> foo = rtn();
  	if (foo) {
  		int foo_val = *foo;
  	}
  }
  ```

- variant

  ```c++
  variant < int, bool > rtn() {
  	if (random() % 2) return { true };
  	return 99;
  }
  
  int main() {
  	variant<int, bool> foo = rtn();
  	if (holds_alternative<int>(foo)) {
  		cout << get<int>(foo);
  	} else {
  		cout << get<bool>(foo);
  	}
  }
  ```

- sequel

  ```C++
  // seuqls return to end of block in which it was declared
  { // new block
  	sequel Foo(...) { ... }
  	class stack {
  		void push(int x) {
  			// some logic
  			Foo(); // jumps to and executes Foo
  		}
  	}
  }
  // Foo then returns and continues here
  ```

- Non-Local transfer

  ```
  jump_buf name;
  setjmp(jump_buf)
  lonjmp(jump_buf, xxx)
  
  jmp_buf env; //delcare
  int main() {
  	int val = setjmp(env); // L = L1
  
    if (val == 0) {
      // first time through
      foo()
    } else {
      // second time through; val was re-assigned after longjmp
    }
  }
  
  func foo() {
  	longjmp(env, 1); 
  }
  
  ```
  
  

- coroutine

  ```
  _Coroutine Fib {
  private:
  	int fn;
  	void main() {
  		int fn1 = 0;
  		int fn2 = 0;
  		fn = 0;
  		suspend(); // first get() call will return fn = 0
  		fn = 1;
  		fn1 = 0;
  		fn2 = 0;
  		suspend() // second get() call will return fn = 1
  		for (;;) {
  			fn = fn1 + fn2;
  			fn2 = fn1;
  			fn1 = fn;
  			suspend();
  		}
  	}
  public:
  	int get() {
  		resume(); // call main to run until next suspend() call
  		return fn;
  	}
  }
  ```

  - This vs uThisCoroutine
    - This is the current object we are inside of; uThisCoroutine is the current stack we are running on 
    - So C1 is running. uThisCoroutine is C1. This is C1
    - C1 calls C2.publicMethod()
    - Inside of C2.publicMethod(), while it is running, uThisCoroutine is C1, and this is C2.
    - Once c2.publicMethod() calls resume, it suspends uThisCoroutine (c1), and resumes this (c2)

- uC++ Exception Handling

  - Can only throw exception types that are of type `_Event`
  - throwing: `_Throw [except]` or `_Resume [except] [_At anotherCoroutineVar]`
    - able to throw by the child class
  - handling: `_CatchResume`; if catchResume'd, then it continues from the next line of the CALLER
  - In order to receive an Event that was "thrown" at you, you need to `_Enable <type1><type2> <...>`
  - When a Coroutine throws E(), but does not catch it, then `uBaseCoroutine::UnhandledException` is raised at its last RESUMER, not at its starter

## Chapter 5: Concurrency

- low-level thread creation

  ```
  void p(int i) { ... }
  int f(int i) { ... }
  auto tp = START(p,5); // creates thread, runs p(5)
  auto tf = START(f,3);
  WAIT(tp); // waits for tp thread to finish
  int ret = WAIT(tf); // waits for tf thread to finish
  ```

- Mutual exclusion game (SPALS)
  
  - Security (only 1 in critical section at a time)
  
- software locks
  
  - Yale (simple, bad spin lock; board says open or closed; breaks rule 1 (2ppl can be in same time)
  
  - Alternation (board says who was last in; can't go twice in a row; breaks rule 3; you control who else can go in while you are away at school)
  
  - Declare Intent (declare if you want to go in; breaks rule 4; if we both put our names on the blackboard then we can never go in)
  
  - retract intent (breaks down eventually)
  
  - prioritized retract intent (higher priority can keep cutting in front, starvation, breaks rule 5)
  
  - arbiter
  
    ```
    _Task Client {
    	int me // get initialized via constructor
    	void main() {
    		for i from 1 to 100
    			intents[me] = true
    			while serving[me] is false, wait
    			critical_section()
    			serving[me] = false
    	}
    }
    
    _Task Arbiter {
    	void main() {
    		int i = N;
    		while true
    			while intents[i] is false, wait
    			// now, intents[i] is true
    			intents[i] = false
    			serving[i] = true
    			while serving[i] is true, wait
    	}
    }
    ```
  
- N thread prioritized entry

  - First you look down (towards higher priority), you let them go first
  - Then once you check all of them, you look up the queue. While looking up at lower priority, and you see a check mark, then the check mark means they have already started
  - Then you run
  - Breaks rule 5 (priority starvation)

- Tournament algorithm

  - You create a tournament graph using Dekker's algorithm. You have different tournaments, eventually a thread wins at the bottom level, and enters their critical section and runs
  - Then, the thread travels back (along the tournaments) and retracts its intent
  - Then the tournament goes again. Because Dekker is fair, by having retracted your intent you know the other thread will win

- Bakery algorithms

  - Breaks rule 1 if you overflow
  - Doesn't have starvation because tickets are granted in FIFO order
  - All start off at infinity
  - Go in, set your value to 0 (indicates that you want to grab a ticket)
  - Then go and search for highest value
  - Once you get it, you go through the list from start to end and any time you find something something
    - With lower priority
    - Or, with same priority but lower index,
  - Then you WAIT
  - After getting to end, you run your critical section

- hardware implementations
  
  - Test and set; swap; fetch and increment



## Chapter 6: Locks

- locks
  - general
    - synchronization: be able to let S1 in T1 come before S2 in T2
    - mutual exclusion: have two tasks T1, T2 each with the same code, and some critical section in their main, but only 1 of them is allowed executing in that critical section at a time
  - spin locks (uSpinLock -> non-yielding, uLock -> yielding)
  - blocking locks
    - majority of responsibility put on releasing task
    - MutexLock
      - why do you need to pass in the lock when you call `yieldNoSchedule`? because you could get pre-empted, put on the ready queue, then some unblocking task sees that you are waiting in the block's queue, and then puts you on the ready queue (AGAIN)
      - Solution: atomically yield no schedule AND release spin lock
    - barging avoidance: hold `avail` between releasing and unblocking task
    - barging prevention: hold the lock between releasing and unblocking task
    - uOwnerLock: multi acquisition mutex lock
    - Synchronization lock (condition lock)
      - Calling `wait(uLownerLock lock)` atomically releases the owner lock and re-acquires it when it returns
  
- barriers

  - uBarrier example
    - Perform matrix sum, but instead of summing up values in ascending order of task number, sum them up by finishing time
    - No actual improvement, since you have to delete them in $O(n)$ time anyways

  ```c++
  _Cormonitor Accumulator : public uBarrier {
  private:
  	int total, temp;
  protected:
  	void last {
  		temp = total;
  		// reset other members
  	}
  public:
  	Accumulator(int r) : uBarrier(rows) {}
  	
  	void block (int rowTotal) { // called my Adder Tasks
  		total += rowTotal;
  		uBarrier::block(); // still need o get uBarrier::block() functionality
  	}
  }
  
  _Task Adder {
  private:
    Accumulator & acc;
  	void main() {
    	int rowTotal = 0;
      for each val in row, subtotal += val
      acc.block(subtotal) // will now be added up in order of finishing time
  	}
  public:
    Adder(int[] row, int nm, Accumulator & a) : ..., acc(a) {}
    int getTotal() { return temp; }
  }
  
  int main() {
    // get matrix
    // create adder array of pointers
    Accumulator acc(rows)
    // create adder tasks
    // last adder task will reset acc; total is now in temp
    cout << "total is " << acc.getTotal() << endl;
  }
  ```

- semaphores

  - `lock.P()` is acquire: wait until lock val is 1, or >= 1, and then acquire the lock, and decrement the value
  - `lock.V()` is release: you have the lock, now you release it, increment the counter, and wake up an acquirer
  - Binary implementation: basically the same as a mutexLock or condition variable
  - Counting semaphore:

  ```
  _Task T {
  private:
  	CountSemaphore & lock;
  	int main() {
  		lock.P();
  		// lock initialized to max count 5, so 5 different tasks
  		// can be in here at once
  		// critical section
  		lock.V();
  	}
  public:
  	T(CountSemaphore & lock) : lock(lock) {}
  }
  
  ...
  
  int main() {
  	CountSemaphore lock(5);
  	T t0(lock), t1(lock), t2(lock),... tn(lock);
  }
  ```




## Chapter 7:  Concurrent Errors

- Race condition: missing synchronization or mutual exclusion
- Live-lock: indefinite postponement (you go first)
- Starvation: selection algorithm ignores one or more tasks (other person goes first, I send a text message and miss that I was able to go, repeats forever)
- Deadlock
  - Synchronization: wait on a semaphore that is closed, will always be closed
  - Mutex: multiple ppl enter critical section
  - 5 conditions (CHNCS)
    - Concrete shared resource (intersection)
    - Holding (hold and wait)
    - No Pre-emption (don't give back the resource)
    - Circular Wait
    - Simultaneous
- Prevention: ordered resource policy
- Avoidance:
  - Banker's algorithm
  - Allocation graph
    - (Task) --> [resource] means task is waiting on a resource
    - [resource] --> (Task) means task is currently holding that resource



## Chapter 8: Indirect Communication

- `_Monitor`

  - All public members are mutex; does not begin execution if there is another active mutex member
  - EXT
    - `_Accept(foo)` blocks; waits for foo to be called and finished, or foo to be called and blocked
    - Can't be used when
      - Depends on member param values OR cannot guarantee that accepting(x) will fulfil their desired cooperation (bank balance of student, calling withdraw / deposit)
  - INT
    - `uCondition ::wait(), ::signal(), ::signalBlock(), ::empty()`
    - The signaller (person that calls `signal()`) does NOT block. Signalled task continues waiting
    - Signaller will block if they use `::signalBlock()`
    - When you call `myUCondition.wait()` inside of a member function, it releases the monitor lock and lets someone else in

  - Rendezvous failure

- Monitor Types
  - Explicit, implicit, calling (C; outside the function; waiting to get in), signalled (W; waited inside the function, blocked on some queue), signaller (S; signalled a waiter that they can go) queues
  - uC++ does C < W < S (signallers have top priority. Sit on bench, then stand back up right away)

- `_Cormonitor`: monitor coroutine

  - Monitor but with a main; suspend(); resume()

- Java

  - Uses a single condition variable; doesn't even have a name, just `wait(), notify(), notifyAll()`
  - When you wait inside a `synchronized` function, you get put back onto the entry queue ==(C = W < S)==
  - Need to use a `while` loop around waits due to spurious wakeup
  - ==Need a ticket, keep track of your current generation==
  - Waits need to be surrounded by

  ```
  try {
  	wait();
  } catch (InterruptedException e) {}
  ```



## Chapter 9: Direct Communication

```
_Task {
private:
	void main() {
		_When (..) _Accept(a,b,c)
		or _When (..) _Accept(d)
		_Else {
			...
		}
	}
}
```

- External Scheduling
  
  - Try and do as much work inside of `_Task::main` inside of `_Accept(foo) { .. do work .. }` calls, such that public API methods do less work (callers do less work)
- Internal Scheduling
  - Need to have duplicate order of Accept(insert) and Accept(remove) so there is no starvations
  - Need to signalBlock() while in Task::main() else you will still be inside of mutex and no one else will be allowed in
- Accepting a destructor: when you do so, the caller blocks and waits for the task to finish (can't delete its storage while it is being used)
  - So main calls ~foo
  - foo accepted destructor in Foo::main()
  - Accept statement gets executed EVEN THOUGH the destructor was never actually called (started destructor execution)

- Increased concurrency

  - Server, client
  - Administrator (manager), worker relationship
  - Admin does almost no work, it's job is to manage and create jobs for workers to do

- Asynchronous work

  - tickets: Error prone; fake a ticket; use a ticket twice; bad idea

  - Call back routine

  - future

    - `available()` is the async call finished?

    - `cancelled()`

    - `operator()` gets a read-only copy of the future RESULT

    - `reset()` mark as unavailable

    - `cancel()` : waiting clients are woken up, exception `uCancellation` is raised

    - Wait for different futures to be ready? `_Select` statement

      ```
      _When (...) _Select (f1, f2)
      or 
      	_When (...) _Select (f3)
      	and _When (...) _Select (f4)
      _Else { ... }
      ```

    - ==when a guard if false (When (false) _Select( f ) ) execution does NOT wait for that future to be come available==

  



- terms
  - Cooperation
  - single acquisition
  - multiple acquisition
  - Race condition
  - live-lock
  - starvation
  - deadlock (synchronization vs mutex)
  - thread
  - process
  - Task
- 