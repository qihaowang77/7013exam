# 2025 May Standard Answers

Source exam: `2025May.pdf`

These solutions show detailed working steps. Eigenvectors and principal components may differ by a sign.

Notation is aligned with the lecture slides:

- Jacobi uses the splitting $A=D+R$ and $x^{(k+1)}=D^{-1}(b-Rx^{(k)})$.
- For ordinary PCA, the requested principal component $w$ is the loading vector. The PC score for a datum $x_i$ is $\langle x_i,w\rangle$.
- For kernel PCA, the Gram matrix is $K_{ij}=K(x_i,x_j)=\langle K(x_i,\cdot),K(x_j,\cdot)\rangle_{\mathcal H}$. If $Kq_j=\rho_jq_j$ and $\|q_j\|_2=1$, then the normalized RKHS principal component is $v_j=\sum_i a_{ji}K(x_i,\cdot)$ with $a_j=q_j/\sqrt{\rho_j}$, matching the slide condition $Ka_j=n\omega_ja_j$ and $a_j^TKa_j=1$.

## Question 1

The matrix is

$$
A_n=
\begin{pmatrix}
2&-1&0&\cdots&0\\
-1&2&-1&\cdots&0\\
0&-1&2&\ddots&0\\
\vdots&\vdots&\ddots&\ddots&-1\\
0&0&0&-1&2
\end{pmatrix}.
$$

### 1(a) LU factorization: show $n=3,4,5$, then any $n$

Use Doolittle LU:

$$
A_n=LU,
$$

where $L$ has diagonal entries equal to 1.

The useful exam strategy is:

1. Work out a few small cases.
2. Observe the diagonal pattern in $U$.
3. Write the general formula.
4. Justify the formula by the elimination recurrence.

#### Case 1: $n=3$

For

$$
A_3=
\begin{pmatrix}
2&-1&0\\
-1&2&-1\\
0&-1&2
\end{pmatrix},
$$

the first pivot is

$$
d_1=2.
$$

The multiplier used to eliminate the entry $-1$ below the first pivot is

$$
\ell_{21}=\frac{-1}{2}=-\frac12.
$$

The second pivot is therefore

$$
d_2=2-\ell_{21}(-1)
=2-\left(-\frac12\right)(-1)
=2-\frac12
=\frac32.
$$

The next multiplier is

$$
\ell_{32}=\frac{-1}{d_2}
=\frac{-1}{3/2}
=-\frac23.
$$

The third pivot is

$$
d_3=2-\ell_{32}(-1)
=2-\left(-\frac23\right)(-1)
=2-\frac23
=\frac43.
$$

So

$$
L_3=
\begin{pmatrix}
1&0&0\\
-1/2&1&0\\
0&-2/3&1
\end{pmatrix},
\qquad
U_3=
\begin{pmatrix}
2&-1&0\\
0&3/2&-1\\
0&0&4/3
\end{pmatrix}.
$$

Notice the diagonal of $U_3$:

$$
2,\frac32,\frac43.
$$

#### Case 2: $n=4$

For

$$
A_4=
\begin{pmatrix}
2&-1&0&0\\
-1&2&-1&0\\
0&-1&2&-1\\
0&0&-1&2
\end{pmatrix},
$$

the first three pivots and multipliers are the same as in the $n=3$ case:

$$
d_1=2,\qquad
\ell_{21}=-\frac12,
$$

$$
d_2=\frac32,\qquad
\ell_{32}=-\frac23,
$$

$$
d_3=\frac43.
$$

Now eliminate the entry below the third pivot:

$$
\ell_{43}=\frac{-1}{d_3}
=\frac{-1}{4/3}
=-\frac34.
$$

The fourth pivot is

$$
d_4=2-\ell_{43}(-1)
=2-\left(-\frac34\right)(-1)
=2-\frac34
=\frac54.
$$

Thus

