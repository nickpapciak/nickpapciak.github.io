---
layout: post
title: Reductions and NP-completeness
subtitle: Understanding hardness through reductions
categories: blog
permalink: blog/reductions
mathjax: true
featured: true
thumbnail: /images/blog/2025-5-30-np-complete/3SAT.png
---

<!--more-->

<style>
.proof-box {
  background-color: #f9f9f9;
  border-left: 4px solid #ccc;
  padding: 1rem 1.25rem;
  margin: 2rem 0;
  border-radius: 6px;
  font-size: 1rem;
}

.proof-box::before {
  content: "🔍 Proof";
  display: block;
  font-weight: bold;
  margin-bottom: 0.5rem;
  color: #333;
}
</style>

## Reductions

### Polynomial Time Reductions

NP-completeness can seem to be very abstract and not useful when designing algorithms. Why does it help to recognize that a certain problem can be “verified” quickly?

Well, it turns out that many of the problems mentioned above are strictly different flavors of the same problem. That is, if you can solve one of the problems quickly (in polynomial time), you would be able to solve any other problem in this list quickly as well. Karp, in his seminal paper, found 21 such problems, but today, we know of hundreds of thousands of them.
These problems are known as *NP-complete* problems.

How was Karp able to prove that all these problems are effectively the same? This is what the idea of a reduction is. We say that some decision problem <span class="inlinecode">$A$</span> is polytime-reducible to <span class="inlinecode">$B$</span> (sometimes we use the notation <span class="inlinecode">$A \le_p B$</span>) if there is some algorithm <span class="inlinecode">$f$</span> which runs in polynomial time such that
<div>$$x \in A \iff f(x) \in B$$</div>

Here’s what this intuitively says. Let’s say we had a magic black box that could tell us the yes/no value of if a problem is in <span class="inlinecode">$B$</span>. Then, if there is a reduction <span class="inlinecode">$f$</span> which allows us to solve <span class="inlinecode">$A$</span> by using this black box for <span class="inlinecode">$B$</span>, we can say that <span class="inlinecode">$B$</span> is strictly “harder” than <span class="inlinecode">$A$</span>. Because, if we are ever able to solve <span class="inlinecode">$B$</span> with some algorithmic black-box, our reduction would immediately give us an algorithm for solving <span class="inlinecode">$A$</span>.

So here’s what Karp was able to prove. He took <span class="inlinecode">$\texttt{CLIQUE}$</span>, and then showed how it could be solved via <span class="inlinecode">$\texttt{IS}$</span>. Then he took <span class="inlinecode">$\texttt{IS}$</span>, and showed how it could be solved via <span class="inlinecode">$\texttt{HP}$</span>. Then he took <span class="inlinecode">$\texttt{HP}$</span>, and showed how it could be solved via <span class="inlinecode">$\texttt{TSP}$</span>. Finally, he took <span class="inlinecode">$\texttt{TSP}$</span>, and showed how it could be solved with <span class="inlinecode">$\texttt{CLIQUE}$</span>. In other words, Karp took his 21 problems and made a reduction chain something along the lines of
<div>$$\texttt{CLIQUE} \le_p \texttt{IS} \le_p \texttt{HP} \le_p \texttt{TSP} \le_p \texttt{CLIQUE}$$</div>
thereby showing
<div>$$\texttt{CLIQUE} \equiv_p \texttt{IS} \equiv_p \texttt{HP} \equiv_p \texttt{TSP}$$</div>
which says that if we could solve any one of these problems, we could solve them all.

<small>Note, this works because reductions are transitive, <span class="inlinecode">$\texttt{A} \le_p \texttt{B}$</span> and <span class="inlinecode">$\texttt{B} \le_p \texttt{C} \Rightarrow \texttt{A} \le_p \texttt{C}$</span>, which is true because the reduction from <span class="inlinecode">$\texttt{A}$</span> to <span class="inlinecode">$\texttt{B}$</span> can be composed with the reduction from <span class="inlinecode">$\texttt{B}$</span> to <span class="inlinecode">$\texttt{C}$</span> to get a reduction from <span class="inlinecode">$\texttt{A}$</span> to <span class="inlinecode">$\texttt{C}$</span>.</small>

### NP-completeness

Seeing this, a natural question is “is there a hardest problem in P?”, or “is there a hardest problem in NP?”. With reductions, we can answer this. First, what does it mean to be the “hardest” problem?

