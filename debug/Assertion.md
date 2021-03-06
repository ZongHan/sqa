Assertion
===

>   幾乎百分百的確認，但在開發階段，我們還是會謹慎一點。

An assertion 斷言 is a statement in the Java programming language that enables you to test your `assumptions` about your program. For example, if you write a method that calculates the speed of a particle, you might assert that the calculated speed is less than the speed of light.

Each assertion contains a boolean expression that you believe will be true when the assertion executes. If it is not true, the system will throw an error. 

By verifying that the boolean expression is indeed true, the assertion confirms your assumptions about the behavior of your program, increasing your confidence that the program is free of errors.

One of the quickest and most effective ways to detect and correct bugs. 

Enhance maintainability: assertions serve to document the inner workings of your program

### Form

Form 1

     assert Expression1 ;

Form 2

     assert Expression1 : Expression2 ; 
 
Expression1 is a boolean expression. Expression2 is an expression that has a value. 

Enable and disable
---
To ensure that assertions are not a performance liability in deployed applications, assertions can be enabled or disabled when the program is started, and are disabled by default.

### Use Assertion when ...

#### Internal Invariants

```
if (i % 3 == 0) {
   ...
} else if (i % 3 == 1) {
   ...
} else {
   assert i % 3 == 2 : i;
   … 
}
```

#### class Invariants

A class invariants is a type of internal invariant that applies to every instance of a class at all times

```
// Returns true if this tree is properly balanced 
private boolean balanced() {
   ...
}
```

Because this method checks a constraint that should be true before and after any method completes, each public method and constructor should contain the following line immediately prior to its return:

```
assert balanced();
```

> 類別內任何物件、任何方法執行後都會 balance (例如會計的帳）, 所以就執行一下 assert

#### Control-Flow Invariants

> 對控制流程的斷言

```
void foo() { 
   for (...) {
      if (...) return;
   }
   assert false; // Execution should never reach this point!
}
```

#### Postconditions

You can test postcondition with assertions in both public and nonpublic methods. 

> 在一段複雜的運算後，你斷定會成為真的事情，用斷言來加強。

```
public BigInteger modInverse(BigInteger m) {
   if (m.signum <= 0)
      throw new ArithmeticException("Modulus not positive: " + m);
      ... // Do the computation
   assert this.multiply(result).mod(m).equals(ONE) : this;
   return result; 
}
```

### Do NOT use 


#### Do not use assertions for argument checking in public methods.

> 公開方法對參數的檢查本來就應該做，不要用 assertion 來檢查。

You can use an assertion to test a nonpublic method's precondition that you believe will be true no matter what a client does with the class.

```
private void setRefreshInterval(int interval) {
   // Confirm adherence to precondition in nonpublic method
   assert interval > 0 && interval <= 1000/MAX_REFRESH_RATE : interval;
   ... // Set the refresh interval 
}
```


#### Do not use assertions to do any work that your application requires for correct operation.

> 不要把程式整成該做的工作放在 assert 中。因為 assert 以後可能會被移除。

```
// wrong! - action is contained in assertion
assert names.remove(null);

// right - action precedes assertion
boolean nullsRemoved = names.remove(null);
assert nullsRemoved; // Runs whether or not asserts are enabled
```
