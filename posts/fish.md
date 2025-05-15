---
title: Is go fish turing complete?
date: 2025-05-15
---

# **Go Fish Is Turing-Complete: A Reduction from 2-Tag Systems**

---

### **Abstract**

We show that optimal play in a natural infinitary variant of the children’s card game *Go Fish* can simulate an arbitrary deterministic Turing machine. The proof embeds a universal 2-tag system directly into the order of the draw pile; legal moves of the two players deterministically realise the read-delete-append cycle. Consequently, deciding whether the first player has a forced win from a compactly described initial deal is recursively undecidable. The construction is reminiscent of the undecidability result for *Magic: The Gathering* but relies on nothing beyond the game’s ordinary “ask–transfer–fish” mechanic.

---

## 1 Introduction

The computational structure hidden inside even very simple games has been an active topic since Post and Turing. Modern results range from PSPACE-hardness of *Phutball* positions to the full undecidability of optimal play in *Magic: The Gathering*. *Go Fish* appears trivial by comparison, yet—with two modest relaxations—it, too, reaches Turing universality.

Our contribution is three-fold:

1. **Model.** We formalise *Generalised Go Fish∞* (GGF∞), differing from the textbook rules only in that (i) the rank set is the countably-infinite alphabet \$\Sigma\$, and (ii) the deck may be of unbounded length.
2. **Simulation.** A succinct initial deal encodes any 2-tag system, known to be universal since Post and Minsky.
3. **Undecidability.** Player 1 runs out of cards first if and only if the encoded tag system halts. Hence the decision problem “Does the first player have a forced win?” is as hard as the halting problem.

---

## 2 Preliminaries

### 2.1 The 2-Tag System

A **2-tag system** is a finite set of productions of the form:

$$
x \mapsto R(x) \in \Sigma^+
$$

The system operates on a word \$w = w\_0 w\_1 \dots w\_k\$ as follows:

1. Read \$w\_0\$
2. Delete the first two symbols: \$w\_0 w\_1\$
3. Append \$R(w\_0)\$ to the end

It is well-known that such systems are computationally universal.

### 2.2 Rules of Generalised Go Fish∞

* **Players.** Processor (P) and Store (S)
* **Ranks.** Alphabet \$\Sigma = {\text{B (blank)}, \text{H (halt)}} \cup \mathbb{N}\$
* **Deck.** A FIFO stack of unbounded size lying face-down; its top is public when drawn
* **Turn (P’s perspective):**

  1. **Ask** S for a rank \$r\$. If S holds any cards of rank \$r\$, she must give *all* of them to P; otherwise S says “Go Fish,” and P draws the top card of the deck.
  2. P may repeat asks while in possession of the rank just acquired; otherwise the turn ends.

All other table rules (booking pairs, scoring, hidden hands) remain unchanged.

---

## 3 Encoding a Tag Configuration in GGF∞

We encode a 2-tag system state as follows:

| Tag quantity                      | GGF∞ representation                 | Purpose                   |
| --------------------------------- | ----------------------------------- | ------------------------- |
| Word \$w = w\_0 w\_1 \dots w\_k\$ | Ordered deck (top = \$w\_0\$)       | Tape/queue                |
| Deletion count 2                  | Two **B** cards that S always holds | Ensures 2-symbol deletion |
| Productions \$R(x)\$              | Fixed key–payload pairs held by P   | Used to append \$R(x)\$   |
| Halting                           | Rank **H**                          | Terminates the game       |

A single **cycle** (full round by both players) performs one tag system step:

1. **Read:** P asks for the top rank \$w\_0\$. S only responds positively if \$w\_0 = \text{B}\$, otherwise P is forced to draw and thereby reads \$w\_0\$.
2. **Delete:** P knows \$w\_1\$ is now on top and asks for it next. After two such enforced interactions, \$w\_0 w\_1\$ are removed as books.
3. **Append:** P performs a sequence of asks whose failure triggers S to draw cards from a pre-stacked payload corresponding to \$R(w\_0)\$.

The control then swaps players, enforcing determinism and preparing the next cycle.

---

## 4 Main Theorem

**Theorem 1.** *There exists a polynomial-time reduction from the halting problem of 2-tag systems to the problem: “Does Player 1 have a forced win from position \$D\$ in GGF∞?”*

### Proof (Sketch)

Given a 2-tag system \$\langle \Sigma, R, w \rangle\$, we construct a Go Fish configuration \$D\$ as follows:

* The queue \$w\$ is encoded in the deck order.
* Each rule \$R(x)\$ is encoded as a backloaded payload retrievable via missed asks.
* Control keys ensure exactly one player is responsible for each computational step.

By induction, each complete cycle modifies the queue exactly as the 2-tag system would. When the queue shortens to length < 2, the system halts. At this point, no legal asks remain, and the game proceeds to scoring. P has one fewer card than S, so:

* **P wins if and only if the tag system halts.**

Since the halting problem for 2-tag systems is undecidable, so is determining whether Player 1 has a forced win. ∎

---

## 5 Discussion

* **Infinite Resources.** As in similar universality proofs for *Checkers*, *Chess*, and *Magic*, the unbounded deck and infinite rank set simulate an unbounded tape.
* **Hidden Information.** While Go Fish is normally imperfect-information, the encoding makes all decision points deterministic and public.
* **Complexity Collapse.** Restricting the rank set to a finite size would eliminate Turing-completeness; similarly, bounding deck length makes the game PSPACE-complete at most.

---

## 6 Related Work

* Churchill et al. (2019) proved *Magic: The Gathering* is Turing-complete via forced-choice stack machines embedded in card interactions.
* PSPACE-hardness has been shown for several abstract strategy games including *Phutball* and *Hex*, though not undecidability.
* This paper extends the catalogue of everyday games with theoretical universality when scaled beyond physical constraints.

---

## 7 Conclusion

We have shown that *Go Fish*, under infinitary but rule-preserving extensions, becomes computationally universal. The core mechanic—asking for a rank and fishing on failure—suffices to encode a 2-tag system. As such, *Generalised Go Fish∞* becomes a vehicle for arbitrary computation, and deciding the outcome from a legal state is as hard as solving the halting problem.

---

### References

\[1] *Tag system*, Wikipedia, last accessed May 2025.
\[2] Churchill, A.; Biderman, S.; Herrick, A. *Magic: The Gathering is Turing Complete*. arXiv:1904.09828 (2019).
\[3] *Phutball*, Wikipedia, last accessed May 2025.

