# Final Revision Quick Review

Source basis: `Final-revision.pdf` is the main source. Past exam style is used only to calibrate what "details steps or derivations" usually means.

Exam signal from final revision:
- Exam format: 8 long questions, 18:30-21:00, May 18, 2026.
- The lecturer explicitly says to write detailed steps or derivations.
- Some questions test understanding of methods; for these, clear technical reasoning matters more than long calculation.
- One handwritten A4 sheet, both sides, is allowed.

Use this file as a final fast checklist: for every topic below, you should be able to write the formula, the algorithm steps, the convergence/assumption statement, and one common trap.

---

## 1. Highest-Yield Map From Final Revision

| Priority | Topic | Final revision asks | What to prepare |
|---|---|---|---|
| Very high | PCA and kernel PCA | How to find dominant components? Why does PCA/kernel PCA work? | Eigenproblem, variance/MSE interpretation, kernel PCA loading vs score. |
| Very high | Eigenvalue iterations | Power, inverse, Rayleigh quotient iteration: implementation, convergence conditions, rates. | Step-by-step iteration tables and convergence statements. |
| Very high | ODE solvers | How to solve ODEs numerically? Stability and convergence rates? | Euler/theta methods, local/global error derivation. |
| Very high | CNN calculations | Cross-correlation, layer output sizes, parameter counts, convolution backprop. | Shape formula, parameter formula, gradient formula. |
| Very high | Kernel methods and KRR | PD kernels, Aronszajn theorem, kernel trick, representer theorem, KRR matrix form. | Gram matrix, RKHS argument, KRR derivation. |
| High | LU / Gaussian elimination | Complexity, LU decomposition, solving a system. | Doolittle/LU steps, forward/back substitution. |
| High | Jacobi / Gauss-Seidel | Complexity, error equation, convergence. | Splitting notation, iteration matrix, spectral-radius condition. |
| High | SVD and low-rank approximation | How to get low-rank approximation, why needed? | SVD construction, Eckart-Young, compression/noise reduction. |
| High | ResNet and Neural ODE | What are they? How to compute gradient? Memory requirements? | Residual block as Euler step, adjoint method, memory tradeoff. |
| Medium | Pivoting, stability, condition number | What are they used for? | Conceptual answer with one formula. |

---

## 2. CNN Questions

Source: `Final-revision.pdf`, page 2.

### 2.1 CNN Inductive Biases

Likely wording:
- "What are the inductive biases/features of CNNs compared with fully connected neural networks?"

Answer skeleton:
1. CNNs assume data have local spatial structure.
2. Sparse local connectivity: each filter looks at a local receptive field, not all pixels.
3. Parameter sharing: the same filter is reused at all spatial locations.
4. Translation-related behavior: features can be detected regardless of position; pooling can add local invariance.
5. Fewer parameters than fully connected layers, so better sample efficiency for image/grid data.

Do not write only "CNNs are good for images." Give the technical reasons: locality, sparse connectivity, parameter sharing, translation behavior.

### 2.2 Cross-Correlation

Likely wording:
- "What is cross-correlation?"
- "Compute the output of this convolutional layer."

Use slide convention: CNN "convolution" usually means cross-correlation, with no filter flip.

For a 2D input $X$ and filter $K$,

$$
Y_{i,j}=\sum_{a,b}K_{a,b}X_{i+a,j+b}.
$$

For multi-channel input and output channel $m$,

$$
Y_{i,j,m}=\sum_{c=1}^{C_{\text{in}}}\sum_{a,b}W_{a,b,c,m}X_{i+a,j+b,c}.
$$

Common trap: flipping the filter as in mathematical convolution when the question/slides use cross-correlation.

### 2.3 CNN Output Shape and Parameters

For input height $H$, width $W$, filter size $F_H\times F_W$, padding $P_H,P_W$, stride $S_H,S_W$:

