# Chapter 3 Revision Notes

## 1. Chapter overview

Chapter 3 studies eigenvalues, eigenvectors, PCA, and kernel PCA. The exam repeatedly asks you to implement iterative eigenvalue algorithms and apply PCA/kernel PCA to small data sets. This chapter connects directly to Chapter 2 because eigenvalue methods use matrix-vector products and linear solves, and it connects to Chapter 1 because kernel PCA uses Gram matrices.

Main exam message: know the algorithms step by step, know the convergence conditions/rates, and know how PCA turns variance maximization into an eigenvalue problem. Sources: `chapter3.pdf`, `Final-revision.pdf`, `2023May.pdf`, `2024May.pdf`, `2025May.pdf`.

## 2. High-priority topics from final revision

| Priority | Topic | Why it matters |
|---|---|---|
| High priority | SVD connection | Final revision asks how to get SVD for a given matrix; SVD also supports PCA. |
| High priority | Power iteration | Final revision asks implementation, convergence conditions, and rates; tested in 2023, 2024, 2025. |
| High priority | Inverse iteration | Final revision asks implementation and convergence; tested in `2023May.pdf` and `2024May.pdf`. |
| High priority | Rayleigh quotient iteration | Final revision asks implementation and convergence; tested in `2023May.pdf` and `2024May.pdf`. |
| High priority | PCA | Final revision asks how PCA finds dominant components and why it works; tested in all three past exams. |
| High priority | Kernel PCA | Final revision explicitly lists kernel PCA; tested in 2023, 2024, 2025. |
| Medium priority | Rayleigh quotient theory | Needed to explain why eigenvectors maximize variance and how RQI updates eigenvalue estimates. |
| Medium priority | MSE / information percentage | Common PCA calculation in past exams. |
| Low priority / background only | Long finance/Netflix application examples | Useful context, but not the main exam calculation target. |

## 3. Key concepts and definitions

- Eigenpair: $(\lambda,v)$ with $Av=\lambda v$ and $v\ne0$.

- Rayleigh quotient:

$$
R_A(v)=\frac{v^TAv}{v^Tv}.
$$

For symmetric $A$, the Rayleigh quotient ranges over the eigenvalue interval, and eigenvectors give stationary values. Source: `chapter3.pdf`.

- Dominant eigenvalue: eigenvalue with largest absolute value. Power iteration targets it when it is separated from the rest.

- Power iteration: repeated multiplication by $A$ and normalization.

- Inverse iteration: repeated solution of $(A-\mu I)w=v$ and normalization, targeting the eigenvalue closest to shift $\mu$.

- Rayleigh quotient iteration: inverse iteration where the shift is updated to the current Rayleigh quotient.

- PCA: a dimensionality-reduction method that finds orthonormal directions of maximum variance. Equivalently, it minimizes reconstruction mean squared error.

- Covariance matrix for centered data $x_1,\ldots,x_n$:

$$
V=\frac{1}{n}\sum_{i=1}^{n}x_ix_i^T.
$$

The eigenvectors of $V$ are principal components.

- Kernel PCA: apply PCA in a feature space $\phi(x)$ using only the Gram matrix $K_{ij}=K(x_i,x_j)$.

## 4. Core theories / models / frameworks

### Power iteration

Intuition: repeated multiplication by $A$ amplifies the component in the dominant eigenvector direction.

Algorithm:
1. Choose normalized $v^{(0)}$.
2. Compute $w=Av^{(k-1)}$.
3. Normalize $v^{(k)}=w/\|w\|$.
4. Estimate eigenvalue by $\lambda^{(k)}=(v^{(k)})^TAv^{(k)}$.

Convergence:
If $|\lambda_1|>|\lambda_2|\ge\cdots$ and the initial vector has nonzero component in the $\lambda_1$ eigenvector direction, then

$$
\|v^{(k)}-\pm u_1\|=O\left(\left|\frac{\lambda_2}{\lambda_1}\right|^k\right),
$$

and the eigenvalue estimate converges like

