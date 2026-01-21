- Obtain deep understanding of essential programming language concepts (variable scoping, closures, classes, …)
- Understand design choices in language
- Select an appropriate language for given task

---
- #CPL/question What essential programming language concepts
- #CPL/exam-info/unclear which languages to study and for which purpose? E.g. objects, types in minplait? Something else in Racket? Do I need to learn languages like Swift, Java, … ?

# Exam questions
#CPL/exam-info/tip: “What is the result of this program”: find all **hard and unexpected answers** (they might focus on this)
#CPL/exam-info/tip: I think you should **study by looking at code examples** and noting what is peculiar, making these exercises as you go.
#CPL/exam-info/dont : don’t study specific APIs or libraries (e.g. for Rust or javascript, when something specific is required)
#CPL/exam-info/tbc do the ungraded exercises but **be careful for time-sinks when implementing**. They might be helpful however
#CPL/exam-info/todo identify what the ungraded assignments try to teach you and extract valuable knowledge from it (either by making exercise yourself, or just looking at model solution)

[[Exam questions - 29 january 2025]]
[[Exam questions - 24 january 2025]]

## SMoL: understanding scoping, aliasing and mutation

What is the result of evaluating these programs?
![[3. Resources/MOCs/CPL/Notes/Exam info/image.png|587x118]]

```plait
(defvar a 1) 
(deffun (foo)
	(lambda (b)
		(+ a b)))
(defvar bar (foo))
(set! a 3)
(bar 0)
```

`(deffun (foo) ...)` defines a **function** `foo` that takes no arguments. If you call foo using `(foo)` it creates a new lambda that takes argument `b`.

- `bar` and `foo` are not identical! `bar` takes parameter b while `foo` takes no arguments

---

```plait
(defvar a (mvec 88))
(defvar c (mvec a 88 88))
(set! a (mvec 76))
```

The vector that a points to is set to a different address, but the address that c points to in the first element remains unchanged. So the result is `@1 88 88` where `@1 = #(88)`.

`mvec a 88 88` $\neq$ `(mvec a) 88 88`! It is simply the keyword to represent vectors.

---

```plait
(deffun (k b)
	(lambda (a)
		(+ a b)))
(defvar foo (k 3))
(defvar bar (k 2))
(foo 3)
(bar 3)
```

`(foo 3)`: function call to the lambda with arg `a` in an environment binding `a` to 3 that extends the environment created by `foo`, where `k` is bound to 3. 

This creates the environment necessary to evaluate `(+ a b)`.

---

```plait
(deffun (inc n)
	(+ n 1))
(defvar v (mvec inc inc))
((vec-ref v 0) 2)
```

`(deffun (inc n) (+ n 1))` = `(defvar inc (lambda (n) (+ n 1)))`

---

Which of the following edits are not observable compared to the original program?
![[Exam info 2026-01-21 15.26.18.excalidraw]]

## SMoL: using Stacker to understand program evaluation
#CPL/exam-info/tip: ignore address values.

- Given a SMoL program
	- Compute its value
	- Draw environment and heap at end of program execution
	- → (solution help) invent your own address values
	- → (solution help) ignore details like line-numbers for function values.
	- #CPL/exam-info/important **train on these exercises + how to write it down to save time**

```plait
(defvar x 1)
(deffun (addx y)
	(defvar x 2)
	(+ x y))
(+ (addx 0) x)
```
---

Given the configuration below. **Write a program** that results in this configuration.
- (ignore values of randomly generated addresses)
![[3. Resources/MOCs/CPL/Notes/Exam info/image-4.png|example configuration that is provided|325x263]]

Strategy: first write with *named functions*, then if need be, replace all named functions by lambdas → easier to understand what is happening!

```plait
(deffun (f x)
	(deffun (g y) 
		(+ ((lambda () 0)) y))
	(+ (g 6) x))
(f 3)
```

```
((lambda (x)
	(+ ((lambda (y)
		(+ ((lambda () 0)) y)) 6) x)) 3)
```

## Evaluation
 #CPL/exam-info/dont  **don’t need to memorize these type / grammar definitions** . Definitions will be given as part of exam question. #CPL/exam-info/unclear: what definitions exactly? Find this out properly.

![[3. Resources/MOCs/CPL/Notes/Exam info/image-5.png|276x268]]

