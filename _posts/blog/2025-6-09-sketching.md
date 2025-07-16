---
layout: post
title: Dealing with high-dimensional data
subtitle: Sketching & the JL-Lemma
categories: blog
permalink: blog/sketching
mathjax: true
featured: true
thumbnail: /images/blog/2025-6-09-sketching/clustering.png
---

<!--more-->

<style>
.proof-box {
  display: inline-block;
  background-color: #f9f9f9;
  border-left: 4px solid #ccc;
  padding: 1rem 1.25rem;
  margin: 2rem 0;
  border-radius: 6px;
  font-size: 1rem;

  overflow-x: auto;
  max-width: 100%;
  box-sizing: border-box;
}





.proof-box::before {
  content: "🔍 Proof";
  display: block;
  font-weight: bold;
  margin-bottom: 0.5rem;
  color: #333;
}
</style>

It's extremely common in computer science to be dealing with very high-dimensional vectors. A natural question is can we *sketch*, or reduce the dimensionality, of these vectors while still maintaining the larger structure of the data. A common application of this is clustering, where we cluster datapoints based on their distances to other data. For this kind of application, we need to be able to maintain the pairwise distances between any two datapoints.

<img src="/images/blog/2025-6-09-sketching/clustering.png" style="display: block; margin: auto; width: 70%;">




In particular, given vectors <span class="inlinecode">$v_1, \dots, v_n \in \mathbb{R}^d$</span>, where the dimension <span class="inlinecode">$d$</span> is large, we want to find a function <span class="inlinecode">$f: \mathbb{R}^d \to \mathbb{R}^m$</span> where <span class="inlinecode">$m \ll d$</span> and pairwise distances are preserved. In other words, we want

**Definition ($\epsilon$ Pairwise Distance Property)**
For all pairs <span class="inlinecode">$v_i, v_j \in (v_1, \dots, v_n)$</span> and some small <span class="inlinecode">$\epsilon$</span>
<div>$$
(1 - \epsilon)\|v_i - v_j\|\le \|f(v_i) - f(v_j)\| \le (1 + \epsilon)\|v_i - v_j\|
$$</div>

which tells us that <span class="inlinecode">$f$</span> should be able to preserve the distance between any two vectors with greater and greater accuracy as our parameter <span class="inlinecode">$\epsilon$</span> approaches 0.

**Theorem (Johnson-Lindenstrauss)**
For <span class="inlinecode">$m > O(\log(n)/\epsilon^2)$</span>, there exists <span class="inlinecode">$f$</span> satisfying the equation above. This means that <span class="inlinecode">$n$</span> points in a high dimensional space can be mapped onto <span class="inlinecode">$m$</span> dimensions and still preserve pairwise distance. Actually, it turns out <span class="inlinecode">$f$</span> is a linear map and can be computed in a computationally efficient way.

*Algorithm idea.* Our first attempt at dimensionality reduction may involve trying to choose some subset of coordinates to include from each vector at random. However, this attempt fails because the accuracy depends a lot on the basis the vectors are represented in. The actual idea is as follows. We are going to generate a vector <span class="inlinecode">$g \in \mathbb{R}^d$</span> that points in a random direction and take the product <span class="inlinecode">$\langle v, g \rangle$</span>. We do this <span class="inlinecode">$m = \mathcal{O}(\log(n))$</span> times, storing each result in a vector <span class="inlinecode">$\in \mathbb{R}^m$</span>. The main intuition behind this is that <span class="inlinecode">$\langle v, g \rangle = \|v\|\|g\|\cos(\theta)\propto \|v\|$</span>, and that viewing the vector from many different directions gives you a lot of information about the vector.

<img src="/images/blog/2025-6-09-sketching/projection.svg" style="display: block; margin: auto; width: 40%;">

The way we generate a vector in a random direction is by generating a vector where each individual entry is i.i.d from a Gaussian <span class="inlinecode">$\mathcal{N}(0, 1)$</span>. We won't prove why this works, but the intuition is that applying any orthogonal matrix (i.e. a distance-preserving linear mapping, where <span class="inlinecode">$U^TU = I$</span>) to a Gaussian random vector leads to another random vector with the same distribution. Since rotation preserves distances, the distribution is invariant to rotation, so it must be that the Gaussian random vectors are uniformly distributed across all directions. Putting this all together, this gives rise to the following algorithm.

---

*Algorithm.* Build the matrix <span class="inlinecode">$A \in \mathbb{R}^{m \times d}$</span> where each row is a random Gaussian vector, which just means that each entry <span class="inlinecode">$A_{ij}\sim \mathcal{N}(0, 1)$</span>, and then scale each entry by <span class="inlinecode">$\frac{1}{\sqrt{m}}$</span>. Then compute <span class="inlinecode">$f(v) = Av$</span>, which takes the product of all the scaled, random <span class="inlinecode">$d$</span>-dimensional Gaussian row vectors.


