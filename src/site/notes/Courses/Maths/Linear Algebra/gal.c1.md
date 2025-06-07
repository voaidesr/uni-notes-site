---
{"dg-publish":true,"permalink":"/courses/maths/linear-algebra/gal-c1/"}
---

# Matrice. Determinanți. Rang. Forma eșalon

Fie $(\mathbb{K}, +, \cdot)$ un corp comutativ.

## Determinantul 

### *Definiție*

$\operatorname{det} : \mathcal{M}_n(\mathbb{K}) \to \mathbb{K}$, $\quad A = (a_{ij})_{i,j = \overline{1,n}}$ o matrice.

Se numește determinantul matricii $A$:

$$
\operatorname{det} A = \sum_{\sigma \in S_n}\varepsilon(\sigma)\,a_{1\sigma(1)}\,a_{2\sigma(2)}\dots \,a_{n\sigma(n)}
$$
Unde $(S_n, \cdot)$ grupul permutărilor. $|S_n| = n!$ 

$$
\sigma : \{1, 2, \dots, n \} \to \{1, 2, \dots, n \} \quad \text{ o bijectie}
$$
$$
\sigma = 
\begin{pmatrix}
1 & 2 & \dots  & n\\
\sigma(1) & \sigma(2) & \dots & \sigma(n)
\end{pmatrix}
$$
Iar unde signatura este $\varepsilon(\sigma) = (-1)^{m(\sigma)}$ , iar $m(\sigma)$ = numărul inversiunilor. 

>[!note] Reminder
>$(i,j)$ se numește inversiune a lui $\sigma \iff \begin{cases} i < j \\ \sigma(i) > \sigma(j) \end{cases}$ 

### Matrice singulară, nesingulară

*Definiție*

$A \in \mathcal{M}_n(\mathbb{K})$ o matrice 
- $A$ se numește **nesingulară** $\iff \operatorname{det} A \neq 0$
- $A$ se numește **singulară** $\iff \operatorname{det} A = 0$

*Proprietate*
$A$ nesingulară $\iff$ $A$ inversabilă.

### Proprietăți ale determinantului

- $\operatorname{det}(AB) = \operatorname{det} A \cdot \operatorname{det} B$

- $\operatorname{det}(A^{\top}) = \operatorname{det} A$, unde $A^{\top} = (b_{ij})_{i,j = \overline{1,n}} \quad b_{ij}  = a_{ji}$

- $\operatorname{Tr}(A + B) = \operatorname{Tr}A + \operatorname{Tr} B$     Unde $\operatorname{Tr} A = \sum_{i=1}^n a_{ii}$ **urma** matricii $A$

- $\operatorname{Tr}(AB) = \operatorname{Tr}(BA)$

- $\operatorname{Tr}(\alpha A) = \alpha \operatorname{Tr}A$

- $\operatorname{det}A^{-1} = \frac{1}{\operatorname{det} A}$

Motivație:
$$
\begin{aligned}
\operatorname{det} (AA^{-1}) = \operatorname{det}(I_n) = 1 \iff
\operatorname{det}(A) \operatorname{det}(A^{-1}) = 1
\end{aligned}
$$
- $\det{A^*} = \det{A}^{n-1}$  

*Observație* - inversarea unei matrici
$$
A \to A^{\top} \to A^* \to A^{-1} = \frac{1}{\det{A}}A^*
$$
Unde $A^*$
$$
A^*_{ij} = (-1)^{i+j}\, \delta_{ij} \qquad \delta_{ij} = \text{ minorul}
$$
## Grupuri de matrici 

- $\operatorname{GL}(n, \mathbb{K}) = \{A \in \mathcal{M}_n(\mathbb{K}) \,|\, \det{A} \neq 0 \}$ **Grupul General Liniar**

- $\operatorname{SL}(n, \mathbb{K}) = \{A \in \operatorname{GL}(n, \mathbb{K}) \,|\, \det{A} = 1) \}$ **Grupul Special Liniar** 

- $\operatorname{O}(n, \mathbb{K}) = \{A \in \mathcal{M}_n(\mathbb{K}) \,|\, A \cdot A^{\top} = I_n \}$ **Grupul Ortogonal** ($\det{A} = \pm 1$)

- $\operatorname{SO}(n, \mathbb{K}) = \{A \in \operatorname{O}(n, \mathbb{K}) \,|\, \det{A} = 1) \}$ **Grupul Special Ortogonal**

***Observație***
$$
\operatorname{SO}(n, \mathbb{K})  =  \operatorname{SL}(n, \mathbb{K})\; \cap \;\operatorname{GL}(n, \mathbb{K})
$$
## Rang

Pentru o matrice $A \in \mathcal{M}_{m,n}(\mathbb{K})$ ($A \neq O_{m,n}$), rangul este $k$ $\iff$ $\exists$ un minor ordin $k$ nenul, iar toți minorii de ordin mai mare (dacă există) sunt nuli.

Notăm
$$
\operatorname{rg}A = k
$$
Convenție: $\operatorname{rg} O_{m,n} = 0$ 

*Observație*

