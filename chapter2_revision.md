# Chapter 2 Revision Notes

## 1. Chapter overview

Chapter 2 is about numerical linear algebra for solving linear systems and compressing matrices. The exam repeatedly turns these ideas into calculations: perform LU factorization, solve a system without forming an inverse, run Jacobi iterations, explain convergence, compute condition number effects, and use SVD for low-rank approximation.

This chapter is high-yield because it supplies the linear algebra used by KRR, PCA, kernel PCA, and numerical eigenvalue methods. Sources: `chapter2.pdf`, `Final-revision.pdf`, `2024May.pdf`, `2025May.pdf`.

## 2. High-priority topics from final revision

| Priority | Topic | Why it matters |
|---|---|---|
| High priority | Gaussian elimination and LU decomposition | Final revision asks complexity and whether you can perform LU and solve a system. |
| High priority | Doolittle LU | Tested in `2024May.pdf`; tridiagonal LU tested in `2025May.pdf`. |
| High priority | Pivoting | Final revision directly asks what pivoting is and what it is used for. |
| High priority | Stability and condition number | Final revision directly asks stability and condition number. |
| High priority | Jacobi and Gauss-Seidel methods | Final revision asks complexity, error equation, and convergence. |
| High priority | Low-rank approximation and SVD | Final revision asks how to get low-rank approximations and why they are needed. |
| Medium priority | Matrix norms | Needed for condition number and error bounds. |
| Medium priority | Sparse matrix motivation | Useful background for iterative methods. |
| Low priority / background only | Optional storage formats and long application slides | Useful context, but not likely as detailed exam questions. |

## 3. Key concepts and definitions

- Linear system: $Ax=b$, where $A$ is a matrix, $x$ is unknown, and $b$ is known.

- Direct method: a method such as Gaussian elimination or LU factorization that solves in finitely many algebraic steps in exact arithmetic. It may still suffer from roundoff in floating-point arithmetic.

- Forward substitution: solve $Lx=b$ when $L$ is lower triangular.

- Back substitution: solve $Ux=b$ when $U$ is upper triangular.

- LU factorization: factor $A=LU$, where $L$ is lower triangular and $U$ is upper triangular. With Doolittle, $L$ has diagonal entries equal to 1. Source: `chapter2.pdf`.

- Pivoting: row exchange during elimination to avoid zero or very small pivots. It improves numerical stability and prevents division by bad pivot elements.

- Stability/sensitivity: how errors in input data, especially $b$, affect the computed solution $x$.

- Condition number:

$$
\kappa(A)=\|A\|\|A^{-1}\|.
$$

Large $\kappa(A)$ means small relative input errors can cause large relative solution errors. Source: `chapter2.pdf`.

- Iterative method: repeatedly improve approximations $x^{(k)}$ instead of solving exactly in one factorization.

- Jacobi method: updates every component using only values from the previous iteration.

- Gauss-Seidel method: updates components sequentially and immediately reuses newly computed values.

- SVD: for $A\in\mathbb{R}^{m\times n}$,

$$
A=U\Sigma V^T,
$$

where $U,V$ are orthonormal and $\Sigma$ contains nonnegative singular values $\sigma_1\ge\sigma_2\ge\cdots\ge0$. Source: `chapter2.pdf`.

- Low-rank approximation: approximate $A$ by a matrix of smaller rank to reduce storage, computation, or noise.

## 4. Core theories / models / frameworks

### Gaussian elimination and LU

Intuition: reduce a general system to triangular systems, which are easy to solve.

Key components:
- Elimination multipliers.
- Upper triangular matrix $U$.
- Lower triangular matrix $L$ storing multipliers.
- Optional row permutations when pivoting is needed.

How it is used:
- Solve $Ax=b$ by $A=LU$, then solve $Ly=b$ and $Ux=y$.
- Reuse $LU$ for multiple right-hand sides.