$$
L_4=
\begin{pmatrix}
1&0&0&0\\
-1/2&1&0&0\\
0&-2/3&1&0\\
0&0&-3/4&1
\end{pmatrix},
\qquad
U_4=
\begin{pmatrix}
2&-1&0&0\\
0&3/2&-1&0\\
0&0&4/3&-1\\
0&0&0&5/4
\end{pmatrix}.
$$

Now the diagonal of $U_4$ is

$$
2,\frac32,\frac43,\frac54.
$$

#### Case 3: $n=5$

For $A_5$, continue the same elimination one more step. From the $n=4$ case,

$$
d_4=\frac54.
$$

The next multiplier is

$$
\ell_{54}=\frac{-1}{d_4}
=\frac{-1}{5/4}
=-\frac45.
$$

The fifth pivot is

$$
d_5=2-\ell_{54}(-1)
=2-\left(-\frac45\right)(-1)
=2-\frac45
=\frac65.
$$

Therefore

$$
L_5=
\begin{pmatrix}
1&0&0&0&0\\
-1/2&1&0&0&0\\
0&-2/3&1&0&0\\
0&0&-3/4&1&0\\
0&0&0&-4/5&1
\end{pmatrix},
$$

and

$$
U_5=
\begin{pmatrix}
2&-1&0&0&0\\
0&3/2&-1&0&0\\
0&0&4/3&-1&0\\
0&0&0&5/4&-1\\
0&0&0&0&6/5
\end{pmatrix}.
$$

The diagonal of $U_5$ is

$$
2,\frac32,\frac43,\frac54,\frac65.
$$

This makes the pattern clear:

$$
d_i=\frac{i+1}{i}.
$$

The subdiagonal entries of $L$ are also visible:

$$
-\frac12,-\frac23,-\frac34,-\frac45,\ldots
$$

so the expected general form is

$$
L_{i+1,i}=-\frac{i}{i+1}.
$$

#### General case: any $n$

Let the diagonal entries of $U$ be $d_i$. Since $A_n$ is tridiagonal, the LU factors keep the same banded structure:

$$
U=
\begin{pmatrix}
d_1&-1&0&\cdots&0\\
0&d_2&-1&\cdots&0\\
0&0&d_3&\ddots&0\\
\vdots&\vdots&\ddots&\ddots&-1\\
0&0&0&0&d_n
\end{pmatrix},
$$

and

$$
L=
\begin{pmatrix}
1&0&0&\cdots&0\\
\ell_{21}&1&0&\cdots&0\\
0&\ell_{32}&1&\ddots&0\\
\vdots&\vdots&\ddots&\ddots&0\\
0&0&0&\ell_{n,n-1}&1
\end{pmatrix}.
$$

At step $i$, the multiplier is

$$
\ell_{i+1,i}=\frac{-1}{d_i}.
$$

The next pivot is obtained by subtracting the effect of the previous elimination:

$$
d_{i+1}=2-\ell_{i+1,i}(-1).
$$

Since

$$
\ell_{i+1,i}=-\frac{1}{d_i},
$$

we get the recurrence

$$
d_{i+1}=2-\frac{1}{d_i}.
$$

From the small cases,

$$
d_1=2=\frac21,\qquad
d_2=\frac32,\qquad
d_3=\frac43,\qquad
d_4=\frac54,\qquad
d_5=\frac65.
$$

So conjecture

$$
d_i=\frac{i+1}{i}.
$$

Check by induction. Assume

$$
d_i=\frac{i+1}{i}.
$$

Then

$$
d_{i+1}
=2-\frac{1}{d_i}
=2-\frac{i}{i+1}
=\frac{2i+2-i}{i+1}
=\frac{i+2}{i+1}.
$$

This is exactly the same formula with $i$ replaced by $i+1$. Therefore, for any $n$,

$$
\boxed{
U_{ii}=\frac{i+1}{i},\quad i=1,\ldots,n,
\qquad
U_{i,i+1}=-1,\quad i=1,\ldots,n-1.
}
$$

and

$$
\boxed{
L_{ii}=1,\quad
L_{i+1,i}=-\frac{i}{i+1},\quad i=1,\ldots,n-1.
}
$$

