---
{"dg-publish":true,"permalink":"/ai/ai/uni/03-knn-curseofdimensionality/"}
---

# KNN 

### Metoda celor mai apropiați k vecini - k-NN

1. pentru fiecare exemplu de test găsim cei mai apropiați k vecini 
2. atribuim eticheta majoritară conform celor k vecini

Dacă avem egalitate, putem: 
- alege din etichetele egale în mod aleator
- 1-NN 
- utilizăm distanțele ca ponderi 

>[!note] Presupuneri ale k-NN 
>- datele de antrenare și datele de test respectă aceeași distribuție 

### Parametrii k-NN 

- avem nevoie de o funcție de distanță (distanța Euclidiană, distanță Manhattan, Minkowski, distanța de Editare, distanța Hamming) 
- Câți vecini luăm în considerare

Exemplu 

- hamming = util pentru clasificare pe date categorice sau secvențe ADN (câte trăsături diferă între cei doi vectori) 
- editare = șiruri de caractere, secvențe temporale (câte modificări sunt necesare pentru a transforma un obiect în cel de-al doilea).

## k-NN pentru regresie 

1. pentru fiecare exemplu de test $x$, găsim cei mai apropiați $k$ vecini și etichetele lor. 
2. outputul este media etichetelor celor $k$ vecini 

$$
f(x) = \frac{1}{K} \sum_{i= 1}^K y_{i}
$$
## Avantaje și proprietăți 

- suprafața de decizie este neliniară 
- un singur parametru
- cu creșterea lui $k$ , eroarea de clasificare pe antrenare crește, dar suprafața de decizie devine mai netedă 

## Dezavantaje 

- trebuie menținută în memorie întregul set de date de antrenare și trebuie parcurs 
- suferă de "curse of dimensionality"

# Blestemul dimensionalitățiia

- pentru a umple un spațiu $nD$ avem nevoie de un număr exponențial de puncte.

## Fenomenul Hughes 

De la un moment dat, adăugarea mai multor caracteristici, păstrând dimensiunea setului de antrenare degradează performanța clasificatorului.

![Pasted image 20260530130831.png](/img/user/AI/Images/Pasted%20image%2020260530130831.png)

