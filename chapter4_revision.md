# Chapter 4 Revision Notes

## 1. Chapter overview

Chapter 4 covers numerical methods for ordinary differential equations and their connection to deep learning through ResNet and Neural ODEs. The exam emphasis is both computational and conceptual: solve small IVPs using Euler-type methods, derive error estimates, explain stability/convergence, formulate ResNet as a discretized ODE, and compute Neural ODE gradients using the adjoint method.

This chapter is high-yield because `Final-revision.pdf` directly names ODE solvers, stability, convergence rates, ResNet, Neural ODEs, gradient computation, and memory requirements. Sources: `chapter4.pdf`, `Final-revision.pdf`, `2023May.pdf`, `2024May.pdf`, `2025May.pdf`.

## 2. High-priority topics from final revision

| Priority | Topic | Why it matters |
|---|---|---|
| High priority | Numerical ODE solving | Final revision asks how to solve ODEs numerically. |
| High priority | Stability and convergence rates | Final revision asks directly; tested in `2024May.pdf` and `2025May.pdf`. |
| High priority | Forward Euler and Backward Euler | `2023May.pdf` asks both; 2024 and 2025 ask Euler/theta-style schemes. |
| High priority | Theta-method / trapezium rule | `2025May.pdf` asks theta-method with $\theta=0.5$. |
| High priority | Truncation and global error | `2024May.pdf` and `2025May.pdf` ask error estimates. |
| High priority | ResNet | Final revision asks what ResNet is; `2024May.pdf` and `2025May.pdf` ask residual networks. |
| High priority | Neural ODE and adjoint method | Final revision asks gradient computation; `2023May.pdf` tests adjoint method. |
| High priority | Memory requirements for ResNet vs Neural ODE | Explicitly listed in final revision. |
| Medium priority | Picard theorem and IVP well-posedness | Useful for assumptions in convergence proofs. |
| Low priority / background only | Neural ODE applications such as normalizing flows | Good context, but not likely as detailed calculation. |

## 3. Key concepts and definitions

- Initial value problem (IVP): an ODE plus initial condition,

$$
x'(t)=f(t,x(t)),\qquad x(0)=x_0.
$$

- Mesh/step size: divide time interval into $t_i=ih$; numerical solution $x_i$ approximates $x(t_i)$.

- Explicit method: computes $x_{i+1}$ directly from known previous values.

- Implicit method: $x_{i+1}$ appears inside $f$ and usually requires solving an equation at each step.

- Forward Euler:

$$
x_{i+1}=x_i+h f(t_i,x_i).
$$

- Backward Euler:

$$
x_{i+1}=x_i+h f(t_{i+1},x_{i+1}).
$$

- Theta-method:

$$
x_{i+1}=x_i+h\left[(1-\theta)f(t_i,x_i)+\theta f(t_{i+1},x_{i+1})\right].
$$

$\theta=0$ is Forward Euler, $\theta=1$ is Backward Euler, and $\theta=0.5$ is trapezium/Crank-Nicolson style. Source: `chapter4.pdf`.

- Truncation/consistency error: error made in one step when the exact solution is inserted into the numerical scheme.

- Global error: $e_i=x(t_i)-x_i$, the accumulated error after many steps.

- ResNet: a residual neural network uses skip connections, often in the form

$$
u_{i+1}=u_i+h\sigma(K_i u_i+b_i),
$$

which resembles Forward Euler applied to an ODE. Source: `chapter4.pdf`.

- Neural ODE: a model where hidden states evolve continuously:

$$
\frac{dx(t)}{dt}=f(x(t),t,\theta).
$$

The output is computed by an ODE solver rather than a fixed number of discrete layers.

## 4. Core theories / models / frameworks

### Euler and theta-methods

Intuition: approximate the ODE slope over a small time step.

Forward Euler:
- Uses slope at the beginning of the interval.
- Explicit and easy.
- First-order global accuracy.

Backward Euler:
- Uses slope at the end of the interval.
- Implicit, more expensive per step.
- Often more stable.

Theta-method:
- Interpolates between explicit and implicit behavior.
- $\theta=0.5$ has higher accuracy under smoothness assumptions.

Exam appearance:
- `2023May.pdf` asks exact solution, Forward Euler, and Backward Euler.
- `2024May.pdf` asks a custom predictor/trapezoidal-style scheme and error estimate.
- `2025May.pdf` asks the theta-method with $\theta=0.5$ and global error estimate.

Limitations:
- Small $h$ improves accuracy but increases computation.
- Implicit methods may require nonlinear solves.

### Error analysis framework

Intuition: global error is controlled by local truncation error plus stability of the update.

For Forward Euler, consistency error is

$$
T_i=\frac{x(t_{i+1})-x(t_i)}{h}-f(t_i,x(t_i)).
$$

Using Taylor expansion:

$$
|T_i|\le \frac{h}{2}\max |x''(t)|.
$$

If $f$ is Lipschitz in $x$ with constant $L$, the global error satisfies a recursion like

$$
|e_{i+1}|\le (1+hL)|e_i|+h|T_i|.
$$

