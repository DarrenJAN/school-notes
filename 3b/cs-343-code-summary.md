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

- uC++ Exception Handling

  - throwing: `_Throw [except]` or `_Resume [except] [_At anotherCoroutineVar]`
    - able to throw by the child class
  - handling: `_CatchResume`; if catchResume'd, then it continues from the next line of the CALLER

- low-level thread creation

  ```
  void p(int i) { ... }
  int f(int i) { ... }
  auto tp = START(p,5); // creates thread, runs p(5)
  auto tf = START(f,3);
  WAIT(tp); // waits for tp thread to finish
  int ret = WAIT(tf); // waits for tf thread to finish
  ```

- software locks
  - Yale (simple, bad spin lock; board says open or closed; breaks rule 1 (2ppl can be in same time)
  - Alternation (board says who was last in; can't go twice in a row; breaks rule 3; you control who else can go in while you are away at school)
  - Declare Intent (declare if you want to go in; breaks rule 4; if we both put our names on the blackboard then we can never go in)
  - retract intent (breaks down eventually)
  - prioritized retract intent (higher priority can keep cutting in front, starvation, breaks rule 5)
  
- hardware implementations
  
  - Test and set; swap; fetch and increment
  
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

  

- terms
  - Cooperation
  - single acquisition
  - multiple acquisition 