We say that a problem <span class="inlinecode">$A$</span> is “harder” than any problem in NP if
<div>$$B \le_p A$$</div>
for all <span class="inlinecode">$B \in \text{NP}$</span>. Usually, we call the set of all such hard problems NP-hard, and so we would say that the <span class="inlinecode">$A$</span> above satisfies <span class="inlinecode">$A \in \text{NP-hard}$</span>.

However, not all of these “hard” problems are interesting. For example, one such problem <span class="inlinecode">$A$</span> could be the problem “always output the correct answer of a nondeterministic Turing machine” and it can clearly trivially solve all the problems in NP. However, this problem is definitely not in NP.

So, to make this more interesting, we define a class of problems that are the hardest *within* NP. That is, they are both NP-hard, and they are in NP. These problems are called NP-complete.

### Cook-Levin Theorem

**Theorem:** <span class="inlinecode">$\texttt{3SAT}$</span> is NP complete
*Proof:*
First we need to show <span class="inlinecode">$\texttt{3SAT} \in \text{NP}$</span>. This is pretty simple—given some boolean formula, our *witness* can be an answer to the boolean formula, and our *verifier* can be an algorithm that plugs in that answer and checks if it evaluates to true or not.

The second thing we need to show is that, <span class="inlinecode">$\texttt{3SAT} \in \text{NP-hard}$</span>. This, unfortunately, is pretty difficult. How do we show that every single problem in NP can be reduced to a polynomial sized instance of <span class="inlinecode">$\texttt{3SAT}$</span>? Thankfully, Cook and Levin were able to do it, with some interesting trick involving simulating Turing machines and their tapes. Their construction is pretty complicated though, so we will spare you the details here.

Fortunately, thanks to Cook and Levin’s hard work, showing a problem is NP-hard is no longer as time-consuming and difficult! Now, since we know <span class="inlinecode">$\texttt{3SAT}$</span> is NP complete, if we want to show <span class="inlinecode">$\texttt{A}$</span> is NP-complete it suffices to first show <span class="inlinecode">$\texttt{A} \in \text{NP}$</span>, and then to find some reduction
<div>$$\texttt{3SAT} \le_p \texttt{A}$$</div>
This is because <span class="inlinecode">$\texttt{3SAT}$</span> is harder than every problem, and <span class="inlinecode">$\texttt{A}$</span> is harder than <span class="inlinecode">$\texttt{3SAT}$</span>, so <span class="inlinecode">$\texttt{A}$</span> is harder than every problem!

Below is the picture to keep in your head.

<img src="/images/blog/2025-5-30-np-complete/chart.png" style="display: block; margin: auto; width: 75%;">

It’s important to note that the picture above is what computer scientists generally agree is *probably* true. It’s still *possible* that we will be able to find some fast algorithm to solve an NP-complete or NP-hard problem, in which case, the three sets collapse to give <span class="inlinecode">$\text{P = NP = NP-complete}$</span>.

Let’s go take a slight detour to analyze a specific reduction between
<span class="inlinecode">$\texttt{3SAT}$</span> and <span class="inlinecode">$\texttt{CLIQUE}$</span>, and see what it allows us to say.

### Reduction of 3SAT to Clique

**Reduction:** We want to convert instances of 3CNF formulas to instances of graphs in polynomial time. In the constructed graph, cliques of a specified size correspond to satisfying assignments of the 3CNF formula. So a solution for a <span class="inlinecode">$k$</span>-clique problem can be directly used as a solution for the 3CNF problem.

- Let <span class="inlinecode">$\phi = (a_1 \vee b_1 \vee c_1) \wedge (a_2 \vee b_2 \vee c_2) \wedge ... \wedge (a_k \vee b_k \vee c_k)$</span>
- Reduce this 3CNF into an undirected graph <span class="inlinecode">$G$</span> by grouping the vertices of <span class="inlinecode">$G$</span> into <span class="inlinecode">$k$</span> groups of 3 vertices, each called a triple
- Each triple corresponds to one of the clauses in <span class="inlinecode">$\phi$</span>. Each vertex in a triple corresponds to a literal in the corresponding clause.
- Connect all vertices by an edge except (1) vertices in the same triple or (2) vertices representing complementary literals, e.g., <span class="inlinecode">$x$</span> and <span class="inlinecode">$\neg x$</span>.


