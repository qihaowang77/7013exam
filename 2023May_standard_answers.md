# 2023 May Standard Answers

Source exam: `2023May.pdf`

These solutions show the main working steps. For eigenvectors and principal components, the sign is arbitrary: if $v$ is correct, then $-v$ is also correct.

Notation is aligned with the lecture slides:

- For ordinary PCA, the principal component/loading vector is the unit eigenvector of the covariance matrix. If the loading is $h_j$, the PC score is $y_j=Xh_j$.
- For kernel PCA, the Gram matrix is $K_{ij}=k(x_i,x_j)=\langle \Psi(x_i),\Psi(x_j)\rangle_{\mathcal H}$. If $Kq_j=\rho_j q_j$ with $\|q_j\|_2=1$, then the normalized RKHS principal component is

$$
v_j=\sum_i a_{ji}\Psi(x_i),\qquad a_j=\frac{q_j}{\sqrt{\rho_j}},
$$

which is the same as the slide condition $Ka_j=n\omega_j a_j$ and $a_j^TKa_j=1$, with $\rho_j=n\omega_j$.

## Question 1

### 1(a) Kernel expression for average distance to the center of mass

Let

$$
\bar \psi=\psi_S(x)=\frac{1}{n}\sum_{j=1}^{n}\psi(x_j).
$$

For each point $x_i$, compute the squared distance first:

$$
\begin{aligned}
\|\psi(x_i)-\bar\psi\|_{\mathcal H}^2
&=\langle \psi(x_i)-\bar\psi,\psi(x_i)-\bar\psi\rangle_{\mathcal H} \\
&=\langle \psi(x_i),\psi(x_i)\rangle_{\mathcal H}
-2\langle \psi(x_i),\bar\psi\rangle_{\mathcal H}
+\langle \bar\psi,\bar\psi\rangle_{\mathcal H}.
\end{aligned}
$$

Using $k(x,y)=\langle \psi(x),\psi(y)\rangle_{\mathcal H}$,

$$
\langle \psi(x_i),\bar\psi\rangle_{\mathcal H}
=\frac{1}{n}\sum_{j=1}^{n}k(x_i,x_j),
$$

and

$$
\langle \bar\psi,\bar\psi\rangle_{\mathcal H}
=\frac{1}{n^2}\sum_{j=1}^{n}\sum_{\ell=1}^{n}k(x_j,x_\ell).
$$

Therefore

$$
\boxed{
\|\psi(x_i)-\bar\psi\|_{\mathcal H}^2
=k(x_i,x_i)
-\frac{2}{n}\sum_{j=1}^{n}k(x_i,x_j)
+\frac{1}{n^2}\sum_{j=1}^{n}\sum_{\ell=1}^{n}k(x_j,x_\ell)
}.
$$

The average distance asked in the question is therefore

$$
\boxed{
\frac{1}{n}\sum_{i=1}^{n}
\sqrt{
k(x_i,x_i)
-\frac{2}{n}\sum_{j=1}^{n}k(x_i,x_j)
+\frac{1}{n^2}\sum_{j=1}^{n}\sum_{\ell=1}^{n}k(x_j,x_\ell)
}
}.
$$

If an examiner instead intended average squared distance, then remove the square root.

### 1(b) Positive definiteness of the Gaussian kernel

The kernel is

$$
k(x,y)=\exp\left(-\frac{\|x-y\|_2^2}{2}\right),\qquad x,y\in\mathbb R^2.
$$

Rewrite it:

$$
\begin{aligned}
k(x,y)
&=\exp\left(-\frac{\|x\|^2+\|y\|^2-2x^Ty}{2}\right)\\
&=\exp\left(-\frac{\|x\|^2}{2}\right)
\exp(x^Ty)
\exp\left(-\frac{\|y\|^2}{2}\right).
\end{aligned}
$$

Now

$$
\exp(x^Ty)=\sum_{m=0}^{\infty}\frac{(x^Ty)^m}{m!}.
$$

Each $(x^Ty)^m$ is a positive definite kernel, nonnegative sums of positive definite kernels are positive definite, and multiplication by $g(x)g(y)$ preserves positive definiteness. Hence $k$ is positive definite.

