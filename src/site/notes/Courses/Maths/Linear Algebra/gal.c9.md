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

