---
{"dg-publish":true,"permalink":"/courses/maths/linear-algebra/curs-7/"}
---


[[Courses/Maths/Linear Algebra/Curs 7#Proiecții și Simetrii proiecții simetrii\|Proiecții și simetrii]]
[[Courses/Maths/Linear Algebra/Curs 7#Vectori proprii. Valori proprii. Diagonalizarea\|Vectori proprii, Valori proprii, Diagonalizarea]]

### Proiecții și Simetrii #proiecții #simetrii

Fie un spațiu vectorial $(V, +,  \cdot)|_{\mathbb{K}}$,  astfel încât $V = V_1 \oplus V_2$.

O **proiecție** pe $V_1$ este o funcție $p:V1 \oplus V2 \to V1$:
- $p \in End(V)$
- $p$ proiecție $\Leftrightarrow p \circ p = p$
- $p(x_1 + x_2) = x_1$ proiecția față de $V_1$

O **simetrie** față de $V_2$ este o funcție $s:V1 \oplus V2 \to V1$:
- $s \in End(V)$
- $s$ proiecție $\Leftrightarrow s \circ s = id_V$
- $p$ proiecție față de $V_1$ $\Leftrightarrow s = 2p - id_V$
- $s(x_1 + x_2) = x_1 - x_2$ simetria față de $V_1$

```ad-note
Fie:
$
\mathcal{R} = \mathcal{R}_1 \cup \mathcal{R}_2 \quad \text{reper în } V \quad \mathcal{R}_i \text{ este reper în } V_i
$
$
\mathcal{R}_1 = \{ e_1, ... , e_k \},
\quad \mathcal{R}_2 = \{ e_{k+1}, ... , e_n \} \quad \dim V = n
$

$
p(e_i) = e_i, \quad \forall i = \overline{1,k}
\qquad
p(e_j) = 0, \quad \forall j = \overline{k+1,n}
$

$
[p]_{\mathcal{R}, \mathcal{R}} = A_p =
\begin{pmatrix}
I_k & O_{k(n-k)} \\
O_{(n-k)k} & O_{(n-k)(n-k)}
\end{pmatrix}
\in \mathcal{M}_n(\mathbb{K})
$
```


#vectori_proprii #valori_proprii #diagonalizare
### Vectori proprii. Valori proprii. Diagonalizarea

```ad-Definition
$
\text{fie } x \in V, \quad x \text{  s.n. vector propriu } \Leftrightarrow \exists \lambda \in \mathbb{K} \text{ (valoare proprie) } \text{a.î. } f(x) = \lambda x.
$
```

```ad-Notation
$
V_\lambda = \{ x \in V | f(x) = \lambda x \} \text{ subspațiul propriu corespunzător valorii } \lambda
$
```

$V_\lambda$ este un subspațiu invariant al lui $f$ , i.e. $f(V_\lambda) \subseteq V_\lambda$

##### Polinom Caracteristic

Fie:
$$
f \in End(V), \quad \mathcal{R} = \{ e_1, ... , e_n \} \text{  reper în V }, \quad [f]_{\mathcal{R},\mathcal{R}} = A.
$$

**Polinomul caracteristic** este definit ca:
$$
P_A(\lambda) = \det(A - \lambda I_n) =
\begin{vmatrix}
a_{11}-\lambda && a_{12} && \cdots && a_{1n} \\
a_{21} && a_{22} - \lambda && \cdots && a_{2n} \\
\vdots && \vdots && \ddots && \vdots \\
a_{n1} && a_{n2} && \cdots && a_{nn} - \lambda
\end{vmatrix}
$$

Și este egal cu:
$$
P_A(\lambda) = (-1)^n \sum_{k = 0}^n (-1)^k \sigma_{k} \lambda^k
$$
Unde $\lambda_k$ este suma minorilor diagonali de ordin $k$.

```ad-Notation
$
\delta_{ij} = 
\begin{cases}
0, & i \neq j \\
1, & i = j
\end{cases}
$
```

Astfel, din $f(x) = \lambda x$, poate fi dedus:
$$
\sum_{i=1}^n (a_{ji} - \lambda \delta_{ji})x_i = 0 \quad \forall j = \overline{1,n}
$$
SLO care are și soluții nenule $\Rightarrow$ SCN.

Este echivalent cu $P_A(\lambda) = 0$.

```ad-important
- Valoriile proprii sunt rădăcinile din $\mathbb{K}$ ale polinomului caracteristic.
- Polinomul caracteristic este **invariant** la schimbarea de reper.
```

De asemenea, polinomul caracteristic se poate rescrie 
$$
P_A(\lambda) = (-1)^n(\lambda - \lambda_1)^{m_1}(\lambda - \lambda_2)^{m_2}\dots (\lambda - \lambda_n)^{m_n} \quad \text{ unde } \sum_{i=0}^nm_i = n, \text{ multiplicitățile}
$$
```ad-Notation
$
\sigma_f = \{ \lambda_1, \dots, \lambda_n \} \text{ - spectrul lui f}
$
```

```ad-important
Fie $f \in End(V)$ Vectorii proprii corespunzători la valori proprii distincte formează un SLI.
```

În cazul $f \in End(V)$, unde $\lambda$ = valoare proprie cu multiplicitate $m_\lambda$ avem:
$$
\dim V_\lambda \le m_\lambda
$$
**Teorema de Diagonalizare**
Fie $f \in End(V)$.
$$
\exists \mathcal{R}  = \{ e_1, \dots, e_n \} \text{ în } V \text{ astfel încât }
[f]_{\mathcal{R,R}} \text{ diagonală } \Leftrightarrow 
$$

$$
\begin{cases}
1) & \lambda_1, \dots, \lambda_k \in \mathbb{K} \\
2) & V_{\lambda_i}= m_i, & \forall i \in \overline{1,k}
\end{cases}
$$


