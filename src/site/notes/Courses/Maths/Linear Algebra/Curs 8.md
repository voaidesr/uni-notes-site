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
\quad G = (g_{ij})_{i,j = \overline{1,n}}
$$
Unde $G$ este matricea asociată lui $g$ în raport cu reperul $\mathcal{R}$. 

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

**Demonstrație bijectivitate**: 

Fie $Q:V \to \mathbb{K}$ o formă pătratică $\Rightarrow$ $\exists g \in L^s(V, V;\mathbb{K}) \text{ astfel încât } Q(x) = g(x, x), \forall x \in V.$

Construim g. 
$$
g(x+y, x+y) = Q(x+y) \Leftrightarrow g(x,x) + g(y, x) + g(x,y) + g(y,y) = Q(x,y)
$$
Deci:
$$
Q(x) + Q(y) + 2g(x,y) = Q(x,y)
$$
De unde rezultă forma polară a lui $g$: #forma_polară
$$
g(x,y) = 2^{-1}[Q(x,y) - Q(x) - Q(y)] \qquad
$$

```ad-note
$
Q(x) = \sum_{i = 1}^ng_{ij}x_i^2 + 2\sum_{i = 1}^ng_{ij}x_ix_j
$
```

>[!definition] Nucleul lui $g$
>Fie $g \in L^s(V, V; \mathbb{K})$
>$\operatorname{ker} g = \{ x \in V | \quad g(x,y) = 0, \quad \forall y \in V \}$
>
>Mai mult:
>$g$ se numește **nedegenerat** $\Leftrightarrow$ $\operatorname{ker} g = \{ 0_V \}$

**Observație**
Fie $\mathcal{R} = \{ e_1, \dots, e_n \}$ reper în $V$ și $x \in \operatorname{Ker} g$
$$
\begin{cases}
g(x, e_1) = 0 \\
\vdots \\
g(x, e_n) = 0
\end{cases}
\implies
\begin{cases}
\sum_{i=1}^m g_{1i} x_i = 0 \\
\vdots \\
\sum_{i=1}^m g_{ni} x_i = 0
\end{cases}
$$
Sistemul este un SLO. Are soluție unică nenulă  $\Leftrightarrow$ $\operatorname{det}((g_{ij})_{i,j}) = \operatorname{det} G \neq 0$

>[!definition]
> Fie $Q : V \to \mathbb{R}$ fromă pătratică reală 
> Se numește pozitiv definită dacă și numai dacă:
> 		1) $\operatorname{Q}(x) > 0, \quad \forall x\neq 0_V$ și
> 		2) $\operatorname{Q}(x) = 0 \Leftrightarrow x = 0_V$

Fie $g$ forma polară asociată lui $Q$. Dacă $Q$ este pozitivă definită, atunci $g$ este pozitiv definită. 

***Proprietate***
$\text{Fie } g \in L^s(V, V; \mathbb{R}).$
$g$ pozitiv definită $\implies$ $g$ este nedegenerată

##### ***Exemplul 1***

Fie
$$
g : \mathbb{R}^3 \times \mathbb{R}^3  \to \mathbb{R}, \quad g(x,y) = x_1y_1 + x_2y_2 + x_3y_3
$$
- $G$ = ? -> matricea asociată lui $g$ în raport cu reperul canonic
- $Q: \mathbb{R}^3  \to \mathbb{R}$ forma pătratică asociată lui $g$. Este pozitiv definită? 

***Soluție***

a) $g(x,y) = \sum_{i,j=1}^3g_{ij}x_iy_j = x_1y_1 + x_2y_2 + x_3y_3 = \sum_{i = 1}^3x_iy_i$

Deci:
$$
G = I_3 = 
\begin{pmatrix}
1 & 0 & 0\\
0 & 1 & 0\\
0 & 0 & 1
\end{pmatrix}
$$
b) $Q$ formă pătratică. 

$$
Q(x) = g(x, x) = \sum_{i = 1}^3x_i^2
$$
Avem 
$$
Q(x) > 0, \quad \forall x \in \mathbb{R}^3 \quad (1)
$$
Și
$$
Q(x) = 0 \iff x_1 = x_2 = x_3 = 0 \iff x = \sum_{i = 1}^3x_ie_i = 0_{\mathbb{R}^3} \quad (2)
$$
Deci, (1), (2), $\implies$ $Q$ este pozitiv definită 

###### ***Exemplul 2***

Fie 
$$
g : \mathbb{R}^3 \times \mathbb{R}^3  \to \mathbb{R}, \quad G = 
\begin{pmatrix}
1 & 3 & 2 \\
3 & 4 & -1\\
2 & -1 & 0
\end{pmatrix}
$$
$g$ forma biliniară, $G$ matricea asociată lui $g$ în raport cu reperul canonic $\mathcal{R}_0$

a) $g$ este formă simetrică 
b) $g$ = ?, $Q : \mathbb{R}^3 \to \mathbb{R}$ formă pătratică asociată, $Q$ = ?  

*Soluție*

a) $G = G^{\top} \implies g\in L(\mathbb{R}^3, \mathbb{R}^3; \mathbb{R})$ 

b)
$$
\begin{aligned}
g(x,y) &= \sum_{i,j=1}^3 g_{ij} x_i y_j \\
       &= x_1 y_1 + 3 x_1 y_2 + 2 x_1 y_3 \\
       &\quad + 3 x_2 y_1 + 4 x_2 y_2 - x_2 y_3 \\
       &\quad + 2 x_3 y_1 - 1 x_3 y_2
\end{aligned}
$$