$$
H_{\text{out}}=\left\lfloor\frac{H+2P_H-F_H}{S_H}\right\rfloor+1,\qquad
W_{\text{out}}=\left\lfloor\frac{W+2P_W-F_W}{S_W}\right\rfloor+1.
$$

Number of convolution weights:

$$
F_HF_WC_{\text{in}}C_{\text{out}}
$$

Add $C_{\text{out}}$ only if the question includes bias terms.

Answer process:
1. Write input size including channels.
2. Apply padding.
3. Apply shape formula.
4. Count weights using filter size times input channels times output channels.
5. For pooling, update shape but usually add no trainable weights.
6. For flatten, multiply all dimensions.

### 2.4 Backpropagation for Convolution Layer

Likely wording:
- "Can you use backpropagation for a convolutional layer to calculate the gradient with respect to the parameters?"

If $G_{i,j,m}=\partial L/\partial Y_{i,j,m}$, then

$$
\frac{\partial L}{\partial W_{a,b,c,m}}
=\sum_{i,j}G_{i,j,m}X_{i+a,j+b,c}.
$$

Interpretation: each filter weight receives the sum of upstream gradients multiplied by the input entries it touched at every spatial location.

Common trap: forgetting parameter sharing. Because the same filter is reused, gradients from all spatial positions must be summed.

---

## 3. Kernel Methods and KRR

Source: `Final-revision.pdf`, page 2.

### 3.1 Positive Definite Kernel

Likely wording:
- "Show that this kernel is positive definite."
- "What is a positive definite kernel?"

Definition: a symmetric kernel $K$ is positive definite if for any points $x_1,\ldots,x_n$ and coefficients $c_1,\ldots,c_n$,

$$
\sum_{i=1}^{n}\sum_{j=1}^{n}c_ic_jK(x_i,x_j)\ge 0.
$$

Equivalent matrix form: the Gram matrix $K_{ij}=K(x_i,x_j)$ is positive semidefinite for every finite sample.

Useful proof routes:
- Inner product form: if $K(x,y)=\langle \Psi(x),\Psi(y)\rangle_{\mathcal H}$, then

$$
\sum_{i,j}c_ic_jK(x_i,x_j)
=\left\|\sum_i c_i\Psi(x_i)\right\|_{\mathcal H}^2\ge0.
$$

- Closure properties: sums, products, positive scalar multiples, and limits of positive definite kernels are positive definite when conditions hold.

### 3.2 Aronszajn Theorem and Kernel Trick

Aronszajn theorem: every positive definite kernel corresponds to a unique RKHS where

$$
K(x,y)=\langle K(x,\cdot),K(y,\cdot)\rangle_{\mathcal H}.
$$

Exam explanation:
- Positive definite kernel guarantees a valid feature-space inner product.
- The kernel trick avoids explicitly constructing $\Psi(x)$.
- Replace $\langle \Psi(x_i),\Psi(x_j)\rangle_{\mathcal H}$ by $K(x_i,x_j)$.

### 3.3 Representer Theorem

Likely wording:
- "State/use the representer theorem."

For a regularized empirical risk problem in an RKHS,

$$
\min_{f\in\mathcal H}\frac{1}{n}\sum_{i=1}^{n}L(f(x_i),y_i)+\beta\|f\|_{\mathcal H}^2,
$$

the minimizer has the finite form

$$
\hat f(x)=\sum_{i=1}^{n}\varsigma_iK(x_i,x).
$$

Use this to turn an infinite-dimensional optimization problem into a finite-dimensional coefficient problem.

### 3.4 KRR Matrix Form

KRR objective in slide-compatible notation:

$$
\min_{f\in\mathcal H}
\frac{1}{n}\sum_{i=1}^{n}(f(x_i)-y_i)^2+\beta\|f\|_{\mathcal H}^2.
$$

By representer theorem, write

$$
f(x)=\sum_{i=1}^{n}\varsigma_iK(x_i,x).
$$

