---
{"dg-publish":true,"permalink":"/courses/maths/linear-algebra/gal-c9/"}
---

# Spații vectoriale euclidiene

## Produs scalar

>[!definition] Produs scalar
>Fie $(V, +, \cdot)|_{\mathbb{R}}$ spațiu vectorial real
>$g : V \times V \to \mathbb{R}$ se numește produs scalar $\iff$ 
>- $g \in L^s(V, V; \mathbb{R})$ (formă biliniară și simetrică) vezi [[Courses/Maths/Linear Algebra/gal.c8#Forme biliniare\|aici]]
>- $g$ este [[Courses/Maths/Linear Algebra/gal.c8#Forme pozitiv definite\|pozitiv definita]]

>[!Notation]
>- $(V,g)$, $(E,g)$, $(E, (\cdot, \cdot))$, $(E, \langle \cdot, \cdot \rangle)$ = spațiu vectorial euclidian real (s.v.e.r) 
>- $\|x\| \overset{\text{def}}{=} \sqrt{\langle x, x \rangle}$ - norma vectorului 

Matricea asociată lui $g$ funcționează la fel ca matricea asociată unei [[Courses/Maths/Linear Algebra/gal.c8#Matricea asociată unei forme biliniare\|forme biliniare simetrice]]

## Repere ortogonale și ortonormate

>[!definition] Reper ortogonal, reper ortonormat
>$(V, g)$ s.v.e.r. și $\mathcal{R} = \{e_1, \dots, e_n \}$ reper în $V$
>- $\mathcal{R}$ s.n. **reper ortogonal** $\iff g(e_i, e_j) = 0, \quad \forall \, i \neq j$
>-  $\mathcal{R}$ s.n. **reper ortonormat** $\iff g(e_i, e_j) = \delta_{ij}$ 
>Unde $\delta_{ij} = \begin{cases} 1, \quad i = j\\ 0, \quad i \neq j \end{cases}$ --> simbolul Kronecker
>
>Bază ortonormată $\iff$ vectori ortogonali doi câte doi și versori (adică de normă 1). 

***Proprietate***

Fie $(V, g)$ s.v.e.r
Atunci, pentru două repere ortonormate și $C$ matricea de trecere.
$$
\mathcal{R} = \{e_1, \dots, e_n\} \overset{C}{\to} \mathcal{R'} = \{e_1', \dots, e_n'\}
$$
Avem: 
$$
C \in \operatorname{O}(n) \iff CC^{\top} = I_n \qquad \text{matrice ortogonala}
$$
*Demonstrație*
$$
\begin{aligned}
\delta_{kl} = g(e_k', e_l') &= g(\sum_{i=1}^nc_{ik}e_i,\sum_{j=1}^nc_{jl}e_j) =\\\\
&= \sum_{i,j = 1}^nc_{ik}c_{jl}g(e_i, e_j) = \\\\
&= \sum_{i,j = 1}^nc_{ik}c_{jl}\delta_{ij} = \\\\
&= \sum_{i = 1}^nc_{ik}c_{jl} \implies C^{\top}C = I_n \implies\\\\
&\implies C \in \operatorname{O}(n) \quad \text{Q.E.D.}
\end{aligned} 
$$
***Observație***
$\mathcal{R} \overset{C}{\to} \mathcal{R'}$ repere ortonormate la fel orientate ($\operatorname{det} C > 0$) $\implies$ $C \in \operatorname{SO}(n)$ 

$\operatorname{SO}(n)=\{A \in \operatorname{O}(n) \, | \operatorname{det}A = 1 \}$ - matricile special ortogonale

```ad-tip 
***Propoziție***
A da un produs scalar $\iff$ A preciza un reper ortonormat.
```

*Demonstrație*

1. $\implies$
$g: V \times V \to \mathbb{R}$ produs scalar, $\mathcal{R} = \{e_1, \dots, e_n \}$ reper

$\mathcal{R}$ poate fi ales astfel încât relația $g(e_i,e_j) = \delta_{ij}, \quad \forall\,i,j = \overline{1,n}$  să fie respectată. 
Atunci $\mathcal{R}$ este reper ortonormat. 

2. $\impliedby$ 
$\mathcal{R} = \{e_1, \dots, e_n \}$ reper ortonormat. Definim o funcție $g: V \times V \to \mathbb{R}$, astfel încât:
$$
g(e_i,e_j) = \delta_{ij}, \quad \forall\,i,j = \overline{1,n}
$$
$g$ -produs scalar 
Extindem prin liniaritate
$$
g(x,y) = \sum_{i,j = 1}^ng_{ij}x_iy_j= \sum_{i=1}^nx_iy_i = x_1y_1 + \dots + x_ny_n
$$
Pentru că $g_{ij} = \delta_{ij}$ 

### Exemplu

$g_0 : \mathbb{R}^n \times \mathbb{R}^n \to \mathbb{R}, \quad g_0(x,y) = x_1y_1 + \dots  x_ny_n$  Produsul scalar **canonic**, $\mathcal{R}_0$ reperul canonic. (1)

Avem
$$
g_0(e_i, e_j) = 0
$$
Deci $\mathcal{R}_0$ reper ortonormat *în raport cu* $g$. 

$G = I_n$ - matricea asociată lui $g$ în raport cu $\mathcal{R}_0$ $\implies G = G^{\top}$ (2)

(1), (2) $\implies$ $g_0 \in L^s(\mathbb{R}^n,\mathbb{R}^n;\mathbb{R})$ 
$$
g_0(x,x) = Q(x) = x_1^2 + \dots + x_n^2  

\implies 

\begin{cases}
g_0(x,x) > 0, \quad \forall \, x \neq 0_{\mathbb{R}^n}\\
g_0(x,x) = 0 \iff x = 0_{\mathbb{R}^n}
\end{cases}
$$
Deci $g_0$ e pozitiv definită. 

## Produs vectorial 

$(\mathbb{R}^3, g_0)$ s.v.e.r cu str. canonică. Unde $g_0(x,y) = x_1y_1 + x_2y_2 + x_3y_3$

Definim **produsul vectorial** $x \times y$ astfel: 
1) Dacă $\{x, y \}$ SLD, atunci $x \times y = 0_{\mathbb{R}^3}$
2) Dacă $\{x, y \}$ SLI, atunci avem: 

a) 
$$
\| x \times y \| = 
\begin{vmatrix}
g_0(x,x) & g_0(x,y)\\
g_0(y,x) & g_0(y,y)
\end{vmatrix} =
\|x\|^2\cdot\|y\|^2-g_0^2(x,y) 
$$
b) 
$$
g_0(x\times y, x) = g_0(x\times y, y) = 0
$$
c) $\mathcal{R} = \{ x, y, x \times y\}$ reper pozitiv orientat în $\mathbb{R}^3$ (adică la fel orientat cu $\mathcal{R}_0$)

***Observație***
$\times$ este un *determinant formal*  -> format din vectori și scalari, întoarce vector.
$$
\begin{aligned}
x \times y = 
\begin{vmatrix}
e_1 & e_2 & e_3\\
x_1 & x_2 & x_3\\
y_1 & y_2 & y_3
\end{vmatrix}
= &e_1
\begin{vmatrix}
x_2 & x_3\\
y_2 & y_3
\end{vmatrix}
- e_2 
\begin{vmatrix} 
x_1 & x_3\\
y_1 & y_3
\end{vmatrix}
+ e_3
\begin{vmatrix}
x_1 & x_2\\
y_1 & y_2
\end{vmatrix}\\\\
= e_1\Delta_1 - e_2\Delta_2 + e_3\Delta_3
\end{aligned}
$$

Deci 
$$
x \times y = (\Delta_1, -\Delta_2, \Delta_3)
$$
>[!proof] Proprietăți 
>- $x \times y = - y \times x$ 
>- $(x\times y)\times z = g_0(x, z)y - g_0(y, z)x$
>- Identitate Jacobi: $\sum_{x, y, z}^{cyc}(x\times y)z$

## Produsul mixt 

Fie $(\mathbb{R}^3, g_0)$ s.v.e.r cu struct. canonică și $x, y, z \in \mathbb{R}^3$ 
$$
\begin{aligned} 
z \land x \land y \overset{\text{def.}}{=} g_0(z, x \times y) &=
\begin{vmatrix}
z_1 & z_2 & z_3\\
x_1 & x_2 & x_3\\
y_1 & y_2 & y_3
\end{vmatrix} = \\\\
&= \begin{vmatrix}
x_1 & x_2 & x_3\\
y_1 & y_2 & y_3\\
z_1 & z_2 & z_3
\end{vmatrix}
= x \land y \land z
\end{aligned}
$$
### Exemplu 
Fie $(\mathbb{R}^3,g_0)$ s.v.e.r,  
$\mu = (1, -1, 2)$,   $\nu = (0, 1, 3)$,   $\omega = (1, 1, 0)$.

a) $\mu \times \nu$  
b) $\omega \wedge \mu \wedge \nu$

 *Soluție*
a) Calculăm $\mu \times \nu$:

