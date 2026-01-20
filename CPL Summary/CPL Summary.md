# Chapter 1: Introduction
## Why so many languages?
- 4 factors:
	- **Evolution**: learned how to do things better → e.g. structured programming (such as if-statements) over gotos
	- **Purpose-built**: Javascript for web programs, Rust for systems programming
	- For **program specific hardware**: CUDA for GPUs
	- **Socio-economic factors**: learn Swift to program iPhone apps, Java to program android apps
		- Proprietary interests & network effects → works in apple ecosystem, not in other ecosystems

## SMoL
- **Minimal (+ but all core features)** programming language → designed for learning

- **Has all the core functions of most programming languages**
	- Eager evaluation of expressions
	- Variables and assignment
	- Lexical scoping rules
	- First-class functions and closures
	- First class mutable structures (arrays, vectors)
	- Automated memory management (e.g. garbage collection)

- What is **NOT** present in SMoL
	- Not always present in every programming language: **static types**
	- Much variation between programming languages: **objects**

## Example programs
- **<mark style="background: #BBFABBA6;">Interpreter</mark>**: consumes a program in a language and produces the result of that program. $\text{Interpreter::Program}_L \to \text{Value}$
- **<mark style="background: #BBFABBA6;">Type-checker</mark>**: consume program → determines true/false based on whether type annotations are correct or not
- **<mark style="background: #BBFABBA6;">Pretty-printer</mark>**: consumes program and prints a pretty version of that program
- **<mark style="background: #BBFABBA6;">Transformer</mark>**: consumes programs in a language and produce related but different programs **in the same language**
	- Different program → text is different
	- Example: macros, for/while loop?
- **<mark style="background: #BBFABBA6;">Compiler:</mark>** consumes programs in one language and produce related programs in a different language $\text{compiler:: Program}_L \to \text{Program}_T$

## “The interpreter of a programming language is just another program”

## PLAI and PLAIT

- plai: large overlaps with Racket but has two extra features: allows pattern matching, allows the definition of unit tests
- plait: typed version of plai

# 

- (verify?)<mark style="background: #BBFABBA6;"> A is a **metalanguage** for B</mark>: language B is implemented in another language A
- (verify?) language B is implemented in another another language A: there exists an interpreter written in language A for the language B

![[image.png|223x105]]

## Modern programming languages core features

- **Lexical scoping**: search for a variable binding in the current scope first, if it is not present, search for the binding in the outer scopes recursively
![[image-1.png|search for binding y of y in inner scope first, then outers scope]]

- Eager evaluation of expressions: compute results before you need them
- Updatable variables (= mutations/assignments)
- Functions can be 
	- higher-order (lambdas): 1) passed as arguments, 2) bound to variables 3) serve as return values
	- close over lexical bindings (closures)
- Scope can be nested
- Automatic memory management (e.g. garbage collection)
- Mutable first-class structures: vectors/arrays and objects
	- **First-class**: 1) passed as arguments, 2) bound to variables 3) serve as return values

## Function definitions vs Function calls

![[image-2.png|Function definition|424x91]]

![[image-3.png|Function application|186x128]]

- Argument to
	- Function definition: **formal parameter**
	- Function application/calls: **actual parameter**

## S-Expressions

- Able to express: expressions or pure data

![[image-4.png|atom = pure data]]

## Stacker: key concepts

**Stack**: list of function calls waiting for a value → most recent function call: at the bottom
**Environment**: list of bindings: variable name → value
- environments extend other environments: variable binding not found locally → search in extended environment
- acts like a **cache** for substituted values
**Current task**: expression being evaluated + environment its being evaluated in
**Heap**: set of heap-allocated values (vectors or functions)
**Address**: unique references to environments and heap values.

## SMoL syntax

![[image-5.png]]


# Chapter 3: Evaluation

Two types of **evaluators**:
- **Interpreters**: easier to write, debug and update (in case we need extra features in the language)
- **Compilers**: aim to generate efficient code in a target language

**Meta-programs**: programs that operate on other programs

There is no such thing as an interpreted language or compiled language → a language does not specify how it should be evaluated

**Just-in-time compilation**: the evaluator for the programming language is first the interpreter, but switches to compiler for pieces of code that are executed often.

**Abstract syntax**: representation of a program in the computer
- Commonly represented as a **tree**: **Abstract Syntax Tree (AST)**
	- Represents programs in programs
	- ASTs enforce unambiguity in the program representation
		- Program evaluation can be done unambiguously

**Parsing**: turns a linear sequence of tokens into a tree-shaped representation
- Parsing always produces a consistent unambiguous representation, even if the input is ambiguous

## BNF format

- Initial language
![[image-6.png|355x164]]