By Aronszajn's theorem, this kernel induces a unique RKHS $\mathcal H_k$ with reproducing kernel $k$. It is the Gaussian RKHS. It is infinite-dimensional; one possible feature representation uses all monomials:

$$
k(x,y)=\sum_{\alpha\in\mathbb N^2}
\left(e^{-\|x\|^2/2}\frac{x^\alpha}{\sqrt{\alpha!}}\right)
\left(e^{-\|y\|^2/2}\frac{y^\alpha}{\sqrt{\alpha!}}\right).
$$

### 1(c) Average distance for the given point set

The points are

$$
x_1=(0,1),\quad x_2=(-1,3),\quad x_3=(2,4),\quad x_4=(3,-1),\quad x_5=(-1,-2).
$$

Use the formula from 1(a) with $n=5$.

#### Kernel i: $k(x,y)=\langle x,y\rangle$

The Gram matrix is

$$
K=
\begin{pmatrix}
1&3&4&-1&-2\\
3&10&10&-6&-5\\
4&10&20&2&-10\\
-1&-6&2&10&-1\\
-2&-5&-10&-1&5
\end{pmatrix}.
$$

The squared distances to the center of mass are

$$
\frac{9}{25},\quad
\frac{164}{25},\quad
\frac{274}{25},\quad
\frac{244}{25},\quad
\frac{289}{25}.
$$

Therefore the average distance is

$$
\frac{1}{5}\left(
\frac{3}{5}
+\frac{\sqrt{164}}{5}
+\frac{\sqrt{274}}{5}
+\frac{\sqrt{244}}{5}
+\frac{17}{5}
\right)
=
\boxed{2.5992\text{ approximately}}.
$$

#### Kernel ii: $k(x,y)=\langle x,y\rangle^2$

The Gram matrix is

$$
K=
\begin{pmatrix}
1&9&16&1&4\\
9&100&100&36&25\\
16&100&400&4&100\\
1&36&4&100&1\\
4&25&100&1&25
\end{pmatrix}.
$$

The squared distances are

$$
\frac{933}{25},\quad
\frac{1018}{25},\quad
\frac{5018}{25},\quad
\frac{2298}{25},\quad
\frac{293}{25}.
$$

Hence

$$
\boxed{
\frac{1}{25}
\left(\sqrt{933}+\sqrt{1018}+\sqrt{5018}+\sqrt{2298}+\sqrt{293}\right)
=7.9337\text{ approximately}.
}
$$

#### Kernel iii: $k(x,y)=(\langle x,y\rangle+1)^5$

The Gram matrix is

$$
K=
\begin{pmatrix}
32&1024&3125&0&-1\\
1024&161051&161051&-3125&-1024\\
3125&161051&4084101&243&-59049\\
0&-3125&243&161051&0\\
-1&-1024&-59049&0&7776
\end{pmatrix}.
$$

The squared distances are

$$
\frac{4577499}{25},\quad
\frac{5455004}{25},\quad
\frac{64826314}{25},\quad
\frac{7063084}{25},\quad
\frac{5335879}{25}.
$$

Thus the average distance is

$$
\boxed{
\frac{1}{25}
\left(
\sqrt{4577499}+\sqrt{5455004}+\sqrt{64826314}
+\sqrt{7063084}+\sqrt{5335879}
\right)
=699.7673\text{ approximately}.
}
$$

## Question 2

### 2(a) Expression for $\epsilon_k$

Given

$$
x_0=a_1u_1+\cdots+a_mu_m,\qquad a_1\ne0,
$$

and $Au_i=\lambda_i u_i$, we have

$$
A^kx_0=\sum_{i=1}^{m}a_i\lambda_i^ku_i.
$$

Factor out $a_1\lambda_1^k$:

$$
\begin{aligned}
A^kx_0
&=a_1\lambda_1^k
\left[
u_1+\sum_{i=2}^{m}
\frac{a_i}{a_1}
\left(\frac{\lambda_i}{\lambda_1}\right)^k u_i
\right].
\end{aligned}
$$