With exact initial condition, Forward Euler has global error $O(h)$. Source: `chapter4.pdf`.

Exam appearance:
- `2024May.pdf` asks to derive truncation and global error bounds.
- `2025May.pdf` asks to repeat the global error estimate for $\theta=0.5$.

Common limitation:
- You must state smoothness/Lipschitz assumptions; otherwise the estimate is not justified.

### ResNet as Euler discretization

Intuition: each residual block adds a learned increment to the current hidden state.

Discrete residual block:

$$
u_{i+1}=u_i+h f(u_i,\theta_i).
$$

Continuous analogy:

$$
\frac{du(t)}{dt}=f(u(t),\theta(t)).
$$

How it is used:
- Explain why skip connections help train deeper networks.
- Express a residual block output in terms of convolution/ReLU layers and input skip connection.

Exam appearance:
- `2024May.pdf` asks to formulate a residual block output and count weights.
- `2025May.pdf` asks how to modify a CNN into a residual network and explain the advantage.

Limitations:
- Skip addition requires compatible dimensions; if dimensions differ, use projection or matching convolution.

### Neural ODE and adjoint method

Intuition: replace a discrete stack of layers with continuous dynamics solved by an ODE solver.

Forward pass:

$$
x(t_1)=\operatorname{ODESolve}(f(x(t),t,\theta),x(t_0),t_0,t_1).
$$

Adjoint state:

$$
a(t)=\frac{\partial L}{\partial x(t)}.
$$

Adjoint dynamics:

$$
\frac{da(t)}{dt}
=-a(t)^T\frac{\partial f(x(t),t,\theta)}{\partial x(t)}.
$$

Parameter gradient:

$$
\frac{\partial L}{\partial \theta}
=-\int_{t_1}^{t_0}
a(t)^T\frac{\partial f(x(t),t,\theta)}{\partial \theta}\,dt
=\int_{t_0}^{t_1}
a(t)^T\frac{\partial f(x(t),t,\theta)}{\partial \theta}\,dt,
$$

depending on integration direction convention. Source: `chapter4.pdf`.

Memory comparison:
- ResNet backprop usually stores many layer activations, so memory grows with number of layers/steps.
- Neural ODE adjoint can reduce memory by recomputing states backward through the ODE solver, but may introduce numerical instability or extra computation.

Exam appearance:
- `2023May.pdf` asks where training data come from and how to use the adjoint method to compute gradients.

## 5. Important formulas / calculations, if any

### Forward Euler

$$
x_{i+1}=x_i+hf(t_i,x_i).
$$

Step method:
1. Identify $f(t,x)$.
2. Start from $x_0$.
3. For each step, compute $f(t_i,x_i)$.
4. Update $x_{i+1}$.

Common mistake: using $t_{i+1}$ or $x_{i+1}$ in Forward Euler.

### Backward Euler

$$
x_{i+1}=x_i+hf(t_{i+1},x_{i+1}).
$$

Step method:
1. Substitute $t_{i+1}$ and unknown $x_{i+1}$.
2. Solve the resulting equation.
3. Repeat.

Common mistake: treating Backward Euler as explicit when $f$ depends on $x$.

### Theta-method

$$
x_{i+1}=x_i+h\left[(1-\theta)f(t_i,x_i)+\theta f(t_{i+1},x_{i+1})\right].
$$

For $\theta=0.5$:

$$
x_{i+1}=x_i+\frac{h}{2}\left[f(t_i,x_i)+f(t_{i+1},x_{i+1})\right].
$$

This is implicit because $x_{i+1}$ appears on the right when $f$ depends on $x$.

### Forward Euler global error

If $f$ is Lipschitz in $x$ with constant $L$ and $|x''(t)|\le M_2$, then with exact initial condition:

$$
\max_i |e_i|
\le
\frac{hM_2}{2L}\left(e^{L(T-t_0)}-1\right)
=O(h).
$$

Use this as a template for global error derivations.

### ResNet residual block

For a block with two convolutions and ReLU:

$$
Y(X;\theta)=X+K_2 * \sigma(K_1 * X),
$$

with padding chosen so dimensions match. If each convolution has one $5\times5$ grayscale filter and bias is ignored, total convolution weights are $25+25=50$.

Common mistake: adding the skip connection before making dimensions compatible.

## 6. Exam question patterns

### Pattern 1: Euler numerical calculation

Possible wording:
- "Use Forward Euler with $h=0.25$ to calculate $x(ih)$ for $i=1,2,3$."
- "Use Backward Euler with $h=0.25$..."

How to answer:
1. Write the update formula.
2. Substitute $h$.
3. Make a table with $t_i$, $x_i$, slope, and next value.
4. For backward Euler, solve the implicit equation each step.

Common mistakes:
- Skipping intermediate steps.
- Mixing exact and numerical values.

Suggested answer length: calculation table.

### Pattern 2: Truncation/global error derivation

Possible wording:
- "Derive an upper bound for the truncation error."
- "Hence derive an upper bound for $\max_i|e_i|$."