Let $K_{ij}=K(x_i,x_j)$, $y=(y_1,\ldots,y_n)^T$. Then fitted values are $K\varsigma$, and the finite objective is

$$
\frac{1}{n}\|K\varsigma-y\|_2^2+\beta\varsigma^TK\varsigma.
$$

The normal equation gives

$$
(K+\beta nI)\varsigma=y,
$$

so

$$
\varsigma=(K+\beta nI)^{-1}y,\qquad
\hat f(x)=\sum_i\varsigma_iK(x_i,x).
$$

Common traps:
- Solving for coefficients but not writing the final predictor $\hat f(x)$.
- Treating KRR as ordinary ridge regression on raw input coordinates.
- Forgetting the factor $n$ when the loss is averaged by $1/n$.

---

## 4. Direct and Iterative Linear Solvers

Source: `Final-revision.pdf`, page 3.

### 4.1 Gaussian Elimination and LU

Likely wording:
- "Perform LU decomposition."
- "Use LU to solve $Ax=b$."
- "What is the computational complexity?"

Core facts:
- Gaussian elimination / dense LU factorization costs $O(n^3)$.
- Once $A=LU$, solving $Ax=b$ costs $O(n^2)$ by forward and backward substitution.
- Never form $A^{-1}$ unless explicitly asked.

Answer process:
1. Factor $A=LU$.
2. Solve $Ly=b$ by forward substitution.
3. Solve $Ux=y$ by backward substitution.
4. State final $x$.

For Doolittle LU, usually $L$ has unit diagonal.

### 4.2 Pivoting, Stability, Condition Number

Pivoting:
- Row swaps during elimination.
- Used to avoid zero or tiny pivots.
- Improves numerical stability.

Stability of linear system:
- A system is sensitive if small perturbations in $A$ or $b$ can create large perturbations in $x$.

Condition number:

$$
\kappa(A)=\|A\|\|A^{-1}\|.
$$

For perturbation in $b$:

$$
\frac{\|\delta x\|}{\|x\|}
\le
\kappa(A)\frac{\|\delta b\|}{\|b\|}.
$$

Interpretation: large $\kappa(A)$ means ill-conditioned; the problem itself is sensitive even if the algorithm is correct.

### 4.3 Jacobi Method

Final revision asks: complexity, error equation, convergence.

Slide notation:

$$
A=D+R.
$$

Jacobi iteration:

$$
x^{(k+1)}=D^{-1}(b-Rx^{(k)}).
$$

Component form:

$$
x_i^{(k+1)}
=\frac{1}{a_{ii}}
\left(b_i-\sum_{j\ne i}a_{ij}x_j^{(k)}\right).
$$

Error equation:

$$
e^{(k+1)}=B_Je^{(k)},\qquad B_J=-D^{-1}R.
$$

Convergence condition:

$$
\rho(B_J)<1.
$$

Sufficient condition: strict diagonal dominance often implies convergence, but check the exact matrix because some exam matrices are not strictly diagonally dominant everywhere.

Common trap: Jacobi uses only old values from iteration $k$.

### 4.4 Gauss-Seidel Method

Write $A=L+D+U$ where $L$ is strictly lower triangular and $U$ strictly upper triangular.

$$
x^{(k+1)}=(D+L)^{-1}(b-Ux^{(k)}).
$$

Component form:

$$
x_i^{(k+1)}
=\frac{1}{a_{ii}}
\left(b_i-\sum_{j<i}a_{ij}x_j^{(k+1)}
-\sum_{j>i}a_{ij}x_j^{(k)}\right).
$$

Common trap: using all old values turns Gauss-Seidel into Jacobi.

### 4.5 Low-Rank Approximation and SVD

Final revision asks:
- "How to get the low-rank approximation?"
- "Why do we need it?"

SVD:

$$
A=U\Sigma V^T=\sum_{i=1}^{r}\sigma_iu_iv_i^T,\qquad
\sigma_1\ge\sigma_2\ge\cdots\ge0.
$$

