---
{"dg-publish":true,"permalink":"/ai/ai/uni/04-seminar-knn-bayes/"}
---

# Bayes 

Fie mulțimea de antrenare 

$$
S = \left\{
\begin{align}
&([0.5, -0.7], 1), \\
&([0.2, 0.1], 1), \\
&([0.4, 0.8], 2),  \\
&([-0.3, -0.8], 3), \\
&([0.4, -0.3], 1),  \\
&([0.9, 0.3], 2),  \\
&([-0.5, -0.5], 3),  \\
&([0.2, 0.4], 2)
\end{align}
\right\}
$$

Și mulțimea de testare 

$$
T = \left\{ 
\begin{align}
&([0.3, -0.1], 1),  \\
&([-0.1, -0.1], 2),  \\
&([0.2, 0.3], 2),  \\
&([-0.1, -0.5], 3 )
\end{align}
\right\}
$$

Datele trebuiesc discretizate. Discretizăm în 2 intervale, după regula 

$$
\begin{cases}
0, x_{i} \leq \mu_{i}  \\
1, x_{i} > \mu_{i}
\end{cases}
$$
Calculăm $\mu_{1}$

$$
\begin{align}
\mu_{1} &= \frac{0.5 + 0.2 + 0.4 -0.3 + 0.4 + 0.9 - 0.5 + 0.2}{8}  \\
&= \frac{1.2 + 0.6}{8} = \frac{1.8}{8} = 0.225
\end{align}
$$
Calculăm $\mu_{2}$ 

$$
\begin{align}
\mu_{2} &= \frac{-0.7 + 0.1 + 0.8 -0.8 -0.3 + 0.3 - 0.5 + 0.4}{8}  \\
&= \frac{-0.6 - 0.1}{8} = -0.0875
\end{align}
$$
Mulțimea de antrenare discretizată este:



$$
S = \left\{
\begin{align}
&([1, 0], 1), \\
&([0, 1], 1), \\
&([1, 1], 2),  \\
&([0, 0], 3), \\
&([1, 0], 1),  \\
&([1, 1], 2),  \\
&([0, 0], 3),  \\
&([0, 1], 2)
\end{align}
\right\}
$$

Iar mulțimea de testare este 

$$
T = \left\{ 
\begin{align}
&([1, 0], 1),  \\
&([0, 0], 2),  \\
&([0, 1], 2),  \\
&([0, 0], 3 )
\end{align}
\right\}
$$
Acum calculăm probabilitățile 

1) apriori: 

$$
\begin{align}
&\mathbb{P}(Y =1 ) = \frac{3}{8} \\
&\mathbb{P}(Y =2 ) = \frac{3}{8} \\
&\mathbb{P}(Y =3 ) = \frac{2}{8}
\end{align}
$$

2) likelihood 

3) Likelihood:

$$
\begin{aligned}
\mathbb{P}(x_{1} = 0 \mid Y = 1) &= \frac{1}{3} & \mathbb{P}(x_{1} = 0 \mid Y = 2) &= \frac{1}{3} & \mathbb{P}(x_{1} = 0 \mid Y = 3) &= 1 \\[1ex]
\mathbb{P}(x_{1} = 1 \mid Y = 1) &= \frac{2}{3} & \mathbb{P}(x_{1} = 1 \mid Y = 2) &= \frac{2}{3} & \mathbb{P}(x_{1} = 1 \mid Y = 3) &= 0 \\[2ex]
\mathbb{P}(x_{2} = 0 \mid Y = 1) &= \frac{2}{3} & \mathbb{P}(x_{2} = 0 \mid Y = 2) &= 0 & \mathbb{P}(x_{2} = 0 \mid Y = 3) &= 1 \\[1ex]
\mathbb{P}(x_{2} = 1 \mid Y = 1) &= \frac{1}{3} & \mathbb{P}(x_{2} = 1 \mid Y = 2) &= 1 & \mathbb{P}(x_{2} = 1 \mid Y = 3) &= 0
\end{aligned}
$$
a) 

Prezicem pentru datele de test, calculând $\mathbb{P}(Y | X)$

