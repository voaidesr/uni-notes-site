---
{"dg-publish":true,"permalink":"/ai/ai/uni/08-neural-nets/"}
---

>[!important] 
>Rețelele neuronale sunt funcții universale de aproximare. (Ele pot aproxima orice funcție, cu numărul corect de parametrii).

Este mai bine să adăugăm regularizare mai puternică decât să micșorăm capacitatea modelului. 

>[!info]
>Performanța crește cu cât arhitectura este mai adâncă, dar trebuie să folosim regularizare mai puternică. 

## Pași în antrenarea unei rețele neuronale 

- trebuiesc stabilite 
	- funcțiile de activare
	- preprocesarea
	- inițializarea ponderilor
	- regularizarea
	- verificarea gradientului 
- dinamica antrenării 
	- asistarea procesului de învățare 
	- actualizarea parametrilor 
	- optimizare hiperparametrilor 
- evaluare
	- ansamble de modele

# Funcții de activarea 

- sigmoidă - aduce numerele în intervalul $[0,1]$, inspirată din biologie 

1. Neuronii saturați omoară gradienții. 
2. Outputul nu e centrat în zero, iar costul funcției exp e ridicat
$$
\sigma(x) = \frac{1}{1 + e^{-x}}
$$
- tangentă hiperbolică 

1. tot se saturează, dar măcar e de medie 0
$$
\tanh (x)
$$
- ReLU
1. nu se saturează în partea pozitivă, este eficient computațional, converge repede 
2. nu are media zero, gradientul este o când outputul e mai mic
$$
\mathrm{\mathrm{Re}LU} = \max(0,x)
$$
- Leaky ReLU 
$$
\max(0.1x, x)
$$

- PReLU - Parametric Relu 

$$
f(x) = \max(\alpha x, x)
$$
- Maxout (generalizează ReLU și Leaky ReLU)

$$
\max(w_{1}^{\top}x + b_{1}, w_{2}^{\top}x + b_{2})
$$
- ELU (are toate beneficiile ReLU)

$$
f(x) = 
\begin{cases}
x, \qquad x > 0  \\
\alpha (\exp(x) -1), \qquad x \leq 0
\end{cases}
$$
# Preprocesarea datelor 

- centrare = scădem media 
- normalizare = împărțim cu deviația standard 

La imagini este suficient să centrăm datele, putem să scădem fie media pe fiecare canal, fie imaginea medie.

# Inițializarea ponderilor 

Ponderile se inițializează cu numere aleatorii aproape de zero.. Asta funcționează bine pentru rețele mici, dar poate duce la distribuiții neomogene a funcțiilor de activare. Toți neuronii se saturează complet iar gradienții vor fi zero (vanishing gradients). 

#### Inițializare Xavier 

Ponderile provin dintr-o distribuție de medie 0 și varianță dată de numărul de perceptroni de pe stratul anterior/posterior:
$$
\mathbb{V} \! \mathrm{ar} [W] = \frac{2}{n_{in} + n_{out}}
$$
#### Normalizarea Batch

Vrem să avem activări de medie 0 și deviație standard 1. Le normalizăm 

$$
\hat{x}^{(k)} = \frac{x^{(k)} - E[x^{(k)}]}{\sqrt{ Var[x^{(k)}] }}
$$
Se introduce între fully connected layers și non-liniarități. 

# Sfaturi 

- verificăm că funcția de pierdere este ok fără regularizare 
- ne asigurăm că putem face overfit pe o parte mică din setul de date 


# SGD cu moment/impuls 

Adaugă inerție în învățare. Se folosește viteza (inițial zero), care este updatată așa 

$$
V_{n} = \mu V_{n-1}- \mathrm{\lambda} \nabla L
$$
Unde $\mu$ este coeficientul impulsului, care spune cât de mult să păstrăm din viteza anterioară. $\mu = 1$ ar însemna oscilație la infinit, pe când $\mu = 0$ va face ca învățarea să se facă doar prin gradient, $\lambda$ este learning rate-ul și $\nabla L$ gradientul funcției de pierdere.

Iar weight-urile vor fi updatate așa 

$$
W_{n} = W_{n-1} + V_{n }
$$

# Dropout 

Setăm random o parte din ponderi ca fiind egale cu zero pentru o parte din neuroni 

Este echivalent cu antrenarea unui ansamblu de multe modele care au în comun parametri. 