Best rank-$k$ approximation:

$$
A_k=\sum_{i=1}^{k}\sigma_iu_iv_i^T.
$$

Error:

$$
\|A-A_k\|_2=\sigma_{k+1},\qquad
\|A-A_k\|_F^2=\sum_{i>k}\sigma_i^2.
$$

Why needed:
- compression,
- denoising,
- dimensionality reduction,
- efficient storage and computation,
- foundation for PCA.

---

## 5. Eigenvalue Solvers

Source: `Final-revision.pdf`, page 4.

### 5.1 Rayleigh Quotient

For nonzero $v$,

$$
R_A(v)=\frac{v^TAv}{v^Tv}.
$$

If $\|v\|_2=1$, then $R_A(v)=v^TAv$.

Use it to estimate the eigenvalue corresponding to a current eigenvector estimate.

### 5.2 Power Iteration

Goal: dominant eigenpair.

Algorithm:
1. Start with $v^{(0)}$ having nonzero component in dominant eigenvector direction.
2. Compute $w^{(k+1)}=Av^{(k)}$.
3. Normalize:

$$
v^{(k+1)}=\frac{w^{(k+1)}}{\|w^{(k+1)}\|}.
$$

4. Estimate eigenvalue:

$$
\omega^{(k+1)}=R_A(v^{(k+1)}).
$$

Convergence condition:
- dominant eigenvalue is unique in magnitude: $|\lambda_1|>|\lambda_2|$;
- starting vector has nonzero component in the dominant eigenvector direction.

Rate:

$$
O\left(\left|\frac{\lambda_2}{\lambda_1}\right|^k\right).
$$

### 5.3 Inverse Iteration

Goal: eigenvalue closest to shift $\mu$.

Algorithm:
1. Choose shift $\mu$.
2. Solve

$$
(A-\mu I)w^{(k+1)}=v^{(k)}.
$$

3. Normalize:

$$
v^{(k+1)}=\frac{w^{(k+1)}}{\|w^{(k+1)}\|}.
$$

4. Estimate eigenvalue by $R_A(v^{(k+1)})$.

Common trap: do not compute $(A-\mu I)^{-1}$ explicitly; solve the linear system.

### 5.4 Rayleigh Quotient Iteration

Algorithm:
1. Start with normalized $v^{(0)}$.
2. Set shift

$$
\mu^{(k)}=R_A(v^{(k)}).
$$

3. Solve

$$
(A-\mu^{(k)}I)w^{(k+1)}=v^{(k)}.
$$

4. Normalize $v^{(k+1)}=w^{(k+1)}/\|w^{(k+1)}\|$.
5. Repeat.

Convergence:
- for symmetric matrices and a sufficiently good initial vector near a simple eigenpair, convergence is usually cubic.
- can fail or behave poorly if the shift makes the system nearly singular too early or the starting vector is poor.

---

## 6. PCA and Kernel PCA

Source: `Final-revision.pdf`, page 4.

### 6.1 Why PCA Works

PCA finds directions that maximize projected variance. For centered data $x_i\in\mathbb R^d$, define covariance/scatter matrix

$$
C=\frac{1}{n}\sum_{i=1}^{n}x_ix_i^T.
$$

The first principal component loading $v_1$ solves

$$
\max_{\|v\|=1}v^TCv.
$$

The solution is the eigenvector of $C$ with largest eigenvalue.

Interpretations:
- largest variance direction;
- best rank-one reconstruction direction;
- eigenvalue equals variance explained along that direction.

### 6.2 PCA Answer Process

For a calculation question:
1. Center the data if required.
2. Form $C=(1/n)XX^T$ or the relevant scatter/covariance matrix.
3. Find eigenvalues/eigenvectors.
4. Loading/component: eigenvector $v_j$ in original data space.
5. Score for sample $x_i$: $v_j^Tx_i$.
6. Explained variance ratio:

