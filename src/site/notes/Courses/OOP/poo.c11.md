---
{"dg-publish":true,"permalink":"/courses/oop/poo-c11/"}
---

# [[Courses/OOP/poo.c1#Pointeri\|Pointeri]]

>[!tip] Array 
>Numele unui array este un pointer. 
>```cpp
>lista[5] == *(lista+5)
>```
>

Array de pointer -> numele listei este un pointer către pointeri (dublă indirectare).

## Array de obiecte 

Putem crea array-uri cu obiecte de orice tip. 

Ele pot fi inițializate

```cpp 
clasa lista[4] = {1, 2, 3, 4};
```

Sau neinițializate 

```cpp 
clasa lista[4];
```

Pentru cazul inițializat avem nevoie de constructor care primește parametru întreg. 

```cpp 
class cl{
	int i;
public: 
	cl(int j) : i(j) {}
	int get_i() { return 1; }
}

int main() {
	cl ob[3] = {1, 2, 3} // initializatori 
}
```

Pot fi inițializați și clase cu constructori cu mai mulți parametri: 

```cpp 
clasa lista[3] = {clasa(1,2), clasa(2, 3), clasa(4, 5)};
```

## Pointerul `this` 

Orice funcție membru are pointerul `this`, care arată către obiectul asociat cu funcția respectivă. 

>[!danger] `this` în alte funcții 
>- Funcțiile prieten nu au `this` 
>- Funcțiile statice nu au `this` 

## Pointeri între bază și clase derivate 

>[!warning] 
>Aritmetica pe pointeri nu funcționează dacă incrementăm un pointer către bază și suntem în derivată. 

## Pointeri către membrii în clase 

Putem aplica doar `.*` și `->*`.

```cpp 
struct X {
    int v;
    int get() const { return v*2; }
};

int main() {
    X x{5};

    // data-member pointer
    int X::* dp = &X::v;
    std::cout << "v = " << x.*dp << "\n";

    // function-member pointer
    int (X::* fp)() const = &X::get;
    std::cout << "get() = " << (x.*fp)() << "\n";

    return 0;
}
```

## Apelul prin [[Courses/OOP/poo.c1#Tipul referință\|referință]].

Nou în C++. 

Dacă transmitem obiecte prin referință, nu se mai creează obiecte temporare, ci se lucrează direct prin obiectul transmis ca parametru. 

Nu se map apelează constructorul de copiere și destructorul. La fel și la întoarcerea unei referințe

## Referințe independente în clase 

```cpp 
class ob{
    int& x;
public:
    ob(int a) : x(a) {cout << x << " " << a++ << " " << x << " ";}
    void afis() { cout << x; }
};

int main(){
    ob ob1(10); // 10 10 11
    ob ob2(20); // 20 20 21
    ob1.afis(); // 21
}
```

# Const și volatile 

Înlocuiește `#define`, care era bazat pe substituție de valoare. 

`const` implică internal linkage (e vizibil nu mai în fișierul respectiv la linkare).

Trebuie dată o valoare la declararea `const`, singura excepție:

```cpp 
extern const int a;
```

## Argumente de funcții, parametrii de întoarcere

Apel prin valoare cu `const` : parametru formal nu se schimbă în funcție. 

`const` la întoarcere: valoarea returnată nu se poate schimba. 

>[!info] 
>Obiectele temporare sunt const 

```cpp 
class X{};

X foo() { return X(); }

void g1(X&) {} 

void g2(const X&) {}

int main() { 
	g1(foo()); // eroare, const la referință nonconst 
	g2(foo()); // merge ok
}
```


>[!danger] Static const 
>Static const trebuie inițializat la declarare, nu poate fi inițializat prin constructor. 

>[!warning]
>Declararea unei funcții cu const nu garantează că nu se modifică starea obiectului! Părți ale obiectului care sunt `mutable` pot fi modificate. 
>
>Nu pot fi apelate funcții non-`const`. 
>
>Pot fi apelate atât de obiecte const cât și de obiecte non-const. 

Poate fi ignorat `const`-ul prin casting la pointer din tipul obiectului

```cpp 
class Y {
	int i;
public:
	Y();
	void f() const;
}

Y::Y() : i(0) {}

void Y::f() const {
	// i++ ar da eroare, const function 

	// C-style cast
	((Y*)this) -> i++; // merge, this era in mod normal == const Y*
	// C++ cast 
	(const_cast<Y*>(this))->i++; // merge	
}
```

### Mutable 

```cpp 
class Y {
	int i;
	mutable int j;
public:
	Y();
	void f() const;
}

//
void Y::f() const {
	// i++ ar da eroare 
	j++; // Merge!
}
```


### Mutable și [[Courses/OOP/poo.c10#Lambda expressions\|lambda expresii]]

Dacă capturăm o valoare prin valoare:

```cpp 
int i = 2;

auto f = [i]() {
	i = i*2; // da eroare, i e constantă
	return i;
}
```

Putem să schimbăm valoarea local în lambda expresie cu mutable.

```cpp 
int i = 2;

auto f = [i]() mutable {
	i = i*2;
	return i;
}
```

```cpp
int main() {
    int i = 2;
    auto f = [i]() mutable {
        i = i * 2;
        cout << i << " ";
    };
    cout << i << " "; // 2
    f(); // 4
    cout << i << " "; // 2
}
```

## Volatile 

Lasă compilatorul să știe că variabila poate suferi modificări în afara controlului compilatorului (de exemplu, de către SO).

- utilizat în multithreading, multitasking, întreruperi

Nu se fac optimizări de cod 

```cpp 
int x = 10;

while (x == 10) {} // deoarece x nu se schimbă în while, poate fi optimizat la while(true) {};

volatile int i = 10;
while(i == 10) {} // nu se face optimizare
```

## Static 

### Obiecte statice 

Sunt inițializate o singură dată, dar se distrug de-abia la finalul programului, **indiferent de unde apar**. 

```cpp
class X {
    int i;
public:
    X(int i = 0) : i(i) { cout << "C" << i << " ";}
    ~X() {cout << "D" << i << " ";}
};

void f() {
    static X x1(10);
    static X x2;
    static X x3(10);
}

int main() {
    f();
    f();
    f();
    cout << "10 ";
}
```

Se va afișa 

```
C10 C0 C10 10 D10 D0 D10 
```

Pentru că obiectele statice sunt construite o singură dată. 

Dacă ar exista și obiecte globale, acestea ar fi distruse ultimele. 

>[!warning]
>`::` se poate comporta astfel, atunci când există mai multe denumiri

```cpp 
int x = 100;

class X {
	static int x;
	static int y;
	//
}

int X::x = 1;
int X::y = x + 1; // aici va fi folosit X::x, nu ::x, deci y = 2 

// daca voiam y = 101 
// int X::y = ::x + 1;
```

>[!tip] `static` în cazuri particulare 
>- Clasele nested pot avea membri statici.
>- Clasele locale nu pot avea membri statici. (de exemplu în funcții)

```cpp 
// Static members & local classes
#include <iostream>
using namespace std;

// Nested class CAN have static data members:
class Outer {
    class Inner {
        static int i;  // OK: nested (member) class can have static data
    };
};

// Definition of the static member:
int Outer::Inner::i = 47;

// Local class CANNOT have static data members:
void f() {
    class Local {
    public:
        // static int i;  // Error: no place to define Local::i outside this scope
    } x;
}

int main() {
    Outer o;
    f();
    return 0;
}
```

>[!warning]
>Funcțiile membre statice nu sunt asociate unui obiect, așa că nu au pointer-ul `this`.



