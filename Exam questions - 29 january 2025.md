### Question 1
```SMoL
 (deffun (p x)
	(lambda (y) (- y x)))
 (defvar p1 (p 1))
	 (let ([x 10])
		(- (p1 3) ((p 2) 4)))
```
- What is the result of this program?
- Draw the environments and heap at the end of execution

---
### Question 2
```SMoL
 (defvar v (mvec 3 5))
 (defvar vv (mvec 1 2 v))
 (deffun (f k)
   (vec-set! k 0 4) 
 (f v)
 vv
```

Which choice best describes the heap at the end of the program above? (followed by some options like in the SMoL tutor exercises)

---
### Question 3

Given some code for an interpreter, implement (`set! <expr>`) and (`seq <expr1> <expr2>`). It was given to use Boxof for set with the API, and that the env would be changed to Hashof Symbol (Boxof Value).

### Question 4

```
 (define (mk-counter x)
   (lambda (m)
     (case m
       [("inc") (lambda (y) (set! x (+ y x) x))]
       [("get") (lambda () x)])))
 (defvar o (mk-counter 5)
 ((o "inc") 5)
 ((o "get"))
```
Given smol program with an object pattern and the stacker representation at the end of running the program. Write the address(es) of the heap section(s) referring to a class, an object and list all if any fields of the object(s).

### Question 5
Given rust code. Fill in the `&` and `&mut`. Also an easy multiple choice question on the mutation and aliasing mechanics of rust.

