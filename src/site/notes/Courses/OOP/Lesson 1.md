---
{"dg-publish":true,"permalink":"/courses/oop/lesson-1/"}
---


## Completări aduse de C++

```ad-index
[[Lesson 1#Supraîncărcarea funcțiilor|Supraîncărcarea funcțiilor]]
[[Lesson 1#Pointeri|Pointeri]]
[[Lesson 1#Const|Const]]
[[Lesson 1#Tipul referință|Referință]]
[[Lesson 1#Principiile POO|Principiile POO]]
```

#polimorfism
### Supraîncărcarea funcțiilor:

Merge pentru: 

+ *tipuri* diferite ale parametrilor

```cpp
double func(double i);

int func(int i);
```

- număr diferit de parametri

```cpp
int funct(int a);

int funct(int a, int b);
```

#erori #polimorfism 
**Erori de compilare**:
- diferența doar în tipul de date întors
- tipurile doar par diferite:
```cpp
int v[] and int* v

const int* and int const*
```

```ad-tip
`int x` si `int& x` sunt diferite
```

```ad-caution
Double vs float causes ambiguity when casting from something else.
Analog pt char si unsigned char (problema apare la casting)

```cpp
float myfunc(float i){ return i;}
double myfunc(double i){ return -i;}

int main(){
cout << myfunc(10.1) << " "; // unambiguous, calls myfunc(double)
cout << myfunc(10); // ambiguous
}
```

#erori Funcțiile cu valoare implicită pot duce la erori:

```cpp
int foo(int i);
int foo(int i, int j = 0);

int main() {
	foo(1, 2); // works just fine
	foo(3); // ambiguitate
}
```

#antet #prototip
Prototipul unei funcții permite declararea în afară a tipului/numărului de parametri:

```cpp
void f(int);

int main() {
	// ...
}

void f(int a) {
	// do smth
}
```

### Pointeri
#pointeri 

un **pointer** = variabilă care ține minte o adresă din memorie 
- are tip, compilatorul știe spre ce pointează
- operații aritmetice ce țin cont de tip 
- `tip *nume_pointer

##### Operatori pe pointeri:
- \*
- &
- schimbare de tip

```cpp
int x = 0;

int *p = &x; // ia adresa lui x

cout << p; // adresa lui x
cout << *p; // valoarea lui x
```

```ad-warning
Conversiile trebuie făcute cu schimbare de tip: #erori 
```cpp
float x = 16.8;
int *p = &int(x); // eroare

int *y;
y = (int*)&x; // merge
```

```ad-important
`new` si `delete` aloca, respectiv dezaloca spatiu si returneaza un pointer

Fiecare `new` trebuie sa fie urmat de un `delete`
```

Utilizare:

```cpp
int *pi;

pi = new int();

delete pi; // elibereaza zona de memorie

pi = new int(2); // aloca spatiu de int si intializeaza cu 2

delete pi;

pi = new int[2]; // aloca vector cu 2 elemente de tip intreg

delete [] pi; 
```

```ad-info
#erori `std::bad-alloc`
```

### Const 

- dacă știm că o variabilă nu se schimbă, declarăm `const`
- dacă încercăm să o schimbăm, primim eroare de compilare

```cpp
const int *u; // pointer la un int constant
int const *u; // pointer la un int constant

int *const u; // pointer care nu isi schimba adres
```

```ad-caution
- se poate face atribuire de adresa de obiect non-const pentru un pointer const;
- nu se poate face atribuire de adresa de obiect const catre pointer non-const;
```

##### Utilizarea const în apelul/returnarea de funcții
+ returnarea de `const` cu valoare nu reprezinta nimic pentru tipurile built-in
+ #erori dacă încerci să modifici un parametru const al unei funcții, **eroare de compilare**

### Tipul referință 

referință = pointer implicit, acționează ca un alt nume al aceluiași obiect (alias)

```cpp
int i;
int& ri = i; // alias pentru i
```

```ad-warning
O referință trebuie inițializată. #erori (dă eroare de compilare)

nu trebuie inițializată pentru: clase, parametru de funcții, returnare;
	-  **pentru o clasă tip referință, membrii trebuie inițializați în constructori**

referința nulă e interzisă

nu se poate afla **adresa** unei referințe:
&ref ne dă adresa variabilei 

nu se pot crea **tablouri** de referințe

nu se poate face referință către câmp de biți
```

### Transmiterea parametrilor 

- la transmiterea **prin valoare** se copiază valoarea în parametru normal
- la transmiterea **prin referință** se copiază doar adresa

### [[Courses/OOP/Lesson 2#Principiile POO\|Principiile POO]] 

#clasa

O **clasă** definește atribute și metode.

```cpp
class A{
	int atr1;
	int atr2
public:
	void method1();
	int method2(int x, int y);
};
```

#obiect

Un **obiect** este o instanță a unei clase, are anumite stări (valoarea) și un comportament (prin funcții)

+ au stare și acțiuni (metode, funcții)
+ au interfață (acțiuni) și o parte ascunsă (starea)

#încapsulare

**Încapsularea** = modularizarea prin contopirea datelor cu codul, în clase;

#moștenire

**Moștenire**  = posibilitatea de a extinde o clasă prin adăugarea de noi funcționalități