- 3 things to support conditionals: 
	1) extend datatype representing expressions
	2) extend the evaluator to handle these new expressions
	3) extend the parser to produce these new representations

- The branches of a conditional statement must have the same type in statically typed languages

![[image-7.png|new abstract syntax representation]]

- We need to distinguish between type-constructors that produce **abstract syntax** and type-constructors that produce **values**
![[image-8.png|150x101]]

(?) If you don’t have variables and functions → you don’t have a program

**Binding**: assign a value to variable
**Local**: limited to some region of the program, not outside of it
- `(let (x 1))`: creates a new environment that **extends** the current environment. Binds `x` to `1` in this **new** environment
- `(set! x 1)` → **updates** the binding in the **current** environment

![[image-9.png|510x160]]

![[image-10.png|356x78|<var> to use a bound variable]]

**Shadowing**: a binding in the child environment overrides a binding in the parent environment 

![[image-11.png|210x171]]

### Functions in the language

- Functions can be
	- Top-level
	- Nested inside other functions

Functions are **first-class** values (see definition of first class)

**Anonymous functions (= lambdas)**: functions without a name

Two new types of expressions: lambda expressions & function call expressions

Lambda expressions should evaluate to a new function type that can be represented in AST

Argument evaluation order:
- 1) get the function value corresponding to function variable `interp fun env`
- 2) get the values corresponding to the arguments
- 3) bind the values to the variables → get a new environment
- 4) use the new environment to evaluate the body
![[image-12.png|451x134]]

**Eager evaluation**: evaluate argument expression before passing them as arguments to the function
**Lazy evaluation**: defer evaluation of argument expressions when referenced in the body of the function

![[image-13.png|Maxwell’s equations of software]]
**Static scoping**: binding of a variable is determined by the variable’s position in the program, not the program’s execution
**Dynamic scoping**: binding of a variable is determined by its most recently executed binding
- binding != value of a variable. Value can change for the same binding.

- Dynamic scoping: evaluation result of `coin-flip` cannot be determined before execution → cannot determine binding of x before execution!
![[image-14.png|dynamic scoping problem illustration]]

- A person that reads programs can’t be sure about the binding structure of our programs

**Closure**: function value that contains environment the 
function was defined in
- From function expression to closure: `[(e-lam param body) (v-fun param body env)]`
**Closed expression**: expression with no unbound variables. 
	- `(+ x 1)` where x is not bound → unclosed expression

**Syntactic sugar**: extra constructs that make your code easier to program and understand
**Desugaring**: translating from a bigger surface language into a smaller core language

![[CPL Summary 2026-01-19 18.17.07.excalidraw]]

**Macro systems**: systems in which a (slightly abstracted) program source is rewritten into program source before parsing takes place
- ??? Macrossystem? `(+ 1 2)` → `(+ 2 1)`

- Few languages expose macros to the programmer
- Some do with varying degrees flexibility

- In languages with eager evaluation you cannot define new control structures as plain functions

**Hygiene**: property of keeping the variable bindings in the macro definition separate from the variable bindings at the point of macro expansion

# Chapter 5

We define objects as syntactic sugar

![[image-17.png|class pattern (without extras)|391x93]]

![[image-18.png|class pattern (with private state)|472x114]]

![[image-15.png|class pattern (with private state and static members)|605x175]]

![[image-16.png|class pattern (private, static and self-reference)|572x204]]

## Important concepts

**Objects**: functions that dispatch based on a message name (= **object pattern**)
-  Bundling of data and operations over the data
- Generalisation of closures
**Classes**: functions that make objects (**= the object pattern**)
**Static members**: defined in environments extended by all objects of the same class
 - bindings are **shared** across objects of this class

Function can be represented as an object with a single entry point
Object can be represented as a function with multiple entry points

**Dynamic dispatch**: the method that should be called is determined **at runtime** based on the structure of the object

**Members**: fields and methods of a class or object

Two interesting questions about classes/objects:
- Are the **set of member names** statically fixed or changeable dynamically?
- Is the **member being accessed** statically fixed or can it be computed dynamically?
	- (the member name you use)

![[image-19.png|747x175]]
[[Member name design space examples]]

## Inheritance

**Inheritance**: allows the child object to reuse the members of the parent object without having to modify it
- The lookup for a message in the child object continues in the parent object

#### FYI


![[Difference a-child vs make-child]]

---

![[image-20.png|creates a new parent instance for every msg lookup in parent|266x200]]

![[image-21.png|creates only one instance of the parent|275x221]]

## Open recursion

**Open recursion**: parent object allows the child object to modify the behaviour of existing methods by overriding members on self.
## Other forms of inheritance

