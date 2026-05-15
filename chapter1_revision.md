# Chapter 1 Revision Notes

## 1. Chapter overview

Chapter 1 covers two learning-method families that appear repeatedly in the final revision: convolutional neural networks and kernel methods. The exam value is practical: you may be asked to compute a CNN layer output or parameter count, explain why CNNs differ from fully connected networks, prove or use positive definite kernels, and derive the finite-dimensional matrix form of kernel ridge regression.

Main exam message: this chapter is not only definitions. You must be able to calculate with kernels and CNN layers, then explain the intuition behind the methods. Sources: `chapter1.pdf`, `Final-revision.pdf`, `2023May.pdf`, `2024May.pdf`, `2025May.pdf`.

## 2. High-priority topics from final revision

| Priority | Topic | Why it matters |
|---|---|---|
| High priority | CNN inductive biases | Final revision explicitly asks what features CNNs have compared with fully connected neural networks. |
| High priority | Cross-correlation | Directly listed in final revision and tested in `2025May.pdf`. |
| High priority | CNN output shapes and number of parameters | Final revision asks this directly; `2025May.pdf` asks layer shapes and weights. |
| High priority | Backpropagation for convolutional layers | Final revision asks whether you can calculate gradients with respect to parameters. |
| High priority | Positive definite kernels and RKHS | Final revision lists positive definite kernels and Aronszajn theorem; `2023May.pdf` and `2025May.pdf` test this style. |
| High priority | Kernel trick | Central to kernel methods and explicitly listed in final revision. |
| High priority | Representer theorem | Explicitly listed in final revision; needed for KRR. |
| High priority | Kernel Ridge Regression matrix form | Explicitly listed in final revision and tested in `2024May.pdf`. |
| Medium priority | Pooling, padding, stride | Important for CNN shape questions, especially `2025May.pdf`. |
| Medium priority | Polynomial and Gaussian kernels | Useful examples for proving/recognizing positive definite kernels. |
| Low priority / background only | Historical details, image examples, long application slides | Useful intuition, but unlikely to require detailed memorization. |

## 3. Key concepts and definitions

- Cross-correlation: for input $I \in \mathbb{R}^{m_1 \times n_1}$ and kernel $K \in \mathbb{R}^{m_2 \times n_2}$, the valid cross-correlation output is

$$
S(i,j) = (I * K)(i,j) =
\sum_{k_1=1}^{m_2}\sum_{k_2=1}^{n_2}
I(i+k_1-1,j+k_2-1)K(k_1,k_2).
$$

In neural networks this is often called convolution, but mathematically it does not flip the kernel. Source: `chapter1.pdf`.

- CNN inductive bias: CNNs assume local spatial structure matters. Compared with fully connected networks, they use sparse local interactions, parameter sharing, and often pooling to reduce sensitivity to small translations. In an exam, write these as features that reduce parameters and exploit image/grid structure.

- Padding: adding values, usually zeros, around an input before applying a kernel. Same padding is chosen so spatial output size is preserved when stride is 1. Valid padding means no added boundary values, so output shrinks. Source: `chapter1.pdf`, `2025May.pdf`.

- Stride: step size used when moving the kernel across the input. Larger stride reduces output size.

- Pooling: a non-trainable operation such as max pooling or average pooling that reduces spatial size. It has no trainable kernel weights. Source: `chapter1.pdf`.

- Convolutional backpropagation: gradients are obtained by applying the chain rule through the sliding-window operation. If $S=I*K$ and $G(i,j)=\partial L/\partial S(i,j)$ is the upstream gradient, then each kernel-gradient entry accumulates over all positions where that kernel entry was used:

$$
\frac{\partial L}{\partial K(k_1,k_2)}
=\sum_{i,j}G(i,j)I(i+k_1-1,j+k_2-1).
$$

In words: match each upstream gradient with the input patch that produced it. The input gradient is found by accumulating all upstream gradients whose receptive fields include that input pixel.

- Positive definite kernel: a symmetric function $K:X \times X \to \mathbb{R}$ such that for any $N$, points $x_1,\ldots,x_N \in X$, and coefficients $a_1,\ldots,a_N \in \mathbb{R}$,

$$
\sum_{i=1}^{N}\sum_{j=1}^{N} a_i a_j K(x_i,x_j) \ge 0.
$$

