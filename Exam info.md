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

## SMoL: understanding scoping, aliasing and mutation

What is the result of evaluating these programs?
![[3. Resources/MOCs/CPL/Notes/Exam info/image.png|587x118]]

Which of the following edits are not observable compared to the original program?
![[3. Resources/MOCs/CPL/Notes/Exam info/image-1.png|511x166]]

## SMoL: using Stacker to understand program evaluation
#CPL/exam-info/tip: ignore address values (still a bit unclear to me). #CPL/unclear : how are address values related in this stacker thing?

- Given a SMoL program
	- Compute its value
	- Draw environment and heap at end of program execution
	- → (solution help) invent your own address values
	- → (solution help) ignore details like line-numbers for function values.
	- #CPL/exam-info/important **train on these exercises + how to write it down to save time**

![[3. Resources/MOCs/CPL/Notes/Exam info/image-2.png|smol example program]]

![[3. Resources/MOCs/CPL/Notes/Exam info/image-3.png|I think this is the Stacker|342x185]]

---

Given the configuration below. **Write a program** that results in this configuration.
- (ignore values of randomly generated addresses)
![[3. Resources/MOCs/CPL/Notes/Exam info/image-4.png|example configuration that is provided|325x263]]

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

![[3. Resources/MOCs/CPL/Notes/Exam info/image-6.png|Racket macro definition + example using it is provided|225x197]]

- What is the result of evaluation with this program
	- Remember: variable binding and hygiene #CPL/exam-info/unclear : what is hygiene?

- Write down example program **after macro expansion**
- For each variable use, **draw an arrow to the location** where the variable is bound

![[3. Resources/MOCs/CPL/Notes/Exam info/image-7.png|arrows to bound-variable locations|225x154]]

---

## Objects 
- “Translate this example (e.g. Racket → Java,  Java → Racket)”

---

What is the **value** of this program?

**Extend** the code with a third object that inherits from a-child and overrides the add2 method and implements add2 in terms of add1
- Does this require changing the method call protocol? How?

**Translate the example** to a Java / JavaScript (your choice) equivalent using that language’s standard syntax for classes and methods (Racket→Java?)

![[3. Resources/MOCs/CPL/Notes/Exam info/image-8.png|253x165]]

---

**Rewrite** this Java code example in Racket **using the class-pattern.** (Java → Racket)
![[3. Resources/MOCs/CPL/Notes/Exam info/image-9.png|279x183]]

---
## Types

Given **a typed miniPlait program**, be able to determine type-correctness  and/or **(manually) compute the type of a program**
- #CPL/unclear what is type-correctness?

**What is the type** of this program? If it does not have a type, indicate why.
- Know the **conditions on when a type can be formed** and when not

![[3. Resources/MOCs/CPL/Notes/Exam info/image-10.png|349x79]]


---

Example question about judgement trees
![[3. Resources/MOCs/CPL/Notes/Exam info/image-11.png|369x233]]

---
## Javascript & Typescript
- Questions will focus on async control flow, Promises, Promise combinators, …
- Reason about control flow in async code. **Translating** a piece of code to/from callbacks, promises, async functions.

## Rust
- Questions about ownership & borrowing, similar to the quiz questions in the Brown version of the Rust book
- Given a piece of Rust code: **reason about ownership & borrowing**, add annotations or small pieces of missing code.