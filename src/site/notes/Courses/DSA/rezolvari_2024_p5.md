---
{"dg-publish":true,"permalink":"/courses/dsa/rezolvari-2024-p5/"}
---

# Rezolvări examen 2024 - Partea 5

*Credit: Matteo Verzotti*
## Exercițiul 1
Fie `P` o permutare cu `n` elemente. Scopul este să determinăm această permutare știind pentru fiecare poziție `i` `(1 <= i <= n)` câte elemente aflate înaintea ei sunt mai mici decât `P[i]`. Dacă există mai multe permutări posibile, se va afla cea minim lexicografică.

**Input**
```
N = 4
0 1 1 0
```

**Output:**
```
2 4 3 1
```

## Rezolvare

Vom itera crescător prin numerele de la `1` la `n`. Fie `i` numărul la care am ajuns. De fiecare dată vom vrea să-l plasăm pe `i` pe cea mai din dreapta poziție care încă are numărul `0`. De ce? Presupunem că există mai multe poziții cu valoarea `0` și nu îl punem pe `i` pe cea mai din dreapta. Asta înseamnă că există o valoare `j` care va fi pusă ulterior la dreapta față de `i`. Contradicție, pentru că am spus că iterăm prin valori crescător, deci `j > i`, ceea ce ar însemna că `j` ar avea cel puțin un element la stânga mai mic decât el.

După ce îl plasăm pe `i` pe poziția `p`, vom scădea `1` din toate valorile la dreapta de `p`. Algoritmul apoi se reia.

Exemplu:
Pas 1.
```
v = [0 1 1 0]
P = [0 0 0 0]
i = 1
```
Cea mai din dreapta poziție `p` cu `v[p] = 0` este `3`, deci `P[3] = 1` și scădem `1` din toate valorile mai la dreapta (nu se aplică în cazul acesta).

Pas 2.
```
v = [0 1 1 -1]
P = [0 0 0 1]
i = 2
```
Cea mai din dreapta poziție `p` cu `v[p] = 0` este `0`, deci `P[0] = 2` și scădem `1` din toate valorile mai la dreapta.

Pas 3.
```
v = [-1 0 0 -2]
P = [2 0 0 1]
i = 3
```
Cea mai din dreapta poziție `p` cu `v[p] = 0` este `2`, deci `P[2] = 3` și scădem `1` din toate valorile mai la dreapta.

Pas 4.
```
v = [-1 0 -1 -3]
P = [2 0 3 1]
i = 4
```

Cea mai din dreapta poziție `p` cu `v[p] = 0` este `1`, deci `P[1] = 4` și scădem `1` din toate valorile mai la dreapta.

La final, avem `P = [2 4 3 1]`.

Pentru a găsi cea mai din dreapta valoare de `0` și a scădea ulterior `1` pe un sufix, se poate parcurge liniar vectorul. Această soluție ar face `N` operații la fiecare pas (i.e. parcurgerea), și ar avea o complexitate de timp $O(N^2)$ și de memorie suplimentară $O(1)$. Asta ar trebui să obțină probabil `0.5 / 1p`.

Putem optimiza folosind *Șmenul lui Batog*, unde fiecare bucket ne va spune dacă conține sau nu vreun `0`. Apoi este suficient să iterăm doar prin bucket-uri atât la update, cât și la query. Complexitatea finală de timp ar fi de $O(N \sqrt N)$ și de memorie suplimentară $O(\sqrt N)$. Asta ar trebui să obțină `0.8 / 1p`.

Pentru punctaj maxim ne vom folosi de același truc ca anterior, doar că vom ține un arbore de intervale. Fiecare nod ne va spune cea mai din dreapta poziție din intervalul determinat de nod care încă are `0` (sau `-1` dacă nu există). Update-ul pe sufix se va face cu lazy updates. Complexitatea finală de timp ar fi de $O(N \log N)$ și de memorie suplimentară $O(N)$. Asta ar trebui să obțină `1p`.

## Exercițiul 2
Se considera un șir `S` de paranteze rotunde (închise sau deschise) de lungime `n` și `q` query-uri `(i, j)`. Pentru fiecare query, să se răspundă dacă subsecvența de la `i` la `j` `(S[i…j])` este corect parantezată.

Codificăm fiecare paranteză deschisă `(` cu valoarea `1` și fiecare paranteză închisă `)` cu valoarea `-1`. Pentru ca o subsecvență să fie corect parantezată, trebuie îndeplinite două condiții:
1. Suma valorilor din subsecvență să fie `0`.
2. Niciun prefix să nu aibă suma mai mică decât `0` (asta ar însemna că la un moment dat se închid mai multe paranteze decât s-au deschis). Cu alte cuvinte, **minimul** sumelor pe prefix să fie `>= 0`.