Exam appearance:
- `2024May.pdf` asks for Doolittle LU of $K+\alpha I$ and then solving without inversion.
- `2025May.pdf` asks for general LU pattern of a tridiagonal matrix.

Limitations:
- Expensive for huge dense systems.
- Without pivoting, small pivots can cause large roundoff error or failure.

### Pivoting and stability

Intuition: dividing by a tiny pivot amplifies roundoff. Pivoting chooses a better pivot, usually by swapping with a row having a larger absolute entry.

How it is used:
- If a pivot is zero, row exchange is necessary.
- If a pivot is nonzero but tiny, row exchange is recommended for numerical stability.

Exam appearance:
- Explanation question: "What is pivoting, and what is it used for?"

Common limitation:
- Pivoting changes row order; if writing $PA=LU$, include the permutation matrix if asked formally.

### Iterative solvers

Intuition: split $A$ into easier parts, then repeatedly solve a simpler equation.

For $A=D+R$, Jacobi is

$$
x^{(k+1)}=D^{-1}(b-Rx^{(k)}).
$$

For $A=L_*+U$, where $L_*$ includes the diagonal and lower triangular part, Gauss-Seidel is

$$
x^{(k+1)}=L_*^{-1}(b-Ux^{(k)}).
$$

Convergence framework:
- Write an iteration as $x^{(k+1)}=Bx^{(k)}+c$.
- Error satisfies $e^{(k+1)}=Be^{(k)}$.
- Convergence is guaranteed if $\rho(B)<1$; a sufficient condition is $\|B\|<1$.

Exam appearance:
- `2025May.pdf` asks for first three Jacobi iterations and whether Jacobi converges.

Limitations:
- Jacobi is parallel-friendly but may be slower.
- Gauss-Seidel often converges faster but is sequential and order-dependent.

### SVD and low-rank approximation

Intuition: singular values rank directions by importance. Keeping the largest $k$ singular values gives the best rank-$k$ approximation in standard matrix norms.

Given

$$
A=\sum_{i=1}^{r}\sigma_i u_i v_i^T,
$$

the rank-$k$ truncated SVD is

$$
A_k=\sum_{i=1}^{k}\sigma_i u_i v_i^T.
$$

Eckart-Young-Mirsky theorem:

$$
\|A-A_k\|_2=\sigma_{k+1},\qquad
\|A-A_k\|_F=\sqrt{\sigma_{k+1}^2+\cdots+\sigma_r^2}.
$$

How it is used:
- Image compression.
- Noise reduction.
- Matrix completion and recommendation systems.
- PCA connections.

Exam appearance:
- `2025May.pdf` asks to write down SVD and compare PCA vs kernel PCA performance.

Limitations:
- Full SVD is expensive for large matrices.
- Low-rank approximation is good only when singular values decay sufficiently.

## 5. Important formulas / calculations, if any

### Gaussian elimination complexity

Forward elimination for dense $n\times n$ systems has leading cost

$$
O(n^3),
$$

more specifically about $\frac{2}{3}n^3$ operations in the chapter notes. Back substitution costs $O(n^2)$.

Common mistake: saying triangular solve is $O(n^3)$; it is $O(n^2)$.

### Matrix multiplication complexity

For $A\in\mathbb{R}^{m\times n}$ and $B\in\mathbb{R}^{n\times r}$, the standard matrix product $AB$ costs

$$
O(mnr).
$$

For two $n\times n$ matrices, this is $O(n^3)$. This appeared directly in `2025May.pdf`.

### Doolittle LU formulas

For $A=LU$ with $\ell_{kk}=1$:

$$
u_{kj}=a_{kj}-\sum_{s=1}^{k-1}\ell_{ks}u_{sj},\qquad j=k,\ldots,n,
$$

$$
\ell_{ik}=\frac{a_{ik}-\sum_{s=1}^{k-1}\ell_{is}u_{sk}}{u_{kk}},\qquad i=k+1,\ldots,n.
$$

