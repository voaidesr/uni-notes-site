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

