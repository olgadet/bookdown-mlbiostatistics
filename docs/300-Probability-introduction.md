# (PART) Probability {-}


# Probability: reasoning under uncertainty

**Learning outcomes**

- understand the concept of probability
- manipulate probabilites by their rules
- assign probabilities in very simple cases




## Introduction

Some things are more likely to occur than others. Compare:

- the chance of the sun rising tomorrow with the chance that no-one is infected with COVID-19 tomorrow
- the chance of a cold dark winter in Stockholm with the chance of no rainy days over the summer months in Stockholm

We intuitively believe that the chance of sun rising or dark winter occurring are enormously higher than COVID-19 disappearing over night or having no rain over the entire summer. **Probability** gives us a scale for measuring the likeliness of events to occur. **Probability rules** enable us to reason about uncertain events. The probability rules are expressed in terms of [sets](https://en.wikipedia.org/wiki/Set_(mathematics)), a well-defined collection of distinct objects.


## Basic set definitions

- **set**: a well-defined collection of distinct objects, e.g. $A = \{2, 4, 6\}$
- **subset, $\subseteq$**: if every element of set A is also in B, then A is said to be a subset of B, written as $A \subseteq B$ and pronounced A is contained in B, e.g. $A \subseteq B$, when $B = \{2, 4, 6, 8, 10\}$. Every set is a subset if itself.
- **empty set, $\emptyset$**: is a unique set with no members, denoted by $E = \emptyset$ or $E = \{\}$. The empty set is a subset of very set.

## Basic set operations

- **union of two sets, $\cup$ **: two sets can be "added" together, the union of A and B, written as $A \cup B$, e.g. $\{1, 2\} \cup \{2, 3\} = \{1, 2, 3\}$ or $\{1, 2, 3\} \cup \{1, 4, 5, 6\} = \{1, 2, 3, 4, 5, 6\}$
- **intersection of two sets, $\cap$**: a new set can be constructed by taking members of two sets that are "in common", written as $A \cap B$, e.g. $\{1, 2, 3, 4, 5, 6\} \cap \{2, 3, 7\} = \{2, 3\}$ or $\{1, 2, 3\} \cap \{7 \} = \emptyset \}$
- **complement of a set, $A´$, $A^C$**: are the elements not in A
- **difference of two sets, $\setminus$**: two sets can be "substracted", denoted by $A \setminus B$, by taking all elements that are members of A but are not members of B, e.g. $\{1, 2, 3, 4\} \setminus \{1, 3\} = \{2, 4\}$. This is also in other words a relative complement of A with respect to B.
- **partition of a set**: a partition of a set S is a set of nonempty subset of S, such that every element x in S is in exactly one of these subsets. That is, the subset are pairwise *disjoint*, meaning no two sets of the partition cotain elements in common, and the union of all the subset of the partition is S, e.g. Set $\{1, 2, 3\}$ has five partitions: i) $\{1\}, \{2\}, \{3\}$, ii) $\{1, 2\}, \{3\}$, iii) $\{1,3\}, \{2\}$, iv) $\{1\}, \{2, 3\}$ and v) $\{1,2,3\}$


<!-- **Some further properties** -->

<!-- - intersection $(\cap)$ and union $(\cup)$ are commutative: $A \cap B = B \cap A$ and $A \cup B = B \cup A$  -->
<!-- - intersection and union are associative: $(A \cap B) \cap C = A \cap (B \cap C)$ and $(A \cup B) \cup C = A \cup (B \cup C)$  -->
<!-- - union is distributive over intersection and vice versa: $A \cup (B \cap C) = (A \cup B) \cap (A \cup C)$ and $A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$  -->
<!-- - de Morgan's laws: $(A \cup B)´ = A´ \cap B´$ and $(A \cap B)´ = A` \cup B´$ -->

<!-- **Example**  -->

<!-- ```{block2, type = "example"} -->

<!-- Here comes example -->

<!-- a = 1 -->


<!-- ``` -->


## Exercises

\BeginKnitrBlock{exercise}
<span class="exercise" id="exr:unnamed-chunk-1"><strong>(\#exr:unnamed-chunk-1) </strong></span>Here is my exercise.
\EndKnitrBlock{exercise}

## Answers to exercises
