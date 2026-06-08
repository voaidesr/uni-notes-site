---
{"dg-publish":true,"permalink":"/ai/ai/uni/05-metode-kernel-ridge-svm/"}
---

# Perceptronul 

Ia inputuri și prezice pe baza unei ecuații liniare: 

$$
y = \mathrm{sgn}(\mathbf{w}^T \cdot \mathbf{x} + b)
$$

Unde $\mathbf{w}$ este vectorul de greutăți și $\mathbf{x}$ este inputul. Separarea va fi una liniară: 

Vectorul $\mathbf{w}$ va fi mereu perpendicular pe hiperplanul de separație.

![Pasted image 20260531133546.png](/img/user/AI/Images/Pasted%20image%2020260531133546.png)

Problema XOR nu este liniar separabilă. (funcția XOR nu poate fi învățată). 

Soluții 
- rețele neuronale: granița de decizie devine non-liniară 
- metode kernel 

# Metode kernel 

1. Datele sunt scufundate într-un spațiu cu mai multe dimensiuni 
2. Relațiile liniare sunt căutate în noul spațiu

În fapt, nu se găsește o funcție de scufundare $\phi$, ci această scufundare se realizează explicit prin specificarea unui **produs scalar î**ntre exemple.

Funcția de kernel este, practic 

$$
k(x, z) = \langle\phi(x), \phi(z) \rangle
$$
## Forma primală și forma duală 

Ambele forme reprezintă un mode de a formula problema de clasificare. 

În forma primală:
se încearcă  aflarea vectorului $\mathbf{w}$, pentru a găsi o greutate asociată fiecărei trăsături. Dacă aplicăm trucul kernel, dimensiunea lui $\mathbf{w}$ va crește și va deveni dificil/imposibil de calculat/memorat (imposibil dacă scufundarea se face într-un spațiu de dimensiune infinită). 

În forma duală: 

Considerăm că vectorul $\mathbf{w}$ poate fi scris ca o combinație liniară a punctelor din setul de antrenare. Așa că vom căuta un set de ponderi $\alpha_{i}$ pentru fiecare punct. Acum putem aplica kernel trick-ul fără a mai avea problema dimensiunii. 

Predicția va fi 

$$
y = \left\langle  \sum_{i = 1}^n \alpha_{i}x_{i}, x\right\rangle 
$$Unde $x_{i}$ sunt punctele din antrenare și $x$ este un punct oarecare.
Deci, din proprietățile produsului scalar 

$$
y = \sum_{i = 1}^n \alpha_{i} \langle x_{i}, x \rangle 
$$
Se poate aplica ușor trucul kernel prin înlocuirea produsului scalar cu funcția kernel $k$ 

$$
y = \sum_{i = 1}^n \alpha_{i} k( x_{i}, x )
$$
Forma primală optimizează pe numărul de trăsături și are nevoie de coordonate. Forma duală optimizează pe numărul de exemple de antrenare

>[!note]
>Funcția nucleu mai este numită și funcție de **similaritate**.

Pentru a scrie în forma duală, putem scrie matricea Gram (matricea tuturor produselor scalare posibile)
$$ X = \begin{pmatrix} x_{1} \\ x_{2} \\ \vdots \\ x_{n} \end{pmatrix} $$ Unde $x_{i}$ este un rând cu un număr de feature-uri.  Matricea Gram $$ K_{X} = X X^{\top}   $$ Practic  $$ K_{X} = \begin{pmatrix} x_{1} x_{1}^{\top} & x_{1} x_{2}^{\top}  &  \dots & x_{1} x_{n}^{\top} \\ \dots  & \dots & \dots & \dots \\ x_{n} x_{1}^{\top} & x_{n} x_{2}^{\top} &\dots & x_{n} x_{n}^{\top} \end{pmatrix} $$ Iar această matrice va avea dimensiunea $n \times n$.  Putem scrie 
$$
\mathbf{w} \cdot x + b = K_{X} \cdot \alpha' + b
$$
unde 
$$
\alpha' = (\alpha_{1}, \alpha_{2}, \dots, \alpha_{n})^{\top}
$$
Deci clasificatorul depinde numai de numărul de exemple $n$. 