Vom face sumele parțiale pe vectorul nostru.
```
(()()))(()
v  = [1 1 -1 1 -1 -1 -1 1 1 -1]
sp = [1 2  1 2  1  0 -1 0 1  0]
```

Subsecvența `S[1...4]` este corect parantezată, deoarece `sp[4] - sp[0] = 1 - 1 = 0`, iar minimul pe `[1...4]` este egal cu `1`, care nu este mai mic decât `sp[0]`.

Subsecvența `S[5...8]` **nu este** corect parantezată, deoarece `sp[8] - sp[4] = 1 - 1 = 0`, dar minimul pe `[5...8]` este egal cu `-1`, care este mai mic decât `sp[4] = 1`.

Deci, reformulând, o subsecvență `S[i...j]` este corect parantezată dacă:
1. `sp[j] - sp[i - 1] = 0`
2. $\min\{sp[i \dots j]\} \ge sp[i - 1]$

> [!WARNING] Atenție
> Mare grijă când `i = 0`. (asta se poate rezolva considerând secvența indexată de la `1`).

Ca să satisfacem a doua condiție, doar trebuie să aflăm rapid pentru fiecare subsecvență minimul pe aceasta. Asta se rezolvă fie brut (punctaj parțial), fie cu un *RMQ* (complexitate optima, punctaj maxim).

Complexitate timp: $O(N \log N + Q)$
Complexitate memorie: $O(N \log N)$

## Exercițiul 3
Se dă un vector `A` cu `N` elemente. `1 <= A[i] <= N`. Pentru fiecare `i` să se găsească `j` minim a.î. `A[j] < A[i]`.

Similar cu exercițiul 1, vom lua valorile în ordine crescătoare și vom avea un vector inițial gol, pe care îl vom umple pe măsură ce parcurgem numerele. Presupunem că am ajuns la o valoare  `A[i]`, care se află evident pe poziția `i`. Atunci trebuie doar să vedem dacă există vreun număr deja pus în vector în intervalul `[1, i - 1]`. Dacă da, luăm primul dintre ele. Dacă nu, atunci știm sigur că nu există `j < i` cu `A[j] < A[i]`.

> [!WARNING] Atenție
> Trebuie să avem grijă la valorile egale. O variantă ar fi parcurgerea lor în ordine descrescătoare după poziția din vector.

Avem deci, două tipuri de operații:
1. **Update (i, x)**: Poziția `i` este acum ocupată de `x`.
2. **Query i:** Care este poziția minimă `j < i` astfel încât `A[j] != 0`?

Similar cu exercițiul 1, avem 3 tipuri de complexități:
1. Parcurgere brută pentru a-l afla pe `j`: complexitate finală $O(N^2)$, probabil `0.5 / 1p`
2. Folosim *Șmenul lui Batog* pentru a-l afla pe `j`: complexitate finală $O(N \sqrt N)$, probabil `0.8 / 1p`
3. Folosim un *Arbore de intervale* pentru a-l afla pe `j`: complexitate finală $O(N \log N)$, punctaj maxim.

> [!TIP] Tip
> Nu am explicat în detaliu cum se folosește un arbore de intervale la problema aceasta. Dacă ați lucrat suficient cu el, devine foarte clar cum se implementează operațiile de mai sus în timp eficient. Which means, e posibil să nu fie nevoie să detaliați totul nici la examen. Dacă aveți ceva exercițiu care se reduce la **Update Query**, posibil ca dacă trântiți un arbore de intervale acolo fără să detaliați prea mult (dacă nu știți), tot să luați punctaj.

## Exercițiul 4
Se dă un vector `A` cu `N` elemente, `1 <= A[i] <= N`. Să se numere tripletele `(i, j, k)` a.î. `1 <= i < j < k <= N` și `A[i] < A[j] < A[k]`.

Precalculăm vectorii `cntst[]` și `cntdr[]`, cu semnificația:
* `cntst[j]` = numărul de poziții `i < j` cu `A[i] < A[j]`
* `cntdr[j]` = numărul de poziții `k > j` cu `A[k] > A[j]`

Acești vectori se pot precalcula similar cu exercițiul anterior, dar ținând un contor, în loc de poziția minimă.

Acum, este clar că dacă fixăm `j`, atunci `i` și `k` vor fi independenți unul față de celălalt. Deci, pentru o valoare fixată `j`, numărul de triplete va fi `cntst[j] * cntdr[j]`. Astfel, rezultatul total va fi, de fapt
$$
\sum_{j = 2}^{N - 1} \text{cntst}[j] \cdot \text{cntdr}[j]
$$
Complexitatea finală va fi dată de precalcularea vectorilor, care se face în $O(N \log N)$.