# Exam Patterns and Final Checklist

## 1. Overall exam structure

Based on `2023May.pdf`, `2024May.pdf`, and `2025May.pdf`, past papers were 2.5 hours and required all questions. The 2026 final revision says the exam is 18:30-21:00 on May 18, 2026, with 8 long questions, and students are advised to write detailed steps or derivations. Source: `Final-revision.pdf`.

Likely marking style:
- Method marks matter; past papers warn that conclusions only may receive no score.
- Calculations must show intermediate steps.
- Questions mix numerical computation, derivation/proof, and conceptual explanation.
- Small matrices and small networks are common because they are hand-computable.
- A handwritten double-sided A4 cheat sheet is allowed, so expect questions requiring application rather than pure memorization.

Likely question types:
- Compute a CNN/cross-correlation output, layer shape, or weight count.
- Prove/use positive definite kernels and RKHS identities.
- Derive or apply KRR matrix form.
- Perform LU/Jacobi/eigenvalue iterations.
- Compute PCA/kernel PCA components, projections, MSE, or explained variance.
- Solve ODE steps and derive truncation/global error estimates.
- Explain ResNet, Neural ODE, and adjoint gradients.

## 2. High-yield topic ranking

| Rank | Topic | Chapter | Why it is high-yield | Evidence |
|---|---|---|---|---|
| 1 | PCA and kernel PCA | 3 | Appears in every past exam and is emphasized in final revision | `Final-revision.pdf`, `2023May.pdf`, `2024May.pdf`, `2025May.pdf` |
| 2 | Iterative eigenvalue solvers | 3 | Power/inverse/RQI algorithms and convergence are repeatedly tested | `Final-revision.pdf`, `2023May.pdf`, `2024May.pdf`, `2025May.pdf` |
| 3 | ODE solvers and error estimates | 4 | Euler/theta methods and global error appear repeatedly | `Final-revision.pdf`, `2023May.pdf`, `2024May.pdf`, `2025May.pdf` |
| 4 | CNN/ResNet shape, weights, and interpretation | 1/4 | Direct final-revision emphasis and tested in 2024/2025 | `Final-revision.pdf`, `2024May.pdf`, `2025May.pdf` |
| 5 | Kernel methods and KRR | 1 | Strong final-revision emphasis and tested in 2023/2024/2025 | `Final-revision.pdf`, `2023May.pdf`, `2024May.pdf`, `2025May.pdf` |
| 6 | LU/Jacobi/linear-system stability | 2 | Final revision names them; 2024/2025 test calculations | `Final-revision.pdf`, `2024May.pdf`, `2025May.pdf` |
| 7 | SVD and low-rank approximation | 2/3 | Final revision emphasizes SVD; 2025 tests SVD and PCA connection | `Final-revision.pdf`, `2025May.pdf`, `chapter2.pdf` |
| 8 | Neural ODE adjoint and memory | 4 | Direct final-revision item; tested in 2023 | `Final-revision.pdf`, `2023May.pdf` |

## 3. Must-know definitions

- Cross-correlation: sliding kernel operation without flipping the kernel.
- CNN inductive bias: locality, sparse connectivity, parameter sharing, and translation-related robustness.
- Positive definite kernel: symmetric kernel with nonnegative quadratic form for all finite point sets.
- Gram matrix: $K_{ij}=K(x_i,x_j)$.
- RKHS: Hilbert space associated with a reproducing kernel.
- Kernel trick: replace feature-space inner products by kernel evaluations.
- Representer theorem: regularized RKHS minimizers lie in the span of training kernel sections.
- Condition number: $\kappa(A)=\|A\|\|A^{-1}\|$.
- Jacobi method and Gauss-Seidel method.
- SVD: $A=U\Sigma V^T$.
- Rayleigh quotient: $R_A(v)=v^TAv/(v^Tv)$.
- Principal component: unit direction maximizing projected variance.
- Kernel PCA: PCA in feature space using a kernel Gram matrix.
- Truncation error and global error.
- ResNet: network with residual/skip connections.
- Neural ODE: continuous-depth model $x'(t)=f(x(t),t,\theta)$.
- Adjoint state: $a(t)=\partial L/\partial x(t)$.