$$
|\lambda^{(k)}-\lambda_1|
=O\left(\left|\frac{\lambda_2}{\lambda_1}\right|^{2k}\right).
$$

Source: `chapter3.pdf`.

Exam appearance:
- `2023May.pdf` asks proof-style convergence questions.
- `2024May.pdf` and `2025May.pdf` ask for first iterations.

Limitations:
- Only finds the dominant eigenpair.
- Slow when $|\lambda_2|$ is close to $|\lambda_1|$.

### Inverse iteration

Intuition: eigenvalues of $(A-\mu I)^{-1}$ are $(\lambda_j-\mu)^{-1}$, so the eigenvalue closest to $\mu$ becomes dominant.

Algorithm:
1. Choose shift $\mu$ and normalized $v^{(0)}$.
2. Solve $(A-\mu I)w=v^{(k-1)}$.
3. Normalize $v^{(k)}=w/\|w\|$.
4. Estimate $\lambda^{(k)}=(v^{(k)})^TAv^{(k)}$.

Convergence:
If $\lambda_J$ is closest to $\mu$ and $\lambda_K$ is second closest, then the vector convergence factor is approximately

$$
\left|\frac{\mu-\lambda_J}{\mu-\lambda_K}\right|.
$$

Source: `chapter3.pdf`.

Exam appearance:
- `2024May.pdf` asks to find PCA components using inverse iteration.

Limitations:
- Requires solving a linear system each iteration.
- If $\mu$ is an eigenvalue exactly, the shifted matrix is singular.

### Rayleigh quotient iteration

Intuition: improve inverse iteration by updating the shift to the current Rayleigh quotient.

Algorithm:
1. Normalize $v^{(0)}$.
2. Set $\lambda^{(0)}=(v^{(0)})^TAv^{(0)}$.
3. Solve $(A-\lambda^{(k-1)}I)w=v^{(k-1)}$.
4. Normalize $v^{(k)}=w/\|w\|$.
5. Set $\lambda^{(k)}=(v^{(k)})^TAv^{(k)}$.

Convergence:
For symmetric matrices, when it converges near a simple eigenpair, convergence is ultimately cubic:

$$
\|v^{(k+1)}-\pm u_J\|=O(\|v^{(k)}-\pm u_J\|^3).
$$

Source: `chapter3.pdf`.

Exam appearance:
- `2023May.pdf` asks to formulate RQI and state convergence rate.
- `2024May.pdf` asks for first RQI iterations on a Gram matrix.

### PCA framework

Intuition: choose directions where projected data have maximum variance; this is equivalent to minimizing reconstruction error.

For centered data matrix $X=[x_1,\ldots,x_n]^T$, the variance along unit vector $w$ is

$$
\sigma_w^2
=\frac{1}{n}\sum_{i=1}^{n}|\langle w,x_i\rangle|^2
=w^T\frac{X^TX}{n}w.
$$

Maximizing this subject to $\|w\|=1$ gives the eigenvalue problem

$$
Vw=\lambda w,\qquad V=\frac{X^TX}{n}.
$$

The first principal component is the eigenvector with the largest eigenvalue; later components are orthogonal eigenvectors with next largest eigenvalues.

Exam appearance:
- Past exams ask mean, centering, covariance, eigenvectors, information percentage, and MSE.

Limitations:
- Standard PCA captures linear structure.
- If data are not centered, the exam may explicitly say not to center; follow the question.

### Kernel PCA framework

Intuition: map data to a nonlinear feature space and run PCA there, but use only kernels.

Let $K_{ij}=K(x_i,x_j)=\langle\phi(x_i),\phi(x_j)\rangle_H$. Principal components in feature space have form

$$
v_j=\sum_{i=1}^{n}a_{ji}\phi(x_i).
$$

The coefficient vector satisfies

$$
Ka_j=n\lambda_j a_j
$$

for centered feature data, with normalization

$$
a_j^TKa_j=1.
$$

Projection of a new point $x$:

$$
\langle \phi(x),v_j\rangle_H
=\sum_{i=1}^{n}a_{ji}K(x,x_i).
$$

