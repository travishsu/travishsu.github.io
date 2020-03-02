---
layout: post
title:  "MathJax Example"
date:   2015-08-10
excerpt: "MathJax Example for Moon Jekyll Theme."
tag:
- markdown 
- mathjax
- example
- test
- jekyll
comments: true
---

## Derivation of Evidence Lower Bound

By Baye's rule
\[p(X)=\frac{p(X, Z)}{p(Z|X)}\frac{q(Z)}{q(Z)}=\frac{p(X,Z)}{q(Z)}\frac{q(Z)}{p(Z|X)}\]
thus,
\[\ln p(X)=\ln \frac{p(X,Z)}{q(Z)} + \ln \frac{q(Z)}{p(Z|X)}\]

From EM algorithm, we can decompose the log marginal probability using \[\ln p(X)=\mathcal{L}(q) + KL(q\|p\] where

- \(\mathcal{L}(q)=\int q(Z)\ln\frac{p(X,Z)}{q(Z)}\mathrm{d} Z=E_q\{\ln\frac{p(X,Z)}{q(Z)}\}\)
- \(KL(q\|p)=\int q(Z)\ln\frac{q(Z)}{p(Z|X)}\mathrm{d} Z=E_q\{\ln\frac{q(Z)}{p(Z|X)}\\)

The objective of Variantional Inference (VI) is to maximize the Evidence Lower Bound (i.e. \(\mathcal{L}(q)\)) with respect to \(q\), which is equivalent to minimize the KL-divergence because \(\ln(X)\) shall be constant regardless of any \(q\).

## The mean field method

By Mean-field Assumption, \(q\) can be factorized into independent distributions
\[q(Z)=\prod_i q_i(Z_i) = \prod_i q_i\]
thus, we are able to plug this form of factorization into Evidence Lower Bound (i.e. \(\mathcal{L}\))

\[\mathcal{L}(q)=\int (\prod_i q_i)(\ln p(X, Z)-\sum_i \ln q_i)\mathrm{d} Z\]
\[=\int (\prod_i q_i)\ln p(X, Z)\mathrm{d} Z - \int (\prod_i q_i)(\sum_i \ln q_i) \mathrm{d}Z\]

Now we select one index \(j \in \{i|i=1,2,\ldots,M\}\), write \(\mathcal{L}(q)\) as integral of a function of \(Z_j\) \[\mathcal{L}(q) = \int q_j \{\int (\prod_{i\neq j} q_i)\ln(X, Z)\mathrm{d} Z_i\}\mathrm{d}Z_j - \int q_j(\prod_{i\neq j}q_i)(\ln q_j+\sum_{i\neq j}\ln q_i)\mathrm{d} Z \\ = \int q_j \{\int (\prod_{i\neq j} q_i)\ln(X, Z)\mathrm{d} Z_{i\neq j}\}\mathrm{d}Z_j - \int q_j(\prod_{i\neq j}q_i)(\ln q_j)\mathrm{d} Z - \int q_j(\prod_{i\neq j}q_i)(\sum_{i\neq j}\ln q_i)\mathrm{d} Z \\ = \int q_j \{\mathbb{E}_{i\neq j} \ln p(X, Z)\}\mathrm{d}Z_j - \int q_j\ln q_j \mathrm{d} Z_j - \text{const w.r.t.}_j \\ = \int q_j \ln \frac{\exp{\mathbb{E}_{i\neq j} \ln p(X, Z)}}{q_j} \mathrm{d} Z_j - \text{const w.r.t.}_j\]

Let's define a constant \[M=\int \exp{\mathbb{E}_{i\neq j} \ln p(X, Z)}\mathrm{d}Z_j\] we can write above equation as \[\mathcal{L}(q) = \int q_j \ln \frac{\frac{1}{M}\exp{\mathbb{E}_{i\neq j} \ln p(X, Z)}}{q_j}\mathrm{d}Z_j + \ln M - \text{const w.r.t.}_j \\ = \int q_j \ln \frac{\frac{1}{M}\exp{\mathbb{E}_{i\neq j} \ln p(X, Z)}}{q_j}\mathrm{d}Z_j + \text{const w.r.t.}_j \\ = \int q_j\ln\frac{q^*_{j|i\neq j}}{q_j} + \text{const w.r.t.}_j \\= -KL(q_j \|q^*_{j|i\neq j}) + \text{const w.r.t.}_j\] where \(q^*_{j|i\neq j}\) denotes the distribution \(q_j\) conditioned that other distribution \(q_{i\neq j}\) is fixed. It's more convenient to write \[\ln q^*_{j|i\neq j} = \mathbb{E}_{i\neq j} \ln p(X,Z) + \text{const}\]

## Conclusion

From above derivation, we obtain that \(\mathcal{L}(q)\) and \(-KL(q_j \|q^*_{j|i\neq j})\) has maximum when \(q_j\) is exactly same as \(q^*_{j|i\neq j}\).

It tells us an important truth: In the setting of Variational Inference, **we shall update each individual approximate distribution by computing the expectation of other fixed approximate distributions.**
