---
{"dg-publish":true,"permalink":"/courses/oop/poo-exam/"}
---

# Strategie examen 

>[!tip]
>1. Verifică lucrurile care pot face programul să nu compileze. 
>	- Redeclarări
>	- Funcții cu aceeași signatură 
>	- Specificatori de acces 
>		- Dacă se accesează ceva private/protected 
>		- Dacă se accesează la moștenire (moșteniri private, protected) ceva ce nu ar putea fi accesat 
>	- Static 
>		- Dacă se accesează ceva non-static în context static 
>		- Dacă static-ul e inițializat 
>	- Const 
>		- Dacă se apelează o metodă non-const pentru o variabilă const 
>		- Dacă se modifică o variabilă const 
>		- Funcții const 
>	- Constructori fără parametru 
>	- Referințe (cum se apelează funcțiile care au parametru referințe) 
>	- Clase abstracte -> nu pot fi instanțiate 
>2. Verifică ce arată, dacă compilează. 

# Rezolvări 

## [Quiz](https://oopquiz.fly.dev/index.php?id=1)

### Ex. 1 

```cpp
#include <iostream>
using namespace std;
class cls1
{
protected:
    int x;

public:
    cls1() { x = 13; }
};
class cls2 : public cls1
{
    int y;

public:
    cls2() { y = 15; }
    int f(cls2 ob) { return (ob.x + ob.y); }
};
int main()
{
    cls2 ob;
    cout << ob.f(ob);
    return 0;
}
```

*Soluție*

**Compilează.** Afișează `28`. 

>[!info] Explicație 
>- `x` poate fi accesat pentru că e protected și avem moștenire `public`. 
>- `cls1` are constructor fără parametri, deci, când este construit `cls2`, baza este construită implicit, prin acel constructor fără parametri. Deci `ob2.x = 13`

### Ex. 2 

```cpp 
#include <iostream>
using namespace std;

class A {
    static int x;

public:
    A(int i = 0) { x = i; }
    int get_x() { return x; }
    int& set_x(int i) { x = i; }
    A operator=(A a1)
    {
        set_x(a1.get_x());
        return a1;
    }
};
int main()
{
    A a(212), b;
    cout << (b = a).get_x();
    return 0;
}
```

*Soluție*

**Nu compilează**. Avem variabilă statică neinițializată.

Pentru a corecta e suficient să adăugăm, după declararea clasei:

```cpp 
int A::x = 0;
```

### Ex. 3 

```cpp 
#include<iostream>
using namespace std;

class A
{ int i;
  public: A(int x=2):i(x+1) {}
  virtual int get_i() { return i; }};
class B: public A
{ int j;
  public: B(int x=20):j(x-2) {}
  virtual int get_j() {return A::get_i()+j; }};
int main()
{ A o1(5);
  B o2;
  cout<<o1.get_i()<<" ";
  cout<<o2.get_j()<<" ";
  cout<<o2.get_i();
  return 0;
}
```

*Soluție*

**Compilează**. Afișează `6 21 3`.

>[!info] Explicație 
>- `o2` este construit cu constructorul implict al bazei (deci i = 2 + 1 = 3) și cu constructorul derivatei tot implicit (deci j = 20 - 2 = 18). 

### Ex. 4

```cpp 
#include <iostream>
using namespace std;
class problema {
    int i;

public:
    problema(int j = 5) { i = j; }
    void schimba() { i++; }
    void afiseaza() { cout << "starea curenta " << i << "\n"; }
};
problema mister1() { return problema(6); }
void mister2(problema& o)
{
    o.afiseaza();
    o.schimba();
    o.afiseaza();
}
int main()
{
    mister2(mister1());
    return 0;
}
```

*Soluție*

**Nu compilează**

>[!info] Explicație 
>`mister1()` returnează un obiect temporar, care este `const`. `mister2` primește o referință non-const, deci eroare. Ca modificare, am putea adăuga `const` înaintea argumentului din `mister2`, dar după nu am mai putea apela `o.schimbă`. 
>
>Soluția este să eliminăm referința și să transmitem doar prin valoare `void mister2(problema o) {...}`

### Ex. 5 