Use when asked to compute LU by hand. Common mistake: dividing when computing $u_{kj}$; only $\ell_{ik}$ has division by $u_{kk}$.

### Condition number error bound

If $A$ is exact and $b$ is perturbed to $b+\delta b$, then

$$
\frac{\|\delta x\|}{\|x\|}
\le
\kappa(A)\frac{\|\delta b\|}{\|b\|},
\qquad
\kappa(A)=\|A\|\|A^{-1}\|.
$$

Use this to explain stability. A large condition number means the system is ill-conditioned.

### Jacobi update

$$
x_i^{(k+1)}
=\frac{1}{a_{ii}}\left(b_i-\sum_{j\ne i}a_{ij}x_j^{(k)}\right).
$$

Step method:
1. Isolate each unknown.
2. Substitute the previous iteration values for all right-side unknowns.
3. Repeat for the requested number of iterations.

### Gauss-Seidel update

$$
x_i^{(k+1)}
=\frac{1}{a_{ii}}\left(
b_i-\sum_{j<i}a_{ij}x_j^{(k+1)}
-\sum_{j>i}a_{ij}x_j^{(k)}
\right).
$$

Common mistake: using old values for all terms, which turns Gauss-Seidel into Jacobi.

### SVD computation route

For $A\in\mathbb{R}^{m\times n}$:
1. Compute $A^TA$.
2. Find eigenpairs $A^TAv_i=\sigma_i^2v_i$.
3. Singular values are $\sigma_i=\sqrt{\lambda_i}$.
4. Left singular vectors are $u_i=Av_i/\sigma_i$ for nonzero $\sigma_i$.
5. Write $A=U\Sigma V^T$.

Common mistakes:
- Forgetting singular values are nonnegative.
- Confusing eigenvalues of $A$ with singular values of $A$.
- Not normalizing singular vectors.

## 6. Exam question patterns

### Pattern 1: LU factorization and solve

Possible wording:
- "Can you use Doolittle LU factorization to obtain the LU decomposition?"
- "Hence calculate $(K+\alpha I)^{-1}y$ without inverting the matrix."

How to answer:
1. Confirm pivots are nonzero or state if pivoting is needed.
2. Compute $L$ and $U$.
3. Solve $Lz=y$ by forward substitution.
4. Solve $Ux=z$ by back substitution.

Common mistakes:
- Computing $A^{-1}$ explicitly when the question says without inversion.
- Skipping substitution steps.

Suggested answer length: one to two pages for a full numerical LU.

### Pattern 2: Jacobi iterations and convergence

Possible wording:
- "Solve $Ax=b$ using Gauss-Jacobi with $x^{(0)}=0$. Present the first three iterations."
- "Will Jacobi converge? Explain."

How to answer:
1. Write component update formulas.
2. Compute $x^{(1)},x^{(2)},x^{(3)}$.
3. For convergence, use strict diagonal dominance, positive definiteness where applicable, or iteration matrix criterion.

Common mistakes:
- Mixing Jacobi and Gauss-Seidel updates.
- Giving only the final vector without showing iteration calculations.

Suggested answer length: calculation table plus 3-5 lines of convergence explanation.

### Pattern 3: Stability / condition number explanation

Possible wording:
- "What is the stability of a linear equation system?"
- "What is the condition number?"

How to answer:
1. Define condition number.
2. State the relative error bound.
3. Interpret large vs small condition number.
4. Connect to pivoting or roundoff if relevant.

Suggested answer length: 1/2 page.

### Pattern 4: SVD and low-rank approximation

Possible wording:
- "Write down the SVD decomposition for $X$."
- "How do we get the low-rank approximation, and why do we need it?"

How to answer:
1. Find or state singular values and vectors.
2. Write $X=U\Sigma V^T$.
3. Keep top $k$ singular values for $X_k$.
4. Explain compression, denoising, or computation saving.
5. State error if asked.

