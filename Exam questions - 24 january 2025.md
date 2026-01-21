```
 (deffun (k b) (lambda (a) (+ a b)))
 (defvar f (k 3))
 (defvar g (k 2))
 (+ (f 1) (g 0))
```
What is the result of the program above?
Draw the environments and the heap at the end of execution (no stack).

```
 (defvar v (mvec 1 2 3))
 (defvar vv (mvec v v))
 (vec-set! (vec-ref vv 1) 0 4) 
 (vec-ref vv 0)
```
Which choice best describes the heap at the end of the program above? (followed by some options like in the SMoL tutor exercises)

### Question 3

1. Given some code for an interpreter, fill in the blank lines to complete it (just open your book). e-lam and e-app implementation was missing as well as the e-equal (num= in the project)
2. There is a bug in one of the expression cases in the interpreter, describe it and write the correct implementation. <- This was the e-if that evaluated the condition and **both** branches instead of only evaluating the relevant branch based on the condition.

### Question 4

1. Given a few (5?) programs e.g. `(lam (x: Num) (+ x 6))` , write down the type of the program if it has one, otherwise write a type error with a relevant error message.
2. For the last two questions you were given a type (e.g. (List (List Num) -> List Num)) and had to write a program that has this type.

### Question 5

Similar to the example question, you were given the code for a function that calculates the total cost price of a customer's shopping cart using a callback approach and had to convert this to a Promise API equivalent. Customers are objects with an ID and a list of item ID's that represent their cart, items are objects with an ID and a price. Some toy data was given together with two tests (for the callback approach) for existing customers, and you had to write a new test for a non-existing user. getItemById(itemId) which returns the item object, and getCustomerById(customerId) which returns the customer already existed. In the original code "user not found" was errored if the user didn't exist, and the price of an item that wasn't found was 0.