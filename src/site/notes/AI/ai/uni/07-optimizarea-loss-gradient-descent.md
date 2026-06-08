---
{"dg-publish":true,"permalink":"/ai/ai/uni/07-optimizarea-loss-gradient-descent/"}
---

# Clasificator liniar pentru mai multe clase 


$$
\text{input} \to f(\mathbf{x}, \mathbf{W}) \to \text{scor}
$$
![Pasted image 20260601135003.png](/img/user/AI/Images/Pasted%20image%2020260601135003.png)

Este aleasă clasa cu cel mai mare score. 

## SVM Multi-clasă (Hinge Loss)

Funcția de pierdere a unui SVM multiclasă este 

$$
L_{i} = \sum_{j\neq y_i}  \max(0, s_{j} - s_{y_{i}} + 1)
$$
Unde $j$ sunt toate clasele greșite iar $y_{i}$ este clasa corectă. Practic, funcția vrea să asigure că există o margine de siguranță între scorul clasei corecte și scorul claselor false mai mare decât 1. 

$$
s_{j} - s_{y_{i}} < -1 \iff s_{y_{i}} > s_{j} + 1
$$
![Pasted image 20260601140712.png](/img/user/AI/Images/Pasted%20image%2020260601140712.png)

În acest exemplu 

$$
\begin{align}
L_{1} & = \max(0, 5.1 - 3.2 + 1) = \max(0, -1.7 - 3.2 + 1)   \\
 & = \max(0, 2.9) + \max(0, -4.9 + 1) =  \\
 & 2.9
\end{align}
$$
$$
\begin{align}
L_{2} & = \max(0, 1.3 - 4. 9 + 1) +\max(0, 2.0 -4.9 + 1)  \\
 & = \max(0, -2.6) + \max(0, -1.9) =  \\
 & = 0
\end{align}
$$
$$
\begin{align}
L_{3}  &  = \max(0, 2.2 + 3.1 + 1) + \max(0, 2.5 + 3.1 + 1)  \\
 &  = \max(0, 6.3) + \max(0, 6.6) =  \\
 & = 6.3 + 6.6 = 12.9
\end{align}
$$
## Softmax / Multinomial Logistic Regression 

Scopul clasificatorului Softmax este să ia scoruri și să le transforme în probabilități. 

Probabilitatea reiese din exponențierea scorurilor și normalizarea lor 

$$
P(Y = k | X = x_{i}) = \frac{e^{s_{k}}}{\sum_{j} e^{s_{j}}}
$$
Unde $s_{i}$ este scorul unei clase $i$, adică 
$$
s_{i} = f(\mathbf{x}_{i}, \mathbf{W}) $$
Pentru exemplul anterior, cu pisica 

$$
\begin{pmatrix}
3.2 \\
5.1 \\
-1.7
\end{pmatrix}

 \overset{\text{exp}}{\to} 

\begin{pmatrix}
25.5  \\
164.0 \\
0.18
\end{pmatrix}

 \overset{\text{norm}}{\to} 
\begin{pmatrix}
0.13  \\
0.87 \\
0.00
\end{pmatrix}
$$
>[!important]
>Vrem să maximizăm log-probabilitatea (adică, pentru o funcție de pierdere), să minimizăm probabilitatea negativă a clasei corecte. 

$$
L_{i} = -\log P(y - y_{i} | X = x_{i})
$$
- Softmax este sensibil la perturbații mici 
- Hinge nu este 

Deci 

$$
L = \frac{1}{N} \sum_{i=1}^N L_{i} + R(W)
$$
Unde $R(W)$ era un factor de regularizare. 

# Gradient Descent 

>[!def] Gradient 
>Gradientul este un vector de derivate parțiale a unei funcții (pe toate dimensiunile). 
>$$
>\nabla f = \begin{pmatrix}
> \frac{\partial f}{\partial x_{1}} \\
> \frac{\partial f}{\partial x_{2}} \\
> \vdots \\
> \frac{\partial f}{\partial x_{n}} \\
\end{pmatrix}
>$$

## Metode de evaluare 

- metoda numerică: se alege $h > 0$, aproape de zero și se folosește formula 
$$
\frac{df(x)}{dx} = \lim_{ h \to 0 } \frac{f(x+h)-f(x)}{h}
$$

Foarte lent de calculat, și e doar o valoarea aproximativă!

- metoda analitică: folosim analiza numerică pentru a determina formula gradientului. Mai înclinat de greșeli. 

>[!info] 
>În practică se folosește mereu gradientul analitic, dar se verifică implementarea cu gradientul numeric. (Verificarea gradientului/gradient checking).

O implementare naivă este 