**Extend the language** with support for variable assignment: 
- `{set! <var> <exp>}` updates the value of variable `<var>` in the environment to the value of `<exp>` and should evaluate to false.
-  `{begin2 <exp1> <exp2>}` is a restricted version of Racket’s begin that evaluates both argument expressions but only returns the value of the last expression.
- **Extend the definition** of Value, Expr, Operator as needed. Extend the definition of interp as needed

Add a language feature **by writing a desugar function** (rather than by extending interp)

Write a basic unit test to test your extension, using plait’s (`test <expr> <expectedvalue>`) function

---

## Syntactic sugar

What is the result of evaluation of the program below?
Write down example program **after macro expansion**
![[3. Resources/MOCs/CPL/Notes/Exam info/image-6.png|example macro usage: expand + evaluate|225x197]]

Macro expanded code:
```plait
(let ([v2 5])
	(if (< 0 v2)
		(let ([v2' (< v2 10)])
			(if v2'
				v2'
				#false
				))
		#false)]))
		)
	))
```

The expression evaluates to `#true`. Because `0 < 5` and `v2 = (5 < 10) = #true`.
Due to hygienic macro expansion: `v2` from the macro becomes `v2'`

---

- For each variable use, **draw an arrow to the location** where the variable is bound

![[3. Resources/MOCs/CPL/Notes/Exam info/image-7.png|arrows to bound-variable locations|225x154]]

---

## Objects 
- “Translate this example (e.g. Racket → Java,  Java → Racket)”

What is the **value** of this program?

**Extend** the code with a third object that inherits from a-child and overrides the add2 method and implements add2 in terms of add1
- Does this require changing the method call protocol? How?

**Translate the example** to a Java / JavaScript (your choice) equivalent using that language’s standard syntax for classes and methods (Racket→Java?)

![[3. Resources/MOCs/CPL/Notes/Exam info/image-8.png|253x165]]

```
(define a-parent
	(lambda (m)
		(case m
			[(add1) (lambda (x) (+ x 1))]
			[(sub1) (lambda (x) (- x 1))])))

(define a-child
	(lambda (m)
		(case m
			[(add2) (lambda (x) (+ x 2))]
			[(sub2) (lambda (x) (- x 2))]
			[else (a-parent m)])))

# Overrides add2 and implements add2 in terms of add1
(define a-grand-child
	(lambda (m)
		(case m
			[(add2) (lambda (x) ((a-child 'add1 ((a-child 'add1) x)))]
			[else (a-child m)])))

```

```javascript
// TODO check correctness

class Parent {

function add1(x) { x + 1 };
function sub1(x) { x - 1 };

}

class Child extends Parent {

function add2(x) {x + 2};
function sub2(x) {x - 2};

}
```

Result of program: `6` because `add1` is in the parent and we apply lambda: `(+ x 1)`


---

**Rewrite** this Java code example in Racket **using the class-pattern.** (Java → Racket)
![[3. Resources/MOCs/CPL/Notes/Exam info/image-9.png|279x183]]

```plait
(define (make-parent)
	(lambda (m)
		(case m
			[(add1 (lambda (x) (+ x 1)))
			 (sub1 (lambda (x) (- x 1)))
			]
		)
	)
)

(define (make-child)
	(let ([parent (make-parent)])
		(lambda (m)
			(case m
				[
				(add2 (lambda (x) (+ x 2)))
				(sub2) (lambda (x) (- x 2))
				(else parent m)
				]
			)
		)
	)
)

(let ([a-child (make-child)])
	(a-child 'add1)
)

```

---
## Types

Given **a typed miniPlait program**, be able to determine type-correctness  and/or **(manually) compute the type of a program**

**What is the type** of this program? If it does not have a type, indicate why.
- Know the **conditions on when a type can be formed** and when not

![[3. Resources/MOCs/CPL/Notes/Exam info/image-10.png|349x79]]


---

Example question about judgement trees
![[3. Resources/MOCs/CPL/Notes/Exam info/image-11.png|369x233]]

![[image.png|judgement tree worked out example]]

---
## Javascript & Typescript
- Questions will focus on async control flow, Promises, Promise combinators, …
- Reason about control flow in async code. **Translating** a piece of code to/from callbacks, promises, async functions.

## Rust
- Questions about ownership & borrowing, similar to the quiz questions in the Brown version of the Rust book
- Given a piece of Rust code: **reason about ownership & borrowing**, add annotations or small pieces of missing code.