Source: `chapter3.pdf`.

Limitations:
- Centering in feature space matters unless the question says no centering.
- Kernel PCA components live in feature space, not original input space.

## 5. Important formulas / calculations, if any

### Rayleigh quotient

$$
R_A(v)=\frac{v^TAv}{v^Tv}.
$$

Use for eigenvalue estimates in power, inverse, and Rayleigh quotient iterations. If $v$ is normalized, $R_A(v)=v^TAv$.

### Power iteration expansion

If

$$
v^{(0)}=a_1u_1+\cdots+a_nu_n,
$$

then

$$
\begin{aligned}
A^kv^{(0)}
&=a_1\lambda_1^k
\left[
u_1+\sum_{j=2}^{n}\frac{a_j}{a_1}
\left(\frac{\lambda_j}{\lambda_1}\right)^k u_j
\right].
\end{aligned}
$$

Use this to prove convergence. Common mistake: omitting the condition $a_1\ne0$.

### PCA information percentage

If covariance eigenvalues are $\lambda_1\ge\lambda_2\ge\cdots\ge0$, then the percentage explained by the first $s$ components is

$$
\frac{\lambda_1+\cdots+\lambda_s}{\lambda_1+\cdots+\lambda_d}\times 100\%.
$$

Use in `2023May.pdf`-style PCA questions.

### PCA reconstruction MSE

For orthonormal components $w_1,\ldots,w_s$:

$$
\text{MSE}
=\frac{1}{n}\sum_{i=1}^{n}
\left\|x_i-\sum_{j=1}^{s}\langle x_i,w_j\rangle w_j\right\|^2.
$$

If all data are centered and components are covariance eigenvectors, the residual variance is the sum of omitted eigenvalues.

### Kernel PCA projection

$$
y_j(x)=\sum_{i=1}^{n}a_{ji}K(x,x_i).
$$

Common mistakes:
- Not normalizing $a_j$ with $a_j^TKa_j=1$.
- Projecting raw $x$ onto $a_j$ instead of using kernels.

## 6. Exam question patterns

### Pattern 1: Power iteration proof or iterations

Possible wording:
- "Use the expansion of $x_0$ to prove convergence."
- "Show the first three iterations."

How to answer:
1. If proof: expand in eigenbasis and factor out $\lambda_1^k$.
2. If calculation: multiply by $A$, normalize, compute Rayleigh quotient.
3. State convergence rate and condition if asked.

Common mistakes:
- Forgetting normalization.
- Using largest algebraic eigenvalue instead of largest absolute eigenvalue for power iteration.

Suggested answer length: one page for proof, compact table for iterations.

### Pattern 2: Inverse/Rayleigh quotient iteration

Possible wording:
- "Use inverse iteration with initial eigenvalue guess..."
- "Formulate Rayleigh quotient iteration and state convergence rate."

How to answer:
1. Write the shifted linear system.
2. Solve for $w$.
3. Normalize to get the next vector.
4. Update eigenvalue by Rayleigh quotient.
5. For RQI, state cubic convergence near a simple eigenpair.

Common mistakes:
- Multiplying by $A-\mu I$ instead of solving.
- Forgetting that inverse iteration targets the eigenvalue closest to $\mu$.

### Pattern 3: PCA calculation

Possible wording:
- "Find the mean and centered data."
- "Find covariance matrix, eigenvalues/eigenvectors, first principal component."
- "How much information is contained?"

How to answer:
1. Center data unless question says not to.
2. Compute covariance or $X^TX/n$.
3. Find eigenpairs.
4. Sort eigenvalues descending.
5. Compute projections, MSE, or explained percentage.

Common mistakes:
- Not centering when required.
- Sorting eigenvalues incorrectly.
- Treating eigenvectors with opposite sign as different answers; sign is arbitrary.

Suggested answer length: two pages for a full PCA calculation.

### Pattern 4: Kernel PCA

Possible wording:
- "Calculate the Gram matrix."
- "Use kernel PCA to find the first principal component."
- "Calculate projection or MSE in RKHS."

