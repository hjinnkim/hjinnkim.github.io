---
title: "[AI toolkit] 01. Linear Algebra"
excerpt: "Basics of Linear Algebra"

categories:
  - AI toolkit
tags:
  - [Linear Algebra]

permalink: /courses/ai-toolkit/001/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-19
last_modified_at: 2022-10-19
order: 2
published: true
use_math: true

---
# Lecture 01

## 0. Table of Contents

1. [Scalars and Vectors](#1-confirmation)
2. [Matrices and Tensors](#2-matrices-and-tensors)
3. [Linear Algebra](#3-linear-algebra)
4. [Two ways of representing linear equations](#4-two-ways-of-representing-linear-equations)
5. [Matrix Addition and Multiplication](#5-matrix-addition-and-multiplication)
6. [Associativity and Distributivity of Multiplication](#6-associativity-and-distributivity-of-multiplication)
7. [Identity, Inverse and Transpose](#7-identity-inverse-and-transpose)
8. [Vector Space](#8-vector-space)
9. [Vector Subspace](#9-vector-subspace)
10. [Column Space of A, C(A)](#10-column-space-of-a-ca)
11. [Null Space of A, N(A)](#11-null-space-of-a-na)
12. [Solving Ax=b: Particular and General Solution](#12-solving--particular-and-general-solution)
13. [Gaussian Elimination](#13-gaussian-elimination)
14. [Back to Previous Example: Particular and General Solution](#14-back-to-previous-example-particular-and-general-solution)
15. [Linear IndependencePermalink](#15-linear-independence)
16. [Span and Rank](#16-span-and-rank)
17. [Basis for Vector Space](#17-basis-for-vector-space)
18. [Linear Transformations](#18-linear-transformations)


## 1. Scalars and Vectors

- **Scalars,** $x\in\mathbb{R}$
  - A single number, (real valued)
  - Usually written with $x, y$ (normal lower-case letters)
- **Vector**, $\mathbf{x}\in\mathbb{R}^d$
  - A fixed-length arrays of scalars. $\mathbf{x}=[x_1, \ldots, x_d]^T$
    - Each scalar $x_i$ is called as "element", "entry", or "component"
  - Usually written with $\mathbf{x}$ (bold lower-case letters)
  - Can be represented with arrows
  <p align="center">
    <img src="{{site.gdrive_url_prefix}}1QWRhnNvyXscxq6-IEYnIZWu1d2NTZC75" title="lec01-p2-vector image" style="width:50%">
  </p>

## 2. Matrices and Tensors

  - **Matrix,** $\mathbf{A}\in\mathbb{R}^{m\times n}$ ($m$ rows and $n$ columns)
    - Usually written with $\mathbf{A}$ (bold upper-case letters)
      <p align="center">
      $ \mathbf{A}=\begin{bmatrix}
      a_{11} & \cdots & a_{1n} \\
      \vdots & & \vdots \\ 
      a_{m1} & \cdots & a_{mn} \\
      \end{bmatrix}, a_{ij}\in\mathbb{R}$
      </p>
      - Can be represented as $\mathbf{a}\in\mathbb{R}^{mn}$, by stacking all $n$ columns vertically, or concatenating $m$ rows horizontally.

  - **Tensor**
    - Stack $k$ number of $\mathbf{X}\in\mathbb{R}^{m\times n}$.
    - The results can be called as 3th-order array, tensor $\mathsf{X}\in\mathbb{R}^{k\times m\times n}$
    - Usually written $\mathsf{X}$ (capital letters with a special font face)

## 3. Linear Algebra

  - Gives us the tool for solving linear equations
    <p>
    $ \begin{align*}
      a_{11}x_1+\cdots +a_{1n}x_n& = b_1 \\
      \vdots\qquad  & \\
      a_{m1}x_1+\cdots +a_{mn}x_n& = b_n \\
      \end{align*}
    $ 
    </p><br>
  - $\mathbf{x}=[x_1,\ldots ,x_n]\in\mathbb{R}^n\quad$: the unknow variable of the system.
  - Every $\mathbf{x}$ that satisfies above euqasion is a **solution** of the linear equation system.
  - There can be *one* solution, *no* solution or *inifinite* solutions.

### 3.1 Linear Equation: Example

<p align="center">
  <img src="{{site.gdrive_url_prefix}}1MYB_icHMhww75tUvPk-m90-APraF2PgJ" title="lec01-p5-linear equation image" style="width: 75%">
</p>

Each linear equation defines a *line* on $x_1x_2-plane$.
  - A solution set is the intersection of these lines.
  - Infinite solution&nbsp;&nbsp;&nbsp;:If two equations refer to the same line.
- No solution&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:If two lines are parallel.

When there are three variables,
- Each linear equation defines a **plane** in 3-dimensional space.
- A solution set is the intersection between planes
  - Point
  - Line
  - Plane
  - Empty

## 4. Two ways of representing linear equations

<ol>
<li>$
\begin{bmatrix}
      a_{11} \\
      \vdots \\
      a_{m1} \\
\end{bmatrix}x_1+\begin{bmatrix}
      a_{12} \\
      \vdots \\
      a_{m2} \\
\end{bmatrix}x_2+\cdots +\begin{bmatrix}
      a_{1n} \\
      \vdots \\
      a_{mn} \\
\end{bmatrix}x_n=\begin{bmatrix}
      b_{1} \\
      \vdots \\
      b_{m} \\
\end{bmatrix}
$
</li>
  <ul>
    <li><p>Finding a proper weight vector $\mathbf{x}=[x_1,\ldots ,x_n]\in\mathbb{R}^n$ for this linear combination betwen column vectors, wich are $a_j=[a_{1j},\ldots ,a_{mj}]^T$.<br><br></p></li>
  </ul>
<li>
$
\begin{bmatrix}
      a_{11} & \cdots & a_{1n} \\
      \vdots & & \vdots \\
      a_{m1} & \cdots & a_{mn} \\
\end{bmatrix}x_1+\begin{bmatrix}
      x_{1} \\
      \vdots \\
      x_{2} \\
\end{bmatrix}=\begin{bmatrix}
      b_{1} \\
      \vdots \\
      b_{m} \\
\end{bmatrix}
$
</li>
<ul><li><p>$\mathbf{Ax}=\mathbf{b}\rightarrow$  How to solve matrix form?<br></p></li></ul>
</ol>

## 5. Matrix Addition and Multiplication

- **Addition** : an element-wisse sum between two same-sized matrices.
- **Multiplication** 
  - For $\mathbf{A}\in\mathbb{R}^{m\times n}, \mathbf{B}\in\mathbb{R}^{n\times l}$, the element $c_{ij}$ of the product $\mathbf{C}=\mathbf{AB}\in\mathbb{R}^{m\times l}$ is:
    - $c_{ij}=\Sigma_{k=1}^na_{ik}b_{kj}=\mathbf{a}_i^T\mathbf{b}_j, i=1,\ldots , m, j=1,\ldots , l$

      - An inner product between $i$-th row of $\mathbf{A}$ with the $j$-th column of $\mathbf{B}$.
    - \# of columns of $\mathbf{A}$ (dim. of row vector) == \# of rows of $\mathbf{B}$ (dim. of column vector)
  - $\mathbf{BA}$ is not defined if $l\ne m$
  - $\mathbf{AB}\ne \mathbf{BA}$
  - Element-wise multiplication is called as a Hadamard product.

## 6. Associativity and Distributivity of Multiplication

- **Associativity**
  - $(\mathbf{AB})\mathbf{C} = \mathbf{A}(\mathbf{BC})$
  - For scalar $\lambda$ and $\phi$
    - $(\lambda\phi)\mathbf{C} = \lambda(\phi\mathbf{C})$
    - $\lambda(\mathbf{BC}) = (\lambda\mathbf{B})\mathbf{C} = \mathbf{B}(\lambda\mathbf{C})= (\mathbf{BC})\lambda$
    - $(\lambda\mathbf{C})^T = \mathbf{C}^T\lambda^T = \mathbf{C}^T\lambda = \lambda\mathbf{C}^T$
- **Distributivity**
  - $(\mathbf{A}+\mathbf{B})\mathbf{C} = \mathbf{A}\mathbf{C} + \mathbf{B}\mathbf{C}$
  - $\mathbf{A}(\mathbf{C}+\mathbf{D}) = \mathbf{A}\mathbf{C} + \mathbf{A}\mathbf{D}$
  - For scalar $\lambda$ and $\phi$
    - $(\lambda+\phi)\mathbf{C} = \lambda\mathbf{C}+\phi\mathbf{C}$
    - $\lambda(\mathbf{B}+\mathbf{C}) = \lambda\mathbf{B}+\lambda\mathbf{C}$
- $\mathbf{x}^T\mathbf{A}\mathbf{x}$
  - $\mathbf{x}^T\mathbf{x}$ is scalar
  - $\mathbf{x}^T\mathbf{x}A\mathbf{y}^T\mathbf{y} = \mathbf{x}^T\mathbf{x}\mathbf{y}^T\mathbf{y}A=\mathbf{y}^T\mathbf{y}A\mathbf{x}^T\mathbf{x}$

## 7. Identity, Inverse and Transpose

<ul>
  <li><b>Identity Matrix</b> (square)</li>
  <ul>
    <li>$\mathbf{I}\in\mathbb{R}^{n\times n}$ is an identity matrix if 1 on the diagonal and 0 everywhere else.</li>
    <li><p>ex) $
\begin{bmatrix}
      1 & 0 & 0 \\
      0 & 1 & 0 \\
      0 & 0 & 1 \\
\end{bmatrix}
$ $3\times 3$ identity matrix</p></li>
<li>$\mathbf{AI}=\mathbf{IA}=\mathbf{A}$</li>
</ul>
<li><b>Inverse Matrix</b> $\mathbf{A}^{-1}$</li>
<ul>
<li>For a square matrix $\mathbf{A}\in\mathbb{R}^{n\times n}$, another square matrix $\mathbf{B}\in\mathbb{R}^{n\times n}$ is $\mathbf{A}^{-1}$ if $\mathbf{AB}=\mathbf{BA}=\mathbf{I}$</li>
<li>"Singular", noninvertible" <b>A</b> : $\mathbf{A}^{-1}$ does not exist.</li>
</ul>
<li><b>Transpose:</b> for $\mathbf{A, B}$ with $b_{ij}=a{ji}$ is called the transpose of $\mathbf{A}$, written as $\mathbf{A}^T$.</li>
<ul><li>A matrix $\mathbf{A}\in\mathbb{R}^{n\times n}$ is symmetric if $\mathbf{A}=\mathbf{A}^T$.</li></ul>
</ul>

## 8. Vector Space

- A set of elements (vectors) with rules for **"linear combinations"**
  1. Addition,
  2. Multiplication by real numbers (scalars).
- $\mathbb{R}^n$ : a space consists of all column vectors with $n$ components.
  - $x\in\mathbb{R}^n$ : n-dim vector
- Addition and Scalar Multiplications are required to satisfy below rules:
  - $\mathbf{x}+\mathbf{y}=\mathbf{y}+\mathbf{x}$
  - $\mathbf{x}+(\mathbf{y}+\mathbf{z})=(\mathbf{x}+\mathbf{y})+\mathbf{z}$
  - $\mathbf{x}+\mathbf{0}=\mathbf{x}$
  - There is a unique "zero vector" such that $\mathbf{x}+\mathbf{0}=\mathbf{x}$ for all $\mathbf{x}$
  - $1\mathbf{x}=\mathbf{x}$
  - $(c_1c_2)\mathbf{x}=c_1(c_2\mathbf{x})$
  - $c(\mathbf{x}+\mathbf{y})=c\mathbf{x}+c\mathbf{y}$
  - $(c_1+c_2)\mathbf{x}=c_1\mathbf{x}+c_2\mathbf{x}$

## 9. Vector Subspace

- A non-empty subset of a vector space:
  1. If $\mathbf{x}$ $\mathbf{y}$ are in the subspace, $\mathbf{x}+\mathbf{y}$ is in the subspace.
  2. If multiply $\mathbf{x}$ in the subspace by any scalar $c$, $c\mathbf{x}$ is in the subspace.
  
      - Linear combinations stays in the subspace.
      - "Closed" under addition and scalar multiplication.
- A zero vector belongs to every subspace, due to zero scalar.
- Example
  - For a vector space by $3\times 3$ matrices, $\mathbb{V}$
    - A set of symmetric matrices. &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(o)
    - A set of lower triangular matrices in its subspace (o)
    - A set of matrices with all positive elements&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(x)
      
      $\rightarrow$ It is because for $c\mathbf{A}, c\lt 0, c\mathbf{A}\notin\mathbb{V}$

## 10. Column Space of A, C(A)$

<ul>
<li>All linear combinations of columns of $\mathbf{A}\in\mathbb{R}^{m\times n}$.</li>
<li>Example</li>
<ul>
<li>$\mathbf{Ax}=\mathbf{B}$ when $m=3$ and $n=2$,
</li>
<li><p>$\begin{bmatrix}
1 & 0 \\
5 & 4 \\
2 & 4 \\
\end{bmatrix}\begin{bmatrix}
u \\
v \\
\end{bmatrix}=\begin{bmatrix}
b_1 \\
b_2 \\
b_3 \\
\end{bmatrix} \Rightarrow u\begin{bmatrix}
1 \\
5 \\
2 \\
\end{bmatrix}+v\begin{bmatrix}
0 \\
4 \\
4 \\
\end{bmatrix}=\begin{bmatrix}
b_1 \\
b_2 \\
b_3 \\
\end{bmatrix}$
</p>
</li>
<li>$\mathbf{Ax}=\mathbf{B}$ is solvable $\Leftrightarrow \mathbf{b}$ can be expressed as a linear combination of columns of $\mathbf{A}$.</li>
<li>$\mathbf{Ax}=\mathbf{B}$ is solvable $\Leftrightarrow \mathbf{b}$ lies in <b>plane</b> spanned by two column vectors.</li>
<ul>
<li>The <b>plane</b> is a subspace, a column space of $\mathbf{A}$, consisting of all linear combinations of the columns.</li>
</ul>
</ul>
<li>For an invertible matrix, $\mathbf{Ax}=\mathbf{b}$ will have a solutino for every $\mathbf{b}$.</li>
<ul>The column space is the whole space $\mathbb{R}^m$</ul>
</ul>

## 11. Null Space of A, N(A)

<ul>
<li>A vector space formed by the solutions to $\mathbf{Ax}=\mathbf{0}. x\in\mathbb{R}^n$</li>
<li>$\mathbf{N(A)}$ is a subspace of $\mathbb{R}^n$.</li>
<li>If $\mathbf{N(A)}$ contains only the zero vector, $\mathbf{A}$ has "independent columns"</li>
<ul>
<li>$\mathbf{Ax}=\mathbf{0} \rightarrow x=\mathbf{0}$<br>
$\Rightarrow[a_1, \ldots, a_n]x=\overrightarrow{a_1}x_1+\cdots+\overrightarrow{a_n}x_n=\mathbf{0}$<br>
$(c_1\overrightarrow{v_1}+\cdots+c_n\overrightarrow{v_n}=\mathbf{0}, c_1=\cdots=c_n=0$ which means $v_i: L.I.)$</li>
</ul>
<li>For an invertible matrix, $\mathbf{N(A)}$ contains only $\mathbf{x}=\mathbf{0}$</li>
<ul><li>$\mathbf{Ax}=\mathbf{0}\rightarrow \mathbf{x}=\mathbf{A}^{-1}\mathbf{0}=\mathbf{0}$</li></ul>
</ul>

## 12. Solving Ax=b: Particular and General Solution

- Any vector $\mathbf{x_n}$ in the nullspace can be added to a particular solution $\mathbf{x_p}$.
  - So that solutions are $\mathbf{x}=\mathbf{x_p}+\mathbf{x_n}$.
    - $\mathbf{Ax_p}=\mathbf{b}, \mathbf{Ax_n}=\mathbf{0}\Rightarrow \mathbf{A}(\mathbf{x_p}+\mathbf{x_n})=\mathbf{b}$
- (Simple) Example

<p>$
\begin{bmatrix}
1 & 0 & 8 & -4 \\
0 & 1 & 2 & 12 \\
\end{bmatrix}\begin{bmatrix}
x_1 \\ x_2 \\ x_3 \\ x_4 \\
\end{bmatrix} = \begin{bmatrix} 42 \\ 8 \\ \end{bmatrix}$, <br>$\begin{Bmatrix} \mathbf{x}\in\mathbb{R}^4 : \mathbf{x}=\begin{bmatrix}42\\8\\0\\0\\\end{bmatrix}+\lambda_1\begin{bmatrix}8\\2\\-1\\0\\\end{bmatrix}+\lambda_2\begin{bmatrix}-4\\12\\0\\-1\\\end{bmatrix}, \lambda_1, \lambda_2\in\mathbb{R}\end{Bmatrix}
$</p>

where $\begin{bmatrix} 42 \\\\ 8 \\\\ 0 \\\\ 0 \\\\ \end{bmatrix}$ is a particular solution, and $\begin{bmatrix}8\\\\2\\\\-1\\\\0\\\\ \end{bmatrix}, \begin{bmatrix}-4\\\\12\\\\0\\\\-1\\\\ \end{bmatrix}$ are nullspace vectors. 

## 13. Gaussian Elimination

- An algorithm that performs "elementary transformations to bring a system of linear equations into reduced "row-echelon form" (REF)
- **Elementary Transformations**
  - Exchange of two equations (rows in the matrix).
  - Multiplications of an equation (row) with a non-zero constant.
  - Addition of two equations (rows).
- **Row-Echelon Form of a Matrix**
  - All rows containing only zeros are at the bottom
  - All rows containing at least one non-zero element are on top of rows containing zeros.
  - For non-zero rows, the first non-zero number from the left **(pivot)** is always strictly to the right of the pivot of the row above it.
- Example
  <p align="center">
    <img src="{{site.gdrive_url_prefix}}1wCZgA22unVJW954u04u8bKai1i90awhP" title="lec01-p17-ref image" style="width: 100%">
  </p>
- The variables corresponding to the pivots in REF are "pivot variables", otherwise, "free variables".
- A matrix is "reduced" row-echelon form if,
  - Every pivot is 1,
  - The pivot is the only non-zero entry in its column.

## 14. Back to Previous Example: Particular and General Solution

- Particular solution can be found by expressing equation by using only pivot columns.
  <p align="center">
    <img src="{{site.gdrive_url_prefix}}1ZVaFCFUtv2eF9UMHwBzhHiiSBl5BDwG7" title="lec01-p17-ref image" style="width: 100%">
  </p>

  $\lambda_3=1, \lambda_2=-1, \lambda_1=2$

  $\mathbf{x}=[2, 0, -1, 1, 0]^T$

- Solutions of $\mathbf{Ax}=\mathbf{0}$ can be found by expressing non-pivot columns in terms of sums and multiples of pivot columns that are on their left.
  <p align="center">
    <img src="{{site.gdrive_url_prefix}}1GekOEuXJZjYFyr0fo9uhZxvszxui6W4I" title="lec01-p17-ref image" style="width: 100%">
  </p>
  <p align="center">
    <img src="{{site.gdrive_url_prefix}}1qiMoQmQ3BM1gC8mxRQExeaCbsjL2CX-M" title="lec01-p17-ref image" style="width: 100%">
  </p>
- If $\mathbf{AX}=\mathbf{b}$ has more unknows than equations $n\gt m$, there are at least $n-m$ free variables.
- When there are $r$ pivot rows and columns, the **rank** of matrix is $r$.

## 15. Linear Independence

- Vectors $v_1, \ldots, v_k$ are linearly independent if $c_1\mathbf{v}_1+\ldots+c_k\mathbf{v}_k=\mathbf{0}$ only happens when $c_1=\ldots=c_k=0$.
- If any $c$'s are non-zero, the $\mathbf{v}$'s are linearly dependent
  - One vector is a linear combination of the others.
- The columns of $\mathbf{A}$ are independent exactly when $\mathbf{N(A)}=\{\mathbf{0}\}.$
  - which means $\mathbf{Ax}=\mathbf{0}$  has the $\mathbf{0}$ as a unique solution.

## 16. Span and Rank

- **Span**
  - If a vector space $\mathbb{V}$ consists of all linear combinations of $\mathbf{w}_1, \ldots, \mathbf{w}_l$, then these vectors **span the space.**
  - Every vector in $\mathbb{V}$ is some combination of the $\mathbf{w}$'s
    - $\mathbf{v}=c_1\mathbf{w}_1+\ldots+c_l\mathbf{w}_l$
  - The column space of $\mathbf{A}$ is "spanned" by its columns.

- **Rank** $r$
  - The number of $min$(\# of rows, \# of columns)
  - The number of genuinely independent rows/columns in the matirx.
  - A dimension of the column space of matrix.
  - When $r=m=n$, the matrix has an inverse.

## 17. Basis for Vector Space

<ul>
<li>A sequence of vectors having below two properties:</li>
<ol>
<li>The vectors are linearly independent (not too many vectors).</li>
<li>The vectors span the vector space (not too few vectors).</li></ol>
<li>There is one and only one way to write a vector as a linear combination of basis vectors.</li>
<ul>
<p>
$For\ \mathbf{v}\in\mathbb{V}\text{ assume that }$<br>
$\mathbf{v}=\alpha_1\overrightarrow{b}_1+\cdots+\alpha_n\overrightarrow{b}_n=\beta_1\overrightarrow{b}_1+\cdots+\beta_n\overrightarrow{b}_n (\alpha_i\ne \beta_i, i=1,\ldots,n)$<br>
$\rightarrow(\alpha_1-\beta_1)\overrightarrow{b}_1+\cdots+(\alpha_n-\beta_n)\overrightarrow{b}_n=\mathbf{0}\rightarrow\alpha_i-\beta_i=0$<br>
becuase $b_i's$ are basis vectors (thus linearly independent). It contradicts the assumption that $\alpha_i\ne\beta_i$
</p></ul>
<li>If columns of a matrix is a basis for $\mathbb{R}^n$, then the matrix must be square and invertible.</li>
<ul><p>
$\mathbf{A}\in\mathbb{R}^{m\times n} \rightarrow [\mathbf{a}_1, \ldots, \mathbf{a}_n]$<br>
$i)\ \text{if m}\lt\text{n}$<br>
$\quad dim(A)\lt dim(\mathbb{R}^n),\ \text{columns of A is not a basis of }\mathbb{R}^n$<br>
$ii)\ \text{if m}\geq\text{n}$<br>
$\quad\text{for columns of A to be L.I., m=n}$<br>
</p>
</ul>
<li>"Dimension" of a vector space is "the number of basis vectors"</li>
<ul><li>Dimension of $\mathbf{C(A)}$ is rank $r$ (# of pivot variables, # of independent row/columns)</li>
<li>Dimension of $\mathbf{N(A)}$ is $n-r$ (# of non-pivot/free variables)</li></ul>
</ul>

## 18. Linear Transformations

<ul><li>$\mathbf{T(x)}$ is a linear transformation if $\mathbf{T}(c\mathbf{x}+d\mathbf{y})=c\mathbf{T}(\mathbf{x})+d\mathbf{T}(\mathbf{y})$</li>
<li>Consider transformation by matrix $\mathbf{A} : \mathbf{Ax}$</li>
<li>If basis : $\mathbf{x}_1,\ldots,\mathbf{x}_n$ and $\mathbf{x}=c_1\mathbf{x_1}+\ldots+c_n\mathbf{x_n}$</li>
<ul>
<li>$\mathbf{Ax}=c_1(\mathbf{Ax_1})+\ldots+c_n(\mathbf{Ax_n})$</li>
<li>If we know $\mathbf{Ax}$ for each vector in a basis, then we know $\mathbf{Ax}$ for each vector in the entire space.</li></ul>
<li>Is composition of two linear transformations is also linear transform?</li>
<ul>
$\mathbf{y}=\mathbf{T}(\mathbf{x}), \mathbf{\mathbf{z}}=\mathbf{U}(\mathbf{y})$<br>
$\mathbf{z}=\mathbf{U}(\mathbf{T}(\mathbf{x}))=\mathbf{R}(\mathbf{x}), \mathbf{x}\leftarrow\ c_1\mathbf{x_1}+c_2\mathbf{x_2}$<br>
$\mathbf{z}=\mathbf{U}(\mathbf{T}(c_1\mathbf{x_1}+c_2\mathbf{x_2}))=\mathbf{U}(c_1\mathbf{T}(\mathbf{x_1})+c_2\mathbf{T}(\mathbf{x_2}))$<br>
$\mathbf{z}=c_1\mathbf{U}(\mathbf{T}(\mathbf{x_1}))+c_2\mathbf{U}(\mathbf{T}(\mathbf{x_2}))$<br>
$\mathbf{z}=c_1\mathbf{R}(\mathbf{x_1})+c_2\mathbf{R}(\mathbf{x_2})$</ul></ul>