```python 
def GradDescent(W0, X, goal, learnig_rate):
	goal_not_met = True
	W = W0 
	
	while goal_not_met:
		# evaluate the gradient at our point
		gradient = eval_gradient(X, W)
		
		W_old = W 
		# gradient acts as a speed, and learnign rate as time
		# this gives us the step
		W = W - learning_rate * gradient
		
		# stop when we not make a significant step 
		# i.e., greater than goal
		goal_not_met = sum(abs(W - W_old)) > goal
		
	return W
```

### Coborârea pre gradient cu mini batch 

Se evaluează doar o mică parte a mulțimii de antrenare pentru a calcula gradientul. 

- mai eficient pentru memorie și computațional 
- poate da o estimare foarte bună a gradientului

>[!note]
>Învățare end-to-end = atunci când procesul de învățare duce și la extragerea de trăsături. 

# Algoritmul ca graf computațional 

## Multi-class SVM

![Pasted image 20260604165249.png](/img/user/AI/Images/Pasted%20image%2020260604165249.png)

### Regula de înlănțuire la derivate parțiale 

$$
\frac{\partial f}{\partial y}  = \frac{\partial f}{\partial q} \cdot \frac{\partial q}{\partial y} 
$$

Un graf computațional arată așa 

![Pasted image 20260604165628.png](/img/user/AI/Images/Pasted%20image%2020260604165628.png)

Unde numerele cu verde sunt **outputul** iar numerele cu roșii sunt gradientul de tipul 

$$
\frac{\partial f}{\partial \text{input}}
$$
![Pasted image 20260604180721.png](/img/user/AI/Images/Pasted%20image%2020260604180721.png)

Practic liniile verzi sunt pasul de forward prin rețea, iar liniile roșii reprezintă backpropagarea gradientului prin rețea. 

Avem 

$$
z = f(x, y)
$$

La $z$ înapoi ajunge gradientul funcției de pierdere față de $z$. 

În nod se păstrează gradienții locali, ca să putem "trimite" gradientul la inputuri și să vedem cum acestea au afectat gradientul. 

$$
\begin{cases}
\frac{\partial L}{\partial x} = \frac{\partial L}{\partial z} \cdot \frac{\partial z}{\partial x}  \\
 \\
\frac{\partial L}{\partial y} = \frac{\partial L}{\partial z} \cdot \frac{\partial z}{\partial y}
\end{cases}
$$
Un exemplu bun este 

![Pasted image 20260604181654.png](/img/user/AI/Images/Pasted%20image%2020260604181654.png)

Această funcție se numește funcția **sigmoidă**. 

$$
\sigma(x) = \frac{1}{1 + e^{-x}}
$$
Iar 
$$
\frac{d\sigma(x)}{dx} = (1 - \sigma(x))\sigma(x)
$$
![Pasted image 20260604181959.png](/img/user/AI/Images/Pasted%20image%2020260604181959.png)

### Alte tipare 

- poarta **add** = distribuie gradientul
- poarta **max** = rutează gradientul 
- poarta **ori** = comută gradientul

![Pasted image 20260604182141.png](/img/user/AI/Images/Pasted%20image%2020260604182141.png)

Iar atunci când se ramifică, gradienții se adună!

![Pasted image 20260604182902.png|300](/img/user/AI/Images/Pasted%20image%2020260604182902.png)

De exemplu, pentru poarta **ori** 

```python 

def forward(x, y): 
	z = x * y 
	layer.input = [x, y]
	return z 

# dz = dL/dz	
def backward(dz):
	# pentru ori dz/dx = d(x*y)/dx = y
	# dL/dx = dL/dz * dz/dx	
	dx = dz * layer.input[1] 
	
	# dL/dy = dL/dz * dz/dy
	dy = dz * layer.input[0]
	return dx, dy
```

În caz **vectorial**, gradientul local (la fiecare neuron funcția derivată față de un input) este matricea Jacobiană (derivata fiecărui element din z în raport cu fiecare element din x) 

$$
\frac{\partial z}{\partial x} = \begin{pmatrix}
\frac{\partial z_{1}}{\partial x_{1}}  & \frac{\partial z_{1}}{x_{2}} \dots  \frac{\partial z_{1}}{x_{n}}
\end{pmatrix}
$$
## Definiții 

- **backpropagare** = aplicarea recursivă a chain rule de-a lungul unui graf computațional pentru calcularea gradienților parametrilor/intrărilor 

Implementările mențin o structură de graf în care nodurile implementează funcțiile de forward și backward. 

- **forward** = calculează rezultatul unei funcții și salvează în memorie intrările și rezultatele intermediare pentru pasul de backward 
- **backward** = aplicarea regulii de înlănțuire pentru calcularea gradientului funcției de pierdere în raport cu intrările 

# Rețele neuronale 

Funcția liniară de scoring este 

$$
f = W x
$$
O rețea cu două nivele 

$$
f = W_{2} \max (0, W_{1}x) = W_{2} \text{ReLU}(W_{1}x)
$$
![Pasted image 20260604184941.png](/img/user/AI/Images/Pasted%20image%2020260604184941.png)
