$\newcommand\Q{\mathbb{Q}}$
$\newcommand\R{\mathbb{R}}$
$\newcommand\hh{\hat{h}}$

# The BSD formula over number fields
[TOC]


The Birch--Swinnerton-Dyer (BSD) formula for elliptic curves over $\Q$ is clearly stated and explained in many texts, but for elliptic curves over general number fields there are a number of subtleties leading to the appearance of factors which are equal to $1$ in the rational case, and clear and explicit statements are harder to find.  (Insert Tate, Gross, T. Dokchitser references).

For simplicity we restrict to the case of elliptic curves, mentioning later the (fairly simple) adjustments needed for Abelian varieties of higher dimension $g$.

Traditionally, the BSD formula is written as a formula for the leading coefficient of the L-series $L(E,s)$ at $s=1$ in terms of other quantities, including the order of the Tate-Shafarevich group &#1064;.   We always assume the standard conjectures, first that $L(E,s)$ has analytic continuation so that its behaviour at $s=1$ is defined, and also that &#1064; is finite.  Then we rearrange the formula so that its expresses $S=$|&#1064;| in terms of quantities which may be computed. 

## Code
Where possible, we give the appropriate commands in Sage and Magma to compute these.  Many of these functions have a precision parameter to control the precision of the output: the input is always exact, but the output may in principle be computed to arbitrary precision.

## Notation
$K$ is a number field of degree $d=r_1+2r_2$, where $r_1$ and $r_2$ are the number of real and complex places, and discrimninant $d_K$.

$E$ is an elliptic curve defined over $K$, given by an integral Weierstrass equation or model.  We do not assume that the model we have is minimal, since global minimal models do not always exist anyway.  We denote by $\Delta(E)$ the discriminant of the model, which is an integral element of $K$, and by $\mathfrak{d}(E)$ the minimal discriminant ideal of $E$; this is independent of the model, and $\mathfrak{d}(E)/(\Delta(E))$ is the $12$^th^ power of a principal ideal.

The Mordell-Weil group $E(K)$ is a finitely-generated abelian group of rank $r=r(E(K))$, torsion subgroup $T=E(K)_{tors}$ and points $P_1,\dots,P_r$ generating $E(K)/T$.

## Outline formula
The formula for $S$ has the form
$$
S = (\text{field factor})(\text{$L$-value})(\text{Mordell-Weil factor})(\text{local factor})
$$

## The field factor
This is $$|d_K|^{1/2}$$ (or $|d_K|^{g/2}$ for Abelian varities of dimension $g$).  Gross includes this in with the local factors at infinite places.

### Magma:
`Sqrt(Abs(Discriminant(K)));`

### Sage:
`RR(K.discriminant().abs()).sqrt()`

### The L-value
This is $$L^{(r)}(E,1)/r!.$$

### Magma:
`Evaluate(LSeries(E),1: Derivative:=r) / Factorial(r);`

### Sage:
$\newcommand\Q{\mathbb{Q}}$
For $K=\Q$ only: `E.lseries().dokchitser().derivative(1,r) / factorial(r)`

## Mordell-Weil factors
$\newcommand\hh{\hat{h}}$
This is $$\frac{|T|^2}{R(P_1,\dots,P_r)},$$ where $R()$ is the regulator, i.e. the determinant of the height pairing matrix.  This needs to be normalised correctly!
1.  The $(i,j)$-entry of the height pairing matrix is $\left<P_i,P_j\right> = \frac{1}{2}(\hh(P_i+P_j) - \hh(P_i) - \hh(P_j))$ so in particular the diagonal entries are the canonical heights;
2.  The canonical height should be defined with respect to the $x$-coordinate (so that $x(P)-\hh(P)$ is bounded independently of $P$).  There is another convention (used in Silverman's papers) which is half of this, as it is defined with respect to the divisor $(O)$ instead of $2(O)$.
3.  The canonical height should **not** be normalized (by dividing by the degree $d$) to be invariant under base-change.  Note that both Magma and Sage by default normalize the height.  Sage has an option to not normalize for heights, but not (as of version 9.1) for the regulator.  Hence in both cases the regulator needs to be adjusted by multiplying by $d^r$.
     
### Magma:
`Regulator(P1,...,Pr) * d^r;`

### Sage:
`E.regulator_of_points([P1,...,Pr]) * d^r`

The expression given here will only be correct if the points $P_1,\dots,P_r$ are **saturated**, i.e. generate $E(K)$ modulo torsion, so that $[E(K):\left<P_1,\dots,P_r\right>] = |T|$. An alternative expression is sometimes seen which avoids this:  $$\frac{[E(K):\left<P_1,\dots,P_r\right>]^2}{R(P_1,\dots,P_r)}.$$  This is not very useful in practice for computations.

**NB** If the points are not saturated but only generate a subgroup (modulo torsion) of index $n$ then this factor, end hence the overall expression for $S$, will be $1/n^2$ times the correct value.   This will (usually) lead to a non-integral value of $S$, which can be detected.

 


## Local factor

The local factor is a product of local factors, one for each place of $K$.

### At finite places

At a finite place $v$ the local factor is $c_v$, the Tamagawa number at $v$.  This is a positive integer, the index $c_v = [E(K_v):E(K_v^0)]$, i.e. the number of connected components of $E(K_v)$.  At a place of good reduction we have $c_v=1$, so this is in effect a finite product.

### At infinite places

At each infinite place the local factor is a sutably normalized period $\Omega_v$ and the total contribution from all infinite places is $\prod_{v\mid\infty}\Omega_v$.   Theoretically the $\Omega_v$ are defined in terms of a N&eacute;ron differential.   We first define the periods for a fixed model of $E$ and then explain how to normalize it.  Let $\omega_E = dx/(2y+a_1x+a_3$.)

#### At a real place
$\newcommand\R{\mathbb{R}}$
If $v$ is a real place define $\Omega_v(E)$ to be the integral of $\omega_E$ over $E(\R)$.   This is equal to the least positive real period multiplied by the number of real components at the place $v$ (which is $1$ when $v(\Delta_E)<0$ and $2$ when $v(\Delta_E)>0$).

  When $\Delta_E>0$ the period lattice of $E$ at $v$ has generating periods $x$, $yi$ with $x,y>0$, and $\Omega_v=2x$. When $\Delta_E<0$ the period lattice of $E$ at $v$ has generating periods $2x$, $x+yi$ with $x,y>0$, and $\Omega_v=2x$.

#### At a complex place
$\newcommand\C{\mathbb{C}}$
If $v$ is a complex place define $\Omega_v(E)$ to be **twice** the integral of $\omega_E$ over $E(\C)$.  If the period lattice of $E$ at $v$ has generating periods $w_1$, $w_2$ with $\Im(w_2/w_1)>0$, then $\Omega_v=2\Im(\overline{w_1}w_2)$ (which is positive).

If we were to define $\Omega_v$ for complex $v$ to be simply the integral of $\omega_E$ over $E(\C)$, then the BSD formula would need an additional factor of $2^{r_2}$.

#### Adjustment for non-minimal models