How to answer:
1. Build $K$ from kernel evaluations.
2. Center $K$ if required.
3. Solve Gram-matrix eigenproblem.
4. Normalize coefficients in RKHS norm.
5. Write principal component as $v=\sum_i a_iK(x_i,\cdot)$.
6. Compute projections using kernel evaluations.

Common mistakes:
- Forgetting feature-space centering.
- Writing a component in raw input coordinates for a nonlinear kernel.

## 7. Past exam connections

| Past exam | Related topic | What the question tests | Required depth |
|---|---|---|---|
| 2023May | Power iteration, inverse iteration fact, RQI; PCA and kernel PCA | Proof of convergence logic, algorithm formulation, PCA computations | Derivation plus calculation |
| 2024May | Power iteration on Gram matrix; inverse iteration for PCA; RQI for kernel PCA | First three iterative solutions and PCA/kernel PCA MSE | Algorithm execution, not just definitions |
| 2025May | PCA, SVD, power iteration, kernel PCA | First PC, MSE, SVD, Gram/eigenfunction form, comparison of PCA vs kernel PCA | Full calculation and interpretation |

## 8. Worked mini-examples / answer templates

### Power iteration template

"Let $v^{(0)}=\sum_i a_i u_i$ with $a_1\ne0$. Then $A^kv^{(0)}=a_1\lambda_1^k[u_1+\sum_{i>1}(a_i/a_1)(\lambda_i/\lambda_1)^ku_i]$. Since $|\lambda_i/\lambda_1|<1$ for $i>1$, the bracket tends to $u_1$. After normalization, the iterates align with $\pm u_1$. The convergence rate is governed by $|\lambda_2/\lambda_1|$."

### PCA calculation template

"Compute the mean $m=\frac{1}{n}\sum_i x_i$ and centered data $\tilde x_i=x_i-m$. Form $V=\frac{1}{n}\sum_i\tilde x_i\tilde x_i^T$. Solve $Vw_j=\lambda_jw_j$ and order eigenvalues descending. The first principal component is $w_1$. The information retained by the first $s$ components is $(\sum_{j=1}^s\lambda_j)/(\sum_j\lambda_j)$."

### Kernel PCA template

"Define $K_{ij}=K(x_i,x_j)$. Solve $Ka=n\lambda a$ for the leading eigenvectors. Normalize by $a^TKa=1$. Then the first feature-space principal component is $v=\sum_i a_i\phi(x_i)$, and a new point $x$ has coordinate $\langle\phi(x),v\rangle=\sum_i a_iK(x,x_i)$."

## 9. Common mistakes and confusing points

- Confusing largest absolute eigenvalue with largest algebraic eigenvalue in power iteration.
- Forgetting the initial vector must have nonzero component in the target eigenvector direction.
- Treating inverse iteration as repeated matrix multiplication instead of solving a shifted system.
- Stating RQI is cubic without the local/simple-eigenvalue assumption.
- Forgetting to center PCA data unless told otherwise.
- Computing covariance as $XX^T$ when the eigenvectors requested are in feature dimension; understand whether data are columns or rows.
- Not normalizing kernel PCA coefficients in RKHS norm.
- Comparing PCA and kernel PCA using raw-space intuition only; kernel PCA can capture nonlinear feature-space variance.

## 10. Quick revision checklist

Before the exam, I should be able to:

- [ ] Define and use the Rayleigh quotient.
- [ ] Run three iterations of power iteration by hand.
- [ ] Prove the basic convergence idea of power iteration.
- [ ] Run inverse iteration with a fixed shift.
- [ ] Formulate Rayleigh quotient iteration and state cubic convergence.
- [ ] Compute PCA mean, centered data, covariance, eigenvalues, and components.
- [ ] Compute explained variance percentages.
- [ ] Compute PCA reconstruction MSE.
- [ ] Build a Gram matrix for kernel PCA.
- [ ] Normalize kernel PCA coefficients and compute projections.
- [ ] Explain why PCA works and why kernel PCA can capture nonlinear structure.