# Regresia liniară 

Vrem să găsim o funcție $g$ de forma 

$$
g(\mathbf{x}) = \langle \mathbf{w}, \mathbf{x} \rangle = \mathbf{w}^{\top} \mathbf{x}
$$
Care interpolează cel mai bine o mulțime de puncte 

$$
S = \left\{ (\mathbf{x_{1}}, y_{1}), (\mathbf{x}_{2}, y_{2}), \dots, (\mathbf{x}_{l}, y_{l}) \right\}
$$
Adică găsim practic un polinom care să producă cea mai mică eroare 

$$
\mathcal{L}(g, S) = \sum_{i = 1}^l(y_{i} - g(\mathbf{x}_{i}))^2
$$
Funcția de pierdere, scrisă vectorial este 

$$
\mathbf{\xi} = \mathbf{y} - \mathbf{X} \mathbf{w}
$$
Iar valoarea este norma la pătrat

$$
\mathcal{L}(g, S) = \xi^{\top}\xi
$$
Valoarea optimă se obține prin a găsi minimul, în funcție de $\mathbf{w}$. 

$$
\frac{\partial\mathcal{L}(\mathbf{w}, S)}{\partial \mathbf{w}} = 0
$$
Deci $$
-2 \mathbf{X}^{\top}\mathbf{y} + 2 \mathbf{X}^{\top}\mathbf{X}\mathbf{w} = \mathbf{0}
$$
$$
\mathbf{X}^{\top} \mathbf{X} \mathbf{w} = \mathbf{X}^{\top}\mathbf{y}
$$
$$
\mathbf{w} = (\mathbf{X}^{\top} \mathbf{X})^{-1}\mathbf{X}^{\top}\mathbf{y}
$$
# Regresia Ridge 

>[!danger]
>Problema apare când $\mathbf{X}^{\top}\mathbf{X}$ nu este inversabilă. 

Atunci problema este prost pusă și trebuie utlizată regularizarea. 

Funcția de pierdere devine 

