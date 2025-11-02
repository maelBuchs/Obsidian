
A [proposition] (= Logic Statement) is a statement that is either true or false.

*Ex:
	Mars is a planet $\rightarrow$ true logical statement
	Pluto is a planet $\rightarrow$ false logical statement
	Good morning $\rightarrow$ not a logical statement (neither true nor false)
	$x + 1 = 1$ $\rightarrow$ Not a logical statement without "x =", it's a [predicate]*


### Logical operations

#### [Negation]
For a logical statement $A$, $\neg A$ denotes the [negation] (= `!A`). 
$$
\textstyle
\begin{array}{c|c}
A & \neg A \\
\hline
T & F \\
F & T
\end{array}
$$

#### [Conjunction]
For two logical statements $A, B$, $A\wedge B$ denotes the [conjunction] (= `A && B`). 
$$
\textstyle
\begin{array}{c|c}
A & B & A\wedge B \\
\hline
T & T & T\\
T & F & F\\
F & T & F\\
F & F & F
\end{array}
$$
#### [Disjunction]
For two logical statements $A, B$, $A \vee B$ denotes the [disjunction] (= `A || B`).
$$
\textstyle
\begin{array}{c|c}
A & B & A\vee B \\
\hline
T & T & T\\
T & F & T\\
F & T & T\\
F & F & F
\end{array}
$$
#### [Equivalence]
Two statements are called logically equivalent if the truth table are the same (= `A == B`.
$$\lnot(A \vee B) \iff (\lnot A) \wedge(\lnot B)$$
#### [Conditional]
For two logical statements $A, B$, $A \to B$ denotes the [conditional] (= `if A : B`).
$$
\textstyle
\begin{array}{c|c}
A & B & A\to B \\
\hline
T & T & T\\
T & F & F\\
F & T & T\\
F & F & T
\end{array}
$$

$A \implies B$ means that $A \to B$ is a [tautology] (if there is A, then there is always B)


#### [Bi-conditional]
For two logical statements $A, B$, $A \leftrightarrow B$ denotes the [bi-conditional] (= `if A: B && if B: A`).
$$
\textstyle
\begin{array}{c|c}
A & B & A\leftrightarrow B \\
\hline
T & T & T\\
T & F & F\\
F & T & F\\
F & F & T
\end{array}
$$

$A \iff B$ means that $A \leftrightarrow B$ is a [tautology].

The contraposition of the statement $A \to B$ is $\lnot B \to \lnot A$

#### Deduction Rules

**Modus Ponens**
*if $A\to B$ true and $A$ is true, then $B$ is true.*

**Chain syllogism**
*if $A\to B$ is true and $B\to C$ is true, then $A\to C$ is true*

**Reductio ad absurdum**
*if $A\to B$ is true and $A\to \lnot B$ is true, then $\lnot A$ is true*


A [predicate] is an expression with undetermined variables that ascribes a property to objects filled in for the variables.


