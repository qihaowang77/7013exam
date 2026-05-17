# 2024 May Standard Answers

Source exam: `2024May.pdf`

These solutions show the calculation route and the final answers. Numerical eigenvectors may differ by sign.

## Question 1

The kernel is

$$
K(x,x')=(1+\langle x,x'\rangle_{\ell^2(\mathbb R^4)})^3.
$$

The data are

$$
x_1=(1,0,-1,0)^T,\quad
x_2=(-1,0,0,1)^T,\quad
x_3=(0,0,0,0)^T,\quad
x_4=(0,1,-1,0)^T,
$$

and

$$
y=(2,2,0,2)^T.
$$

### 1(a) Gram matrix

Compute each dot product, then apply $(1+\langle x_i,x_j\rangle)^3$.

The Gram matrix is

$$
\boxed{
K=
\begin{pmatrix}
27&0&1&8\\
0&27&1&1\\
1&1&1&1\\
8&1&1&27
\end{pmatrix}.
}
$$

For example,

$$
K_{11}=(1+\langle x_1,x_1\rangle)^3=(1+2)^3=27,
$$

and

$$
K_{14}=(1+\langle x_1,x_4\rangle)^3=(1+1)^3=8.
$$

### 1(b) Power iteration for the largest eigenvalue

Start with

$$
v^{(0)}=(1,0,0,0)^T.
$$

Use

$$
w^{(k)}=Kv^{(k-1)},\qquad
v^{(k)}=\frac{w^{(k)}}{\|w^{(k)}\|_2},\qquad
\lambda^{(k)}=(v^{(k)})^TKv^{(k)}.
$$

#### Iteration 1

$$
w^{(1)}=
\begin{pmatrix}
27\\0\\1\\8
\end{pmatrix}.
$$

$$
\|w^{(1)}\|_2=\sqrt{27^2+1^2+8^2}=\sqrt{794}.
$$

Thus

$$
v^{(1)}\approx
\begin{pmatrix}
0.9582\\0\\0.0355\\0.2839
\end{pmatrix}.
$$

The Rayleigh quotient is

$$
\lambda^{(1)}\approx31.4081.
$$

#### Iteration 2

$$
w^{(2)}=Kv^{(1)}
\approx
\begin{pmatrix}
28.1780\\0.3194\\1.2776\\15.3666
\end{pmatrix}.
$$

After normalization,

$$
v^{(2)}\approx
\begin{pmatrix}
0.8772\\0.0099\\0.0398\\0.4784
\end{pmatrix}.
$$

The Rayleigh quotient is

$$
\lambda^{(2)}\approx33.7911.
$$

#### Iteration 3

$$
w^{(3)}=Kv^{(2)}
\approx
\begin{pmatrix}
27.5511\\0.7866\\1.4053\\19.9834
\end{pmatrix}.
$$

After normalization,

$$
v^{(3)}\approx
\begin{pmatrix}
0.8086\\0.0231\\0.0412\\0.5865
\end{pmatrix}.
$$

The Rayleigh quotient is

$$
\boxed{\lambda^{(3)}\approx34.6873.}
$$

### 1(c) Doolittle LU and solving $(K+\alpha I)^{-1}y$

Here $\alpha=0.5$, so

$$
A=K+\alpha I
=
\begin{pmatrix}
27.5&0&1&8\\
0&27.5&1&1\\
1&1&1.5&1\\
8&1&1&27.5
\end{pmatrix}.
$$

There is no zero pivot, so Doolittle LU can be used.

The Doolittle factorization $A=LU$, with diagonal entries of $L$ equal to 1, is

$$
L=
\begin{pmatrix}
1&0&0&0\\
0&1&0&0\\
\frac{2}{55}&\frac{2}{55}&1&0\\
\frac{16}{55}&\frac{2}{55}&\frac{74}{157}&1
\end{pmatrix},
$$

and

$$
U=
\begin{pmatrix}
\frac{55}{2}&0&1&8\\
0&\frac{55}{2}&1&1\\
0&0&\frac{157}{110}&\frac{37}{55}\\
0&0&0&\frac{428629}{17270}
\end{pmatrix}.
$$

To solve $A\beta=y$, first solve $Lz=y$:

$$
Lz=
\begin{pmatrix}
2\\2\\0\\2
\end{pmatrix}.
$$

Then solve $U\beta=z$ by back substitution. The result is

