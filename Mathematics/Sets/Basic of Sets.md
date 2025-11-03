A [set] is a collection of distinct objects into a whole.
Such an object $x$ inside a set $M$ is called **an element** of $M$, written $x \in M$.
If $x$ is not such an object inside the set $M$, we write $x \not\in M$ (equals to $\lnot (x \in M)$).

## Definition

By giving all its elements :
$$A := \{1, 2, 3, 4\}
$$
$$\emptyset := \{\}$$
$$\mathbb{N}:= \{1,2,3,4,5,\dots\}$$
$$\mathbb{N}_{0}:= \{0,1,2,3,4,5,\dots\}$$$$\mathbb{Z}:= \{-2,-1,0,1,2,3,\dots\}$$
$\mathbb{Q}$ = Rationals Numbers      $\mathbb{R}$ = Reels Numbers    $\mathbb{C}$ = Complex numbers


By forming a new set:
$$\bigg\{x \in \mathbb{N}\ |\ x \text{ is an even number}\bigg\}$$
$$\bigg\{x \in \mathbb{Z}\ |\ x \in \mathbb{N}\bigg\}$$

##### Quantifiers: $\forall x \rightarrow$ for all $x$         $\exists \rightarrow x$ it exists $x$

Transform a predicate into a logical statement:

***$x$ is a planet*** is a predicate.
$\forall x:$ $x$ ***is a planet*** is a logical statement ("For all $x$, $x$ is a planet") (it's a false one tho): $\forall x \rightarrow$ for all $x$         $\exists \rightarrow x$ it exists $x$

$\exists x:$ $x$ ***is a planet*** is a logical statement ("It exists at list one $x$ where $x$ is a planet")(true)


: $\forall x \rightarrow$ for all $x$         $\exists \rightarrow x$ it exists $x$
##### Equality for sets

Two sets $A$ and $B$ are the same, written as $A = B$ if the following is true:
$$\forall A: x \in A \leftrightarrow x \in B$$

$$\text{Ex: }\{2,3,4\} = \{3,2,4\}$$
##### Subsets
For two sets $A, B$, we write $A\subseteq B$ if the following is true:
$$\forall x: x\in A \rightarrow x\in B$$
We call $A$ a [subset] of $B$ and $B$ a [superset] of $A$.
By definition, $B\subseteq B$ and $\emptyset \subseteq B$ are always true (because $\forall x: x\in \emptyset\to x\in B$).

##### Union
........................![[Pasted image 20251102092049.png]].....................
$$A\cup B:=\bigg\{x | x\in A\vee x\in B\bigg\}$$
##### Intersection
........................![[Pasted image 20251102092928.png]].....................

$$A\cap B:=\bigg\{x| x \in A \land x \in B\bigg\}$$

##### Set difference
........................![[Pasted image 20251102093300.png]]......................
$$A\cap B':=\bigg\{x| x \in A \land x \not\in B\bigg\}$$
##### Big union/intersection
Need: $I$ set, $A_{i}$ set for each $i\in I$.
$$\bigcup_{i\in I} A_{i}:=\bigg\{ x | \exists i\in I: x\in A_{i}\bigg\}$$
$$\bigcap_{i\in I} A_{i}:=\bigg\{ x | \forall i\in I: x\in A_{i}\bigg\}$$
*Ex:*
$$A_{1} = \{1\}, A_{2}=\{2\},\dots$$
$$I= \mathbb{N}, A_{i} = \{i\}$$
$$\text{ Then:} \bigcup_{i\in I}A_{i}=\{1,2,3,\dots\} = \mathbb{N}$$
$$\text{ And } \bigcap_{i\in I}A_{i}=\emptyset$$
##### Power set
$$\mathcal{P}(A) := \big\{X|X\subseteq \}$$
$$A:=\{1,2\} \text{ }\mathcal{P}(A) := \big\{\emptyset, A, \{1\}, \{2\}\big\}$$
$$|A|=2 \text{ and } |\mathcal{P}(A)|=2^{|A|}=4$$

............![[Pasted image 20251102170135.png]]............


##### Cartesian product

For two sets $A,B$, the **cartesian product** writen $A\times B$ is the set of all ordered pairs $(a,b)$ where $a \in A$ and $b \in B$.

![[Pasted image 20251103135406.png]]
###### Definition of an ordered pair (by [Kuratowski](https://math.stackexchange.com/questions/1767604/please-explain-kuratowski-definition-of-ordered-pairs))

For elements $x, y$, write $(x, y):=\big\{\{x\},\{x,y\}\big\}$ 
$$(x,y) = (\tilde{x}, \tilde{y}) \iff \{x\} =\{\tilde{x}\} \land\{y\}=\{\tilde{y}\}$$$$\iff x=\tilde{x} \land y=\tilde{y}$$
###### Definition of a [cartesian product]

$$
A\times B:=\{(a,b)|a\in A\land b\in B\}$$

