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

##### ***Exemplul 2***

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
Aflăm și forma pătratică
$$
Q(x) = g(x, x) = \sum_{i = 1}^3g_{ii}x_i^2 + 2\sum_{i<j}g_{ij}x_ix_i = x_1^2 + 4x_2^2 + 6x_1x_2 + 4x_1x_3 - 2x_2x_3
$$
##### ***Problemă*** - Forma canonică 
Fie $Q: V \to \mathbb{K}$ formă pătratică. 
$\exists \mathcal{R} = \{e_1 \dots e_n\}$ reper în $V$ astfel încât: 
$$
G = \begin{pmatrix}
a_1 & 0 & \cdots & 0 & 0 & \cdots & 0 \\
0 & a_2 & \cdots & 0 & 0 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots & \vdots & & \vdots \\
0 & 0 & \cdots & a_k & 0 & \cdots & 0 \\
0 & 0 & \cdots & 0 & 0 & \cdots & 0 \\
\vdots & \vdots &  & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 0 & 0 & \cdots & 0
\end{pmatrix} ?
$$
(i.e., G este [[Courses/Maths/Linear Algebra/Curs 7#Vectori proprii. Valori proprii. Diagonalizarea\|diagonalizabilă]])

Dacă există, atunci #forma_canonică
$$
Q(x) = a_1x_1^2 + \dots + a_kx_k^2
$$
Unde $r = \operatorname{rg} G$. (Se numește ***forma canonică***)

```ad-tip
##### **Teorema Gauss**

Fie $Q : V \to \mathbb{K}$ o formă pătratică. 

$\exists \mathcal{R} = \{e_1, \dots, e_n \}$ reper în $V$ astfel încât $Q$ are o formă canonică. 
```

***Demonstrație***

1) $Q = 0 \implies Q$ are formă canonică 
2) $Q \neq 0 \implies G \neq O_n$```
- $g_{11} \neq 0$
- $g_{11} = 0, \exists i_0 \in \{2, \dots, n\} \text{ a.i. } g_{i_0i_0} \neq 0$ 
Aici eventual schimbăm indicii (schimbare de reper) astfel încât $g_{11} \neq 0$ 
- $g_{ii} = 0,\quad \forall i = \overline{1,n}$
$$
G \neq O_n ,\quad  \exists \;g_{ij} \neq 0, \;i \neq j
$$
Fie schimbarea de reper:
$$
\begin{cases}
y_i = x_i + x_j\\
y_j = x_i - x_j \\
y_k = x_k, \; \forall\,k\in\{1, \dots, n\} \setminus \{i, j\}
\end{cases}

\implies 

\begin{cases}
x_i = \frac{1}{2}(y_i + y_j)\\
x_j = \frac{1}{2}(y_i - y_j)\\
x_k = y_k, \; \forall\,k\in\{1, \dots, n\} \setminus \{i, j\}

\end{cases}
$$
Deci, fiindcă există doar $0$ pe diagonală: 
$$
Q(x) = 2\sum_{i < j}g_{ij}x_ix_j
$$
Iar termenul, după schimbarea de reper, este:
$$
2g_{ij}x_iy_j = 2g_{ij}\frac{1}{4}(y_i^2 - y_j^2) = \frac{1}{2}g_{ij}y_i^2 - \frac{1}{2}g_{ij}y_h^2
$$
Deci se reduce la cazul b)
	
Așadar, putem considera $g_{11} \neq 0$

Demonstrăm prin inducție după numărul coordonatelor lui $x$ care apar în $Q$ 
$$
P_1: \qquad Q(x) = g_{11}x_1^2 \quad \text{  - forma canonica}
$$
Presupunem adevărat $P_{k-1}$: $Q$ formă pătratică care conține coordonatele $x_1, \dots, x_{k-1}$ $\implies$ $\exists$  un reper în $V$ astfel încât $Q$ are o formă canonică. 

Demonstrăm $P_{k-1} \implies P_{k}$

Fie $Q$ formă pătratică care conține coordonatele $x_1, \dots, x_{k}$. Demonstrăm că $\exists$ un reper în V astfel încât $Q$ are o formă canonică. 

$$
Q(x) = g_{11}x_1^2 + 2g_{12}x_1x_2 + \dots + 2g_{1k}x_1x_{k} + Q^{'}(x)
$$
Unde $Q^{'}(x)$ conține doar $x_2, \dots, x_k$ 
$$
Q(x) = \frac{1}{g_{11}}(x_1^2 + 2g_{11}g_{12}x_1x_2 + \dots + 2g_{11}g_{1k}x_1x_{k}) + Q^{'}(x)
$$
Care devine: 
$$
Q(x) = \frac{1}{g_{11}}(g_{11}x_1 + \dots +g_{1k}x_k)^2 + Q^{''}(x)
$$
Unde $Q^{''}(x)$ conține doar $x_2, \dots, x_k$ 

Fie schimbarea de reper: 
$$
\begin{cases}
y_1 = g_{11}x_1 + \dots +g_{1k}x_k\\
y_i = x_i, \quad \forall \, i = \overline{1,n}

\end{cases}
$$
Deci: 
$$
Q(x) = \frac{1}{g_{11}}y_1^2 + Q^{''}(x)
$$
Dar $Q^{''}(x)$ conține $k-1$ coordonate ale lui $x$. Aplicăm $P_{k-1}$: 
$$
\exists \; \text{ un reper} \subset V \text{ astfel incat } Q^{''}(x) = a_2z_2^2 + \dots a_kzk^2
$$
Not $y_1 = z_1$, $a_1 = \frac{1}{g_{11}}$ 

Deci Q devine: 
$$
Q(x) = a_1z_1^2 + a_2z_2^2 + \dots a_kz_k^2
$$
$$
\begin{cases}
P_1 \\
P_{k-1} \implies P_k
\end{cases}
\implies 
P_n \quad \forall \,n \in \mathbb{N}
$$
Q.E.D

>[!definition]

