---
{"dg-publish":true,"permalink":"/courses/maths/linear-algebra/gal-c9/"}
---

# Spații vectoriale euclidiene

>[!definition] Produs scalar
>Fie $(V, +, \cdot)|_{\mathbb{R}}$ spațiu vectorial real
>$g : V \times V \to \mathbb{R}$ se numește produs scalar $\iff$ 
>- $g \in L^s(V, V; \mathbb{R})$ (formă biliniară și simetrică) vezi [[Courses/Maths/Linear Algebra/gal.c8#Forme biliniare\|aici]]
>- $g$ este [[Courses/Maths/Linear Algebra/gal.c8#Forme pozitiv definite\|pozitiv definita]]

>[!Notation]
>- $(V,g)$, $(E,g)$, $(E, (\cdot, \cdot))$, $(E, \langle \cdot, \cdot \rangle)$ = spațiu vectorial euclidian real (s.v.e.r) 
>- $\|x\| \overset{\text{def}}{=} \sqrt{\langle x, x \rangle}$ - norma vectorului 

Matricea asociată lui $g$ funcționează la fel ca matricea asociată unei [[Courses/Maths/Linear Algebra/gal.c8#Matricea asociată unei forme biliniare\|forme biliniare simetrice]]

>[!definition] Reper ortogonal, reper ortonormat
>$(V, g)$ s.v.e.r. și $\mathcal{R} = \{e_1, \dots, e_n \}$ reper în $V$
>- $\mathcal{R}$ s.n. **reper ortogonal** $\iff g(e_i, e_j) = 0, \quad \forall \, i \neq j$
>-  $\mathcal{R}$ s.n. **reper ortonormat** $\iff g(e_i, e_j) = \delta_{ij}$ 
>Unde $\delta_{ij} = \begin{cases} 1, \quad i = j\\ 0, \quad i \neq j \end{cases}$ --> simbolul Kronecker
>
>Bază ortonormată $\iff$ vectori ortogonali doi câte doi și versori (adică de normă 1). 