**Class-based inheritance**: each child has its own personal copy of the parent object
- Specify in advance which class you are going to extend
- In object pattern: implemented via ‘chaining’ message lookups
**Prototypal inheritance**: When two or more child objects inherit from a **common parent object**
- Changes in common parent object are visible in all child objects
- prototype-based inheritance
**Mixin-based inheritance (= mixins)**: classes that can be mixed into any class hierarchy as needed
- **Mixin**: class that can be mixed into any class hierachy as needed
- Mixin’s superclass becomes a **parameter** of the mixin → mixin can be viewed as function parameterised with parent class
- Enables modular class extensions
**Multiple inheritance**: a class can inherit multiple classes
- Benefits: flexibility in modelling complex hierarchies without code duplication
- Drawbacks: problems arise when the superclasses define members with the same name → which one to choose?

![[image-22.png|Mixin’s can be applied as needed. Flyable is not implemented in advance.]]

# Lecture 7: Continuations

**Control operation**: operation that causes an operation to proceed because it controls the program counter of the machine.
- **Function call**: 
- **Exception handling:** 

**Local control operation**:
**Non-local control operation**:

**Local I/O**: performed by tradition programs (e.g. command-line program)
**Remote I/O**: performed by web-based programs

**Evaluation context**: function calls stored on the stack
- exists **implicitly** on the stack
- We need to make this evaluation context **explicit**

![[image-23.png|evaluation context (left) stored in a lambda function to make it explicit]]

When a program needs external input → control is passed to the OS.

Making transfer of control explicit:
- 1) Explicitly suspend the program at each point in execution where it performs I/O
- 2) Enable the OS to resume the program with external input from the point at which the program was suspended
	- OS has control: so OS should take action to resume program

![[image-24.png|get-number/k takes a closure representing the rest of the computation (evaluation context)]]


**Continuations**: closures that represent the rest of the computation (= closures that represent evaluation context)
- denoted with the letter **k**
- **Continuation-passing-style (CPS)**: style where programs use continuations

## Inversion of control

**Inversion of control**: instead of the innermost operation being done first and the outermost being done last, the innermost operation is done last and the outermost operation is done first.

![[image-25.png|]]

Non-CPS: `print (+ (get-number) (get-number))`

In **both** cases: `get-number` needs to be called first. In CPS style, `get-number/k` gets the number, and then calls the continuation (evaluation context). This is implicit in non-CPS style.

## Call/CC and Let/CC
- `(let/cc k body ...) = (call/cc (lambda (k) body ...)` 
	- Merely syntactic sugar → `let/cc` is used in the summary

- `let/cc` allows you to customise what the continuation is after evaluation of the body → the evaluation context of the body is forgotten!
	- If `k` is called in the body → result of `let/cc k body` is result of `k`
	- If `k` is not called in the body → result of `let/cc k body` is result of `body`
	- Unlike with a function call → calling a continuation **never returns**: the evaluation context in which continuation is called is discarded!

![[CPL Summary 2026-01-20 16.51.26.excalidraw|example of how early return is implemented using continuations]]

## Advanced control flow

**Cooperative multi-threading**: each thread must explicitly give up ‘control’ to the thread scheduler to resume another thread
**Pre-emptive multi-threading**: any thread can yield implicitly after executing a certain period of time or # of instructions

---

CPS transformation is a **global transformation**: all functions require an extra parameter.
CPS transformation is hard to understand due to inversion of control

# Chapter 8: javascript

- **Javascript on the web**: scripts embedded in web pages, executed on the client
	- **Mobile code**: code is executed remotely (at the client side) 

![[image-26.png]]

- Javascript on the server: node.js
	- Renders support for **asynchronous I/O** on files and sockets
	- **Asynchronous I/O**: IO operations are non-blocking
	- **Node.js**: web and network application server
		- **No execution in a sandbox**

Scripts has access to DOM and Local storage (left). Modules have access to network IO and file IO.
![[image-27.png|535x249]]

Three most important values in JS programs:
- **Object**: mapping from a key to a value
	- Dynamic collection of properties
		- Can add/remove properties **at runtime**
		- Can lookup properties based on **computed key**
		- Can turn all properties of an object into an array and iterate over them
	- **Property**: key-value pair
	- **Object literal:** expressions that evaluate to a fresh object
		- Can be nested
- **Arrays**: sequences of values (similar to Python or Java lists)
	- `length` property: a **computed property**: returns current number of elements
	- Can access elements from index 0 to index `length - 1`
		- Out of range → return `undefined`
	- You can dynamically grow/shrink to add/remove elements
	- Arrays are objects with many utility functions (`forEach`, `map`, …)
- **Functions**