Exam use: to prove a kernel is positive definite, show the Gram matrix is positive semidefinite or express $K(x,x')$ as an inner product in a feature space. Source: `chapter1.pdf`.

- Gram matrix: for data $x_1,\ldots,x_n$, the matrix $K \in \mathbb{R}^{n \times n}$ with entries $K_{ij}=K(x_i,x_j)$. It converts kernel questions into matrix questions.

- RKHS: a Hilbert space of functions associated with a positive definite kernel. The important exam idea is not abstract functional analysis; it is that $K(x,\cdot)$ acts like a feature vector and inner products can be evaluated by kernels.

- Kernel trick: if an algorithm only needs inner products, replace $\langle \phi(x_i),\phi(x_j)\rangle$ by $K(x_i,x_j)$ and avoid explicitly computing $\phi$. Source: `chapter1.pdf`.

- Representer theorem: regularized empirical-risk minimizers in an RKHS can be written as finite kernel expansions

$$
f(x)=\sum_{i=1}^{n}\alpha_i K(x_i,x).
$$

Exam use: this is the bridge from infinite-dimensional optimization to finite-dimensional linear algebra. Source: `chapter1.pdf`.

## 4. Core theories / models / frameworks

### CNN layer framework

Intuition: an image has local spatial patterns. A small filter can detect a feature, and the same filter can be reused across positions.

Key components:
- Input tensor: height, width, channels.
- Convolution/cross-correlation filters: trainable local weights.
- Activation such as ReLU.
- Pooling or stride: spatial downsampling.
- Flatten and fully connected layers: turn feature maps into predictions.

How it is used:
- Compute layer-by-layer output dimensions.
- Count trainable weights.
- Explain why CNNs are better suited than fully connected networks for images.

Exam appearance:
- `2025May.pdf` asks output shape and number of weights for a CNN.
- `2024May.pdf` asks a residual block with two padded convolutional layers.

Limitations:
- CNNs encode locality and translation-related assumptions; they are less natural for data with no grid or local structure.
- Padding and stride choices affect boundary information and feature-map size.

### Positive definite kernels and Aronszajn theorem

Intuition: a positive definite kernel behaves like an inner product after mapping inputs into a Hilbert space.

Core statement:

$$
K(x,x') = \langle \phi(x),\phi(x')\rangle_H
$$

for some Hilbert space $H$ and feature map $\phi$ if and only if $K$ is positive definite. Source: `chapter1.pdf`.

How it is used:
- Prove kernels are valid.
- Compute distances in feature space:

$$
\|\phi(x)-\phi(y)\|_H^2
=K(x,x)+K(y,y)-2K(x,y).
$$

Exam appearance:
- `2023May.pdf` asks for average distance to a center of mass in RKHS.
- `2025May.pdf` asks to prove a Gaussian-type kernel is positive definite.

Limitations:
- A valid kernel does not automatically mean it is a good kernel for the task.
- The feature space may be infinite-dimensional, so use kernel evaluations instead of explicit features.

### Kernel Ridge Regression

Intuition: fit training labels while penalizing rough or complex functions in the RKHS.

Primal RKHS problem:

$$
\hat f \in \arg\min_{f\in H}
\frac{1}{n}\sum_{i=1}^{n}(f(x_i)-y_i)^2+\lambda \|f\|_H^2.
$$

By the representer theorem:

$$
\hat f(x)=\sum_{i=1}^{n}\alpha_iK(x_i,x).
$$

Let $y=(y_1,\ldots,y_n)^T$, $\alpha=(\alpha_1,\ldots,\alpha_n)^T$, and $K_{ij}=K(x_i,x_j)$. Then

$$
(\hat f(x_1),\ldots,\hat f(x_n))^T=K\alpha,\qquad
\|\hat f\|_H^2=\alpha^T K\alpha.
$$

The finite problem is

$$
\min_{\alpha\in\mathbb{R}^n}
\frac{1}{n}(K\alpha-y)^T(K\alpha-y)+\lambda \alpha^T K\alpha.
$$

Setting the gradient to zero gives

$$
\alpha=(K+\lambda n I)^{-1}y
$$

up to null-space ambiguity that does not change the represented function. Source: `chapter1.pdf`.

Exam appearance:
- `2024May.pdf` asks for KRR objective/function after computing a Gram matrix.

Assumptions and limitations:
- $\lambda>0$ stabilizes the solve and controls overfitting.
- The whole method depends on kernel choice and Gram-matrix conditioning.

## 5. Important formulas / calculations, if any

### CNN output size

For a 2D convolution with input size $H \times W$, kernel size $F_H \times F_W$, padding $P_H,P_W$, and stride $S_H,S_W$:

$$
H_{\text{out}}=\left\lfloor \frac{H+2P_H-F_H}{S_H}\right\rfloor+1,\qquad
W_{\text{out}}=\left\lfloor \frac{W+2P_W-F_W}{S_W}\right\rfloor+1.
$$

Use when asked for layer shapes. Common mistake: forgetting padding on both sides, so total added height is $2P_H$.

### CNN parameter count

For $C_{\text{out}}$ filters, each of size $F_H \times F_W$ with $C_{\text{in}}$ input channels:

$$
\text{weights}=F_HF_WC_{\text{in}}C_{\text{out}}.
$$

If biases are included, add $C_{\text{out}}$. The 2025 exam says to ignore bias, so obey the question wording.

### Feature-space distance

$$
\|\phi(x)-\phi(y)\|_H^2=K(x,x)+K(y,y)-2K(x,y).
$$

For a set center $\bar \phi = \frac{1}{n}\sum_{j=1}^{n}\phi(x_j)$:

$$
\frac{1}{n}\sum_{i=1}^{n}\|\phi(x_i)-\bar\phi\|_H^2
=\frac{1}{n}\sum_i K(x_i,x_i)
-\frac{1}{n^2}\sum_i\sum_j K(x_i,x_j).
$$

Use this for RKHS average-distance questions like `2023May.pdf`.

### KRR matrix solve

$$
\alpha=(K+\lambda n I)^{-1}y,\qquad
\hat f(x)=\sum_i\alpha_iK(x_i,x).
$$

Calculation steps:
1. Compute the Gram matrix.
2. Add $\lambda n I$.
3. Solve the linear system, preferably without explicitly writing an inverse if the question asks for numerical work.
4. Write the final function as a kernel expansion.

Common mistakes:
- Using $\lambda I$ instead of $\lambda nI$ when the objective has $\frac{1}{n}$ in the data term.
- Writing only $\alpha$ and forgetting to write $\hat f(x)$.

## 6. Exam question patterns

### Pattern 1: CNN shape and parameter count

Possible wording:
- "What is the output shape from each layer?"
- "How many weights does this network have? Ignore bias."

What the examiner wants:
- Layer-by-layer dimensions and parameter count, not a general description.

How to answer:
1. State input shape.
2. Apply convolution output-size formula.
3. Apply pooling output-size formula.
4. Flatten carefully.
5. Count convolution and fully connected weights separately.

Key points:
- Same padding with a $5\times5$ kernel and padding 2 preserves $100\times100$ when stride is 1.
- A $2\times2$ max pool with stride 2 halves $100\times100$ to $50\times50$.

Suggested answer length: half to one page with clear calculations.

### Pattern 2: Cross-correlation / padding calculation

Possible wording:
- "Let $I$ and $K$ be given. What is $I*K$?"
- "After padding $I$ with zeros, what is the output?"

How to answer:
1. Write the cross-correlation sum.
2. Slide the kernel without flipping.
3. Show at least a few entries to prove you used the correct convention.
4. Present the final matrix.

Common mistakes:
- Flipping the kernel as in mathematical convolution.
- Using invalid output size after padding.

Suggested answer length: compact calculation table plus final matrix.

### Pattern 3: Positive definite kernel proof

Possible wording:
- "Prove that $K(x,x')=\exp(-\gamma\|x-x'\|^2)$ is positive definite."
- "Is this kernel positive definite? If so, what is the corresponding RKHS?"

How to answer:
1. State the definition of positive definite kernel.
2. Express the kernel using closure properties or feature-map inner product.
3. Conclude that every finite Gram matrix is positive semidefinite.
4. If asked for RKHS, state it is the RKHS guaranteed by Aronszajn theorem; do not invent a finite feature space unless known.

Suggested answer length: 1/2 page for proof; shorter if only recognition is needed.

### Pattern 4: KRR derivation/application

Possible wording:
- "Find the objective function $f$ for KRR."
- "Derive the matrix form of Kernel Ridge Regression."

How to answer:
1. State the RKHS objective.
2. Use representer theorem: $f(x)=\sum_i\alpha_iK(x_i,x)$.
3. Define Gram matrix and vector $y$.
4. Derive or state $(K+\lambda nI)\alpha=y$.
5. Write the final predictor.

Common mistakes:
- Not connecting representer theorem to the finite coefficient vector.
- Treating the Gram matrix as the feature matrix.

Suggested answer length: one page if derivation is requested; shorter for numerical substitution.

## 7. Past exam connections

| Past exam | Related topic | What the question tests | Required depth |
|---|---|---|---|
| 2023May | RKHS feature-space distance, positive definite kernels | Derive kernel-only expression for distance to center of mass; evaluate with given kernels | Derivation plus numerical kernel calculations |
| 2024May | Gram matrix and KRR | Compute Gram matrix, use eigen/LU tools, then write KRR function | Matrix calculation and correct kernel expansion |
| 2025May | Cross-correlation, padding, positive definite kernel, CNN shapes/weights | Compute convolution-style outputs, prove kernel validity, calculate CNN dimensions and weights | Direct calculations plus concise explanation |

## 8. Worked mini-examples / answer templates

### CNN answer template

For a $100\times100\times1$ grayscale input, one $5\times5$ filter, stride 1, padding 2:

$$
H_{\text{out}}=W_{\text{out}}=\frac{100+2(2)-5}{1}+1=100.
$$

Output after convolution: $100\times100\times1$. Parameters: $5\cdot5\cdot1\cdot1=25$ if bias ignored.

If a $2\times2$ max pool with stride 2 follows, output is $50\times50\times1$. Flatten gives $2500$ features. A fully connected layer with 100 nodes has $2500\cdot100=250000$ weights if bias ignored.

### KRR derivation template

"By the representer theorem, the minimizer lies in the span of the training kernel sections, so write $f(x)=\sum_i\alpha_iK(x_i,x)$. Let $K_{ij}=K(x_i,x_j)$. Then fitted training values are $K\alpha$ and the RKHS norm is $\alpha^TK\alpha$. Therefore the KRR objective becomes $\frac{1}{n}(K\alpha-y)^T(K\alpha-y)+\lambda\alpha^TK\alpha$. Setting the gradient to zero gives $K((K+\lambda nI)\alpha-y)=0$, so choose $\alpha=(K+\lambda nI)^{-1}y$."

### Positive definite kernel template

"To prove $K$ is positive definite, take arbitrary points $x_i$ and coefficients $a_i$. It is enough to show $\sum_{i,j}a_ia_jK(x_i,x_j)\ge0$. If $K(x,x')=\langle\phi(x),\phi(x')\rangle$, then this sum equals $\|\sum_i a_i\phi(x_i)\|_H^2\ge0$. Hence $K$ is positive definite."

## 9. Common mistakes and confusing points

- Calling cross-correlation "convolution" and then flipping the kernel in the calculation.
- Forgetting that padding 2 adds two rows/columns on both sides.
- Counting pooling as trainable weights.
- Counting one convolution filter per spatial location instead of one shared filter.
- Giving CNN inductive biases as vague "better for images" without saying locality, sparse connectivity, parameter sharing, and translation-related behavior.
- Stating a kernel is positive definite without checking symmetry and Gram-matrix nonnegativity.
- Using Euclidean dot products after mapping to RKHS instead of kernel evaluations.
- Writing KRR as ordinary ridge regression on raw $X$ instead of using the Gram matrix.
- Forgetting the final KRR predictor $f(x)$ after solving for coefficients.

## 10. Quick revision checklist

Before the exam, I should be able to:

- [ ] Explain CNN sparse interactions and parameter sharing compared with fully connected networks.
- [ ] Compute 2D cross-correlation without flipping the kernel.
- [ ] Calculate CNN output shapes with padding and stride.
- [ ] Count CNN, pooling, flatten, and fully connected layer weights.
- [ ] Explain how convolutional layer gradients are obtained by backpropagation.
- [ ] State and use the definition of a positive definite kernel.
- [ ] Explain Aronszajn theorem in exam language.
- [ ] Use the kernel trick to compute feature-space inner products and distances.
- [ ] State the representer theorem and why it makes kernel learning finite-dimensional.
- [ ] Derive the KRR matrix form and write the final predictor.