All other entries are zero.

### 1(b) Solve $A_5x=b$

Here

$$
b=(3,-2,2,-2,1)^T.
$$

Using the LU formula:

$$
L=
\begin{pmatrix}
1&0&0&0&0\\
-1/2&1&0&0&0\\
0&-2/3&1&0&0\\
0&0&-3/4&1&0\\
0&0&0&-4/5&1
\end{pmatrix},
$$

$$
U=
\begin{pmatrix}
2&-1&0&0&0\\
0&3/2&-1&0&0\\
0&0&4/3&-1&0\\
0&0&0&5/4&-1\\
0&0&0&0&6/5
\end{pmatrix}.
$$

First solve $Ly=b$.

Row 1:

$$
y_1=3.
$$

Row 2:

$$
-\frac{1}{2}y_1+y_2=-2
\quad\Rightarrow\quad
y_2=-2+\frac{3}{2}=-\frac{1}{2}.
$$

Row 3:

$$
-\frac{2}{3}y_2+y_3=2
\quad\Rightarrow\quad
y_3=2+\frac{2}{3}\left(-\frac{1}{2}\right)=\frac{5}{3}.
$$

Row 4:

$$
-\frac{3}{4}y_3+y_4=-2
\quad\Rightarrow\quad
y_4=-2+\frac{3}{4}\cdot\frac{5}{3}
=-\frac{3}{4}.
$$

Row 5:

$$
-\frac{4}{5}y_4+y_5=1
\quad\Rightarrow\quad
y_5=1+\frac{4}{5}\left(-\frac{3}{4}\right)=\frac{2}{5}.
$$

So

$$
y=\left(3,-\frac12,\frac53,-\frac34,\frac25\right)^T.
$$

Now solve $Ux=y$ by back substitution.

Row 5:

$$
\frac{6}{5}x_5=\frac{2}{5}
\quad\Rightarrow\quad
x_5=\frac{1}{3}.
$$

Row 4:

$$
\frac{5}{4}x_4-x_5=-\frac{3}{4}
\quad\Rightarrow\quad
\frac{5}{4}x_4=-\frac{3}{4}+\frac{1}{3}
=-\frac{5}{12},
$$

so

$$
x_4=-\frac{1}{3}.
$$

Row 3:

$$
\frac{4}{3}x_3-x_4=\frac{5}{3}
\quad\Rightarrow\quad
\frac{4}{3}x_3=\frac{5}{3}-\frac{1}{3}=\frac{4}{3},
$$

so

$$
x_3=1.
$$

Row 2:

$$
\frac{3}{2}x_2-x_3=-\frac{1}{2}
\quad\Rightarrow\quad
\frac{3}{2}x_2=\frac{1}{2},
$$

so

$$
x_2=\frac{1}{3}.
$$

Row 1:

$$
2x_1-x_2=3
\quad\Rightarrow\quad
2x_1=\frac{10}{3},
$$

so

$$
x_1=\frac{5}{3}.
$$

Thus

$$
\boxed{
x=
\left(
\frac{5}{3},\frac{1}{3},1,-\frac{1}{3},\frac{1}{3}
\right)^T.
}
$$

### 1(c) First three Gauss-Jacobi iterations

Following the slides, write $A=D+R$, where $D=2I$ and $R$ contains the off-diagonal entries. The Jacobi iteration is

$$
x^{(k+1)}=D^{-1}(b-Rx^{(k)}).
$$

The equation is

$$
2x_i-x_{i-1}-x_{i+1}=b_i,
$$

with missing boundary terms omitted. Hence Jacobi updates are

$$
x_1^{(k+1)}=\frac{b_1+x_2^{(k)}}{2},
$$

$$
x_i^{(k+1)}=\frac{b_i+x_{i-1}^{(k)}+x_{i+1}^{(k)}}{2},
\qquad i=2,3,4,
$$

$$
x_5^{(k+1)}=\frac{b_5+x_4^{(k)}}{2}.
$$

Start with

