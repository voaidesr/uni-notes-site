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

Pentru a corecta, e suficient să adăugăm, după declararea clasei:

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