$$
\boxed{
\beta=A^{-1}y=
\begin{pmatrix}
\frac{26076}{428629}\\
\frac{32292}{428629}\\
-\frac{55192}{428629}\\
\frac{24420}{428629}
\end{pmatrix}
\approx
\begin{pmatrix}
0.06084\\
0.07534\\
-0.12876\\
0.05697
\end{pmatrix}.
}
$$

### 1(d) KRR objective function with $\beta=0.125$

KRR solves

$$
\arg\min_{f\in\mathcal H}
\frac{1}{4}\sum_{i=1}^{4}|f(x_i)-y_i|^2
+\beta\|f\|_{\mathcal H}^2.
$$

By the representer theorem,

$$
f(x)=\sum_{i=1}^{4}c_iK(x_i,x).
$$

The matrix form gives

$$
c=(K+4\beta I)^{-1}y.
$$

Since $4\beta=4(0.125)=0.5$, this is exactly the vector computed in 1(c):

$$
c=
\begin{pmatrix}
\frac{26076}{428629}\\
\frac{32292}{428629}\\
-\frac{55192}{428629}\\
\frac{24420}{428629}
\end{pmatrix}.
$$

Therefore

$$
\boxed{
f(x)
=\frac{26076}{428629}K(x_1,x)
+\frac{32292}{428629}K(x_2,x)
-\frac{55192}{428629}K(x_3,x)
+\frac{24420}{428629}K(x_4,x).
}
$$

Since $K(x_i,x)=(1+\langle x_i,x\rangle)^3$, this is the required objective/predictor function.

## Question 2

The data are

$$
x_1=(1,0,-1,0)^T,\quad
x_2=(-1,0,0,1)^T,\quad
x_3=(1,1,0,0)^T,\quad
x_4=(0,1,-1,0)^T.
$$

The uncentered covariance matrix used for PCA is

$$
V=\frac{1}{4}\sum_{i=1}^{4}x_ix_i^T
=
\begin{pmatrix}
0.75&0.25&-0.25&-0.25\\
0.25&0.5&-0.25&0\\
-0.25&-0.25&0.5&0\\
-0.25&0&0&0.25
\end{pmatrix}.
$$

Its eigenvalues are

$$
\frac{5+\sqrt{17}}{8},\quad
\frac{1}{2},\quad
\frac{1}{4},\quad
\frac{5-\sqrt{17}}{8}.
$$

Numerically:

$$
1.1404,\quad 0.5,\quad 0.25,\quad 0.1096.
$$

### 2(a)(i) First principal component by inverse iteration

Use shift $\nu=1$ and initial vector

$$
v^{(0)}=(1,0,0,0)^T.
$$

At each step:

$$
(V-\nu I)w^{(k)}=v^{(k-1)},\qquad
v^{(k)}=\frac{w^{(k)}}{\|w^{(k)}\|_2}.
$$

The first three iterations are:

| Iteration | $v^{(k)}$ | Rayleigh quotient |
|---|---|---|
| 1 | $(0.5669,\ 0.5669,\ -0.5669,\ -0.1890)^T$ | $1.1071$ |
| 2 | $(0.7593,\ 0.4339,\ -0.4339,\ -0.2169)^T$ | $1.1382$ |
| 3 | $(0.7167,\ 0.4727,\ -0.4727,\ -0.1982)^T$ | $1.1402$ |

The exact first PC is the eigenvector for $\lambda_1=(5+\sqrt{17})/8$. One normalized version is approximately

$$
\boxed{
v_1\approx
(0.7257,\ 0.4647,\ -0.4647,\ -0.2037)^T.
}
$$

The negative of this vector is also correct.

### 2(a)(ii) Second principal component by inverse iteration

Use shift $\nu=0.4$ and the same initial vector.

The first three iterations are:

| Iteration | $v^{(k)}$ | Rayleigh quotient |
|---|---|---|
| 1 | $(0.4565,\ -0.3261,\ 0.3261,\ -0.7609)^T$ | $0.4853$ |
| 2 | $(0.5684,\ -0.5050,\ 0.5050,\ -0.4083)^T$ | $0.4956$ |
| 3 | $(0.4840,\ -0.4908,\ 0.4908,\ -0.5328)^T$ | $0.4994$ |

The exact second PC corresponds to eigenvalue $1/2$. One normalized version is

$$
\boxed{
v_2=\frac{1}{2}(-1,1,-1,1)^T.
}
$$

Again, the sign may be reversed.

### 2(a)(iii) MSE using the first two PCs

The MSE is