How to answer:
1. Define local truncation error.
2. Taylor expand the exact solution.
3. Use bounded derivatives to obtain $|T_i|\le Ch^p$.
4. Derive error recurrence using Lipschitz continuity.
5. Apply discrete Gronwall/geometric bound.

Common mistakes:
- Calling local truncation error and global error the same thing.
- Not using the Lipschitz constant.

Suggested answer length: one to two pages.

### Pattern 3: ResNet block

Possible wording:
- "Formulate the expression of $Y$ using $X$."
- "Name and calculate the number of weights."
- "How can this CNN be modified as a residual network?"

How to answer:
1. Write the sequence of operations.
2. Add the skip connection explicitly.
3. Count trainable filters.
4. Explain advantage: easier optimization, gradient flow, depth, identity mapping.

Common mistakes:
- Forgetting ReLU between convolutions.
- Adding tensors of different shapes.

### Pattern 4: Neural ODE adjoint

Possible wording:
- "What is Neural ODE, and how to compute the gradient?"
- "Can you use the adjoint method to calculate $\partial L/\partial\theta$?"

How to answer:
1. Define Neural ODE.
2. State forward solve.
3. Define adjoint $a(t)=\partial L/\partial x(t)$.
4. Write adjoint ODE backward in time.
5. Write parameter-gradient integral.
6. Mention memory tradeoff.

Common mistakes:
- Describing ordinary backprop only, without adjoint ODE.
- Forgetting terminal condition $a(t_1)=\partial L/\partial x(t_1)$.

## 7. Past exam connections

| Past exam | Related topic | What the question tests | Required depth |
|---|---|---|---|
| 2023May | Forward/Backward Euler, Neural ODE adjoint | Exact solution, numerical steps, adjoint gradient method | Calculation plus method explanation |
| 2024May | Custom ODE scheme, truncation/global error, ResNet block | Explicit/implicit identification, error bound, residual expression and training method | Derivation and conceptual explanation |
| 2025May | Theta-method with $\theta=0.5$, CNN-to-ResNet modification | Write method, identify implicitness, derive global error, explain residual advantage | Formula, proof sketch, application explanation |

## 8. Worked mini-examples / answer templates

### Forward Euler template

For $x'=f(t,x)$, $x(0)=x_0$, and $h=0.25$:

| Step | Formula | Result format |
|---|---|---|
| 1 | $x_1=x_0+h f(t_0,x_0)$ | substitute numbers |
| 2 | $x_2=x_1+h f(t_1,x_1)$ | use numerical $x_1$ |
| 3 | $x_3=x_2+h f(t_2,x_2)$ | use numerical $x_2$ |

Write all intermediate slopes so the marker can award method marks.

### Error proof template

"Define $e_i=x(t_i)-x_i$ and $T_i=\frac{x(t_{i+1})-x(t_i)}{h}-f(t_i,x(t_i))$. Taylor expansion gives $|T_i|\le Ch$. Subtracting the numerical scheme from the exact consistency relation gives $e_{i+1}=e_i+h[f(t_i,x(t_i))-f(t_i,x_i)]+hT_i$. By Lipschitz continuity, $|e_{i+1}|\le(1+hL)|e_i|+hCh$. Iterating yields $\max_i|e_i|=O(h)$."

### Neural ODE adjoint template

"Forward solve $x(t_1)=\operatorname{ODESolve}(f,x(t_0),t_0,t_1)$ and compute $L(x(t_1))$. Set terminal adjoint $a(t_1)=\partial L/\partial x(t_1)$. Solve backward with $da/dt=-a^T\partial f/\partial x$. The parameter gradient is accumulated from $a^T\partial f/\partial\theta$ along the trajectory. This reduces activation storage compared with standard ResNet backprop, at the cost of recomputation and possible numerical issues."

## 9. Common mistakes and confusing points

- Confusing explicit and implicit methods.
- Forgetting Backward Euler requires solving for the unknown next value.
- Writing theta-method with $\theta=0.5$ but calling it explicit.
- Mixing local truncation error order and global error order.
- Not stating smoothness/Lipschitz assumptions in error estimates.
- Adding a residual skip connection when tensor dimensions do not match.
- Explaining ResNet only as "deeper CNN" without mentioning identity/skip connection.
- Describing Neural ODE gradients as ordinary discrete backprop without the adjoint equation.
- Claiming Neural ODE always uses less memory without mentioning recomputation and numerical tradeoffs.

## 10. Quick revision checklist

Before the exam, I should be able to:

- [ ] Write Forward Euler, Backward Euler, and theta-method formulas.
- [ ] Identify whether a scheme is explicit or implicit.
- [ ] Compute three Euler steps by hand.
- [ ] Define truncation error and global error.
- [ ] Derive the Forward Euler global error recurrence.
- [ ] Explain why $\theta=0.5$ can improve accuracy but is implicit.
- [ ] Express a ResNet block mathematically.
- [ ] Count weights in a residual convolutional block.
- [ ] Explain ResNet as Forward Euler discretization.
- [ ] Define Neural ODE and write its forward solve.
- [ ] Write adjoint dynamics and parameter-gradient integral.
- [ ] Compare ResNet and Neural ODE memory requirements.
