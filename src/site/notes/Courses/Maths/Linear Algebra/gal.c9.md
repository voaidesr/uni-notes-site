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
$$
0 = \langle e_k, e_j \rangle = \langle f_k ,e_j \rangle + \sum_{i=1}^{k-1}\alpha_{ki}\langle e_i, e_j \rangle
$$
Din $P_{k-1}$ știm că $\langle e_i, e_j \rangle = \delta_{ij}$ (sunt ortogonali).
Relația devine:
$$
0 = \langle f_k ,e_j \rangle + \alpha_{kj}\langle e_j, e_j \rangle \implies
\alpha_{kj} = - \frac{\langle f_k ,e_j \rangle}{\langle e_j, e_j \rangle}
$$
Deci 
$$
e_k = f_k - \sum_{j=1}^{k-1}\frac{\langle f_k ,e_j \rangle}{\langle e_j, e_j \rangle}e_j
$$
$$
\begin{aligned}
\begin{cases}
f_1 = e_1 \\
\vdots\\
f_k = \sum_{j=1}^{k-1} \frac{\langle f_k, e_j \rangle}{\langle e_j, e_j \rangle} e_j + e_k
\end{cases}
\quad &\implies \quad
\mathrm{Sp}\{f_1, \dots, f_k\} = \mathrm{Sp}\{e_1, \dots, e_k\}
\;\;\forall k=\overline{1,k}
\end{aligned}
$$

Deci 
$$
\begin{cases}
f_1 = e_1 \\
f_2 = \frac{\langle f_2, e_1 \rangle}{\langle e_1, e_1 \rangle} e_1 + e_2 \\
\vdots \\
f_n = \frac{\langle f_n, e_1 \rangle}{\langle e_1, e_1 \rangle} e_1 + \dots + \frac{\langle f_n, e_{n-1} \rangle}{\langle e_{n-1}, e_{n-1} \rangle} e_{n-1} + e_n
\end{cases}
\quad
A^{-1} = 
\begin{pmatrix}
1 & \frac{\langle f_2, e_1 \rangle}{\langle e_1, e_1 \rangle} & \cdots & \frac{\langle f_n, e_1 \rangle}{\langle e_1, e_1 \rangle} \\
0 & 1 & \cdots & \frac{\langle f_n, e_2 \rangle}{\langle e_2, e_2 \rangle} \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1
\end{pmatrix}
$$
$A$ este matricea de trecere. $\operatorname{det} A^{-1} = 1$ 

Avem $\mathcal{R}' = \{e_1, \dots, e_n \}$ sistem de vectori ortogonali (doi câte doi). Este și sistem de generatori. 
$$
|\mathcal{R}'| = n = \operatorname{dim} V \implies \mathcal{R'} \quad\text{reper ortogonal}
$$
Schimbările de reper în Gram - Schmidt:
$$
\underbrace{\mathcal{R} = \{f_1,\dots,f_n\}}_{\text{bază inițială}}
\;\xrightarrow{A}\;
\underbrace{\mathcal{R}' = \{e_1,\dots,e_n\}}_{\text{bază ortogonală}}
\;\xrightarrow{B}\;
\underbrace{\mathcal{R}'' = \Bigl\{\tfrac{e_1}{\|e_1\|},\dots,\tfrac{e_n}{\|e_n\|}\Bigr\}}_{\text{bază ortonormală}}
$$
***Sunt la fel orientate.***

***Observație***

$v \neq 0_v$, atunci $\frac{v}{\|v\|}$ este versor. 
$$
g(\frac{v}{\|v\|},\frac{v}{\|v\|}) = \frac{1}{\|v\|^2}g(v,v) = \frac{\|v\|^2}{\|v\|^2} = 1
$$
## Complement ortogonal 

$(V, g)$ s.v.e.r.,    $x\in V \setminus \{ 0_V \}$,    $U\subseteq V$ subspațiu vectorial.

1. $x^{\perp} = \{y \in V \, | \, g(x,y) = 0 \} \subseteq V$ subspațiu vectorial (toți vectorii perpendiculari pe x = complementul ortogonal al lui x)
2. $U^{\perp} = \{y \in V \, | \, g(x, y) = 0 \; \forall x \in U\} \subseteq V$ subspațiu vectorial

Proprietate: 

Pentru $U \subseteq V$ subspațiu vectorial $\exists_{!} \; U^{\perp} \subseteq V$ subspațiu vectorial (complement ortogonal) astfel încât: 
$$
V = U \oplus U^{\perp}
$$
*Demonstrație*
1. $\subseteq$
Din definiție: 
$$
U + U^{\perp} \subseteq V \implies U \oplus U^{\perp} \subseteq V 
$$
Demonstrăm $\oplus$ 

Fie $x \in U \cap U^{\perp} \implies g(x,x) = 0 \implies x = 0_V$ 

2. $\supseteq$ 
Demonstrăm că $V \subseteq U \oplus U^{\perp}$ 
Fie $v \in V$ și $\mathcal{R} = \{e_1, \dots, e_k\}$ un reper ortonormat în $U$. 
Fie 
$$
v' = v - \underbrace{ 
\sum_{i=1}^k \langle v, e_i \rangle e_i
}_{\text{not. } v''\;\in \;U}
$$
Demonstrăm că $v' \in U_{\perp}$ 

$$
\begin{aligned}
&\langle v',e_1\rangle = \langle v, e_1\rangle - 
\underbrace{
\sum_{i=1}^k\langle v, e_i \rangle \underbrace{\langle e_i, e_1 \rangle}_{\delta_{1i}}
}_{\langle v_, e_1 \rangle} = 0\\

