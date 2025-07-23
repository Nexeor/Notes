2025-03-19 13:55

Status: #Leaf

Tags: #ProgrammingConcepts 

# LISP Principles
LISP (LISt Processing) has several core traits: 
1) **Functional Language**: Functions are treated as objects and can be passed as arguments, returned from functions, and stored in data structures
2) **Prefix List Notation**: LISP programs are represented as **prefix** lists. The first element encountered is the operator, which operates on the next N provided args. Those args may themselves be lists, representing more functions, and such we can build complex programs
3) **Dynamic Typing**: variables can be different types at different times (numbers, strings, etc.)
4) **Garbage Collection**: LISP is one of the first languages to feature automatic memory management
## Grammar
LISP has a minimalistic syntax with operators and arguments broken up by parentheses. Each parenthesized set of elements is a single expression in prefix notation, so the first element will always be the operator and the next N elements will be arguments:
- `(operator arg1 arg2 ... argN)`

We can write basic equations like so:
- **Addition**: `(+ 1 2)` evaluates to `3`
- **Multiplication**: `(* 2 3 4)` evaluates to `24`
- `(+ (* 1 2) (- 3 1)`

Variables can be defined using the `setq` operator:
- `(setq x 10)` assigns 10 to variable `x`

Functions can be defined using the `defun` operator:
- `(defun funcName (funcArg1, funcArg2) (funcOp))`
- `(defun square (x) (* x x))` defines `square` as a function that takes a variable `x` and returns its squared value `(* x x)`

Conditional expressions are defined using `if` or `cond`

`if` is the traditional "if X, then Y, else Z" structure
```LISP
(if (condition)
	true-expression
	false-expression)

(if (> x 0)
	"Positive"
	"Non-positive")
```

`cond` is "if X then Y, elif A then B, elif..." structure, more similar to a "switch" statement in other languages
```lisp
(cond 
	((condition) expression)
	((condition) expression)
	(t default-expression))

(cond
	((> x 0) "Positive")
	((< x 0) "Negative")
	(t "Zero"))
```
## Atoms and S-Expressions
Everything in LISP is either **Atoms** or **S-Expressions**:
- **Atom:** Basic data type
	- Numbers, strings, booleans, symbols
- **S-Expression**: An expression presented as a list. The first element will be the operator and the rest will be atoms arguments used by the operator
## Interpreter
1) Tokenize input


## References
