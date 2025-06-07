---
layout: default
title: "How does the Gaussian transition rule lead to an explicit equation for $X_t$?"
date: 2025-06-07
mathjax: true
tags: [math, statistics, machine learning]
---

Your question can be reframed as: why the statement (a) $X_t \mid X_{t - 1} \sim N(\sqrt{1 - \beta_t}X_{t - 1}, \beta_t I)$ is equivalent to the statement (b) $X_t$ has the representation $X_t = \sqrt{1 - \beta_t}X_{t - 1} + \sqrt{\beta_t}Z_t$, where $Z_t \sim N(0, I)$ is independent of $X_{t - 1}$?

While Thomas Lumley has given you a heuristic answer, I agree that you are absolutely entitled to request a more rigorous argument.

The direction $(b) \Rightarrow (a)$ is an immediate application of the theorem below (Lemma 8.7 in Foundations of Modern Probability (3rd edition) by Olav Kallenberg):

> For any random elements $\xi \perp \eta$ in $S, T$ and a measurable function $f: S \times T \to U$, where $S, T, U$ are Borel, define $X = f(\xi, \eta)$. Then $\mathcal{L}(X \mid \xi) = \mathcal{L}\{f(x, \eta)\}$ when $x$ is evaluated at $\xi$ a.s.

Here "$\mathcal{L}(X)$ / $\mathcal{L}(X \mid \xi)$" denotes the law (i.e., distribution) of $X$ / conditional law of $X$ given $\xi$. To apply this result, simply let $\xi = X_{t - 1}, \eta = Z_t$, and $X = X_t = \sqrt{1 - \beta_t}X_{t - 1} + \sqrt{\beta_t}Z_t$. If you are interested in the proof to this lemma, check the reference by yourself.

For the direction $(a) \Rightarrow (b)$, it suffices to prove the conditional distribution of $Z_t := \beta_t^{-1/2}(X_t - \sqrt{1 - \beta_t}X_{t - 1})$ given $X_{t - 1}$ is $N(0, I)$, hence it is independent of $X_{t - 1}$. To this end, we can apply Theorem 8.5(ii) in the same reference (which concerns with the disintegration of the joint distribution of $(X_{t - 1}, X_t)$).  By this theorem, for a Borel set $H \in \mathscr{R}^n$, we have (where the notation $aH + b$ denotes the set $\{ax + b: x \in H\}$):

$$
\begin{align*}
 &\ P(Z_t \in H \mid X_{t - 1}) \\
=&\ P\left(X_t \in \sqrt{\beta_t}H + \sqrt{1 - \beta_t}X_{t - 1} \mid X_{t - 1}\right) \\
=&\ \int_{\sqrt{\beta_t}H + \sqrt{1 - \beta_t}X_{t - 1}}
(2\pi\beta_t)^{-n/2}\exp\left(-\frac{\beta_t^{-n}}{2}(x - \sqrt{1 - \beta_t}X_{t - 1})^\top(x - \sqrt{1 - \beta_t}X_{t - 1})\right)dx \\
=&\ \int_H (2\pi)^{-n/2}\exp\left(-\frac{1}{2}z^\top z\right)dz.
\end{align*}
$$

where the last step uses the transformation $z = \beta_t^{-n/2}(x - \sqrt{1 - \beta_t}X_{t - 1})$ and the property of the Lebesgue integration. The last expression shows that the conditional distribution of $Z_t$ given $X_{t - 1}$ is exactly $N(0, I)$.  This completes the proof. 
