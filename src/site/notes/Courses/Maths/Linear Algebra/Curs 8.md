---
{"dg-publish":true,"permalink":"/courses/maths/linear-algebra/curs-8/"}
---


### Forme biliniare 

```ad-Definition
Fie spațiul vectorial $(V, +, \cdot)|_{\mathbb{K}}$.

$g: V \times V \to \mathbb{K}$ se numește **formă biliniară** dacă și numai dacă:

a) $g(\alpha x  + \beta y, z) = \alpha g(x, z) + \beta g(y, z)$
b) $g(x, \alpha y + \beta z) = \alpha g(x, y) + \beta g(x, z)$  $\forall x, y, z \in V$ și $\forall \alpha, \beta \in \mathbb{L}$
```

```ad-Notation
$
L(V, V; \mathbb{K}) = \{ g: V \times V \to \mathbb{K} \text{ | } g \text{ formă bilinară } \}
$
```

Unde $L(V, V; \mathbb{K})|_{\mathbb{K}}$ este spațiu vectorial.
#forma_simetrica #forma_antisimetrica
- $g$ Este formă **simetrică** $\Leftrightarrow g(x,y) = g(y,x)$
- $g$ este formă **antisimetrică** $\Leftrightarrow g(x,y) = -g(y,x)$

```ad-Notation
Sunt subspații vectoriale:
$
L^s(V, V; \mathbb{K}) = \{ g: V \times V \to \mathbb{K} \text{ | } g \text{ formă simetrică } \}
$

$
L^a(V, V; \mathbb{K}) = \{ g: V \times V \to \mathbb{K} \text{ | } g \text{ formă antisimetrică } \}
$
```

Dacă $g$ este liniară într-un argument și simetrică $\Rightarrow$ g este biliniară.

##### Matricea asociată unei forme biliniare

Fie $g \in L(V, V; \mathbb{K})$ și $\mathcal{R}  = \{ e_1, \dots, e_n \}$ un reper în $V$:
$$
g(e_i, e_j) \overset{\text{not.}}{=} g_{ij} 
\quad \forall i,j = \overline{1,n}
\quad G = (g_{ij})_{i,j = \overline{1,n}} \text{ este matricea asociată lui g în raport cu reperul R}
$$
Avem
$$
g(x, y) = g(\sum_{i=1}^nx_ie_i, \sum_{j=1}^ny_je_j) = \sum_{i, j = 1}^nx_iy_jg_{ij}
$$
Fie un alt reper $\mathcal{R}' = \{e_1', \dots, e_n' \}$ și $C$ matricea de trecere $\mathcal{R} \overset{C}{\to} \mathcal{R}'$.
Atunci:
$$
G' = C^{\top}GC, \quad G \in GL(n, \mathbb{K})
$$
```ad-important
$
\operatorname{rg} G' = \operatorname{rg}(C^{\top}GC) = \operatorname{rg} G \quad \text{ invariant la schimbarea de repre }
$
```

### Forme pătratice 

```ad-Definition
Într-un corp $\mathbb{K}$ cu $\operatorname{ch} \mathbb{K} \neq 2$, $Q : V \to \mathbb{K}$ se numește **formă pătratică** dacă și numai dacă:
$
\exists g \in L^s(V, V;\mathbb{K}) \text{ astfel încât } Q(x) = g(x, x), \forall x \in V.
$

```

Există o corespondență bijectivă între mulțimea formelor pătratice definite pe $V$ și mulțimea formelor biliniare simetrice definite pe $V$.

```ad-proof
title: Demonstrație bijectivitate 
Fie $Q:V \to \mathbb{K}$ o formă pătratică $\Rightarrow$ $\exists g \in L^s(V, V;\mathbb{K}) \text{ astfel încât } Q(x) = g(x, x), \forall x \in V.$

Construim g. 
$
g(x+y, x+y) = Q(x+y) $\Leftrightarrow$ g(x,x) + g(y, x) + g(x,y) + g(y,y) = Q(x,y)
$
Deci:
$
Q(x) + Q(y) + 2g(x,y) = Q(x,y)
$
De unde rezultă: #forma_polară
$
g(x,y) = 2^{-1}[Q(x,y) - Q(x) - Q(y)] \qquad \text{ Forma polară a lui }g.
$
```

```ad-note
$
Q(x) = \sum_{i = 1}^ng_{ij}x_i^2 + 2\sum_{i = 1}^ng_{ij}x_ix_j
$
```