$$
\frac{\sum_{j=1}^{s}\lambda_j}{\sum_j\lambda_j}.
$$

7. Reconstruction/MSE: use discarded eigenvalues if the question asks for low-rank approximation error.

Common trap: confusing loading/component with score. Loading is a direction; score is a projection value.

### 6.3 Kernel PCA: Correct Slide-Compatible Notation

Kernel PCA performs PCA in feature space $\mathcal H$ using the kernel map $\Psi$:

$$
K_{ij}=K(x_i,x_j)=\langle \Psi(x_i),\Psi(x_j)\rangle_{\mathcal H}.
$$

The actual RKHS principal component/loading is in $\mathcal H$:

$$
v_j=\sum_{i=1}^{n}a_{ji}\Psi(x_i).
$$

The coefficient vector $a_j$ is found from the Gram matrix eigenproblem:

$$
Ka_j=n\omega_ja_j,
$$

with normalization

$$
a_j^TKa_j=1.
$$

If instead $Kq_j=\rho_jq_j$ and $\|q_j\|_2=1$, then

$$
a_j=\frac{q_j}{\sqrt{\rho_j}},\qquad \rho_j=n\omega_j.
$$

Projection/score for a point $x$:

$$
\langle \Psi(x),v_j\rangle_{\mathcal H}
=\sum_{i=1}^{n}a_{ji}K(x_i,x).
$$

Important distinction:
- $v_j=\sum_i a_{ji}\Psi(x_i)$ is the Hilbert-space loading/component.
- $Ka_j$ is the vector of training scores, not the loading.
- $K(x_i,\cdot)$ is the canonical RKHS function representation; for slide notation, write the loading with $\Psi(x_i)$.

### 6.4 Kernel PCA Answer Process

1. Build Gram matrix $K$.
2. Center $K$ if the question requires centered feature-space data.
3. Solve the Gram eigenproblem.
4. Normalize coefficients using $a^TKa=1$.
5. Write the actual feature-space loading:

$$
v=\sum_i a_i\Psi(x_i).
$$

6. If asked for scores/projections, then use

$$
y(x)=\sum_i a_iK(x_i,x).
$$

Common traps:
- Returning $Ka$ when the question asks for the first principal component/loading $v$.
- Writing a raw-space vector when the answer should be in $\mathcal H$.
- Forgetting coefficient normalization.

---

## 7. ODE Solvers, ResNet, Neural ODE

Source: `Final-revision.pdf`, page 5.

### 7.1 Numerical ODE Solvers

For IVP

$$
x'(t)=f(t,x(t)),\qquad x(t_0)=x_0,
$$

with step size $h$, grid $t_i=t_0+ih$.

Forward Euler:

$$
x_{i+1}=x_i+hf(t_i,x_i).
$$

Backward Euler:

$$
x_{i+1}=x_i+hf(t_{i+1},x_{i+1}).
$$

Theta-method:

$$
x_{i+1}
=x_i+h\left[(1-\theta)f(t_i,x_i)+\theta f(t_{i+1},x_{i+1})\right].
$$

Special cases:
- $\theta=0$: Forward Euler, explicit, first order.
- $\theta=1$: Backward Euler, implicit, first order.
- $\theta=1/2$: trapezoidal/Crank-Nicolson style, implicit, usually second order for smooth problems.

### 7.2 Stability and Convergence

Local truncation error: error made in one step if starting from the exact solution.

Global error: accumulated error over the whole interval.

Typical exam derivation:
1. Substitute exact solution into the numerical scheme.
2. Use Taylor expansion.
3. Identify local truncation error order.
4. Write error recurrence.
5. Use Lipschitz bound/Gronwall-style argument to bound global error.

Common statements:
- Forward Euler: local truncation error $O(h^2)$ per step, global error $O(h)$.
- Theta-method with $\theta=1/2$: under smoothness assumptions, global error is often $O(h^2)$.