$$
\mu \times \nu = 
\begin{vmatrix}
e_1 & e_2 & e_3 \\
1 & -1 & 2 \\
0 & 1 & 3 \\
\end{vmatrix} 
= \left( \begin{vmatrix} -1 & 2 \\ 1 & 3 \end{vmatrix}, - \begin{vmatrix} 1 & 2 \\ 0 & 3 \end{vmatrix}, \begin{vmatrix} 1 & -1 \\ 0 & 1 \end{vmatrix} \right)
= (-5, -3, 1)
$$
b) Calculăm $\omega \wedge \mu \wedge \nu = g_0(\omega, \mu \times \nu)$:

$$
g_0(\omega, \mu \times \nu) = -5 \cdot 1 - 3 \cdot 1 + 0 = -8
$$
*Observații*
a) 
$$
g_0(\mu \times \nu, \mu) = 0
$$
deoarece
$$
(-5, -3, 1) \cdot (1, -1, 2) = -5 \cdot 1 + (-3)(-1) + 1 \cdot 2 = -5 + 3 + 2 = 0
$$
b)
$$
g_0(\mu \times \nu, \nu) = 0
$$
deoarece

$$
(-5, -3, 1) \cdot (0, 1, 3) = -5 \cdot 0 + (-3) \cdot 1 + 1 \cdot 3 = 0
$$
***Reper ortonormat***
$$
\mathcal{R} = \{ \mu, \nu, \mu \times \nu \}
$$
și
$$
\mathcal{R}_0 = \{ e_1, e_2, e_3 \} \xrightarrow{C} \mathcal{R} = \{ \mu, \nu, \mu \times \nu \}
$$
unde matricea de trecere $C$ este:

$$
C = 
\begin{pmatrix}
1 & 0 & -5 \\
-1 & 1 & -3 \\
2 & 3 & 1 \\
\end{pmatrix}
$$
Calculul determinantului:
$$
\det C = 
\begin{vmatrix}
1 & 0 & -5 \\
-1 & 1 & -3 \\
2 & 3 & 1 \\
\end{vmatrix} = 11 + 24 = 35
$$

$$
\|\mu \times \nu\| = \sqrt{g_0(\mu \times \nu, \mu \times \nu)} = \sqrt{25 + 9 + 1} = \sqrt{35}
$$
## Ortonormarea unui reper oarecare

***Problemă***:

$(V, g_0)$ s.v.e.r. $\mathcal{R} = \{e_1, \dots, e_n\}$ reper arbitrar. Există $\mathcal{R}'$ reper ortonormat? 

### Procedeul Gram - Schmidt

$(V, g_0)$ s.v.e.r. Cu produsul scalar canonic $g_0  = (\cdot, \cdot) = \langle \cdot, \cdot \rangle$  

fie $\mathcal{R} = \{f_1, \dots, f_n \}$ reper arbitrat $\implies \exists  \, \mathcal{R}' = \{e_1, \dots, e_n \}$ reper ortogonal astfel încât
$$
\operatorname{Sp}\{e_1, \dots, e_n \} = \operatorname{Sp}\{f_1, \dots, f_n \}
$$
(Sp = spațiul generat de) 

*Demonstrație*

Abordarea este inductivă. 
$$
f_1 \neq 0_V \quad \text{alegem} \quad e_1 = f_1
$$
Fie: 
$$
e_2 = f_2 + \alpha_{21}e_1
$$
Din condiția de ortogonalitate
$$
\begin{aligned}
\langle e_1, e_2 \rangle = 0 &\implies \langle f_2 + \alpha_{21}e_1, e_1 \rangle = 0 \implies\\
& \implies \langle f_2, e_1 \rangle + \alpha_{21}\langle e_1, e_1 \rangle = 0
\end{aligned}
$$
De unde rezultă
$$
\alpha_{21} = - \frac{\langle f_2, e_1 \rangle}{\langle e_1, e_1 \rangle}
$$
Și
$$
e_2 = f_2 - \frac{\langle f_2, e_1 \rangle}{\langle e_1, e_1 \rangle}
$$
Deci $P_2$:
$$
\begin{cases}
f_1 = e_1 \\
f_2 = \frac{\langle f_2, e_1 \rangle}{\langle e_1, e_1 \rangle}e_1 + e_2
\end{cases}

\implies 

\operatorname{Sp}\{f_1, f_2\} = \operatorname{Sp}\{e_1, e_2 \}
$$
Presupunem $P_{k-1}$ adevărat 
$$
\{e_1, \dots, e_{k-1} \} \quad \text{vectori ortogonali, si} \quad \operatorname{Sp}\{f_1, \dots, f_{k-1} \} = \operatorname{Sp}\{e_1, \dots, e_{k-1} \}
$$

Avem:
$$
e_k = f_k + \sum_{i = 1}^{k-1}\alpha_{ki}e_i
$$
Condiția de ortogonalitate: 