$$
\mathcal{L}_{\lambda}(\mathbf{w}, S) = \sum_{i = 1}^l (y_{i} - g(x_{i}))^2 + \lambda |\!| \mathbf{w} |\!|
{ #2}

$$

Iar forma închisă va deveni 

$$
\mathbf{w} = (\mathbf{X}^{\top}\mathbf{X} + \lambda I_{n})^{-1} \mathbf{X}^{\top} \mathbf{y}
$$

## Regresia Ridge (în formă duală) 

Considerăm ecuația echivalentă cu criteriul de optimizare 

$$
\begin{align}
&\qquad\qquad\mathbf{X}^{\top}\mathbf{X} \mathbf{w} + \lambda \mathbf{w} = \mathbf{X}^{\top}\mathbf{y} &\iff  \\
&\iff  \lambda  \mathbf{w} = \mathbf{X}^{\top} \mathbf{y} - \mathbf{X}^{\top}\mathbf{X}\mathbf{w}  &\iff\\
&\iff \mathbf{w} = \lambda^{-1} (\mathbf{X}^{\top} \mathbf{y} - \mathbf{X}^{\top}\mathbf{X}\mathbf{w}) &\iff \\
&\iff \mathbf{w} = \lambda^{-1}\mathbf{X}^{\top}(\mathbf{y} - \mathbf{X}\mathbf{w}) &\iff  \\
&\iff \mathbf{w} = \mathbf{X}^{\top} \alpha, \quad \alpha = \lambda^{-1}(\mathbf{y} - \mathbf{X}\mathbf{w})
\end{align}
$$

Iar $$
\mathbf{w} = \mathbf{X}^{\top}\alpha
$$
Deci forma finală va fi 
$$
\alpha = \lambda^{-1} (\mathbf{y} - \mathbf{X}\mathbf{X}^{\top}\alpha)
$$
Deci 
$$
(\mathbf{X}\mathbf{X}^{\top} + \lambda I_{n})\alpha = \mathbf{y}
$$

Iar vectorul coloana $\alpha$ va fi 

$$
\alpha = (\mathbf{X} \mathbf{X}^{\top} + \lambda I_{n})^{-1} \mathbf{y}
$$
În forma duală informația despre datele de antrenare este dată de matricea Gram

$$
\mathbf{G} = \mathbf{X}\mathbf{X}^{\top}
$$
Iar funcția de predicție este 

$$
g(\mathbf{x}) = \sum_{i=1}^n \alpha_{i} \langle \mathbf{x_{i}}, \mathbf{x}  \rangle 
$$
>[!important]
>Kernel trick reprezintă doar a înlocui produsul scalar canonic cu o funcție kernel. 

Practic, din matricea Gram $G$ obținem o matrice $K$: 

$$
G = \begin{pmatrix}
 \langle x_{1}, x_{1} \rangle  & \langle x_{1}, x_{2} \rangle  & \dots & \langle x_{1}, x_{n} \rangle  \\
\dots & \dots & \dots & \dots \\
\langle x_{n}, x_{1} \rangle  & \langle x_{n}, x_{2} \rangle  & \dots & \langle x_{n}, x_{n} \rangle 
\end{pmatrix}
\to 
K = \begin{pmatrix}
k(x_{1}, x_{1}) & k(x_{1}, x_{2}) &  \dots & k(x_{1}, x_{n})  \\
\dots  & \dots & \dots & \dots \\
k(x_{n}, x_{1})  & k(x_{n}, x_{2}) &  \dots & k(x_{n}, x_{n})
\end{pmatrix}
$$

Ponderile duale se calculează 

$$
\mathbf{\alpha} = (\mathbf{K} + \lambda I_{n})^{-1} \mathbf{y}
$$
Iar predicția va deveni 

$$
g(\mathbf{x}) = \sum_{i = 1}^n \alpha_{i}k(x_{i}, x)
$$
Deci predicția, vectorial va fi 

$$
\mathbf{\hat{y}} = \mathbf{K}\alpha
$$
>[!important]
>Acesta este simplificat, pentru un kernel liniar, în mod normal avem formula $$ \mathbf{K} = (\langle \mathbf{x_{t}}, \mathbf{x} \rangle + 1) \cdot d $$ unde $d$ este gradul.

# Funcții kernel

Funcția kernel: 

$$
k : X \times X \to \mathbb{R}
$$

Pentru care există o funcție de scufundare din spațiul $X$ în spațiul Hilbert $F$ 
$$
\phi : x \in \mathbb{R}^m \to \phi(x) \in F 
$$
Astfel încât, $\forall x, z \in X$
$$
k(x, z) = \langle \phi(x), \phi(x) \rangle 
$$
>[!definitie] Teorema 
>O funcție $k$ este funcție kernel doar dacă este finit pozitiv semi-definită. 

![Pasted image 20260531195011.png](/img/user/AI/Images/Pasted%20image%2020260531195011.png)

Funcția kernel din exemplul anterior 

$$
\begin{align}
k(\mathbf{x}, \mathbf{z}) &= \langle \phi(\mathbf{x}), \phi(\mathbf{z}) \rangle  \\
&= (x_{1}z_{1} + x_{2}z_{2})^2  \\
 & = \langle \mathbf{x}, \mathbf{z} \rangle
{ #2}
 \\
\end{align}
$$
>[!important]
>Pentru o funcție kernel $k$ există mai multe scufundări asociate.

## Tipuri de funcții kernel 

### Funcția kernel polinomială 

Pentru o constantă $c \in \mathbb{R}_{+}$ și un număr $d \in \mathbb{N}$

$$
k(\mathbf{x}, \mathbf{z}) = (\langle \mathbf{x}, \mathbf{z} \rangle + c)^d
$$
Unde 
- $c$ permite controlul gradului de influență al polinoamelor de diverse grade. un $c$ crescut pune mai mult accent pe termenii de grad mai mic și face ca hiperplanul să devină mai neted.
- $d$ stabilește gradul polinomului

### Funcția kernel Gaussiană (rbf)

$$
k(\mathbf{x}, \mathbf{z}) = \exp\left( - \frac{|\!| \mathbf{x} - \mathbf{z} |\!|
{ #2}
}{2 \sigma^2} \right)
$$
De exemplu, pentru 
$$
\mathbf{x} = (1, 2, 4, 1), \qquad \mathbf{z} = (5, 1, 2, 3)
$$
Considerând $\sigma=1$

$$
\begin{align}
k(\mathbf{x}, \mathbf{z})  & = \exp\left( -\frac{\sqrt{ 4^2 + 1 + 2^2 + 2
{ #2}
 }}{2 \cdot 1} \right) =  \\
 & = \exp\left( -\frac{\sqrt{  16 + 1 + 8}}{2} \right) =  \\
 &  = \exp\left( -\frac{5}{2} \right) 
\end{align}
$$

### Funcția kernel intersecție 

$$
k(\mathbf{x}, \mathbf{z}) = \sum_{i} \min \{x_{i}, z_{i}\}
$$
Deci, aplicat pe exemplu

$$
\begin{align}
k(\mathbf{x}, \mathbf{z}) &  = \min\{1, 5\} + \min\{2, 1\} + \min\{4, 2\}+ \min\{1, 3\}  \\
 &  = 1 + 1 + 2 + 1 =  \\
 & = 5
\end{align}
$$
### Funcția kernel Hellinger 

$$
k(\mathbf{x}, \mathbf{z}) = \sum_{i} \sqrt{ x_{i} \cdot z_{i} }
$$
### Funcția kernel PQ 

$$
k_{PQ}(X, Y) = 2(P - Q)
$$
Unde $P$ și $Q$ reprezintă aceleași măsuri ca în Kendall-Tau ([[AI/ai/uni/02-naive-bayes#Corelația Kendall-Tau\|vezi aici]])

### String kernels 

Acestea măsoară similaritatea între perechi de șiruri de caractere, prin numărarea subsecvențelor (n-grame) de caractere comune între cele două șiruri.

- nu trebuiesc delimitate cuvintele 
- independentă de limbă

Pentru 2 string-uri, se construiesc 2 hash tables $S$, $T$, astfel încât fiecare are structura (key=the n-gram):(value=de câte ori apare n-grama). De exemplu, pentru lungimea n=2 și cuvintele s = "pineapple pi" și t = "apple pie" 

`S = {pi:2, in:1, ne:1, ea:1, ap:1, pp:1, pl:1, le:1, e_:1, _p:1}`

`T= {ap:1, pp:1, pl:1, le: 1, e_:1, _p:1, pi:1, ie:1}`

Kernelul bazat pe biți de prezență
$$
k_{2}^{0/1} (s, t) = \sum_{v \in \Sigma} S^{0/1} (v) \cdot T^{0/1}(v)
$$
Adică numărul de 2-grame comune celor două șiruri de caractere. 

Util pentru reprezentare mai compactă în cazul în care numărul de exemple de antrenare << numărul de trăsături. 

## Noi funcții kernel din combinații 

Fie 2 funcții kernel valide $k_{1}, k_{2}$. O contantă $a \in \mathbb{R}_{+}$, o funcție cu valori reale $f$ și o matrice simetrică și pozitiv semi-definită $B$. Atunci următoarele funcții sunt kernel 

$$
\begin{align}
 & (i)\quad \,\;k(x, z) = k_{1} (x,z) + k_{2}(x, z)  \\
 & (ii) \quad \, k(x, z) = ak_{1}(x, z) \\
 & (iii) \quad \!\! k(x, z) = k_{1}(x, z) \cdot k_{2}(x, z)  \\
 & (iv) \quad \! k(x, z) = f(x) \cdot f(z) \\
 & (v) \quad k(x, z) = x^{\top}Bz
\end{align}
$$

## Normalizarea datelor 

În forma primală 

$$
x \to \phi(x) \to \frac{\phi(x)}{|\!| \phi(x) |\!| }
$$
În forma duală, normalizăm matricea kernel 

$$
\hat{k}(x_{i}, x_{j}) = \frac{k(x_{i}, x_{j})}{\sqrt{ k(x_{i}, x_{i}) \cdot k(x_{j}, x_{j}) }}
$$
Direct pe matrice 

$$
\hat{K}_{ij} = \frac{K_{ij}}{\sqrt{ K_{ii} \cdot K_{jj}}}
$$
Este ușor de codat

```python 
def normalize_dual(K):
    K_norm = np.sqrt(np.diag(K)) # extrage radical din element pe diagonala
    K_norm = K_norm.reshape(-1, 1) # transforma in vector coloana
    K = K / np.matmul(K_norm, K_norm.T) # inmultirea face o matrixe n x n
    return K
```

# SVM

Dacă avem o mulțime de puncte și funcția de predicție 

$$
g(\mathbf{x}) = \langle \mathbf{w}, \mathbf{x} \rangle  + b
$$
$y$ - ne înmulțesște cu label -1 sau +1. 

Atunci problema SVM este să maximizeze $\gamma$, cu variabilele $\mathbf{w}, b, \gamma$, cu restricțiile

- $y_{i}(g(x_{i})) \geq \gamma$
- $i = 1,\dots,l$
- $|\!| \mathbf{w} |\!| = 1$ - condiție care 

Care este echivalentă cu minimizarea normei lui $\mathbf{w}$

Dacă datele nu sunt liniar separabile, se folosește soft-margin SVM. Acceptă o variabilă care permite eroarea $\xi$, care permite depășirea graniței cu această eroare. Pentru a penaliza erorile există argumentul $C$. Algoritmul încearcă să aibă o margine cât mai lată dar și să minimizeze suma erorilor.

- C mic = penalizare mică, preferă o margine lată și să accepte mai multe erori 
- C mare = penalizare puternică, va îngusta marginea și nu va fi la fel de neted, încercând să separe punctele perfect. Risc de overfitting. 

## Clasificatori pe mai multe clase 

Scheme de îmbinare 

- one-versus-one
- one-versus-all

La OvO, o nouă imagine este trecută prin fiecare model și este clasificată după votul majoritar. Exemplu (om vs caine -> om, om vs masina -> om, masina vs caine -> caine) va fi om. 

La OvA, trece imaginea prin toți clasificatorii și îl alege pe cel cu cea mai mare încredre. 

Utilizarea unor metode de clasificare capabile să rezovle direct problema 
- rețele neuronale 
- analiza liniar discriminantă (Fisher) 

## Analiza Liniar Discriminantă 

- fiecare clasă aproximată cu o distribuție Gaussiană
- găsirea unui hiperplan **pe care sunt proiectate punctele** astfel încât distanța dintre mediile claselor este maximizată și dispersia fiecărei clase este minimizată

$$
J(w) = \frac{|\mu_{1} - \mu_{2}|^2}{s_{1}^2 + s_{2}^2}
$$
# No free lunch

Orice doi algoritmi sunt echivalenți pe toate problemele posibile. În spațiul tuturor problemelor există și probleme complet haotice.

- dacă un algoritm funcționează bine pe o anumită problemă e pentru că problema respectă niște constrângeri, iar pe altă problemă care nu respectă constrângerile algoritmul va funcționa prost 