&\vdots\\

&
\langle v',e_k\rangle = \langle v, e_k\rangle - 
\underbrace{
\sum_{i=1}^k\langle v, e_i \rangle \underbrace{\langle e_i, e_k \rangle}_{\delta_{ki}}
}_{\langle v, e_k \rangle} = 0
\end{aligned}
$$
Deci $v' \in U^{\perp}$ 

Și $v$ se poate descompune: 
$$
v = \underset{\in \;U}{u''} + \underset{\in \; U^{\perp}}{u'}
$$
Așadar, din 1, 2 $\implies$ $V = U \oplus U^{\perp}$ 

## Exercițiu 1 

$(\mathbb{R}^3, g_0)$ s.v.e.r, $u = (1, 2, -1)$ $\implies$ $\|u\| = \sqrt{6}$ 

a) $u^{\perp}$
b) determinați un reper ortonormat în $u^{\perp}$ 

*Soluție*
a) 
$$
\begin{aligned}
u^{\perp} = \{x \in \mathbb{R}^3 \, | \,
\underbrace{
g_0(x,u)
}_{x_1 + 2x_2 -x_3} = 0 
\} &= \{(x_1, x_2, x_1 + 2x_2) \, | \, x_1, x_2 \in \mathbb{R}\} = \\
& = \{x_1\underbrace{(1, 0, 1)}_{f_1} + x_2\underbrace{(0, 1, 2)}_{f_2}\, | \, x_1, x_2 \in \mathbb{R} \} = \\
& = \langle \{f_1, f_2 \} \rangle
\end{aligned}
$$
Deci $\operatorname{dim} u^{\perp} = 3 - 1 = 2$ 
$\mathcal{R} = \{f_1, f_2 \}$ reper oarecare în $u^{\perp}$.  Trebuie ortonormat, deci aplicăm G-S. 
$$
\begin{aligned}
& e_1 = f_1 = (1, 0, 1) \qquad \|e_1\| = \sqrt{2}\\\\
& e_2 = f_2 - \frac{\langle f_2, e_1 \rangle}{\langle e_1, e_1 \rangle}e_1 = (0, 1, 2) - \frac{2}{2}(1, 0, 1) = (-1, 1, 1) \qquad \|e_2\| = \sqrt{3}
\end{aligned}
$$
Noile repere: 
$$
\mathcal{R}' = \{e_1, e_2 \} \quad \text{ortogonal} \implies 
\mathcal{R}'' = \{\frac{1}{\sqrt{2}}(1,0,1), \frac{1}{\sqrt{3}}(-1, 1, 1) \} \;
\;\;\text{ortonormal}
$$
Avem: 
$$
\mathbb{R}^3 = u \oplus u^{\perp}
$$
Deci avem următorul reper ortonormat în $\mathbb{R}^3$
$$
\mathcal{R} = \{\frac{1}{\sqrt{6}}(1, 2, -1),\frac{1}{\sqrt{2}}(1,0,1), \frac{1}{\sqrt{3}}(-1, 1, 1) \}
$$
## Exercițiu 2 

$(\mathbb{R}^3, g_0)$ s.v.e.r 

$U = \{x \in \mathbb{R}^3 \, | \, x_1 - x_2 + x_3 = 0 \} \implies x_2 = x_1 + x_3$ 

a) $U^{\perp}$
b) Să se determine $\mathcal{R} = \underbrace{\mathcal{R}_1}_{\text{reper } U^{\perp}} \cup \underbrace{\mathcal{R}_2}_{\text{reper } U}$ reper ortonormat în $\mathbb{R}^3$. 
*Soluție*

a) Dacă luăm coeficienții ecuației planului descris în $U$, obținem. [[Courses/Maths/Linear Algebra/gal.c11\|vezi cursul de geometrie analitica]]
$$
U^{\perp} = \langle \{(1, -1, 1) \} \rangle
$$
Avem reper ortonormat în $U^{\perp}$
$$
\mathcal{R}_1 = \left\{ \frac{e_1}{\|e_1\|} = \frac{1}{\sqrt{3}}(1, -1, 1) \right\}
$$
$$
U = \left\{(x_1, x_1 + x_3, x_3) \, | \, x_1, x_3 \in \mathbb{R} \right\} = \langle \{
\underbrace{(1, 1, 0)}_{f_2}, 
\underbrace{(0, 1, 1)}_{f_3} \} \rangle
$$
Aplicăm G-S
$$
\begin{aligned}
&e_2 = f_2 = (1, 1, 0) \qquad \frac{e_2}{\|e_2\|} = \frac{1}{\sqrt{2}}(1, 1, 0)\\\\
&e_3 = f_3 - \frac{ 
\langle f_3, e_2 \rangle
}{
\langle e_2, e_2 \rangle
}e_2 = \frac{1}{2}(-1, 1, 2) \qquad \frac{e_3}{\|e_3\|} = \frac{1}{\sqrt{6}}(-1, 1, 2)
\end{aligned}
$$
Avem reper ortonormat în $U$
$$
\mathcal{R}_2 = \left\{\frac{1}{\sqrt{2}}(1, 1, 0), \frac{1}{\sqrt{6}}(-1, 1, 2) \right\}
$$
***Observație***
$v = \alpha u, \quad \alpha > 0 \qquad \frac{v}{\|v\|} = \frac{u}{\|u\|}$ 