## 4. Must-know frameworks / models

- CNN layer pipeline: input, convolution/cross-correlation, ReLU, pooling, flatten, fully connected output.
- Kernel method pipeline: choose kernel, build Gram matrix, solve finite problem, evaluate with kernel expansion.
- KRR derivation: RKHS objective, representer theorem, Gram matrix, solve $(K+\lambda nI)\alpha=y$.
- LU solve pipeline: factor $A=LU$, forward substitution, back substitution.
- Iterative solver framework: $x^{(k+1)}=Bx^{(k)}+c$, error $e^{(k+1)}=Be^{(k)}$.
- Eigenvalue iteration framework: multiply or solve, normalize, Rayleigh quotient update.
- PCA framework: center data, covariance matrix, eigenpairs, explained variance, projection/MSE.
- Kernel PCA framework: Gram matrix, eigenproblem $Ka=n\lambda a$, normalization, kernel projection.
- ODE error framework: define local truncation error, derive error recurrence, apply Lipschitz/Gronwall bound.
- ResNet-Neural ODE connection: residual block as Euler step, Neural ODE as continuous limit.

## 5. Must-know formulas / calculations

- CNN output size:

$$
H_{\text{out}}=\left\lfloor\frac{H+2P-F}{S}\right\rfloor+1.
$$

- CNN convolution weights:

$$
F_HF_WC_{\text{in}}C_{\text{out}}
$$

plus biases only if included.

- Feature-space distance:

$$
\|\phi(x)-\phi(y)\|^2=K(x,x)+K(y,y)-2K(x,y).
$$

- KRR:

$$
\alpha=(K+\lambda nI)^{-1}y,\qquad f(x)=\sum_i\alpha_iK(x_i,x).
$$

- Condition-number bound:

$$
\frac{\|\delta x\|}{\|x\|}
\le \kappa(A)\frac{\|\delta b\|}{\|b\|}.
$$

- Jacobi:

$$
x_i^{(k+1)}=\frac{1}{a_{ii}}\left(b_i-\sum_{j\ne i}a_{ij}x_j^{(k)}\right).
$$

- Gauss-Seidel:

$$
x_i^{(k+1)}
=\frac{1}{a_{ii}}\left(b_i-\sum_{j<i}a_{ij}x_j^{(k+1)}-\sum_{j>i}a_{ij}x_j^{(k)}\right).
$$

- SVD low-rank approximation:

$$
A_k=\sum_{i=1}^{k}\sigma_i u_iv_i^T,\qquad
\|A-A_k\|_2=\sigma_{k+1}.
$$

- Power iteration convergence:

$$
\|v^{(k)}-\pm u_1\|
=O\left(\left|\frac{\lambda_2}{\lambda_1}\right|^k\right).
$$

- PCA explained variance:

$$
\frac{\sum_{i=1}^{s}\lambda_i}{\sum_i\lambda_i}\times100\%.
$$

- Kernel PCA:

$$
Ka=n\lambda a,\qquad a^TKa=1,\qquad y_j(x)=\sum_i a_{ji}K(x,x_i).
$$

- Theta-method:

$$
x_{i+1}=x_i+h[(1-\theta)f(t_i,x_i)+\theta f(t_{i+1},x_{i+1})].
$$

- Forward Euler global error:

$$
\max_i|e_i|=O(h).
$$

- Neural ODE adjoint:

$$
\frac{da}{dt}=-a^T\frac{\partial f}{\partial x},\qquad
\frac{\partial L}{\partial\theta}
=\int_{t_0}^{t_1}a(t)^T\frac{\partial f}{\partial\theta}\,dt.
$$

## 6. Common question types and answer templates

### Definition questions