<img src="/images/blog/2025-6-09-sketching/matrix.svg" style="display: block; margin: auto; width: 50%;">


## Johnson-Lindenstrauss Property

**Definition ($(\epsilon,\delta)$ JL Property)**
A distribution over matrices <span class="inlinecode">$A \in \mathbb{R}^{m\times d}$</span> satisfies this property if

whenever <span class="inlinecode">$m = \mathcal{O}((\log(1/\delta)\big/\epsilon^2)$</span>, for any <span class="inlinecode">$v \in \mathbb{R}^d$</span>, the inequality

<div>$$
(1 - \epsilon) \|v\| \le \|Av\| \le (1 + \epsilon)\|v\|
$$</div>

holds with probability <span class="inlinecode">$1 - \delta$</span>.

**Remark**
Notice that if we can show that <span class="inlinecode">$A$</span> satisfies the JL property, then we will be able to prove the JL theorem. Consider picking one of the <span class="inlinecode">$\binom{n}{2}$</span> pairs of vectors <span class="inlinecode">$v_i, v_j$</span>, and use that pair to form <span class="inlinecode">$v_i - v_j$</span>. Then, we know <span class="inlinecode">$\|A(v_i - v_j)\| = \|Av_i - Av_j\| = \|f(v_i) - f(v_j)\|$</span>. If <span class="inlinecode">$A$</span> satisfies the <span class="inlinecode">$(\epsilon, \delta)$</span> JL property, then this pair will preserve distance with probability <span class="inlinecode">$(1 - \delta)$</span>, and will fail to preserve distance with probability <span class="inlinecode">$\delta$</span>. If we let <span class="inlinecode">$F_i$</span> be an indicator r.v. denoting if our pair fails, then we fail any pair with probability

<div>$$
\text{Pr}(F_1 \cup \dots \cup F_n) \le \sum_i \text{Pr}(F_i) = \binom{n}{2}\delta
$$</div>

So choosing <span class="inlinecode">$\delta < \delta^*/\binom{n}{2}$</span>, if <span class="inlinecode">$A$</span> satisfies the <span class="inlinecode">$(\epsilon, \delta)$</span> property, then we preserve all pairwise distances with probability <span class="inlinecode">$1-\delta^*$</span>, as long as <span class="inlinecode">$m = \mathcal{O}((\log(n / \delta)\big/ \epsilon^2)$</span>.

**Remark**
We will show that our matrix <span class="inlinecode">$A$</span> satisfies the property, but it is not the only such matrix. For example, the Rademacher matrices, where each entry is independently chosen to be either <span class="inlinecode">$+1/\sqrt{m}$</span> or <span class="inlinecode">$-1/\sqrt{m}$</span> with equal probability satisfy the property. A random orthogonal matrix will also satisfy the property.

**Theorem**
Our construction of <span class="inlinecode">$A$</span> satisfies the <span class="inlinecode">$(\epsilon,\delta)$</span> JL property

<div class="proof-box">

Consider some <span class="inlinecode">$v \in \mathbb{R}^d$</span>. It suffices to prove the theorem for a unit vector where <span class="inlinecode">$\|v\| = 1$</span>, since to prove it for full generality we just introduce a bunch of scaling factors.

<br><br>

An important fact is the stability of Gaussian r.v.s. If we have two independent Gaussians and take a linear combination of them, <span class="inlinecode">$a_1\mathcal{N}(\mu_1, \sigma_1^2) + a_2\mathcal{N}(\mu_2, \sigma_2^2)$</span> will follow the distribution <span class="inlinecode">$\mathcal{N}(a_1\mu_1 + a_2\mu_2,\; a_1^2\sigma_1^2 + a_2^2\sigma_2^2)$</span>. We can use this to show if we take the product of <span class="inlinecode">$v$</span> with some random Gaussian vector <span class="inlinecode">$g$</span>, it's distributed as:

<div>$$
\langle v, g\rangle = \sum_{i=1}^d [v]_i \cdot \mathcal{N}(0, 1) = \mathcal{N}\left(0,\; \sum_{i=1}^d [v]_i^2\right) = \mathcal{N}(0, \|v\|^2) = \mathcal{N}(0, 1)
$$</div>

If we let <span class="inlinecode">$g_i$</span> denote the <span class="inlinecode">$i$</span>th row of <span class="inlinecode">$A$</span>, this tells us that:

<div>$$
Av = \frac{1}{\sqrt{m}} \left(\langle v, g_1 \rangle, \dots, \langle v, g_m \rangle\right) = \frac{1}{\sqrt{m}} (y_1, \dots, y_m), \quad y_i \sim \mathcal{N}(0, 1)
$$</div>

Now, we want to show that <span class="inlinecode">$\text{Pr}(\|Av\| \ge (1+\epsilon)\|v\|)$</span> and <span class="inlinecode">$\text{Pr}(\|Av\| \le (1-\epsilon)\|v\|)$</span> are both small. We do this by proving the stronger inequalities on the squared norm:

<div>$$
\begin{aligned}
\text{Pr}(\|Av\|^2 \ge (1+\epsilon)\|v\|^2) &= \text{Pr}\left(\sum_{i=1}^m y_i^2 \ge (1+\epsilon)m\right) \\
&= \text{Pr}\left(e^{\lambda \sum y_i^2} \ge e^{\lambda(1+\epsilon)m} \right)
\quad \color{#A0AAB4}{\text{(for all } \lambda \ge 0)} \\
&\le \frac{\mathbb{E}\left[e^{\lambda \sum y_i^2}\right]}{e^{\lambda(1+\epsilon)m}}
\quad \color{#A0AAB4}{\text{(Markov’s inequality)}} \\
&= \prod_{i=1}^m \frac{\mathbb{E}\left[e^{\lambda y_i^2}\right]}{e^{\lambda(1+\epsilon)}} \\
&= \left(\frac{1}{\sqrt{1 - 2\lambda} \cdot e^{\lambda(1+\epsilon)}}\right)^m
\quad \color{#A0AAB4}{\text{($\chi^2(1)$ MGF)}} \\
&\le \left((1 + \epsilon)e^{-\epsilon} \right)^{m/2}
\quad \color{#A0AAB4}{\text{(set } \lambda = \frac{\epsilon}{2(1+\epsilon)})} \\
&\le \delta/2
\quad \color{#A0AAB4}{\text{(substitute } m)}
\end{aligned}
$$</div>

A nearly identical proof shows:

<div>$$
\begin{aligned}
\text{Pr}(\|Av\|^2 \le (1 - \epsilon)\|v\|^2) &= \text{Pr}\left(e^{-\lambda \sum y_i^2} \ge e^{-\lambda(1 - \epsilon)m} \right) \\
&\le \frac{\mathbb{E}\left[e^{-\lambda \sum y_i^2}\right]}{e^{-\lambda(1 - \epsilon)m}} \\
&= \left(\frac{1}{\sqrt{1 + 2\lambda} \cdot e^{-\lambda(1 - \epsilon)}}\right)^m \\
&\le \left((1 - \epsilon)e^{-\epsilon} \right)^{m/2}
\quad \color{#A0AAB4}{\text{(set } \lambda = \frac{\epsilon}{2(1 - \epsilon)})} \\
&\le \delta/2
\quad \color{#A0AAB4}{\text{(substitute } m)}
\end{aligned}
$$</div>

So that <span class="inlinecode">$\text{Pr}(\text{out of bounds}) \le \delta$</span>, proving that our construction satisfies the JL theorem.

</div>


## Algorithmic Speedup

The naive algorithm to compute all pairwise distances takes <span class="inlinecode">$\mathcal{O}((n^2d)$</span> time, to compute the distance in <span class="inlinecode">$\mathcal{O}((d)$</span> time for <span class="inlinecode">$\binom{n}{2}$</span> pairs. Since the matrix is  dimension <span class="inlinecode">$\mathcal{O}((\log(n)) \times d$</span>, it takes <span class="inlinecode">$\mathcal{O}((d\log(n))$</span> time to project a vector down, and we do this <span class="inlinecode">$n$</span> times. Once we have the lower dimensional vectors, however, we just need to compute the <span class="inlinecode">$\binom{n}{2} = \mathcal{O}((n^2)$</span> pairwise distances in <span class="inlinecode">$\mathcal{O}((\log(n))$</span> time each. So the algorithm runs in <span class="inlinecode">$\mathcal{O}((dn\log(n) + n^2\log(n))$</span> time, which is good if <span class="inlinecode">$d \gg \log(n)$</span>. This says we can get a good approximation much quicker than brute-force!

## Subspace Embedding

Let <span class="inlinecode">$U$</span> be a <span class="inlinecode">$d$</span>-dimensional space. We're interested in a <span class="inlinecode">$k$</span>-dimensional subspace of this space. Given <span class="inlinecode">$v_1, v_2, \dots, v_k \in U$</span>, let <span class="inlinecode">$S = \text{span}(v_1, v_2, \dots, v_k)$</span>.

**Theorem**
If we project each <span class="inlinecode">$v\in U$</span> to <span class="inlinecode">$Av$</span>, then when <span class="inlinecode">$m > (k\log(1/\epsilon) + \log(1/\delta))\big/\epsilon^2$</span>, we have for all <span class="inlinecode">$v \in S$</span>

<div>$$
(1-\epsilon)\|v\| \le \|Av\| \le \|v\|(1+\epsilon)
$$</div>

with probability <span class="inlinecode">$1-\delta$</span>.

**Proof**
Ideally, we want to apply the union bound over all vectors in the subspace. However, there's infinitely many vectors, and we can't do that... The main idea is to look at a nearby neighborhood of each vector, called the <span class="inlinecode">$\epsilon$</span>-net. Stay tuned!