Common trap: saying "local error $O(h^2)$ so global error $O(h^2)$" for Forward Euler. The global error is usually one order lower.

### 7.3 ResNet

Final revision asks: "What is the ResNet?"

Residual block:

$$
u_{l+1}=u_l+F(u_l,\theta_l).
$$

As Euler discretization:

$$
u_{l+1}=u_l+hf(u_l,\theta_l).
$$

Why it helps:
- skip connection gives an identity path;
- improves gradient flow;
- makes it easier to train deeper networks;
- can be interpreted as a discretized dynamical system.

Common trap: explaining ResNet only as "a deeper CNN" without mentioning the residual/skip connection.

### 7.4 Neural ODE

Final revision asks:
- "What is Neural ODEs?"
- "How to compute the gradient?"
- "Do you know the memory requirements for ResNet and Neural ODE?"

Neural ODE:

$$
\frac{dx(t)}{dt}=f(x(t),t,\theta),\qquad
x(t_1)=\operatorname{ODESolve}(f,x(t_0),t_0,t_1).
$$

It is a continuous-depth model: instead of applying finitely many residual blocks, evolve the hidden state by an ODE.

### 7.5 Adjoint Gradient Method

Let loss be $L(x(t_1))$. Define adjoint

$$
a(t)=\frac{\partial L}{\partial x(t)}.
$$

Terminal condition:

$$
a(t_1)=\frac{\partial L}{\partial x(t_1)}.
$$

Adjoint equation:

$$
\frac{da(t)}{dt}
=-a(t)^T\frac{\partial f(x(t),t,\theta)}{\partial x}.
$$

Parameter gradient:

$$
\frac{\partial L}{\partial\theta}
=\int_{t_0}^{t_1}
a(t)^T
\frac{\partial f(x(t),t,\theta)}{\partial\theta}
\,dt.
$$

Answer process:
1. Solve forward ODE to get $x(t_1)$ and loss.
2. Set terminal adjoint $a(t_1)$.
3. Solve adjoint equation backward from $t_1$ to $t_0$.
4. Accumulate the parameter-gradient integral.

Memory comparison:
- ResNet backprop usually stores layer activations, memory grows with number of layers.
- Neural ODE adjoint can reduce memory by recomputing states during the backward solve.
- Tradeoff: extra computation and possible numerical instability/error from recomputation.

---

## 8. Final-Revision Question Checklist

Use this section as a self-test. If you cannot answer a row in 2-5 minutes, revise that topic immediately.

| Final revision prompt | Minimal exam-ready answer |
|---|---|
| CNN inductive biases | Locality, sparse connectivity, parameter sharing, translation behavior, fewer parameters. |
| Cross-correlation | Write sliding dot product without filter flip; compute output carefully with padding/stride. |
| CNN layer output and parameters | Use shape formula, then count shared filter weights. |
| Conv-layer backprop | Write upstream-gradient times input patch, summed over all locations. |
| Positive definite kernel | Quadratic form nonnegative; prove using feature-map norm square if possible. |
| Aronszajn theorem | PD kernel corresponds to unique RKHS; kernel acts as inner product/reproducing function. |
| Kernel trick | Replace feature-space inner products by $K(x_i,x_j)$. |
| Representer theorem | Regularized RKHS minimizer has finite kernel expansion. |
| KRR matrix form | Derive $\varsigma=(K+\beta nI)^{-1}y$ and final $\hat f(x)$. |
| LU/Gaussian elimination | Factor $A=LU$, solve $Ly=b$, $Ux=y$, state $O(n^3)$ factorization. |
| Pivoting | Row swaps to avoid zero/tiny pivots and improve stability. |
| Stability/condition number | Large $\kappa(A)$ means sensitive problem; write perturbation bound. |
| Jacobi/Gauss-Seidel | Write updates, error equation, convergence by spectral radius. |
| Low-rank approximation | Truncated SVD, keep largest singular values, compression/denoising. |
| SVD | $A=U\Sigma V^T$; know how to obtain it for a small matrix. |
| Power iteration | Multiply, normalize, Rayleigh quotient; rate $|\lambda_2/\lambda_1|^k$. |
| Inverse iteration | Solve shifted system, normalize; converges to eigenvalue closest to shift. |
| RQI | Shift is current Rayleigh quotient; cubic locally for symmetric simple eigenpair. |
| PCA | Eigenvectors of covariance/scatter maximize variance and minimize reconstruction error. |
| Kernel PCA | Loading $v=\sum_i a_i\Psi(x_i)$; coefficients from Gram eigenproblem; scores use kernels. |
| ODE solvers | Euler/theta formulas; identify explicit/implicit and order. |
| ODE stability/convergence | Taylor expansion, local error, error recurrence, global bound. |
| ResNet | $u_{l+1}=u_l+F(u_l,\theta_l)$; Euler/discrete dynamical-system view. |
| Neural ODE gradient | Forward solve, adjoint terminal condition, backward adjoint solve, gradient integral. |
| Memory requirements | ResNet stores activations; Neural ODE adjoint recomputes trajectory for lower memory but higher cost/risk. |