$$
\frac{1}{4}\sum_{i=1}^{4}
\left\|
x_i-\sum_{j=1}^{2}\langle x_i,v_j\rangle v_j
\right\|_2^2.
$$

For PCA, this equals the sum of omitted covariance eigenvalues:

$$
\lambda_3+\lambda_4
=\frac{1}{4}+\frac{5-\sqrt{17}}{8}
=\frac{7-\sqrt{17}}{8}.
$$

Thus

$$
\boxed{
\text{MSE}=\frac{7-\sqrt{17}}{8}
\approx0.3596.
}
$$

### 2(b)(i) Mean of the functions

The mean in the RKHS is

$$
\boxed{
E=\frac{1}{4}\sum_{i=1}^{4}\Psi(x_i).
}
$$

This is the correct RKHS expression. We do not need to explicitly write $\Psi$.

### 2(b)(ii) Gram matrix for $K(x,y)=\langle x,y\rangle^2$

The dot-product matrix is

$$
\begin{pmatrix}
2&-1&1&1\\
-1&2&-1&0\\
1&-1&2&1\\
1&0&1&2
\end{pmatrix}.
$$

Squaring entrywise gives the Gram matrix

$$
\boxed{
K=
\begin{pmatrix}
4&1&1&1\\
1&4&1&0\\
1&1&4&1\\
1&0&1&4
\end{pmatrix}.
}
$$

### 2(b)(iii) Rayleigh quotient iteration for the largest Gram eigenvalue

Start with

$$
v^{(0)}=(0.5,0.5,0.5,0.5)^T.
$$

This vector is already normalized. The initial Rayleigh quotient is

$$
\lambda^{(0)}=(v^{(0)})^TKv^{(0)}=6.5.
$$

Rayleigh quotient iteration uses

$$
(K-\lambda^{(k-1)}I)w^{(k)}=v^{(k-1)},\qquad
v^{(k)}=\frac{w^{(k)}}{\|w^{(k)}\|_2}.
$$

The first three iterations are:

| Iteration | $v^{(k)}$ | $\lambda^{(k)}$ |
|---|---|---|
| 1 | $(0.5582,\ 0.4341,\ 0.5582,\ 0.4341)^T$ | $6.561538$ |
| 2 | $(0.5573,\ 0.4352,\ 0.5573,\ 0.4352)^T$ | $6.561553$ |
| 3 | $(0.5573,\ 0.4352,\ 0.5573,\ 0.4352)^T$ | $6.561553$ |

The exact largest eigenvalue is

$$
\boxed{
\lambda_{\max}=\frac{9+\sqrt{17}}{2}\approx6.5616.
}
$$

### 2(b)(iv) RKHS MSE using the first kernel PC

The leading unit eigenvector of the Gram matrix may be chosen as

$$
q_1\approx
(0.5573,\ 0.4352,\ 0.5573,\ 0.4352)^T.
$$

For the normalized RKHS principal component,

$$
v=\sum_{i=1}^{4}a_i\Psi(x_i),
\qquad
a=\frac{q_1}{\sqrt{\lambda_{\max}}}.
$$

The MSE after projecting onto one normalized feature-space component is

$$
\frac{1}{4}\sum_{i=1}^{4}
\|\Psi(x_i)-\langle \Psi(x_i),v\rangle_{\mathcal H}v\|_{\mathcal H}^2.
$$

Since one component captures the largest Gram eigenvalue, this equals

$$
\frac{1}{4}\left(\operatorname{tr}(K)-\lambda_{\max}\right).
$$

Here $\operatorname{tr}(K)=4+4+4+4=16$. Therefore

$$
\text{MSE}
=\frac{1}{4}
\left(16-\frac{9+\sqrt{17}}{2}\right)
=\frac{23-\sqrt{17}}{8}.
$$

Thus

$$
\boxed{
\text{MSE}=\frac{23-\sqrt{17}}{8}
\approx2.3596.
}
$$

## Question 3

The residual block is:

1. input $X$, a $100\times100$ grayscale image;
2. convolution with one $5\times5$ filter $K_1$, stride 1, padding 2;
3. ReLU;
4. convolution with one $5\times5$ filter $K_2$, stride 1, padding 2;
5. output equals original input plus the result of Layer 3.

### 3(a) Expression for $Y(X;\theta)$

Let $*$ denote cross-correlation/convolution as used in CNNs, and let $\sigma(z)=\max(z,0)$ be ReLU.

With same padding, both convolutions keep the spatial size $100\times100$. The residual block output is

$$
\boxed{
Y(X;\theta)=X+K_2 * \sigma(K_1 * X).
}
$$

