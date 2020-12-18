---
jupytext:
  cell_metadata_filter: -all
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: '0.8'
    jupytext_version: 1.5.0
kernelspec:
  display_name: 'Python 3.8.6 64-bit (''codeforecon'': conda)'
  language: python
  name: codeforecon
---

# Introduction to Mathematics with Code

```{code-cell} ipython3
:tags: ["remove-cell"]

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# Set seed for reproducibility
np.random.seed(10)
# Set max rows displayed for readability
pd.set_option('display.max_rows', 6)
# Plot settings
plot_style = {'xtick.labelsize': 20,
                  'ytick.labelsize': 20,
                  'font.size': 22,
                  'figure.autolayout': True,
                  'figure.figsize': (10, 5.5),
                  'axes.titlesize': 22,
                  'axes.labelsize': 20,
                  'lines.linewidth': 4,
                  'lines.markersize': 6,
                  'legend.fontsize': 16,
                  'mathtext.fontset': 'stix',
                  'font.family': 'STIXGeneral',
                  'legend.frameon': False}
plt.style.use(plot_style)
```

In this chapter, you'll learn about doing mathematics with code, including solving equations symbollicaly, ...

This chapter uses the **numpy**, **scipy**, and **sympy** packages. If you're running this code, you may need to install these packages using, for example, `pip install packagename` on your computer's command line. (If you're not sure what a command line or terminal is, take a quick look at the basics of coding chapter.)

## Symbolic mathematics

```{code-cell} ipython3
:tags: ["remove-cell"]

from myst_nb import glue
import sympy
a = 8
glue('sqrt', 2*np.sqrt(a))
glue('symsqrt', sympy.sqrt(a))
```

When using computers to do mathematics, we're most often performing numerical computations such as $\sqrt{8} = ${glue:}`sqrt`. Although we have the answer, it's only useful for the one special case. Symbolic mathematics allows us to use coding to solve equations in the general case, which can often be more illuminating. As an example, if we evaluate this in symbolic mathematics we get $\sqrt{8} = ${glue:}`symsqrt`.

The Python package for symbolic mathemtics is [**sympy**](https://www.sympy.org/en/index.html), which provides some features of a computer algebra system.

To define *symbolic* variables, we use sympy's symbols function. For ease, we'll import the entire sympy library into the namespace by using `from sympy import *`.

```{code-cell} ipython3
from sympy import *
x, t, α, β = symbols(r'x t \alpha \beta')
```

```{note}
The leading 'r' in some strings tells Python to treat the string literally so that backslashes are not treated as instructions--otherwise, combinations like `\n` would begin a newline.
```

Having created these symbolic variables, we can refer to and see them just like normal variables--though they're not very interesting *because* they are just symbols (for now):

```{code-cell} ipython3
α
```

Things get much more interesting when we start to do maths on them. Let's see some integration, for example, say we want to evaluate

```{code-cell} ipython3
Integral(log(x), x)
```

(note that the symbols are printed as latex equations) we simply call

```{code-cell} ipython3
integrate(log(x), x)
```

We can differentiate too:

```{code-cell} ipython3
diff(sin(x)*exp(x), x)
```

and even take limits!

```{code-cell} ipython3
limit(sin(x)/x, x, 0)
```

It is also possible to solve equations using **sympy**. The solve function tries to find the roots of $f(x)$ and has syntax `solve(f(x)=0, x)`. Here's an example:

```{code-cell} ipython3
solve(x*5 - 2, x)
```

There are also solvers for differential equations (`dsolve`), continued fractions, simplifications, and more.

Another really important thing to know about symbolic mathematics is that you can 'cash in' at any time by substituting in an actual value. For example,

```{code-cell} ipython3
expr = 1 - 2*sin(x)**2
expr.subs(x, np.pi/2)
```

But you don't have to substitute in a real value; you can just as well substitute in a different symbolic variable:

```{code-cell} ipython3
expr = 1 - 2*sin(x)**2
simplify(expr.subs(x, t/2))
```

I snuck in a simplify here too!

### Symbolic mathematics for economics

The library does a lot, so let's focus on a few features that are likely to be useful for economics in particular.

#### Series expansion

The first is performing **Taylor series expansions**. These come up all the time in macroeconomic modelling, where models are frequently log-linearised. Let's see an example of a couple of expansions together:

```{code-cell} ipython3
expr = log(sin(α))

expr.series(α, 0, 4)
```

This is a 3rd order expansion around $\alpha=0$.

#### Symbolic linear algebra

The support for **matrices** can also come in handy for economic applications. Here's a matrix,

```{code-cell} ipython3
M = Matrix([[1, 0, x], [α, -t, 3], [4, β, 2]])
M
```

and its determinant:

```{code-cell} ipython3
M.det()
```

I can hardly go to a talk in economics that involves matrices that doesn't see those matrices get diagonalised: there's a function for that too.

```{code-cell} ipython3
P, D = Matrix([[1, 0], [α, -t]]).diagonalize()
D
```

#### Lagrangians

Function optimisation using Lagrangians is about as prevalent in economics as any bit of maths: let's see how it's done symbolically.

We're going to find the minimum over x, y of the function $f(x,y)$, subject to $g(x,y)=0$, where $f(x,y) = 4xy - 2x^2 + y^2$ and $g(x,y) = 3x+y-5$.

First we need to specify the problem, and the Lagrangian for it, in code

```{code-cell} ipython3
x, y, λ = symbols(r'x y \lambda', real=True)
f = 4*x*y - 2*x**2 + y**2
g = 3*x+y-5

ℒ = f - λ*g
ℒ
```

The Karush-Kuhn-Tucker (KKT) conditions tell us whether any solutions we find will be optimal. Simply, the constaint is that a solution vector is a saddle point of the Lagrangian, $\nabla \mathcal{L} = 0$. Let's solve this.

```{code-cell} ipython3
gradL = [diff(ℒ, c) for c in [x, y]]
KKT_eqns = gradL + [g]
```

```{code-cell} ipython3
:tags: ["remove-cell"]

KKT_eqns = gradL + [g]
glue('kkt_0', KKT_eqns[0])
glue('kkt_1', KKT_eqns[1])
glue('kkt_2', KKT_eqns[2])
```

This gives 3 equations from the KKT conditions:  {glue:}`kkt_0`,   {glue:}`kkt_1`, and  {glue:}`kkt_2`. (The symbolic manipulation is now over: we solved for the conditions in terms of algebra--now we're looking for real values.) Now we look for the values of $x, y$ that minimise $f$ given that $g=0$ by solving these equations over $x$, $y$, and $\lambda$.

```{code-cell} ipython3
stationary_pts = solve(KKT_eqns, [x, y, λ], dict=True)
stationary_pts
```

Now, we can substitute these in to find the (first--and in this case only) point that minimises our function:

```{code-cell} ipython3
stationary_pts[0][x], stationary_pts[0][y], f.subs(stationary_pts[0])
```

#### Exporting to latex

To turn any equation, for example `diff(sin(x)*exp(x), x)`, into latex and export it to a file that can be included in a paper, use

```python
eqn_to_export = latex(diff(sin(x)*exp(x), x), mode='equation')
open('latex_equation.tex', 'w').write(eqn_to_export)
```

which creates a file called 'latex_equation.tex' that has a single line in it: '\begin{equation}\int \log{\left(x \right)}\, dx\end{equation}'. There are a range of options for exporting to latex, `mode='equation*'` produces an unnumbered equation, 'inline' produces an inline equation, and so on. To include these in your latex paper, use '\input{latex_equation.tex}'.

### Why coding symbolic mathematics is useful

1. Accuracy--using a computer to solve the equations means you're less likely to make a mistake. At the very least, it's a useful check on your by-hand working.

2. Consistency--by making your code export the equations you're solving to your write-up, you can ensure that the equations are consistent across both *and* you only have to type them once.

## Numerical Mathematics

For much of the time, you'll be dealing with numbers rather than symbols. The workhorses of numerical mathematics are the two packages **numpy** and **scipy**. Both have excellent documentation, where you can find out more. In this section, we'll look at how to use them in some standard mathematical operations that arise in economics.

The most basic object is an array, which can be defined as follows:

```{code-cell} ipython3
import numpy as np
a = np.array([0, 1, 2, 3], dtype='int64')
a
```

Arrays are very memory efficient and fast objects that you should use in preference to lists for any heavy duty numerical operation.

To demonstrate this, let's do a time race between lists and arrays for squaring all elements of an array:

Lists:

```{code-cell} ipython3
a_list = range(1000)
%timeit [i**2 for i in a_list]
```

Arrays:

```{code-cell} ipython3
a = np.arange(1000)
%timeit a**2
```

Using arrays was *two orders of magnitude** faster! Okay, so we should use arrays for numerical works. How do we make them? You can specify an array explicitly as we did above to create a vector. This manual approach works for other dimensions too:

```{code-cell} ipython3
mat = np.array([[0, 1, 2], [3, 4, 5], [6, 7, 8]])
mat
```

To find out about matrix properties, we use `.shape`

```{code-cell} ipython3
mat.shape
```

We already saw how `np.arange(start, stop, step)` produces a vector; `np.linspace(start, stop, number)` produces a vector of length `number` by equally dividing the space between `start` and `stop`.

A couple of really useful arrays are `np.ones(shape)`, for example,

```{code-cell} ipython3
np.ones((3, 3))
```

and `np.diag`:

```{code-cell} ipython3
np.diag(np.array([1, 2, 3, 4]))
```

Random numbers are supplied by `np.random.rand()` for a uniform distribution in [0, 1], and `np.random.randn()` for numbers drawn from a standard normal distribution.

You can, of course, specify a function to create an array:

```{code-cell} ipython3
c = np.fromfunction(lambda i, j: i**2+j**2, (4, 5))
c
```

To access values in an array, you can use all of the by-position slicing methods that you've seen already in data analysis and with lists. The figure gives an example of some common slicing operations:

![Examples of array slices](https://scipy-lectures.org/_images/numpy_indexing.png)

Arrays can also be sliced and diced based on boolean indexing, just like a dataframe.

For example, using the array defined above, we can create a boolean array of true and false values from a condition such as `c > 6` and use that to only access some elements of an array (it doesn't have to be the same array, though it usually is):

```{code-cell} ipython3
c[c > 6]
```

As with dataframes, arrays can be combined. The main command to remember is `np.concatenate`, which has an `axis` keyword option.

```{code-cell} ipython3
x = np.eye(3)
np.concatenate([x, x], axis=0)
```

Splitting is performed with `np.split(array, splits, axis=)`, for example

```{code-cell} ipython3
np.split(x, [3], axis=0)
```

Aggregation operations are very similar to those found in dataframes: `x.sum(i)` to sum across the $i$th dimension of the array; similarly for standard deviation, and so on.

As with dataframes, you can (and often should) specify the datatype of an array when you create it by passing a `dtype=` keyword, eg `c = np.array([1, 2, 3], dtype=float)`. To find out the data type of an array that already exists, use `c.dtype`.

Finally, numpy does a lot of smart broadcasting of arrays. Broadcasting is what means that summing two arrays gives you a third array that has elements that are each the sum of the relevant elements in the two original arrays. Put another way, it's what causes `x + y = z` (for arrays x and y with the same shape) to result in an array z for which $z_{ij} = x_{ij} + y_{ij}$.

Summing two arrays of the same shape is a pretty obvious example, but it also applies to cases that are *not* completely matched. For example, multiplication by a scalar is broadcast across all elements of an array:

```{code-cell} ipython3
x = np.ones(shape=(3, 3))
x*3
```

Similarly, numpy functions are broadcast across elements of an array:

```{code-cell} ipython3
np.exp(x)
```

### Numeric linear algebra

The transpose of an array `x` is given by `x.T`.

Matrix multiplation is performed using the `@` operator. Here we perform $ M_{il} = \sum_{k} x_{ik} * (x^T)_{kl}$, where $x^T$ is the transpose of $x$.

```{code-cell} ipython3
x @ x.T
```

To multiply two arrays element wise, ie to do $ M_{ij} = x_{ij} y_{ij}$, it's the usual multiplication operator `*`.

Inverting matrices:

```{code-cell} ipython3
a = np.random.randint(9, size=(3, 3), dtype='int')
a @ np.linalg.inv(a)
```

Computing the trace:

```{code-cell} ipython3
a.trace()
```

Determinant:

```{code-cell} ipython3
np.linalg.det(a)
```

#### Solving systems of linear equations

Say we have a system of equations, $4x + 3y + 2z = 25$,
$-2x + 2y + 3z = -10$, and $3x -5y + 2z = -4$. We can solve these three equations for the three unknowns, x, y, and z, using the `solve` method. First, remember that this equation can be written in matrix form as

$$
M\cdot \vec{x} = \vec{c}
$$

We can solve this by multiplying by the matrix inverse of $M$:

$$
M^{-1} M \cdot \vec{x} = I \cdot \vec{x} = M^{-1} \cdot \vec{c}
$$

which could be called by running `x = la.inv(M).dot(c)`. There's a convenience function in **numpy** called solve that does the same thing: here it finds the real values of the vector $\vec{x}$.

```{code-cell} ipython3
M = np.array([[4, 3, 2], [-2, 2, 3], [3, -5, 2]])
c = np.array([25, -10, -4])
np.linalg.solve(M, c)
```

Finally, eigenvalues and eigenvectors can be found from:

```{code-cell} ipython3
import scipy.linalg as la
eigvals, eigvecs = la.eig(M)
eigvals
```

### Distribution functions

TODO -- maybe in econometrics though?

### Interpolation

This section draws on the **scipy** documentation. There are built-in **pandas** methods for interpolation in dataframes, but **scipy** also has a range of functions for this including for univariate data `interp1d`, multidimensional interpolation on a grid `interpn`, `griddata` for unstructured data. Let's see a simple example with interpolation between a regular grid of integers.

```{code-cell} ipython3
import matplotlib.pyplot as plt
from scipy import interpolate
x = np.arange(0, 10)
y = np.exp(-x/3.0)
f = interpolate.interp1d(x, y, kind='cubic')
# Create a finer grid to interpolation function f
xnew = np.arange(0, 9, 0.1)
ynew = f(xnew)
plt.plot(x, y, 'o', xnew, ynew, '-')
plt.show()
```

What about unstructured data? Let's create a Cobb-Douglas function on a detailed grid but then only retain a random set of the established points.

```{code-cell} ipython3
from scipy.interpolate import griddata

def cobb_doug(x, y):
    alpha = 0.8
    return x**(alpha)*y**(alpha-1)


# Take some random points of the Cobb-Douglas function
points = np.random.rand(1000, 2)
values = cobb_doug(points[:,0], points[:,1])

# Create a grid
grid_x, grid_y = np.mgrid[0.01:1:200j, 0.01:1:200j]

# Interpolate the points we have onto the grid
interp_data = griddata(points, values, (grid_x, grid_y), method='cubic')

# Plot results
fig, axes = plt.subplots(1, 2)
# Plot function & scatter of random points
axes[0].imshow(cobb_doug(grid_x, grid_y).T,
               extent=(0, 1, 0, 1),
               origin='lower', cmap='plasma_r',
               vmin=0, vmax=1)
axes[0].plot(points[:, 0], points[:, 1], 'r.', ms=1.2)
axes[0].set_title('Original + points')
# Interpolation of random points
axes[1].imshow(interp_data.T, extent=(0, 1, 0, 1),
               origin='lower', cmap='plasma_r',
               vmin=0, vmax=1)
axes[1].set_title('Cubic interpolation');
```

### Optimisation

**scipy** has functions for minimising scalar functions, minimising multivariate functions with complex surfaces, and root-finding. Let's see an example of finding the minimum of a scalar function.

```{code-cell} ipython3
from scipy import optimize

def f(x):
    return x**2 + 10*np.sin(x) - 1.2


result = optimize.minimize(f, x0=0)
result
```

The result of the optimisation is in the 'x' attribute of `result`. Let's see this:

```{code-cell} ipython3
x = np.arange(-10, 10, 0.1)
fig, ax = plt.subplots()
ax.plot(x, f(x))
ax.scatter(result.x, f(result.x), s=150, color='k')
ax.set_xlabel('x')
ax.set_ylabel('f(x)', rotation=90)
plt.show()
```

In higher dimensions, the minimisation works in much the same way, with the same function `optimize.minimize`. There are a LOT of minimisation options that you can pass to the `method=` keyword; the default is intelligently chosen from BFGS, L-BFGS-B, or SLSQP, depending upon whether you supply constraints or bounds.

Root finding, aka solving equations of the form $f(x)=$, is also catered for by **scipy**, through `optimize.root`.

In both of these cases, be warned that multiple roots and multiple minima can be hard to detect, and you may need to carefully specify the bounds or the starting positions in order to find the root you're looking for. Also, both of these methods can accept the Jacobian of the function you're working with as an argument, which is likely to improve performance with some solvers.


### Calculus

#### Numerical Integration