```cpp 
#include <iostream>
using namespace std;
//ordinea constructorilor
class B
{
    int i;

public:
    B() { i = 1; }
    virtual int get_i() { return i; }
};
class D : virtual public B
{
    int j;

public:
    D() { j = 2; }
    int get_i() { return B::get_i() + j; }
};
class D2 : virtual public B
{
    int j2;

public:
    D2() { j2 = 3; }
    int get_i() { return B::get_i() + j2; }
};
class MM : public D, public D2
{
    int x;

public:
    MM() { x = D::get_i() + D2::get_i(); }
    int get_i() { return x; }
};
int main()
{
    B *o = new MM();
    cout << o->get_i() << "\n";
    MM *p = dynamic_cast<MM *>(o);
    if (p)
        cout << p->get_i() << "\n";
    D *p2 = dynamic_cast<D *>(o);
    if (p2)
        cout << p2->get_i() << "\n";
    return 0;
}
```

*Soluție*

**Compilează**. Afișează 

```
7
7
7
```

>[!info] Explicație
>Problema [[Courses/OOP/poo.c6#Moștenirea în diamant\|diamantului]] este evitată prin moștenirea virtual, deci compilează.
>
>Se aplică [[Courses/OOP/poo.c9#Run-time type identification (RTTI)\|RTTI]] pentru determinarea outcome-ului funcțiilor.
>
>B este o clasă polimorfică (are  o funcție virtuală `get_i`). Fiecare `override` al acestei funcții este virtual, chiar dacă nu mai este pus cuvântul cheie `virtual`. 
>
>Deci, în cazul `B* o = new MM()` , este cunoscut la runtime că tipul real al lui o este `MM`, deci este apelat get_i pentru tipul `MM`. 
>
>La fel și pentru cast-ul la `D` și la `MM`. 

### Ex. 6 

```cpp 
#include <iostream>
using namespace std;
class B {
    int x;

public:
    B(int i = 7) { x = i; }
    int get_x() { return x; }
    operator int() { return x; }
};
class D : public B {
public:
    D(int i = -12)
        : B(i)
    {
    }
    D operator+(D a) { return get_x() + a.get_x() + 1; }
};
int main()
{
    D a;
    int b = 18;
    b += a;
    cout << b;
    return 0;
}
```

*Soluție*

**Compilează.** Afișează `6`

>[!info] Explicație 
>Esențială este existența lui `operator int() {return x;}`, care este operatorul de conversie a unui obiect de tip `B` la `int`. 
>
>`+=` este un operator diferit de `+`, deci nu se aplică funcția definită în D pentru `+`.
>
>`b+=a` funcționează doar pentru că `a` are, din bază, abilitatea de a fi convertit la `int`. Se face conversie implicită și `b += a` va face ca `b = 18 + (-12) = 6`.

### Ex. 7 

```cpp 
#include <iostream>
using namespace std;
#include <typeinfo>
class B {
    int i;

public:
    B() { i = 1; }
    int get_i() { return i; }
};
class D : B {
    int j;

public:
    D() { j = 2; }
    int get_j() { return j; }
};
int main()
{
    B* p = new D;
    cout << p->get_i();
    if (typeid((B*)p).name() == "D*")
        cout << ((D*)p)->get_j();
    return 0;
}
```

*Soluție*

**Nu compilează**. 

>[!info] Explicație 
>`B* p = new D` nu funcționează decât în cazul moștenirii publice. Aici, moștenirea este privată (implicit), deci codul nu va compila. 
>
>Peste suficient să adăugăm `public` la moștenire pentru a compila. 

### Ex. 8 

```cpp 
#include <iostream>
using namespace std;
class B {
protected:
    int x;

public:
    B(int i = 28) { x = i; }
    virtual B f(B ob) { return x + ob.x + 1; }
    void afisare() { cout << x; }
};
class D : public B {
public:
    D(int i = -32)
        : B(i)
    {
    }
    B f(B ob) { return x + ob.x - 1; }
};
int main()
{
    B *p1 = new D, *p2 = new B, *p3 = new B(p1->f(*p2));
    p3->afisare();
    return 0;
}
```

*Soluție*

**Nu compilează.**

>[!info] Explicație 
>Neintuitiv, `p1->f(*p2)` pare ok, pentru că ar returna un `B` care poate fi convertit implicit la `int`. Problema apare, în schimb, la `return x + ob.x - 1`. 
>
>`ob` este `*p2`, are este un obiect de tip `B`, având `x` protected, deci inaccesibil. (chiar dacă `p1` este obiect derivat, el are acces la datele protected doar din bază, nu a oricărui obiect de tip derivat egal cu baza)
>
>Modificări posibile: 
>- adăugăm `friend class D;` în interiorul lui `B` (cred că e cel mai bine așa)
>- schimbăm `protected` în `public` (vom avea `x` public).
>- adăugăm getter pentru `x` și folosim acea funcție.

### Ex. 9

```cpp 
#include <iostream>
using namespace std;
class B {
protected:
    static int x;
    int i;

public:
    B()
    {
        x++;
        i = 1;
    }
    ~B() { x--; }
    static int get_x() { return x; }
    int get_i() { return i; }
};
int B::x;
class D : public B {
public:
    D() { x++; }
    ~D() { x--; }
};
int f(B* q)
{
    return (q->get_x()) + 1;
}
int main()
{
    B* p = new B[10];
    cout << f(p);
    delete[] p;
    p = new D;
    cout << f(p);
    delete p;
    cout << D::get_x();
    return 0;
}
```

*Soluție*

**Compilează.** Va afișa `1131`.

>[!info] Explicație 
>- Avem variabilă statică care este definită `int B::x;`. Ea nu este inițializată cu valoare, deci compilatorul o inițializează cu valoarea `0`. 
>- `new B[10]` apelează constructorul lui B de 10 ori, deci `x = 10`.
>- `f(p)` va afișa $10 + 1 = 11$ 
>- `delete[] p` apelează destructorul de 10 ori, deci `x = 0`
>- `new D` apelează constructorul pentru bază și pentru derivată, deci vom avea de 2 ori `x++`. Deci `x = 2`.
>- `f(p)` va afișa $2 + 1 = 3$
>- Se apelează doar destructorul pentru D, deci `x` scade doar o dată. 
>- `x = 1`, deci se va afișa doar 1
>- nu sunt spații și newlines, deci vom avea `1131`

### Ex. 10 

```cpp
#include <iostream>
using namespace std;
template <class T, class U>
T f(T x, U y)
{
    return x + y;
}
int f(int x, int y)
{
    return x - y;
}
int main()
{
    int *a = new int(3), b(23);
    cout << *f(a, b);
    return 0;
}
```

*Soluție*

**Compilează.** Afișează 0, garbage sau dă runtime error. 

>[!info] Explicație 
>- `a` este `int*`, iar `b` este `int`. Nu se face conversie de la `int*` la `int`, deci funcția `f(a, b)` va intra pe template.
>- Va fi adunat un `int` la un pointer. Prin aritmetică pe pointeri va fi returnat un pointer la `23` de `sizeof(int)` de locul pointat de `a`. 
>- `*f(a,b)` diferențiază acel pointer și dă o valoare garbage (de obicei 0).

### Ex. 11

```cpp 
#include <iostream>
using namespace std;
class A {
    int x;

public:
    A(int i = 0) { x = i; }
    A operator+(const A& a) { return x + a.x; }
    template <class T>
    ostream& operator<<(ostream&);
};
template <class T>
ostream& A::operator<<(ostream& o)
{
    o << x;
    return o;
}
int main()
{
    A a1(33), a2(-21);
    cout << a1 + a2;
    return 0;
}
```

*Soluție*

**Nu compilează.**

>[!info] Explicație
>Nu compilează pentru că operatorul `<<`. Pentru a executa `cout << a1 + a2` (Argumentele ar trebui să fie, atunci, `ostream& o, const A& a`.
>- Este funcție membru, cu singurul argument `ostream& 0`
>- Deci trebuie apelat ca `obA.operator<<(ostream& o)`
>- Adică `obA << ostream`
>- Deci trebuie să schimbăm în `(a1 + a2)<<cout`. 
>- Nu merge pentru că template-ul nu poate fi particularizat, așa că eliminăm și `template <class T>`

În același sens, nici 

```cpp 
#include <iostream>
using namespace std;

template <typename T>
void f() {
    cout << "a";
}

int main() {
    f();
}
```

Nu compilează. 

## Ex. 12 

```cpp 
#include <iostream>
using namespace std;
class cls {
    int x;

public:
    cls(int i) { x = i; }
    int set_x(int i)
    {
        int y = x;
        x = i;
        return y;
    }
    int get_x() { return x; }
};
int main()
{
    cls* p = new cls[10];
    int i = 0;
    for (; i < 10; i++)
        p[i].set_x(i);
    for (i = 0; i < 10; i++)
        cout << p[i].get_x();
    return 0;
}
```

*Soluție*

**Nu compilează.**

>[!info] Explicație 
> Problema vine de la `new cls[10]` care echivalează cu crearea a 10 obiecte `cls` prin constructorul fără parametri. Nu există un astfel de constructor, deci nu compilează.
> 
> Este suficient să facem constructorul existent într-un constructor cu valoare implicită `cls(int i = 0)`

### Ex. 13

```cpp 
#include <iostream>
using namespace std;

int f(int y)
{
    try
    {
        if (y > 0)
            throw y;
    }
    catch (int i)
    {
        throw;
    }
    return y - 2;
}
int f(int y, int z)
{
    try
    {
        if (y < z)
            throw z - y;
    }
    catch (int i)
    {
        throw;
    }
    return y + 2;
}
float f(float &y)
{
    cout << " y este referinta";
    return (float)y / 2;
}
int main()
{
    int x;
    try
    {
        cout << "Da-mi un numar par: ";
        cin >> x; //se va citi numarul 2
        if (x % 2)
            x = f(x, 0);
        else
            x = f(x);
        cout << "Numarul " << x << " e bun!" << endl;
    }
    catch (int i)
    {
        cout << "Numarul " << i << " nu e bun!" << endl;
    }
    return 0;
}
```

*Soluție*

**Compilează**. Afișează

```
Da-mi un numar par: Numarul 2 nu e bun!
```

>[!info] Explicație 
>`x = 2`, este apelat `f(2)`. Pentru ca `2>0` se face `throw 2`. Este prins, dar se face `throw` din 2, e prins in blocul de `catch` din main.

### Exercițiul 14 

```cpp 
#include <iostream>
using namespace std;

class A {
    int x;

public:
    A(int i) { x = i; }
    int get_x() { return x; }
    int& set_x(int i) { x = i; }
    A operator=(A a1)
    {
        set_x(a1.get_x());
        return a1;
    }
};
class B : public A {
    int y;

public:
    B(int i)
        : A(i)
    {
        y = i;
    }
    void afisare() { cout << y; }
};
int main()
{
    B a(112), b, *c;
    cout << (b = a).get_x();
    (c = &a)->afisare();
    return 0;
}
```

*Soluție*

**Nu compilează**.

>[!info] Explicație 
>Nu există constructor fără parametri în B, pentru a construi `b`. E suficient să adăugăm `B(int i = 0)` pentru a merge.

### Ex. 15

```cpp 
#include <iostream>
using namespace std;

struct cls {
    int x;

public:
    int set_x(int i)
    {
        int y = x;
        x = i;
        return x;
    }
    int get_x() { return x; }
};
int main()
{
    cls* p = new cls[100];
    int i = 0;
    for (; i < 50; i++)
        p[i].set_x(i);
    for (i = 5; i < 20; i++)
        cout << p[i].get_x() << " ";
    return 0;
}
```

*Soluție*

**Compilează**. Afișează

```
5 6 7 8 9 10 11 12 13 14 15 16 17 18 19
```

>[!info] Explicație
>Avem `struct`, deci putem scrie `new cls[100]` fără probleme. 

### Ex 16. 

```cpp 
#include <iostream>
using namespace std;

class A {
protected:
    int x;

public:
    A(int i = 14) { x = i; }
};
class B : A {
public:
    B()
        : A(2)
    {
    }
    B(B& b) { x = b.x - 14; }
    void afisare() { cout << x; }
};
int main()
{
    B b1, b2(b1);
    b2.afisare();
    return 0;
}
```

*Soluție*

**Compilează**. Afișează `-12`.

>[!info] Explicație 
>Avem `protected`, deci `x` e accesibil în B. Moștenirea `private` ar avea efect doar la următoarea moștenire din B. 
>
>La copierea `b2(b1)` se apelează constructorul de copiere și `b2` va avea `x = 2 - 14 = -12`.

### Ex. 17

```cpp
#include <iostream>
using namespace std;

class A {
protected:
    static int x;

public:
    A(int i = 0) { x = i; }
    virtual A schimb() { return (7 - x); }
};
class B : public A {
public:
    B(int i = 0) { x = i; }
    void afisare() { cout << x; }
};
int A::x = 5;
int main()
{
    A* p1 = new B(18);
    *p1 = p1->schimb();
    ((B*)p1)->afisare();
    return 0;
}
```

*Soluție*

**Compilează**. Afișează `-11`.

>[!info] Explicație
>`virtual A schimb() { return (7 - x);` va face conversia de la int la `A`. Avem `7 - 18 = -11`. Deci `p1` va pointa la un obiect cu `x = -11`.

### Ex. 18 

```cpp 
#include <iostream>
using namespace std;

template <class T, class U>
T fun(T x, U y)
{
    return x + y;
}
int fun(int x, int y)
{
    return x - y;
}
int fun(int x)
{
    return x + 1;
}
int main()
{
    int *a = new int(10), b(5);
    cout << fun(a, b);
    return 0;
}
```

*Soluție* - asemănător cu [[Courses/OOP/poo.exam#Ex. 10\|10]].

**Compilează**. De data asta afișează o adresă

```
a + 5 * sizeof(int)
```

### Ex. 19 

```cpp 
#include <iostream>
using namespace std;

class B {
protected:
    int x;

public:
    B(int i = 12) { x = i; }
    virtual B f(B ob) { return x + ob.x + 1; }
    void afisare() { cout << x; }
};
class D : public B {
public:
    D(int i = -15)
        : B(i - 1)
    {
        x++;
    }
    B f(B ob) { return x - 2; }
};
int main()
{
    B *p1 = new D, *p2 = new B, *p3 = new B(p1->f(*p2));
    p3->afisare();
    return 0;
}
```

*Soluție*

**Compilează.** Afișează `-17`.

### Ex. 20 

```cpp 
#include <iostream>
using namespace std;

struct B {
    int i;

public:
    B() { i = 1; }
    virtual int get_i() { return i; }
} a;
class D : virtual public B {
    int j;

public:
    D() { j = 2; }
    int get_i() { return B::get_i() + j; }
};
class D2 : virtual public B {
    int j2;

public:
    D2() { j2 = 3; }
    int get_i() { return B::get_i() + j2; }
};
class MM : public D2, public D {
    int x;

public:
    MM() { x = D::get_i() + D2::get_i(); }
    int get_i() { return x; }
};
{
    MM b;
}
int main()
{
    B* o = new MM();
    cout << o->get_i() << "\n";
    MM* p = dynamic_cast<MM*>(o);
    if (p)
        cout << p->get_i() << "\n";
    D* p2 = dynamic_cast<D*>(o);
    if (p2)
        cout << p2->get_i() << "\n";
    return 0;
}
```

*Soluție*

**Nu compilează.**

>[!info] Explicație
>Nu poți avea blocuri `{}` în afara unei funcții. Putem elimina `{MM b;}`

### Ex. 21

```cpp 
#include <iostream>
using namespace std;

class A {
public:
    int x;
    A(int i = -13) { x = i; }
};
class B : virtual public A {
public:
    B(int i = -15) { x = i; }
};
class C : virtual public A {
public:
    C(int i = -17) { x = i; }
};
class D : virtual public A {
public:
    D(int i = -29) { x = i; }
};
class E : public B, public D, public C {
public:
    int y;
    E(int i, int j)
        : D(i)
        , B(j)
    {
        y = x + i + j;
    }
    E(E& ob) { y = ob.x - ob.y; }
};
int main()
{
    E e1(5, 10), e2 = e1;
    cout << e2.y;
    return 0;
}
```

*Soluție*

**Compilează.** Afișează `-15`.

### Ex. 20 

```cpp 
#include <iostream>
using namespace std;
#include <typeinfo>

class B {
    int i;

public:
    B(int x) { i = x + 1; }
    int get_i() { return i; }
};
class D : public B {
    int j;

public:
    D()
        : B(1)
    {
        j = i + 2;
    }
    int get_j() { return j; }
};
int main()
{
    B* p = new D[10];
    cout << p->get_i();
    if (typeid((B*)p).name() == "D*")
        cout << ((D*)p)->get_j();
    return 0;
}
```

*Soluție*

**Nu compilează.**

>[!info] Explicație 
>`i` e privat, deci nu poate fi accesat în `j = i + 2`. Putem să îl facem protected. 