Here

$$
\theta=(K_1,K_2).
$$

More explicitly:

$$
Z_1=K_1*X,\qquad
Z_2=\sigma(Z_1),\qquad
Z_3=K_2*Z_2,\qquad
Y=X+Z_3.
$$

### 3(b) Number of weights

Each filter is $5\times5$, and there is one input channel and one output channel.

Layer 1 weights:

$$
5\cdot5=25.
$$

Layer 3 weights:

$$
5\cdot5=25.
$$

Therefore, ignoring bias terms,

$$
\boxed{
\text{number of weights}=25+25=50.
}
$$

If biases are included, add one bias for each convolutional filter, giving $52$ parameters. But the main weight count is $50$.

### 3(c) Methodology for finding optimal $\theta$

The scalar output is obtained by summing the components of $Y(X;\theta)$:

$$
\bar Y(X;\theta)=\sum_{p,q}Y(X;\theta)_{p,q}.
$$

The labels satisfy $y_i\in\{-1,1\}$.

A training objective is

$$
\min_\theta
\frac{1}{N}\sum_{i=1}^{N}
\left(\operatorname{sign}(\bar Y(x_i;\theta))-y_i\right)^2.
$$

However, $\operatorname{sign}$ is not differentiable and has zero derivative almost everywhere. Therefore, in practice, use a differentiable surrogate loss during training, for example

$$
\min_\theta
\frac{1}{N}\sum_{i=1}^{N}
\left(\bar Y(x_i;\theta)-y_i\right)^2,
$$

or logistic/hinge loss.

The methodology:

1. Initialize $K_1,K_2$ randomly.
2. For each training image $x_i$, compute the forward pass:

$$
Y(x_i;\theta)=x_i+K_2*\sigma(K_1*x_i).
$$

3. Compute scalar score $\bar Y(x_i;\theta)$.
4. Compute loss over a batch or all data.
5. Use backpropagation through:
   - the summation layer,
   - the residual addition,
   - the second convolution,
   - ReLU,
   - the first convolution.
6. Update $\theta$ using gradient descent, SGD, or Adam:

$$
\theta^{(k+1)}=\theta^{(k)}-\eta\nabla_\theta L(\theta^{(k)}).
$$

7. At prediction time, classify by $\operatorname{sign}(\bar Y(X;\theta))$.

## Question 4

### 4(a)(i) Explicit or implicit?

The scheme is

$$
x_i=x_{i-1}
+\frac{h}{2}
\left[
f(t_{i-1},x_{i-1})
+f(t_i,x_{i-1}+h f(t_{i-1},x_{i-1}))
\right].
$$

This is explicit because $x_i$ is computed only from known quantities at step $i-1$. The right-hand side does not contain $x_i$ inside $f$.

So

$$
\boxed{\text{the scheme is explicit}.}
$$

### 4(a)(ii) Truncation error upper bound

The truncation error is

$$
T_i=
\frac{x(t_i)-x(t_{i-1})}{h}
-\frac{1}{2}
\left[
f(t_{i-1},x(t_{i-1}))
+f(t_i,x(t_{i-1})+h f(t_{i-1},x(t_{i-1})))
\right].
$$

Let $t=t_{i-1}$ and $x=x(t)$. Since $x'(t)=f(t,x(t))$, Taylor expansion gives

$$
\frac{x(t+h)-x(t)}{h}
=x'(t)+\frac{h}{2}x''(t)+O(h^2).
$$

Also,

$$
f(t+h,x+h f(t,x))
=f(t,x)+h(f_t+f_x f)(t,x)+O(h^2).
$$

But along the exact solution,

$$
x''(t)=f_t(t,x(t))+f_x(t,x(t))f(t,x(t)).
$$

Therefore

$$
\frac{1}{2}
\left[
f(t,x)
+f(t+h,x+h f(t,x))
\right]
=x'(t)+\frac{h}{2}x''(t)+O(h^2).
$$

Subtracting,

$$
\boxed{|T_i|\le C h^2}
$$

for some constant $C$ depending on bounds for derivatives of $f$ up to order 3.

### 4(a)(iii) Error identity

Let

$$
e_i=x(t_i)-x_i.
$$

Since

$$
x_i-x_{i-1}
=\frac{h}{2}
\left[
f(t_{i-1},x_{i-1})
+f(t_i,x_{i-1}+hf(t_{i-1},x_{i-1}))
\right],
$$

we have

