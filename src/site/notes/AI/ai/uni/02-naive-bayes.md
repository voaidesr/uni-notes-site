---
{"dg-publish":true,"permalink":"/ai/ai/uni/02-naive-bayes/"}
---

# Cum evaluăm modele 

**Funcția de pierdere** reprezintă un mod de a evalua modelele: 

Regresie 
- media pătratelor erorilor 
- media erorilor în valoarea absolută 

Clasificare 
- numărul de clasificări greșite
- pt clasificare binară: True Positive, True Negative, False Pozitive, False Negative
- pt mai multe clase: matricea de confuzie

## Erori 

Eroarea de generalizare 

$$
\varepsilon(h) = \int_{X \times Y} V(h(x), y)) \rho(x, y) dx dy
$$
$h$ = este ipoteza/modelul predictiv aplicat pe date 
$V$ este funcția de pierdere, care măsoară „cât de departe” este predicția de adevăr ($y$). 
$\rho(x, y)$ este probabilitatea comună, probabilitatea ca perechea de date x,y să apară.

Practic, considerând acestea variabile aleatoare, media este: 

$$
\varepsilon(h) = \mathbb{E}_{(x,y)}[V(h(x), y)]
$$
Nu se cunoaște probabilitatea comună, deci cf Legii numerelor mari, putem măsura eroarea empirică:

$$
E(h) = \frac{1}{n} \sum_{i=1}^n V(h(x_{i}), y_{i})
$$
Eroarea poate fi descompusă în 					
- eroare de modelare = eroarea care apare de la o alegere proastă a modelului, i.e., modelul este prea simplu pentru a modela realitatea 
- eroare de estimare = avem date finite, care fie nu reprezintă bine realitatea fie nu sunt suficiente pentru a ajuta modelul să învețe 
- eroare de optimizare = nu am găsit optimul pentru funcție, fie algoritmul de optimizare s-a oprit prea devreme, fie s-a blocat într-un optim local. 

## Bias-variance trade-off 

- bias = modelul nu învață cu adevărat relațiile dintre date, poate fi corectată cu creșterea complexității modelului
- variance = eroare aleatoare care apare la fluctuații mici (modelul învață și zgomotul === overfit) 

>[!important]
>Dacă avem suficiente date de antrenare $D$ și spațiul de ipoteze $H$ nu este foarte complex, atunci, probabil, modelul va fi capabil să generalizeze.

## Regula lui Bayes 

$$
P(B | A) = \frac{P(B \cap A)}{P(A)} = \frac{P(A | B) P(B)}{P(A)}
$$
## Clasificatorul optimal 

Învățăm $h : \mathbf{X} \to Y$. Presupunem că cunoaștem $P(Y | \mathbf{X})$ (probabilitatea a posteriori, calculata de modelul optim). 

Clasificatorul Bayes este: 

$$
y^* = h^*(\mathbf{x}) = \arg\max_{y} P(Y=y|X=x)
$$
Clasificatorul Bayes este optim!

$$
err_{Bayes} = 1 - \sum_{y = y^*} \int_{x \in H_{i}} P(y | x) P(x) dx
$$
## Clasificatorul Bayes Naiv 

Presupune că trăsăturile sunt independente. 

$$
P(X_{1}, X_{2} | Y) = P(X_{1} | Y) \cdot P(X_{2} | Y)
$$
Considerând un $\mathbf{X}$ binar, dacă consideram trăsăturile corelate, trebuia să calculăm toate probabilitățile $P(\mathbf{X} | Y)$, adică $2^n$ probabilități/parametri. 

În cazul Naive Bayes totul se simplifică, având nevoie doar de $2 \cdot n$ parametri. 

- sunt date probabilitățile apriori ale claselor: $P(Y)$ 
- cele $n$ trăsături $\mathbf{X}$ 
- probabilitățile $P(X_{i} | Y)$ 

Atunci regula de decizie este 

$$
h_{NB}(x) = \arg\max_{y}P(y) \cdot P(x_{1},x_{2},\dots, x_{n} | y)
$$
În practică, se folosește sumă de log 