Therefore

$$
\boxed{
\epsilon_k=
\sum_{i=2}^{m}
\frac{a_i}{a_1}
\left(\frac{\lambda_i}{\lambda_1}\right)^k u_i.
}
$$

### 2(b) Convergence of power iteration

From 2(a),

$$
A^kx_0=a_1\lambda_1^k(u_1+\epsilon_k).
$$

Since

$$
\left|\frac{\lambda_i}{\lambda_1}\right|\le
\left|\frac{\lambda_2}{\lambda_1}\right|<1
\qquad (i\ge2),
$$

we get

$$
\epsilon_k\to0.
$$

The normalized iterate is

$$
x_{k+1}=\frac{A^kx_0}{\|A^kx_0\|_2}
=\frac{a_1\lambda_1^k(u_1+\epsilon_k)}
{|a_1\lambda_1^k|\|u_1+\epsilon_k\|_2}.
$$

Thus $x_{k+1}$ aligns with the direction of $u_1$, up to sign or complex phase.

The Rayleigh quotient satisfies

$$
r_{k+1}=x_{k+1}^TAx_{k+1}\to u_1^TAu_1=\lambda_1.
$$

So

$$
\boxed{x_{k+1}\to \pm u_1,\qquad r_{k+1}\to\lambda_1.}
$$

### 2(c) Convergence rate bound

The question defines

$$
y_k=
\frac{x_k\|A^kx_0\|_2}{a_1\lambda_1^k}.
$$

Since $x_k$ is the normalized version of $A^kx_0$ up to indexing, this is essentially

$$
y_k=\frac{A^kx_0}{a_1\lambda_1^k}.
$$

Using the expansion from 2(a),

$$
y_k=u_1+\epsilon_k.
$$

Therefore

$$
\|y_k-u_1\|_2=\|\epsilon_k\|_2.
$$

Now

$$
\begin{aligned}
\|\epsilon_k\|_2
&=
\left\|
\sum_{i=2}^{m}
\frac{a_i}{a_1}
\left(\frac{\lambda_i}{\lambda_1}\right)^k u_i
\right\|_2\\
&\le
\sum_{i=2}^{m}
\left|\frac{a_i}{a_1}\right|
\left|\frac{\lambda_i}{\lambda_1}\right|^k\\
&\le
\left(\sum_{i=2}^{m}\left|\frac{a_i}{a_1}\right|\right)
\left|\frac{\lambda_2}{\lambda_1}\right|^k.
\end{aligned}
$$

Choose

$$
c=\sum_{i=2}^{m}\left|\frac{a_i}{a_1}\right|.
$$

Then

$$
\boxed{
\|y_k-u_1\|_2\le c\left|\frac{\lambda_2}{\lambda_1}\right|^k.
}
$$

This explains why power iteration is slow when $|\lambda_2|$ is close to $|\lambda_1|$.

### 2(d) Eigenvalues of $A^{-1}$

Suppose $\lambda$ is an eigenvalue of $A$ with eigenvector $u\ne0$:

$$
Au=\lambda u.
$$

Since $A$ is invertible, $\lambda\ne0$. Multiply both sides by $A^{-1}$:

$$
u=\lambda A^{-1}u.
$$

Therefore

$$
A^{-1}u=\frac{1}{\lambda}u.
$$

Hence $u$ is also an eigenvector of $A^{-1}$, and the corresponding eigenvalue is

$$
\boxed{\frac{1}{\lambda}}.
$$

### 2(e) Rayleigh quotient iteration

Start with a normalized vector $x_0$.

1. Compute the Rayleigh quotient:

$$
\mu_k=x_k^TAx_k.
$$

2. Solve the shifted linear system:

$$
(A-\mu_k I)z_k=x_k.
$$

3. Normalize:

$$
x_{k+1}=\frac{z_k}{\|z_k\|_2}.
$$

4. Repeat.

Equivalently:

$$
\boxed{
\mu_k=x_k^TAx_k,\qquad
(A-\mu_kI)z_k=x_k,\qquad
x_{k+1}=\frac{z_k}{\|z_k\|_2}.
}
$$