$$
\frac{e_i-e_{i-1}}{h}
=\frac{x(t_i)-x(t_{i-1})}{h}
-\frac{1}{2}
\left[
f(t_{i-1},x_{i-1})
+f(t_i,x_{i-1}+hf(t_{i-1},x_{i-1}))
\right].
$$

Thus the expression in the question is exactly

$$
\boxed{T_i.}
$$

Equivalently, the useful recurrence form is

$$
\begin{aligned}
\frac{e_i-e_{i-1}}{h}
&=
T_i
+\frac{1}{2}
\big[
f(t_{i-1},x(t_{i-1}))-f(t_{i-1},x_{i-1})
\big]\\
&\quad
+\frac{1}{2}
\big[
f(t_i,x(t_{i-1})+hf(t_{i-1},x(t_{i-1})))\\
&\qquad\qquad
-f(t_i,x_{i-1}+hf(t_{i-1},x_{i-1}))
\big].
\end{aligned}
$$

### 4(a)(iv) Global error bound

Assume $f$ is Lipschitz in $x$ with constant $L$.

The first difference term is bounded by

$$
|f(t_{i-1},x(t_{i-1}))-f(t_{i-1},x_{i-1})|
\le L|e_{i-1}|.
$$

For the predictor terms,

$$
\begin{aligned}
&|x(t_{i-1})+hf(t_{i-1},x(t_{i-1}))
-x_{i-1}-hf(t_{i-1},x_{i-1})|\\
&\le |e_{i-1}|+hL|e_{i-1}|
=(1+hL)|e_{i-1}|.
\end{aligned}
$$

Therefore

$$
|e_i|
\le
\left(1+hL+\frac{h^2L^2}{2}\right)|e_{i-1}|
+h|T_i|.
$$

Since $|T_i|\le Ch^2$,

$$
|e_i|
\le
\left(1+hL+\frac{h^2L^2}{2}\right)|e_{i-1}|
+Ch^3.
$$

Using a discrete Gronwall argument and $e_0=0$, we obtain

$$
\boxed{
\max_{0\le i\le N}|e_i|\le C' h^2.
}
$$

So the method is globally second-order accurate.

### 4(b)(i) Forward Euler for $x'=t-x^2$, $x(0)=0$

Use

$$
x_{i+1}=x_i+h(t_i-x_i^2),\qquad h=0.25.
$$

Start with $x_0=0$.

Step 1:

$$
x_1=0+0.25(0-0^2)=0.
$$

Step 2:

$$
x_2=0+0.25(0.25-0^2)=0.0625.
$$

Step 3:

$$
x_3=0.0625+0.25(0.5-0.0625^2)
=0.1865234375.
$$

Thus

$$
\boxed{x_1=0,\qquad x_2=0.0625,\qquad x_3=0.1865.}
$$

### 4(b)(ii) Scheme from 4(a)

The scheme is

$$
x_i=x_{i-1}
+\frac{h}{2}
\left[
f(t_{i-1},x_{i-1})
+f(t_i,x_{i-1}+hf(t_{i-1},x_{i-1}))
\right].
$$

Here $f(t,x)=t-x^2$ and $h=0.25$.

Step 1:

$$
f(t_0,x_0)=f(0,0)=0.
$$

Predictor:

$$
\hat x_1=x_0+h f(t_0,x_0)=0.
$$

Corrector:

$$
x_1=0+\frac{0.25}{2}\left[0+f(0.25,0)\right]
=0.125(0.25)=0.03125.
$$

Step 2:

$$
f(t_1,x_1)=f(0.25,0.03125)=0.25-0.03125^2=0.2490234375.
$$

Predictor:

$$
\hat x_2=0.03125+0.25(0.2490234375)=0.0935058594.
$$

Then

$$
f(t_2,\hat x_2)=f(0.5,0.0935058594)=0.4912567.
$$

So

$$
x_2=0.03125+0.125(0.2490234+0.4912567)=0.1237850.
$$

Step 3:

$$
f(t_2,x_2)=f(0.5,0.1237850)=0.4846773.
$$

Predictor:

$$
\hat x_3=0.1237850+0.25(0.4846773)=0.2449543.
$$

Then

$$
f(t_3,\hat x_3)=f(0.75,0.2449543)=0.6899974.
$$

Thus

$$
x_3=0.1237850+0.125(0.4846773+0.6899974)=0.2706193.
$$

Final:

$$
\boxed{x_1=0.03125,\qquad x_2=0.1237850,\qquad x_3=0.2706193.}
$$