Pentru exemplul 1. 
$$
\begin{align}
\mathbb{P}(Y = 1 | [1, 0]) &\propto \mathbb{P}(Y = 1) \cdot \mathbb{P}(x_{1} = 1 | Y = 1) \cdot \mathbb{P}(x_{2} = 0 | Y = 1) =  \\
&= \frac{3}{8} \cdot \frac{2}{3}  \cdot \frac{2}{3} = \frac{12}{72}
\end{align}
$$
$$
\begin{align}
\mathbb{P}(Y = 2 | [1, 0]) &\propto \mathbb{P}(Y = 2) \cdot \mathbb{P}(x_{1} = 1 | Y = 2) \cdot \mathbb{P}(x_{2} = 0 | Y = 2) = \\
&= \frac{3}{8}  \cdot \frac{2}{3} \cdot 0 = 0
\end{align}
$$
$$
\begin{align}
\mathbb{P}(Y = 3 | [1, 0]) &\propto \mathbb{P}(Y = 3) \cdot \mathbb{P}(x_{1} = 1 | Y = 3) \cdot \mathbb{P}(x_{2} = 0 | Y = 3) =  \\
&= \frac{2}{8} \cdot 0 \cdot 1 = 0
\end{align}
$$
Deci predicția ($\arg \max_{y}$)  este $1$. **Predicție corectă!**

Pentru exemplul 2. 

$$
\begin{align}
\mathbb{P}(Y = 1 | [0, 0]) &\propto \mathbb{P}(Y = 1) \cdot \mathbb{P}(x_{1} = 0 | Y = 1) \cdot \mathbb{P}(x_{2} = 0 | Y = 1) =  \\
&= \frac{3}{8} \cdot \frac{1}{3}  \cdot \frac{2}{3} = \frac{6}{72} = \frac{1}{12}
\end{align}
$$
$$
\begin{align}
\mathbb{P}(Y = 2 | [0, 0]) &\propto \mathbb{P}(Y = 2) \cdot \mathbb{P}(x_{1} = 0 | Y = 2) \cdot \mathbb{P}(x_{2} = 0 | Y = 2) = \\
&= \frac{3}{8}  \cdot \frac{1}{3} \cdot 0 = 0
\end{align}
$$
$$
\begin{align}
\mathbb{P}(Y = 3 | [0, 0]) &\propto \mathbb{P}(Y = 3) \cdot \mathbb{P}(x_{1} = 0 | Y = 3) \cdot \mathbb{P}(x_{2} = 0 | Y = 3) =  \\
&= \frac{2}{8} \cdot 1 \cdot 1 = \frac{1}{4} 
\end{align}
$$
Deci predicția este 3. **Predicție greșită**.

Pentru exemplul 3. 

$$
\begin{align}
\mathbb{P}(Y = 1 | [0, 1]) &\propto \mathbb{P}(Y = 1) \cdot \mathbb{P}(x_{1} = 0 | Y = 1) \cdot \mathbb{P}(x_{2} = 1 | Y = 1) =  \\
&= \frac{3}{8} \cdot \frac{1}{3}  \cdot \frac{1}{3} = \frac{3}{72} = \frac{1}{24}
\end{align}
$$
$$
\begin{align}
\mathbb{P}(Y = 2 | [0, 1]) &\propto \mathbb{P}(Y = 2) \cdot \mathbb{P}(x_{1} = 0 | Y = 2) \cdot \mathbb{P}(x_{2} = 1 | Y = 2) = \\
&= \frac{3}{8}  \cdot \frac{1}{3} \cdot 1 = \frac{3}{24} 
\end{align}
$$
$$
\begin{align}
\mathbb{P}(Y = 3 | [0, 1]) &\propto \mathbb{P}(Y = 3) \cdot \mathbb{P}(x_{1} = 0 | Y = 3) \cdot \mathbb{P}(x_{2} = 1 | Y = 3) =  \\
&= \frac{2}{8} \cdot 1 \cdot 0 = 0 
\end{align}
$$
Deci predicția este 2. **Predicție corectă!**

Exemplul 4. Aceleași calcule ca la exemplul 2, dar predicția este corectă. 

b) 