>[!note] likelihood = verosimilitatea 

Pentru aflarea probabilităților/parametrilor se folosește Maximum Likelihood Estimation. 

## Împărțirea datelor în date de antrenare, validare și test 

Modelele trebuie testate pe date necunoscute.
 
Putem obține o eroare mai bună dacă tunăm datele pe o mulțime de validare și o evaluăm pe o mulțime de test. (Pentru a nu face overfitting pe spațiul hiperparametrilor). 

### Cross-validation 

Se împart datele în $k$ părți (fold-uri). Se antrenează pe $k-1$ și se evaluează pe fold-ul rămas. Se calculează media rezultatelor. 

### Îmbunătățirea capacității de generalizare 

- early stopping: oprirea învățării atunci când eroarea pe validare începe să crească
- regularizare: adăugarea unui termen care să penalizeze complexitatea funcției de învățare 

## Matricea de confuzie 

![Pasted image 20260529220903.png](/img/user/AI/Images/Pasted%20image%2020260529220903.png)


| Actual \| Predicted -><br>\|<br>V | Car | Dog | Person |
| --------------------------------- | --- | --- | ------ |
| Car                               | 1   | 1   | 0      |
| Dog                               | 0   | 1   | 1      |
| Person                            | 0   | 0   | 2      |

Accuracy = suma elementelor de pe diagonala principala supra sumei componentelor diferite de 0 = $\frac{4}{6}$
Error = suma elementelor în afara diagonalei principale supra sumei componentelor diferite de 0 = $\frac{2}{6}$

În cazul binar

![Pasted image 20260529221424.png](/img/user/AI/Images/Pasted%20image%2020260529221424.png)

|            | PREDICTED YES      | PREDICTED NO       |
| ---------- | ------------------ | ------------------ |
| ACTUAL YES | TRUE POSITIVE = 2  | FALSE NEGATIVE = 1 |
| ACTUAL NO  | FALSE POSITIVE = 1 | TRUE NEGATIVE = 2  |
Precision = the amount of true positives out of all positives. Măsura de calitate, nu obligă detectarea tuturor pozitivelor, dar se asigură că, la precizie mare, pozitivele găsite sunt sigure.

$$
\mathrm{precision} = \frac{TP}{TP + FP} $$
Recall = the amount of true positives out of actual positives. Nu pune accent pe calitate, dar recall mare se asigură că detectăm toate cazurile pozitive.

$$
\mathrm{recall} = \frac{TP}{TP + FN} $$

Average Precision = the area under the Recall/Precision curve. 

TPR = True Positive Rate = Recall 

FPR = False Positive Rate. Câte False Positives avem față de numărul total de negative.

$$
\mathrm{FPR} = \frac{FP}{FP + TN}
$$
Curba ROC (Receiver Operating Characteristic) 

y = TPR, x = FPR. 

AUC = Area under Curve

![Pasted image 20260529222623.png](/img/user/AI/Images/Pasted%20image%2020260529222623.png)

### Măsura $F_{\beta}$

$$
F_{\beta} = (1 + \beta^2) \cdot \frac{\mathrm{precision} \cdot \mathrm{recall}}{\beta^2 \cdot \mathrm{precision} + \mathrm{recall}}
$$
$\beta > 1$ makes recall more important 

$\beta < 1$ makes precision more important 


## Evaluarea unui sistem de regresie 

### MSE 

$$
\mathrm{mse} = \frac{1}{n} \sum_{i =1}^n (y - \hat{f}(x))^2
$$
### Corelația Kendall-Tau 

$$
\uptau_{\alpha} = \frac{P- Q}{\frac{n(n-1)}{2}}
$$
Măsură bazată pe $P$ = perechi concordante vs $Q$ perechi discordante 

$$
P = |\{ (i, j) : 1 \leq i < j \leq n, (x_{i} - x_{j}) (y_{i} - y_{j}) > 0 \}|
$$

$$
Q = |\{ (i, j) : 1 \leq i < j \leq n, (x_{i} - x_{j}) (y_{i} - y_{j}) < 0 \}|
$$