$$
x^{(0)}=(0,0,0,0,0)^T.
$$

Iteration 1:

$$
x^{(1)}
=
\left(
\frac32,-1,1,-1,\frac12
\right)^T.
$$

Iteration 2:

$$
x^{(2)}
=
\left(
1,\frac14,0,-\frac14,0
\right)^T.
$$

Iteration 3:

$$
x^{(3)}
=
\left(
\frac{13}{8},-\frac12,1,-1,\frac38
\right)^T.
$$

So

$$
\boxed{
x^{(1)}=\left(\frac32,-1,1,-1,\frac12\right)^T,\quad
x^{(2)}=\left(1,\frac14,0,-\frac14,0\right)^T,\quad
x^{(3)}=\left(\frac{13}{8},-\frac12,1,-1,\frac38\right)^T.
}
$$

### 1(d) Convergence of Jacobi for any $n$

In the slide notation, the Jacobi scheme has iteration matrix

$$
G_J=-D^{-1}R=I-D^{-1}A_n,
$$

where $D=2I$. Thus

$$
G_J=\frac12
\begin{pmatrix}
0&1&0&\cdots&0\\
1&0&1&\cdots&0\\
0&1&0&\ddots&0\\
\vdots&\vdots&\ddots&\ddots&1\\
0&0&0&1&0
\end{pmatrix}.
$$

The eigenvalues of this Jacobi iteration matrix are

$$
\mu_k=\cos\left(\frac{k\pi}{n+1}\right),\qquad k=1,\ldots,n.
$$

Therefore

$$
\rho(G_J)=\max_k|\mu_k|
=\cos\left(\frac{\pi}{n+1}\right)<1.
$$

Hence Jacobi converges for any initial vector and any $b\in\mathbb R^n$.

$$
\boxed{\text{Yes, Gauss-Jacobi converges for all }n\text{ and all }b.}
$$

## Question 2

### 2(a) Cross-correlation

Given

$$
I=
\begin{pmatrix}
0&2&1\\
4&3&5\\
8&7&6
\end{pmatrix},
\qquad
K=
\begin{pmatrix}
0&1\\
2&3
\end{pmatrix}.
$$

Cross-correlation uses the kernel without flipping.

Top-left entry:

$$
0\cdot0+2\cdot1+4\cdot2+3\cdot3=19.
$$

Top-right entry:

$$
2\cdot0+1\cdot1+3\cdot2+5\cdot3=22.
$$

Bottom-left entry:

$$
4\cdot0+3\cdot1+8\cdot2+7\cdot3=40.
$$

Bottom-right entry:

$$
3\cdot0+5\cdot1+7\cdot2+6\cdot3=37.
$$

Therefore

$$
\boxed{
I*K=
\begin{pmatrix}
19&22\\
40&37
\end{pmatrix}.
}
$$

### 2(b) Padding

Given

$$
I=
\begin{pmatrix}
2&1&0\\
4&3&5\\
6&8&7
\end{pmatrix},
\qquad
K=
\begin{pmatrix}
0&1\\
2&3
\end{pmatrix}.
$$

Pad $I$ with zeros to form

$$
\tilde I=
\begin{pmatrix}
0&0&0&0&0\\
0&2&1&0&0\\
0&4&3&5&0\\
0&6&8&7&0\\
0&0&0&0&0
\end{pmatrix}.
$$

The output size is $4\times4$. Computing each $2\times2$ patch gives

$$
\boxed{
\tilde I*K=
\begin{pmatrix}
6&7&2&0\\
14&18&21&10\\
22&39&42&14\\
6&8&7&0
\end{pmatrix}.
}
$$

### 2(c) Positive definiteness of Gaussian kernel

The kernel is

$$
K(x,x')=e^{-\gamma\|x-x'\|_2^2},
\qquad \gamma>0.
$$

Rewrite:

$$
\begin{aligned}
K(x,x')
&=e^{-\gamma\|x\|^2}
e^{-\gamma\|x'\|^2}
e^{2\gamma x^Tx'}.
\end{aligned}
$$

Now

$$
e^{2\gamma x^Tx'}
=\sum_{m=0}^{\infty}
\frac{(2\gamma)^m}{m!}(x^Tx')^m.
$$

For each $m$, $(x^Tx')^m$ is positive definite because it is the inner product of degree-$m$ tensor features. Nonnegative sums of positive definite kernels are positive definite, so $e^{2\gamma x^Tx'}$ is positive definite.

Finally, multiplying a positive definite kernel by $g(x)g(x')$ preserves positive definiteness. Here

$$
g(x)=e^{-\gamma\|x\|^2}.
$$

Therefore

$$
\boxed{K(x,x')=e^{-\gamma\|x-x'\|^2}\text{ is positive definite}.}
$$

### 2(d) Complexity for two $n\times n$ matrices

Each entry of $AB$ is a length-$n$ dot product, costing $O(n)$. There are $n^2$ entries.

Thus

$$
\boxed{O(n^3).}
$$

### 2(e) Complexity for $A\in\mathbb R^{m\times n}$ and $B\in\mathbb R^{n\times r}$

The product $AB$ has size $m\times r$. Each entry costs $O(n)$.

Therefore

$$
\boxed{O(mnr).}
$$

## Question 3

The data matrix is $X=[x_1,x_2,x_3,x_4]\in\mathbb R^{6\times4}$, with

$$
x_1=
\begin{pmatrix}2\\0\\0\\0\\0\\0\end{pmatrix},
\quad
x_2=
\begin{pmatrix}0\\-1\\0\\0\\0\\0\end{pmatrix},
\quad
x_3=
\begin{pmatrix}0\\1/\sqrt2\\0\\0\\1/\sqrt2\\0\end{pmatrix},
\quad
x_4=
\begin{pmatrix}0\\0\\1/\sqrt2\\0\\0\\1/\sqrt2\end{pmatrix}.
$$

No centering is required.

### 3(a) First principal component/loading and MSE

Because the question says no centering is required, use the uncentered covariance matrix

$$
V=\frac14XX^T.
$$

Multiplying by $1/4$ changes eigenvalues but not eigenvectors. The first data vector has norm $2$, so it contributes the dominant direction along $e_1$. The largest eigenvalue of $XX^T$ is $4$, equivalently the largest eigenvalue of $V$ is $1$, with loading vector

$$
\boxed{
w=e_1=(1,0,0,0,0,0)^T.
}
$$

This is the first principal component/loading vector in the slide sense. The PC score for sample $x_i$ is

$$
y_{i1}=\langle x_i,w\rangle.
$$

The projection of $x_1$ onto $w$ is exactly $x_1$, so it has zero residual. The other three data points are orthogonal to $w$, so their residuals are themselves.

Their squared norms are

$$
\|x_2\|^2=1,\qquad
\|x_3\|^2=1,\qquad
\|x_4\|^2=1.
$$

Therefore

$$
\frac{1}{4}\sum_{i=1}^{4}
\|x_i-\langle x_i,w\rangle w\|_2^2
=\frac{0+1+1+1}{4}
=\boxed{\frac{3}{4}}.
$$

### 3(b) Power iteration for $X^TX$

First compute

$$
X^TX=
\begin{pmatrix}
4&0&0&0\\
0&1&-\frac{\sqrt2}{2}&0\\
0&-\frac{\sqrt2}{2}&1&0\\
0&0&0&1
\end{pmatrix}.
$$

The initial vector is

$$
v^{(0)}=\left(\frac{1}{\sqrt2},0,0,\frac{1}{\sqrt2}\right)^T.
$$

Use the power-iteration notation from the slides:

$$
z^{(k)}=X^TXv^{(k-1)},\qquad
v^{(k)}=\frac{z^{(k)}}{\|z^{(k)}\|_2},\qquad
\omega^{(k)}=(v^{(k)})^TX^TXv^{(k)}.
$$

#### Iteration 1

$$
z^{(1)}=X^TXv^{(0)}
=\left(\frac{4}{\sqrt2},0,0,\frac{1}{\sqrt2}\right)^T.
$$

Normalize:

$$
v^{(1)}
=\left(\frac{4}{\sqrt{17}},0,0,\frac{1}{\sqrt{17}}\right)^T.
$$

Rayleigh quotient:

$$
\omega^{(1)}
=\frac{65}{17}
\approx3.8235.
$$

#### Iteration 2

$$
z^{(2)}=X^TXv^{(1)}
=\left(\frac{16}{\sqrt{17}},0,0,\frac{1}{\sqrt{17}}\right)^T,
\qquad
v^{(2)}
=\left(\frac{16}{\sqrt{257}},0,0,\frac{1}{\sqrt{257}}\right)^T.
$$

Rayleigh quotient:

$$
\omega^{(2)}
=\frac{1025}{257}
\approx3.9883.
$$

#### Iteration 3

$$
z^{(3)}=X^TXv^{(2)}
=\left(\frac{64}{\sqrt{257}},0,0,\frac{1}{\sqrt{257}}\right)^T,
\qquad
v^{(3)}
=\left(\frac{64}{\sqrt{4097}},0,0,\frac{1}{\sqrt{4097}}\right)^T.
$$

Rayleigh quotient:

$$
\omega^{(3)}
=\frac{16385}{4097}
\approx3.9993.
$$

Thus the largest eigenvalue is approached rapidly:

$$
\boxed{\omega_{\max}=4.}
$$

### 3(c) SVD decomposition for $X$

The eigenvalues of $X^TX$ are

$$
4,\qquad
1+\frac{1}{\sqrt2},\qquad
1,\qquad
1-\frac{1}{\sqrt2}.
$$

Therefore the singular values are

$$
\boxed{
\sigma_1=2,\quad
\sigma_2=\sqrt{1+\frac{1}{\sqrt2}},\quad
\sigma_3=1,\quad
\sigma_4=\sqrt{1-\frac{1}{\sqrt2}}.
}
$$

Choose the right singular vectors:

$$
v_1=
\begin{pmatrix}1\\0\\0\\0\end{pmatrix},
\quad
v_2=
\frac{1}{\sqrt2}
\begin{pmatrix}0\\-1\\1\\0\end{pmatrix},
\quad
v_3=
\begin{pmatrix}0\\0\\0\\1\end{pmatrix},
\quad
v_4=
\frac{1}{\sqrt2}
\begin{pmatrix}0\\1\\1\\0\end{pmatrix}.
$$

The left singular vectors are $u_i=Xv_i/\sigma_i$:

$$
u_1=
\begin{pmatrix}1\\0\\0\\0\\0\\0\end{pmatrix},
$$

$$
u_2=
\frac{1}{\sqrt{4+2\sqrt2}}
\begin{pmatrix}
0\\1+\sqrt2\\0\\0\\1\\0
\end{pmatrix},
$$

$$
u_3=
\begin{pmatrix}
0\\0\\1/\sqrt2\\0\\0\\1/\sqrt2
\end{pmatrix},
$$

$$
u_4=
\frac{1}{\sqrt{4-2\sqrt2}}
\begin{pmatrix}
0\\1-\sqrt2\\0\\0\\1\\0
\end{pmatrix}.
$$

Thus the economy SVD is

$$
\boxed{
X=U\Sigma V^T,
}
$$

where

$$
U=[u_1,u_2,u_3,u_4],
\qquad
V=[v_1,v_2,v_3,v_4],
$$

and

$$
\Sigma=
\operatorname{diag}
\left(
2,\sqrt{1+\frac{1}{\sqrt2}},1,\sqrt{1-\frac{1}{\sqrt2}}
\right).
$$

For a full SVD, extend $U$ to a $6\times6$ orthonormal matrix by adding two orthonormal vectors spanning the null space.

### 3(d)(i) Kernel PCA first principal component

The kernel is

$$
K(x,x')=|\langle x,x'\rangle|^2.
$$

The Gram matrix, in the same notation as the kernel PCA slides, is

$$
K_{ij}=K(x_i,x_j)=\langle K(x_i,\cdot),K(x_j,\cdot)\rangle_{\mathcal H}
=|\langle x_i,x_j\rangle|^2.
$$

Since

$$
X^TX=
\begin{pmatrix}
4&0&0&0\\
0&1&-\frac{\sqrt2}{2}&0\\
0&-\frac{\sqrt2}{2}&1&0\\
0&0&0&1
\end{pmatrix},
$$

we square entries to get

$$
\boxed{
K=
\begin{pmatrix}
16&0&0&0\\
0&1&1/2&0\\
0&1/2&1&0\\
0&0&0&1
\end{pmatrix}.
}
$$

The largest Gram eigenvalue is

$$
\rho_1=16,
$$

with Euclidean-normalized Gram eigenvector

$$
q_1=(1,0,0,0)^T.
$$

The slide eigenvalue notation is $Ka_1=4\omega_1a_1$, so $\rho_1=4\omega_1$. The actual RKHS principal component has form

$$
v=\sum_{i=1}^{4}a_iK(x_i,\cdot).
$$

Normalize by requiring

$$
\langle v,v\rangle_{\mathcal H}=a^TKa=1.
$$

Since $Kq_1=\rho_1q_1$ and $\|q_1\|_2=1$, take

$$
a=\frac{q_1}{\sqrt{\rho_1}}
=\left(\frac{1}{4},0,0,0\right)^T.
$$

This gives

$$
a^TKa=\frac{1}{16}\cdot16=1.
$$

Therefore

$$
\boxed{
v(\cdot)=\frac{1}{4}K(x_1,\cdot)
}
$$

is the first kernel principal component.

### 3(d)(ii) Kernel PCA MSE

The MSE is

$$
\frac{1}{4}\sum_{i=1}^{4}
\|K(x_i,\cdot)-\langle v,K(x_i,\cdot)\rangle_{\mathcal H}v\|_{\mathcal H}^2.
$$

For a normalized first kernel principal component, the captured average RKHS variance is $\rho_1/4$. Hence, using the slide PCA error identity,

$$
\text{MSE}
=\frac{1}{4}(\operatorname{tr}(K)-\rho_1).
$$

Here

$$
\operatorname{tr}(K)=16+1+1+1=19,
\qquad
\rho_1=16.
$$

Thus

$$
\boxed{
\text{MSE}=\frac{19-16}{4}=\frac{3}{4}.
}
$$

### 3(e) Comparison of PCA and kernel PCA

From 3(a), linear PCA with the first component gives

$$
\text{MSE}_{\text{PCA}}=\frac{3}{4}.
$$

From 3(d)(ii), kernel PCA gives

$$
\text{MSE}_{\text{kernel PCA}}=\frac{3}{4}.
$$

Therefore, for this particular data set and this first-component comparison,

$$
\boxed{
\text{the two approaches have the same MSE performance.}
}
$$

Interpretation: both methods identify the dominant contribution from $x_1$, which has much larger squared norm/kernel self-similarity than the other samples.

## Question 4

### 4(a) Theta-method with $\theta=0.5$

The general theta-method is

$$
x_{i+1}=x_i+h\left[
(1-\theta)f(t_i,x_i)
+\theta f(t_{i+1},x_{i+1})
\right].
$$

For $\theta=0.5$:

$$
\boxed{
x_{i+1}=x_i+\frac{h}{2}
\left[
f(t_i,x_i)+f(t_{i+1},x_{i+1})
\right].
}
$$

This is not explicit in general because $x_{i+1}$ appears on the right-hand side inside $f(t_{i+1},x_{i+1})$.

So

$$
\boxed{\text{the method is implicit}.}
$$

### 4(b) Global error estimate for $\theta=0.5$

Define the exact global error

$$
e_i=x(t_i)-x_i.
$$

The local truncation error is

$$
T_i=
\frac{x(t_{i+1})-x(t_i)}{h}
-\frac{1}{2}
\left[
f(t_i,x(t_i))+f(t_{i+1},x(t_{i+1}))
\right].
$$

Using Taylor expansion and bounded derivatives up to order 3, the trapezoidal rule has local error

$$
\boxed{|T_i|\le Ch^2.}
$$

Subtract the numerical scheme from the exact consistency relation:

$$
\frac{e_{i+1}-e_i}{h}
=T_i
+\frac{1}{2}
\left[
f(t_i,x(t_i))-f(t_i,x_i)
+f(t_{i+1},x(t_{i+1}))-f(t_{i+1},x_{i+1})
\right].
$$

Assume $f$ is Lipschitz in $x$ with constant $L$. Then

$$
\left|f(t_i,x(t_i))-f(t_i,x_i)\right|
\le L|e_i|,
$$

and

$$
\left|f(t_{i+1},x(t_{i+1}))-f(t_{i+1},x_{i+1})\right|
\le L|e_{i+1}|.
$$

Thus

$$
|e_{i+1}|
\le
|e_i|+h|T_i|+\frac{hL}{2}|e_i|+\frac{hL}{2}|e_{i+1}|.
$$

Move the last term to the left:

$$
\left(1-\frac{hL}{2}\right)|e_{i+1}|
\le
\left(1+\frac{hL}{2}\right)|e_i|+Ch^3.
$$

For sufficiently small $h$, $1-hL/2>0$, so

$$
|e_{i+1}|
\le
\frac{1+hL/2}{1-hL/2}|e_i|
+C'h^3.
$$

Applying a discrete Gronwall argument and using $e_0=0$:

$$
\boxed{
\max_{0\le i\le N}|e_i|\le C''h^2.
}
$$

Therefore the theta-method with $\theta=0.5$ is globally second-order accurate.

## Question 5

### 5(a) Output shape from each layer

Input:

$$
100\times100\times1.
$$

Layer 1: convolution with one $5\times5$ filter, stride 1, padding 2.

The spatial output size is

$$
\frac{100+2(2)-5}{1}+1=100.
$$

So Layer 1 output is

$$
100\times100\times1.
$$

Layer 2: $2\times2$ max pooling with stride 2.

The output becomes

$$
50\times50\times1.
$$

Layer 3: flatten:

$$
50\cdot50\cdot1=2500.
$$

Layer 4: fully connected layer with 100 nodes:

$$
100.
$$

Layer 5: single output unit:

$$
1.
$$

Therefore

$$
\boxed{
100\times100\times1
\to
100\times100\times1
\to
50\times50\times1
\to
2500
\to
100
\to
1.
}
$$

### 5(b) Number of weights

Ignore biases.

Layer 1 convolution:

$$
5\cdot5\cdot1\cdot1=25.
$$

Layer 2 max pooling:

$$
0
$$

trainable weights.

Layer 3 flatten:

$$
0
$$

trainable weights.

Layer 4 fully connected:

$$
2500\cdot100=250000.
$$

Layer 5 output:

$$
100\cdot1=100.
$$

Total:

$$
\boxed{
25+250000+100=250125.
}
$$

### 5(c) Modify the CNN as a residual network

Yes. Add a skip connection around a block whose input and output have the same dimension.

For example, before pooling, define a residual convolutional block:

$$
F(X)=K*X,
$$

where padding keeps the output size $100\times100\times1$. Then replace the plain convolution output by

$$
\boxed{
Y=X+F(X).
}
$$

More generally, one may use

$$
Y=X+K_2*\sigma(K_1*X),
$$

where both convolutions use padding so the dimensions match.

Then continue with pooling, flattening, and fully connected layers.

Advantages:

1. The skip connection allows the network to learn a residual correction instead of the whole mapping.
2. If the best mapping is close to the identity, the residual block can represent it easily.
3. Gradients can flow through the identity path, reducing vanishing-gradient problems.
4. Deeper networks become easier to optimize than plain CNNs.

If the dimensions do not match, one should use a projection such as a $1\times1$ convolution or suitable padding/channel matching before adding.