Suggested answer length: one page for derivation; shorter for explanation.

### Pattern 5: Matrix multiplication complexity

Possible wording:
- "What is the complexity of calculating $AB$ for two $n\times n$ matrices?"
- "What is the complexity where $A$ is $m\times n$ and $B$ is $n\times r$?"

How to answer:
1. State the output size.
2. Each output entry is one length-$n$ dot product.
3. There are $mr$ output entries, so the cost is $O(mnr)$.

Suggested answer length: 3-5 lines.

## 7. Past exam connections

| Past exam | Related topic | What the question tests | Required depth |
|---|---|---|---|
| 2023May | No main Chapter 2 linear-system question | Chapter 2 appears indirectly through eigenvalue/PCA computations | Background linear algebra only |
| 2024May | Doolittle LU, solving without inverse | Factor $K+\alpha I$ and solve using forward/back substitution | Full numerical factorization |
| 2025May | Tridiagonal LU, Jacobi iterations, convergence, matrix multiplication complexity | General LU pattern, exact solve, first three Jacobi iterations, convergence reasoning, $O(mnr)$ product cost | Calculation plus convergence explanation |

## 8. Worked mini-examples / answer templates

### Jacobi convergence template

"For Jacobi, write $A=D+R$ and $x^{(k+1)}=D^{-1}(b-Rx^{(k)})$. The iteration matrix is $B_J=-D^{-1}R$. The error satisfies $e^{(k+1)}=B_Je^{(k)}$. Therefore the method converges if $\rho(B_J)<1$; a common sufficient condition is strict diagonal dominance. Since the given tridiagonal matrix has diagonal entries 2 and off-diagonal sum at most 2, check endpoints/interior carefully before claiming strict diagonal dominance."

### LU solve template

"After obtaining $A=LU$, solve $Ly=b$ first. Since $L$ is lower triangular, compute $y_1,y_2,\ldots,y_n$ in order. Then solve $Ux=y$ by back substitution from $x_n$ to $x_1$. This avoids computing $A^{-1}$ and is numerically preferable."

### SVD low-rank template

"Let $A=U\Sigma V^T=\sum_i\sigma_i u_iv_i^T$. The best rank-$k$ approximation is $A_k=\sum_{i=1}^k\sigma_i u_iv_i^T$. It is best in spectral and Frobenius norms, with errors $\|A-A_k\|_2=\sigma_{k+1}$ and $\|A-A_k\|_F=(\sum_{i>k}\sigma_i^2)^{1/2}$."

## 9. Common mistakes and confusing points

- Saying LU always exists without pivoting.
- Forgetting pivoting when a pivot is zero or tiny.
- Computing an inverse instead of solving triangular systems.
- Mixing Doolittle convention with other LU conventions.
- Using old Jacobi values in Gauss-Seidel, or new Gauss-Seidel values in Jacobi.
- Claiming convergence without checking diagonal dominance, SPD, or iteration matrix.
- Treating condition number as an absolute error bound instead of a relative error amplification factor.
- Confusing singular values with eigenvalues.
- Forgetting low-rank approximation is useful only when singular values decay.

## 10. Quick revision checklist

Before the exam, I should be able to:

- [ ] Perform Gaussian elimination on a small matrix.
- [ ] Explain and compute Doolittle LU.
- [ ] Solve $Ax=b$ using $LU$ without forming $A^{-1}$.
- [ ] Explain pivoting and why it improves stability.
- [ ] Define condition number and use the relative error bound.
- [ ] Write Jacobi and Gauss-Seidel iteration formulas.
- [ ] Derive the iterative error equation.
- [ ] State convergence conditions for Jacobi and Gauss-Seidel.
- [ ] Compute the first few iterations of Jacobi by hand.
- [ ] Compute SVD from $A^TA$ for a small matrix.
- [ ] Write the truncated SVD and its approximation errors.
- [ ] Explain why low-rank approximation matters in AI/ML.