Eroarea de clasificare este 1/4 = 0.25 = 25%.

Confusion matrix:

|          | predicted 1 | predicted 2 | predicted 3 |
| -------- | ----------- | ----------- | ----------- |
| actual 1 | 1           | 0           | 0           |
| actual 2 | 0           | 1           | 1           |
| actual 3 | 0           | 0           | 1           |
# KNN 

$$
S = \left\{  
\begin{align}
&x_{1} = ([-2, 1, 3, 1, -3, 1], 1) \\
&x_{2} = ([3, 1, -3, -1, 4, 0], -1)  \\
&x_{3} = ([-1, 3, 0, 1, -2, 1], 1)  \\
&x_{4} = ([0, 3, -5, -1, 1, 0], -1)
\end{align}
\right\}
$$

Iar singurul exemplu de testare 

$$
T = [0, 1, -2, -4, 2, 0]
$$

$$
\begin{aligned}
& |\!| x_{1} |\!|_{2}  = \sqrt{ 4 + 1 + 9 + 1 + 9 + 1 } = \sqrt{ 25 } = 5 \\
& |\!| x_{2} |\!|_{2} = \sqrt{ 9 + 1 + 9 + 1 + 16 } = \sqrt{ 36 } = 6 \\ 
& |\!| x_{3} |\!|_{2} = \sqrt{ 1 + 9 + 1 + 4 + 1 } = \sqrt{ 16 } = 4 \\ 
& |\!| x_{4} |\!|_{2} = \sqrt{ 9 + 25 + 1 + 1 } = \sqrt{ 36 } = 6
\end{aligned}
$$

Iar vectorii normalizați (i.e. $x_{i}' = \frac{x_i}{|| x_{i}||_{2}}$) vor fi 

$$
S_{\mathrm{\text{norm}}} = \left\{  
\begin{align}
&x_{1}' = \left( \left[ -\frac{2}{5}, \frac{1}{5}, \frac{3}{5}, \frac{1}{5}, -\frac{3}{5}, \frac{1}{5} \right], 1 \right) \\
&x_{2}'= \left( \left[ \frac{3}{6}, \frac{1}{6}, -\frac{3}{6}, -\frac{1}{6}, \frac{4}{6}, 0 \right], -1 \right)  \\
&x_{3}' = \left( \left[ -\frac{1}{4}, \frac{3}{4}, 0, \frac{1}{4}, -\frac{2}{4}, \frac{1}{4} \right], 1 \right)  \\
&x_{4}' = \left( \left[ 0, \frac{3}{6}, -\frac{5}{6}, -\frac{1}{6}, \frac{1}{6}, 0 \right], -1 \right)
\end{align}
\right\}
$$


Iar pentru test 

$$
|\!| t |\!|_{2} = \sqrt{ 1 + 4 + 16 + 4 } = \sqrt{ 25 } = 5
$$
$$
t' = \left[ 0, \frac{1}{5}, -\frac{2}{5}, -\frac{4}{5}, \frac{2}{5}, 0 \right]
$$

Calculăm distanțele $L_{1}$. 

$$
D_{1}(t', x_{1}') = \frac{2}{5} + 0 + \frac{1}{5} + \frac{3}{5} + \frac{1}{5} + \frac{1}{5} = \frac{8}{5}
$$
$$
D_{2}(t', x_{2}') = \frac{3}{6} + \frac{1}{30} + \frac{1}{10} + \frac{23}{30} + \frac{8}{30} = \frac{36}{30} = \frac{6}{5}
$$
$$
D_3(t', x_{3}') = \frac{1}{4} + \frac{14}{20} + \frac{2}{5} + \frac{21}{20} + \frac{18}{20} + \frac{1}{4} = \frac{57}{20} = \frac{14.25}{5}
$$
$$
D_{4}(t', x_{4}') = \frac{9}{30} + \frac{13}{30} + \frac{19}{30} + \frac{7}{30} + 0 = \frac{48}{30} = \frac{8}{5}
$$
Label-urile cele mai apropiate sunt 

$$
L = \{ 1, -1, -1 \}
$$
Deci eticheta, folosind 3-NN va fi $-1$.

