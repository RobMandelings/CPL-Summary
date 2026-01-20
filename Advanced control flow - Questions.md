What is the purpose of the `after-thread` continuation in scheduler-loop?

`after-thread (status)` is called either the thread yields or when the thread has completed its operations
- If it yields: call `after-thread (TSuspended)`
- If it completes: call `after-thread (TDone)`

(then the result of `let/cc k body` will evaluate to a status so that the status variable can be bound to either `TDone` or `TSuspended`)

---

﻿﻿What is the purpose of the `thread-k` continuation in the updated `yielder` lambda?



---

﻿﻿A `thread` expression is expanded into a one-argument function that takes a continuation sched - k. What is the purpose of this `sched-k` continuation?

---

﻿﻿Why do we need `ThreadStatus`? What would happen if we wouldn't care about the difference between being done or being suspended?

---

Two methods to implement continuations:
- By desugaring: convert direct style program $P$ into CPS-style program $P'$ and use the unmodified interpreter to interpret $P'$
	- You add constructs that are desugared: e.g. `yield`, `break`, `return`
- As a core language implementation: program directly into CPS style
	- The interpreter has to be rewritten

- **In both cases**, in order to support continuations, the interpreter should be able to interpret CPS-style code.

![[image.png|example python code: continuation can be implemented via desugaring: transform into CPS-style]]