For symmetric matrices and a sufficiently good starting vector near a simple eigenpair, Rayleigh quotient iteration has ultimately cubic convergence:

$$
\boxed{\|x_{k+1}-u\|=O(\|x_k-u\|^3).}
$$

## Question 3

The data are

$$
x_1=(1,2,3)^T,\quad x_2=(3,4,5)^T,\quad x_3=(2,3,1)^T.
$$

### 3(a)(i) Mean and centered data

The mean is

$$
m=\frac{x_1+x_2+x_3}{3}
=\frac{1}{3}
\begin{pmatrix}
6\\9\\9
\end{pmatrix}
=
\begin{pmatrix}
2\\3\\3
\end{pmatrix}.
$$

Centered data:

$$
\tilde x_1=x_1-m=
\begin{pmatrix}-1\\-1\\0\end{pmatrix},
\quad
\tilde x_2=x_2-m=
\begin{pmatrix}1\\1\\2\end{pmatrix},
\quad
\tilde x_3=x_3-m=
\begin{pmatrix}0\\0\\-2\end{pmatrix}.
$$

### 3(a)(ii) Covariance matrix

Use

$$
C=\frac{1}{3}\sum_{i=1}^{3}\tilde x_i\tilde x_i^T.
$$

Compute

$$
C
=\frac{1}{3}
\left[
\begin{pmatrix}
1&1&0\\1&1&0\\0&0&0
\end{pmatrix}
+
\begin{pmatrix}
1&1&2\\1&1&2\\2&2&4
\end{pmatrix}
+
\begin{pmatrix}
0&0&0\\0&0&0\\0&0&4
\end{pmatrix}
\right].
$$

Thus

$$
\boxed{
C=
\begin{pmatrix}
2/3&2/3&2/3\\
2/3&2/3&2/3\\
2/3&2/3&8/3
\end{pmatrix}.
}
$$

### 3(a)(iii) Eigenvalues and eigenvectors

The eigenvalues are

$$
\boxed{
\lambda_1=2+\frac{2\sqrt3}{3},\qquad
\lambda_2=2-\frac{2\sqrt3}{3},\qquad
\lambda_3=0.
}
$$

Approximate values:

$$
\lambda_1=3.1547,\qquad \lambda_2=0.8453,\qquad \lambda_3=0.
$$

One set of corresponding eigenvectors is:

For $\lambda_1$:

$$
u_1\propto
\begin{pmatrix}
(\sqrt3-1)/2\\
(\sqrt3-1)/2\\
1
\end{pmatrix}
\approx
\begin{pmatrix}
0.3251\\0.3251\\0.8881
\end{pmatrix}.
$$

For $\lambda_2$:

$$
u_2\propto
\begin{pmatrix}
(\sqrt3+1)/2\\
(\sqrt3+1)/2\\
-1
\end{pmatrix}
\approx
\begin{pmatrix}
0.6280\\0.6280\\-0.4597
\end{pmatrix}.
$$

For $\lambda_3=0$:

$$
u_3\propto
\begin{pmatrix}
-1\\1\\0
\end{pmatrix}.
$$

### 3(a)(iv) First principal component/loading and information percentage

Following the slides, the principal component asked for here is the loading vector. It is the unit eigenvector of the covariance matrix corresponding to the largest eigenvalue. Denote it by $h_1$:

$$
\boxed{
h_1\approx
\begin{pmatrix}
0.3251\\0.3251\\0.8881
\end{pmatrix}.
}
$$

The corresponding first PC score for a centered datum $\tilde x$ is

$$
y_1=\langle \tilde x,h_1\rangle.
$$

The total variance is

$$
\lambda_1+\lambda_2+\lambda_3=4.
$$

The percentage carried by the first PC is

$$
\frac{\lambda_1}{4}\times100\%
=\frac{2+2\sqrt3/3}{4}\times100\%
=78.8675\%.
$$

Rounded to one decimal digit:

$$
\boxed{78.9\%.}
$$

### 3(a)(v) Second principal component/loading and first-two-PC information