```pseudo
function Reduce3SATtoKClique(phi):
    V ← empty set
    E ← empty set

    for i from 1 to k:
        t_i ← {a_i, b_i, c_i}
        V ← V ∪ t_i

    for each u in V:
        for each v in V:
            if u ≠ v AND not in same triple AND not complementary:
                E ← E ∪ {u, v}

    return G = (V, E)
```


**Example:**
Here’s an example on the boolean formula

<div>$$\Phi = (x_1 \vee \neg x_2 \vee \neg x_3) \wedge (\neg x_1 \vee x_2 \vee x_3) \wedge (x_1 \vee x_2 \vee x_3)$$</div>
Notice that almost all the edges are included except those within a triplet, and those going between the negation of the same literal (like <span class="inlinecode">$x_1$</span> and <span class="inlinecode">$\neg x_1$</span>).

<img src="/images/blog/2025-5-30-np-complete/3SAT.png" style="display: block; margin: auto; width: 90%;">

<div class="proof-box">
Now that we have our reduction, we need to formally prove that it works! We do this by showing a <span class="inlinecode">$\texttt{3CNF}$</span> input <span class="inlinecode">$\Phi$</span> is satisfiable if and only if the resulting <span class="inlinecode">$G$</span> from the reduction has a <span class="inlinecode">$k$</span>-clique.

<br>
(1) $\Phi$ satisfiable $\Rightarrow G$ has $k$-clique.
If <span class="inlinecode">$\Phi$</span> is satisfiable, then in the satisfying assignment, at least one literal in each clause is true. Each pair of those vertices must be connected, since we connect them all except complementary ones and ones within the same triple. Therefore <span class="inlinecode">$G$</span> contains a <span class="inlinecode">$k$</span>-clique.

<br>
<br>
(2) $G$ has $k$-clique $\Rightarrow \Phi$ satisfiable.
We can prove this by the contrapositive. If <span class="inlinecode">$\Phi$</span> is not satisfiable, then for any assignment there must be at least one clause not satisfied. This means that any selection of literals includes a complementary pair. These are not connected in <span class="inlinecode">$G$</span>, so <span class="inlinecode">$G$</span> does not have a <span class="inlinecode">$k$</span>-clique.
<br>
<br>
<em>Alternatively</em>, assume <span class="inlinecode">$G$</span> has a <span class="inlinecode">$k$</span>-clique. It must include one vertex per clause/triple, and since no two can be complementary, we can set those literals true, creating a consistent truth assignment. Therefore, <span class="inlinecode">$\Phi$</span> is satisfiable.

</div>


### What does this all mean?

Ok, so we’ve successfully performed a reduction. Since <span class="inlinecode">$\texttt{3SAT}$</span> is NP-complete and we have
<div>$$\texttt{3SAT} \le_p \texttt{CLIQUE}$$</div>
we also know
<div>$$\texttt{CLIQUE} \le_p \texttt{3SAT}$$</div>
so
<div>$$\texttt{3SAT} \equiv_p \texttt{CLIQUE}$$</div>

That makes <span class="inlinecode">$\texttt{CLIQUE}$</span> also NP-complete!

Now recall our original chain:
<div>$$\texttt{CLIQUE} \equiv_p \texttt{IS} \equiv_p \texttt{HP} \equiv_p \texttt{TSP}$$</div>
And since <span class="inlinecode">$\texttt{3SAT} \equiv_p \texttt{CLIQUE}$</span>,
<div>$$\texttt{3SAT} \equiv_p \texttt{CLIQUE} \equiv_p \texttt{IS} \equiv_p \texttt{HP} \equiv_p \texttt{TSP}$$</div>

Therefore, all of these problems are NP-complete!

This is how we’ve come up with so many NP-complete problems: we reduce from one known NP-complete problem to another. These chains form a web of reductions, like shown below.

<img src="/images/blog/2025-5-30-np-complete/reduction-web.png" style="display: block; margin: auto; width: 75%;">

Why is this useful? Because NP-completeness gives us permission to give up! If you reduce your hard problem to a known NP-complete problem, you can (often justifiably) say no efficient algorithm likely exists—at least unless P = NP!

### Further Reading

1. [David Lu’s Notes](https://davidjlu.github.io/CS350/Week9notes.html)
2. [P vs NP - Wikipedia](https://en.wikipedia.org/wiki/P_versus_NP_problem)