---

## 9. What To Memorize First

If time is very limited, memorize in this order:

1. PCA/kernel PCA notation, especially loading vs score.
2. Power/inverse/RQI steps and convergence rates.
3. Theta-method, Euler methods, and global/local error logic.
4. KRR derivation from representer theorem.
5. Jacobi/Gauss-Seidel update, error equation, spectral-radius condition.
6. CNN shape and parameter formulas.
7. ResNet block, Neural ODE adjoint, memory comparison.
8. SVD low-rank approximation and why it is useful.
9. Pivoting, condition number, stability definitions.

---

## 10. Fast Answer Templates

### Calculation Template

1. Define notation and dimensions.
2. Write the formula before substituting numbers.
3. Show intermediate matrix/vector values.
4. State the final answer with units/dimensions.
5. Add one sentence interpretation if the question asks "why" or "explain".

### Derivation Template

1. Start from the objective/update/equation.
2. Substitute the required representation.
3. Simplify into matrix form.
4. State assumptions.
5. Box the final formula.

### Conceptual Template

1. Direct answer in the first sentence.
2. Give 2-4 technical reasons.
3. Mention one limitation/tradeoff.
4. Connect back to the wording of the question.

### Algorithm Template

1. State input and initialization.
2. Write the repeated update.
3. State stopping/output.
4. State convergence condition and rate if known.
5. Mention a common numerical issue.

---

## 11. Last-Hour Checklist

Before entering the exam, I should be able to:

- [ ] Explain CNN inductive biases in four technical phrases.
- [ ] Compute cross-correlation without flipping the filter.
- [ ] Compute CNN output shapes and trainable weights.
- [ ] State the PD kernel definition and prove PD using feature maps.
- [ ] Derive KRR matrix form from the representer theorem.
- [ ] Do LU solve by forward/back substitution.
- [ ] Write Jacobi and Gauss-Seidel updates and error equations.
- [ ] State $\rho(B)<1$ convergence condition for stationary iterations.
- [ ] Write SVD and rank-$k$ approximation formulas.
- [ ] Run one or two steps of power/inverse/RQI by hand.
- [ ] Explain PCA as variance maximization and reconstruction-error minimization.
- [ ] For kernel PCA, write $v=\sum_i a_i\Psi(x_i)$ and know that $Ka$ gives scores.
- [ ] Write Euler, backward Euler, and theta-method formulas.
- [ ] Distinguish local truncation error from global error.
- [ ] Explain ResNet as a residual block and as an Euler discretization.
- [ ] Write Neural ODE adjoint equation and parameter-gradient integral.
- [ ] Compare ResNet and Neural ODE memory requirements with the tradeoff.