Există $C_m^{k+1} \cdot C_n^{k+1}$ minori de ordin $k+1$ pentru $A$.

***Teorema***

$\operatorname{rg} A = k \iff$ Există un minor de ordin $k$ nenul și toți minorii de ordin $k+1$ care îl conțin pe $\Delta_k$ (dacă există) sunt nuli.

*Observație*

Există $(m-k)(n-k)$ minori de ordinul $k+1$ care îl conțin pe $\Delta_k$. (am optimizat algoritmul)

### Exercițiul 1

Fie matricea $A \in M_3(\mathbb{R})$ definită prin

$$
A = \begin{pmatrix}
 a & 1 & 1 \\
 1 & a & 1 \\
 1 & 1 & a
\end{pmatrix}.
$$

Determinați $\operatorname{rang}A$ în funcție de $a \in \mathbb{R}$.

**Soluție**

>[!tip]
>$A$ e pătratică $\implies$ mare $\to$ mic.

Calculăm determinantul:

$$
\Delta = \det(A)
= \begin{vmatrix}
 a & 1 & 1 \\
 1 & a & 1 \\
 1 & 1 & a
\end{vmatrix}.
$$

Adunăm liniile:

$$
L_1' = L_1 + L_2 + L_3
\quad \Rightarrow \quad
\Delta = \begin{vmatrix}
 a+2 & a+2 & a+2 \\
 1 & a & 1 \\
 1 & 1 & a
\end{vmatrix}
= (a+2)
\begin{vmatrix}
 1 & 1 & 1 \\
 1 & a & 1 \\
 1 & 1 & a
\end{vmatrix}.
$$
Scădem prima linie din celelalte două:

$$
L_2' = L_2 - L_1,\quad L_3' = L_3 - L_1
\quad \Rightarrow \quad
\begin{vmatrix}
 1 & 1 & 1 \\
 0 & a-1 & 0 \\
 0 & 0 & a-1
\end{vmatrix}
= (a-1)^2.
$$

Prin urmare:

$$
\Delta = (a+2)(a-1)^2.
$$

>[!note] Determinantul se anulează pentru $a = -2$ și $a = 1$.

Din această expresie deducem:

* Dacă $a \neq -2$ și $a \neq 1$, atunci $\Delta \neq 0$, deci $\operatorname{rang} A = 3$.
* Dacă $a = 1$, atunci $A = \begin{pmatrix} 1 & 1 & 1\\1&1&1\\1&1&1\end{pmatrix}$, deci $\operatorname{rang} A = 1$.
* Dacă $a = -2$, se verifică că $\operatorname{rang} A = 2$.

Astfel:

$$
\operatorname{rang} A =
\begin{cases}
 3, & a \neq -2,\,1,\\
 1, & a = 1,\\
 2, & a = -2.
\end{cases}
$$
### Exercițiul 2

>[!tip]
>$A$ nu este pătratică $\implies$ mix $\to$ mare. 

Fie $A$, aflați rangul
$$
A = \begin{pmatrix}
1 & 1 &a & 1\\
0 & 1 & -1 &2\\
6 & 4&8&3
\end{pmatrix}
$$
*Soluție*

$$
\begin{aligned}
&\Delta_1 =  1 \neq 0\\\\
&\Delta_2 = \begin{vmatrix} 
			1 & 1\\
			0 & -1
			\end{vmatrix} = -1 \neq 0\\\\
&\Delta_3 = 
\begin{vmatrix}
1&1&1\\
0&1&2\\
6&4&3
\end{vmatrix} \overset{l_3'\; = \; l_3 - 6l_1}{=} 
\begin{vmatrix}
1&1&1\\
0&1&2\\
0&-2&-3
\end{vmatrix} = 
\begin{vmatrix}
1 & 2 \\
-2 & -3
\end{vmatrix} = 1 \neq 0
\end{aligned} 
$$
Deci $\operatorname{rang} A = 3$.
### Exercițiul 3: Rangul matricilor care satisfac o ecuație polinomială

Fie $A \in M_n(\mathbb{R})$ astfel încât

$$
A^3 - A - I_n = 0_n.
$$

**a)** Determinați $\operatorname{rang}A$.
**b)** Determinați $\operatorname{rang}(A + I_n)$.

**Soluție**

**a)** Din ecuație:

$$
A(A^2 - I_n) = I_n
\quad\Longrightarrow\quad
\det(A)\cdot\det(A^2 - I_n) = \det(I_n) = 1
\;
\Longrightarrow\;
\det(A) \neq 0,
$$

deci $A$ este inversibilă și

$$
\operatorname{rang}A = n.
$$

**b)** Observăm că

$$
(A - I_n)(A + I_n) = A^2 - I_n
\quad\Longrightarrow\quad
A(A - I_n)(A + I_n) = I_n,
$$

de unde, aplicând determinantul,

$$
\det(A)\cdot\det(A - I_n)\cdot\det(A + I_n) = 1
\;\Longrightarrow\;
\det(A + I_n) \neq 0,
$$

astfel și $A + I_n$ este inversibilă, deci

$$
\operatorname{rang}(A + I_n) = n.
$$