Template:
1. Give the formal definition.
2. Explain the intuition in one sentence.
3. State why it matters for computation or learning.
4. Add a simple example if time allows.

Example: for positive definite kernel, write the quadratic-form condition and say it guarantees a Hilbert-space inner-product representation.

### Compare / contrast questions

Template:
1. State both methods.
2. Compare objective or update formula.
3. Compare cost/stability/convergence.
4. Give exam-relevant implication.

Examples:
- Jacobi vs Gauss-Seidel: old values vs immediate new values; parallel vs often faster sequential convergence.
- ResNet vs Neural ODE: discrete layers vs continuous solve; stored activations vs adjoint recomputation.

### Explain-the-process questions

Template:
1. Name the algorithm.
2. Write the mathematical update.
3. Explain each step.
4. State assumptions and convergence/limitation.

Use for power iteration, inverse iteration, LU solve, theta-method, or adjoint method.

### Application questions

Template:
1. Translate the problem into the correct matrix/vector/kernel form.
2. Compute the required objects.
3. State final answer with dimensions.
4. Add a short interpretation.

Use for CNN shapes, Gram matrices, KRR, PCA, kernel PCA, and ResNet blocks.

### Calculation questions

Template:
1. Write formula before substituting numbers.
2. Show intermediate values in a table.
3. Keep exact fractions where possible.
4. Box or clearly mark final result.

Past exams award method marks, so do not give conclusion only.

### Short essay questions

Template:
1. Direct answer in the first sentence.
2. Give two to four technical reasons.
3. Mention limitation/tradeoff.
4. Connect back to the question wording.

Examples:
- "Why choose CNN instead of fully connected network?"
- "Why use low-rank approximation?"
- "Why can Neural ODE save memory?"

## 7. Common traps across the whole subject

- Writing final answers without derivation or intermediate steps.
- Ignoring whether data should be centered in PCA/kernel PCA.
- Mixing raw-space PCA and feature-space kernel PCA.
- Forgetting normalization in eigenvalue iterations.
- Using inverse iteration as multiplication instead of solving a shifted system.
- Flipping the kernel in cross-correlation questions.
- Miscounting CNN weights by treating every spatial location as a separate filter.
- Forgetting pooling has no trainable weights.
- Saying a method converges without checking conditions.
- Confusing local truncation error with global error.
- Claiming implicit methods are explicit because the formula looks like an update.
- Forgetting the final predictor/function after solving coefficients.
- Not mentioning assumptions such as Lipschitz continuity, smoothness, nonzero pivot, or eigenvalue separation.

## 8. Final 1-day revision plan

### Morning: memorize core formulas and algorithms

- Write from memory: CNN shape formula, KRR solve, Jacobi/Gauss-Seidel updates, Rayleigh quotient, power/inverse/RQI algorithms, PCA/kernel PCA formulas, theta-method, adjoint equation.
- Make sure these fit on the handwritten A4 sheet in compact form.

### Midday: practise calculation patterns

- Do one CNN shape/weight count.
- Do one Gram matrix and KRR setup.
- Do one LU solve and one Jacobi three-iteration table.
- Do one power iteration table.
- Do one PCA/kernel PCA setup with explained variance or MSE.
- Do one Forward Euler and theta-method step calculation.

### Afternoon: practise derivation skeletons

- Power iteration convergence proof.
- KRR matrix derivation from representer theorem.
- Forward Euler global error estimate.
- Neural ODE adjoint gradient explanation.
- Positive definite kernel proof template.

### Evening: skim and consolidate

- Skim the "Common mistakes" and checklist sections in all chapter notes.
- Review past-exam mappings to recognize recurring wording.
- Rehearse short answers for: CNN vs fully connected, pivoting, condition number, low-rank approximation, PCA vs kernel PCA, ResNet vs Neural ODE.

### Do not waste time on

- Long historical notes.
- Optional storage-format details.
- Full application case studies unless they clarify a core formula.
- Memorizing large examples without understanding the reusable algorithm.