The second principal component/loading vector is the unit eigenvector corresponding to the second largest eigenvalue. Denote it by $h_2$:

$$
\boxed{
h_2\approx
\begin{pmatrix}
0.6280\\0.6280\\-0.4597
\end{pmatrix}.
}
$$

The corresponding second PC score is

$$
y_2=\langle \tilde x,h_2\rangle.
$$

The first two principal components contain

$$
\frac{\lambda_1+\lambda_2}{\lambda_1+\lambda_2+\lambda_3}\times100\%
=\frac{4}{4}\times100\%
=\boxed{100.0\%}.
$$

This happens because there are only two nonzero eigenvalues after centering.

### 3(b)(i) Gram matrix for kernel PCA

The kernel is

$$
k(x,y)=\langle x,y\rangle^2.
$$

First compute dot products:

$$
\begin{aligned}
\langle x_1,x_1\rangle&=14,&
\langle x_1,x_2\rangle&=26,&
\langle x_1,x_3\rangle&=11,\\
\langle x_2,x_2\rangle&=50,&
\langle x_2,x_3\rangle&=23,&
\langle x_3,x_3\rangle&=14.
\end{aligned}
$$

Squaring each dot product gives

$$
\boxed{
K=
\begin{pmatrix}
196&676&121\\
676&2500&529\\
121&529&196
\end{pmatrix}.
}
$$

### 3(b)(ii) Eigenvalue problem of the Gram matrix

Solve

$$
Kq=\rho q.
$$

Numerically, the eigenvalues are

$$
\boxed{
\rho_1=2796.7466,\qquad
\rho_2=88.9784,\qquad
\rho_3=6.2750.
}
$$

A unit eigenvector for the largest eigenvalue may be chosen as

$$
q_1\approx
\begin{pmatrix}
0.2552\\
0.9451\\
0.2041
\end{pmatrix}.
$$

This $q_1\in\mathbb R^3$ is the Euclidean-normalized Gram eigenvector, not the principal component itself. The slide notation for the actual RKHS principal component/loading is

$$
v_1=\sum_{i=1}^{3}a_i\Psi(x_i),
$$

with the normalization condition

$$
\langle v_1,v_1\rangle_{\mathcal H}=a^TKa=1.
$$

Because $Kq_1=\rho_1q_1$ and $\|q_1\|_2=1$, choose

$$
a=\frac{q_1}{\sqrt{\rho_1}}.
$$

This also matches the lecture formula $Ka_1=n\omega_1a_1$ with $\rho_1=n\omega_1$.

So the loading/eigenfunction is $v_1\in\mathcal H$. The vector $Ka$ would be the PC scores on the training samples, not the Hilbert-space loading.

Since $\sqrt{\rho_1}\approx52.8843$,

$$
a\approx
\begin{pmatrix}
0.004825\\
0.017871\\
0.003860
\end{pmatrix}.
$$

### 3(b)(iii) Projection of $x=(1,1,1)^T$ onto the first kernel PC

Strictly, the object being projected is $\Psi(x)$, and the first kernel PC is the normalized RKHS vector $v_1$. The PC score/projection coordinate is

$$
y_1(x)=\langle \Psi(x),v_1\rangle_{\mathcal H}
=\sum_{i=1}^{3}a_i k(x_i,x).
$$

Compute

$$
k(x_1,x)=(1+2+3)^2=36,
$$

$$
k(x_2,x)=(3+4+5)^2=144,
$$

$$
k(x_3,x)=(2+3+1)^2=36.
$$

Therefore

$$
\begin{aligned}
\langle \Psi(x),v_1\rangle_{\mathcal H}
&\approx
0.004825(36)+0.017871(144)+0.003860(36)\\
&=\boxed{2.8861}.
\end{aligned}
$$

If the opposite sign is chosen for the eigenvector, the projection coordinate is $-2.8861$. This sign difference is not mathematically important.

## Question 4

### 4(a)(i) Exact solution

The ODE is

$$
\frac{dx}{dt}=2tx,\qquad x(0)=3.
$$

Separate variables:

$$
\frac{1}{x}dx=2t\,dt.
$$

Integrate:

$$
\ln |x|=t^2+C.
$$

Thus

$$
x(t)=Ce^{t^2}.
$$

Using $x(0)=3$ gives $C=3$. Therefore

$$
\boxed{x(t)=3e^{t^2}.}
$$

### 4(a)(ii) Forward Euler with $h=0.25$

Forward Euler:

$$
x_{i+1}=x_i+h f(t_i,x_i),
\qquad f(t,x)=2tx.
$$

Here $h=0.25$ and $x_0=3$.

Step 1:

$$
t_0=0,\qquad
x_1=3+0.25(2\cdot0\cdot3)=3.
$$

Step 2:

$$
t_1=0.25,\qquad
x_2=3+0.25(2\cdot0.25\cdot3)=3+0.375=3.375.
$$

Step 3:

$$
t_2=0.5,\qquad
x_3=3.375+0.25(2\cdot0.5\cdot3.375)=4.21875.
$$

So

$$
\boxed{x_1=3,\qquad x_2=3.375,\qquad x_3=4.21875.}
$$

### 4(a)(iii) Backward Euler with $h=0.25$

Backward Euler:

$$
x_{i+1}=x_i+h f(t_{i+1},x_{i+1}).
$$

Here

$$
x_{i+1}=x_i+0.25(2t_{i+1}x_{i+1}).
$$

So

$$
x_{i+1}(1-0.5t_{i+1})=x_i,
\qquad
x_{i+1}=\frac{x_i}{1-0.5t_{i+1}}.
$$

Step 1:

$$
t_1=0.25,\qquad
x_1=\frac{3}{1-0.5(0.25)}
=\frac{3}{0.875}
=\frac{24}{7}=3.4286.
$$

Step 2:

$$
t_2=0.5,\qquad
x_2=\frac{24/7}{1-0.5(0.5)}
=\frac{24/7}{0.75}
=\frac{32}{7}=4.5714.
$$

Step 3:

$$
t_3=0.75,\qquad
x_3=\frac{32/7}{1-0.5(0.75)}
=\frac{32/7}{0.625}
=\frac{256}{35}=7.3143.
$$

Thus

$$
\boxed{x_1=3.4286,\qquad x_2=4.5714,\qquad x_3=7.3143.}
$$

### 4(b)(i) Where does the training data come from?

For a Neural ODE model

$$
\frac{dx}{dt}=f(x(t),t,\theta),
$$

the training data come from observed input-output pairs or observed trajectories. Typical examples:

- initial states $x(0)$ and target labels/classes;
- initial states $x(t_0)$ and observed terminal states $x(t_1)$;
- time-series observations $\{x(t_i)\}$ from real measurements or simulations.

The parameter $\theta$ is trained so that the ODE solution matches the target outputs or observed trajectories under a chosen loss function.

### 4(b)(ii) Adjoint method for $\partial L/\partial\theta$

Forward solve:

$$
x(t_1)=\operatorname{ODESolve}(f,x(t_0),t_0,t_1).
$$

Let the loss be $L=L(x(t_1),y)$.

Define the adjoint state

$$
a(t)=\frac{\partial L}{\partial x(t)}.
$$

The terminal condition is

$$
a(t_1)=\frac{\partial L}{\partial x(t_1)}.
$$

The adjoint ODE is solved backward in time:

$$
\boxed{
\frac{da(t)}{dt}
=-a(t)^T\frac{\partial f(x(t),t,\theta)}{\partial x}.
}
$$

The parameter gradient is accumulated by

$$
\boxed{
\frac{\partial L}{\partial\theta}
=\int_{t_0}^{t_1}
a(t)^T
\frac{\partial f(x(t),t,\theta)}{\partial\theta}
\,dt.
}
$$

Equivalent backward-time formulas may have the opposite sign depending on the integration limits. The essential idea is:

1. solve the Neural ODE forward to get $x(t_1)$;
2. initialize $a(t_1)=\partial L/\partial x(t_1)$;
3. solve the augmented adjoint system backward;
4. accumulate $\partial L/\partial\theta$ from $a(t)^T\partial f/\partial\theta$.
