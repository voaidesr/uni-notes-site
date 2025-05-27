---
{"dg-publish":true,"permalink":"/courses/oop/poo-c5/"}
---

### Index
[[Courses/OOP/poo.c5#Moștenirea\|Moștenirea]]
[[Courses/OOP/poo.c5#Constructorii clasei derivate\|Constructori în moștenire]]
[[Courses/OOP/poo.c5#Redefinirea funcțiilor membre\|Redefinirea funcțiilor membre]]
[[Courses/OOP/poo.c5#Modificatorii de acces la moștenire\|Modificarea de acces la moștenire]]
### Moștenirea 

>[!note] Key terms 
>- prin derivare se obțin **clase derivate**, care moștenesc proprietățile unei **clase de bază**
>- succesiune de derivări -> **ierarhie de clase**
>- clase derivate care au la bază mai multe clase -> **moștenire multiplă**
>- clasă derivată <=> subclasă <=> clasă copil 
>- clasă de bază  <=> superclasă <=> clasă părinte

Particularități ale moștenirii:
- implementarea poate fi comună mai multor clase 
- clasele pot fi extinse, fără a recompila codul care depinde de clasa de bază 
- funcțiile ce utilizează obiecte din clasa de bază pot utiliza și obiecte din clasa derivată 

>[!important]
>Clasele derivate au acces la toți membri **public** și **protected** ai clasei de bază.

>[!todo] Lista de inițializare pentru constructori 
>```cpp
>class Telecomanda {
>	int nr_baterii;
>public:
>	Telecomanda(int nr) = nr_baterii(nr) {}
>} 
>
>class Dispozitiv {
>	int nr_butoane;
>public:
>	Dispozitiv(int nr) = nr_butoane(nr) {}
>}
>class Proiector: public Dispozitiv {
>	Telecomanda telecomanda;
>public:
>	Proiector(int i, int j) : Dispozitiv(i), telecomanda(j) {}
>}
>```

>[!warning] Apelarea constructorilor bazei 
> Se face dacă:
> - nu are constructor default
> - are constructor cu parametri
> - e virtuală

Dacă constructorul bazei are parametri impliciți, putem să nu îl apelăm:

```cpp
class A
{
    int x;
public:
    A(const int a = 0) : x(a) {}
};

class B : public A
{
    int b;
public:
    B(const int x) : b(x) {} // works, B.x = 0
};
```

Tipurile predefinite `int`, `double`, etc. nu au constructori, dar C++ permite tratarea lor asemănător unei clase cu o dată membră și constructor parametrizat ***(pseudo-constructori)***.

### Constructorii clasei derivate

#### Constructorul de Copiere 
- **Nu** este definit constructor de copiere nici pentru bază, nici pentru clasa derivată
	- Se apelează constructorul implicit create de compilator
	- Copiere membru cu membru (shallow copy)
- **Baza are** constructor de copiere, dar **clasa derivată nu**
	- Pentru membrii bazei, se apelează constructorul specific 
	- Pentru membrii specifici clasei derivate, constructor implicit --> shallow copy
- Clasa derivată are constructor de copiere
	- Acesta este folosit pentru copierea integrală a obiectelor
	-
Exemplu:

```cpp
class Dispozitiv  
{  
protected:  
    int nr_butoane;  
public:  
    explicit Dispozitiv(const int n = 0) : nr_butoane(n) {}  
    Dispozitiv(const Dispozitiv& other) : nr_butoane(other.nr_butoane) {}  
};  
  
class Telefon : public Dispozitiv  
{  
    std::string model;  
public:  
    Telefon(const int nr, std::string  model) : Dispozitiv(nr),
											    model(std::move(model)) {}  
    Telefon(const Telefon& other) : Dispozitiv(other), model(other.model) {}  
};
```

#### Ordinea apelării constructorilor 

1. Constructorii clasei de bază 
	- În ordinea în care au fost declarate 
2. Constructorii membrilor din clasa curentă
	- În ordinea în care au fost declarați
3. Constructorul clasei curente

Exemplu:

```cpp
#include<iostream>  
  
#define CLASS(ID) class ID {\  
public:\  
    ID(int) { std::cout << #ID " constuctor\n";}\  
    ~ID() { std::cout << #ID " destructor\n";}\  
};  // macro
  
CLASS(Base1);  
CLASS(Member1);  
CLASS(Member2);  
CLASS(Member3);  
CLASS(Member4);  
  
class Derived1 : public Base1  
{  
    Member1 m1;  
    Member2 m2;  
public:  
    Derived1(int) : m1(1), m2(2), Base1(1)  
    {  
        std::cout << "Derived1 constructor" << std::endl;  
    }  
    ~Derived1() { std::cout << "Derived1 destructor" << std::endl; }  
};  
  
class Derived2 :public Derived1  
{  
    Member3 m3;  
    Member4 m4;  
public:  
    Derived2() : Derived1(1), m3(2), m4(3) { std::cout << "Derived2 constructor" << std::endl; }  
    ~Derived2() { std::cout << "Derived2 destructor" << std::endl; }  
};  
  
int main()  
{  
    Derived2 d2;  
    return 0;  
}
```

Afișează 

```
Base1 constuctor <-- se merge din derivat2 -> derivat1 -> baza
Member1 constuctor
Member2 constuctor <-- membrii derivat1, de-abia dupa construirea derivat1
Derived1 constructor 
Member3 constuctor
Member4 constuctor
Derived2 constructor
Derived2 destructor <-- distrugere in ordine inversa
Member4 destructor
Member3 destructor
Derived1 destructor
Member2 destructor
Member1 destructor
Base1 destructor
```

### Redefinirea funcțiilor membre 

Este premisă supradefinirea funcțiilor membre clasei de bază cu funcții derivate ale clasei derivate
- cu același antet 
	- **redefining*** -> funcții oarecare
	- **overriding*** -> funcții virtuale 
- cu schimbare liste de argumente/tip returna

>[!danger] Eroare posibilă
>La redefinirea unei funcții din clasa de bază, toate celelalte versiuni sunt automat ascunse.

Vezi exemplul: 

```cpp
#include <iostream>
#include <string>
using namespace std;

class Base {
public:
    int f() const {
        cout << "Base::f()\n";
        return 1;
    }

    int f(string) const {
        return 1;
    }

    void g() { }
};

class Derived1 : public Base {
public:
    // Redefinition hides other overloads
    int f() const {
        cout << "Derived1::f()\n";
        return 1;
    }
};

int main() {
    string s("hello");

    Derived1 d1;
    x = d1.f();           // OK: Derived1::f()

    // d2.f(s); // ERROR: Base::f(string) is hidden due to name hiding
}
```

- **Constructorii și destructorii nu sunt moșteniți** automat până în C++11.
  - În versiunile anterioare, trebuie **redefiniți explicit** în clasa derivată.
  - Din C++11, pot fi **moșteniți explicit** cu `using Base::Base;`
- Similar pentru operatorul `=`
```ad-important
Implementarea `=` în clasă derivată
```cpp
class A
{
    int a;
public:
    A& operator=(const A& other)
    {
        if (this != &other)
        {
            a = other.a;
        }
        return *this;
    }
};

class B : public A
{
    int b;
public:
    B& operator=(const B& other)
    {
        if (this != &other)
        {
            this->A::operator=(other); // copie elementele din A
            b = other.b; // copie elementele noi ale clasei derivate
        }
        return *this;
    }
};
```

##### Funcții statice în moșteniri 
- Se moștenesc exact ca o funcție nestatică 
- Nu pot fi virtuale 
Exemplificare:

```cpp
class A  
{  
public:  
    static void f() { std::cout << "Base f" << std::endl; }  
    static void g() { std::cout << "Base g" << std::endl; }  
};  
  
class B : public A  
{  
public:  
    static void f(int) { std::cout << "Derived f(int)" << std::endl; }  
};  
  
int main()  
{  
    // B::f(); // doesn't work due to name hiding   
	B::f(3);  // Derived f(int)
    B::g();  // Base g
    return 0;  
}
```

### Modificatorii de acces la moștenire 

Se scriu astfel:

```cpp
class Derived : public/protected/private Base {}
```

Dacă nu este menționat niciun specificator de acces, moștenirea este `private`.

Modificatorii de acces la moștenire controlează vizibilitatea membrilor moșteniți în clasa derivată.

| Modificator de acces Base | Moștenire public                 | Moștenire protected              | Moștenire private |
| ------------------------- | -------------------------------- | -------------------------------- | ----------------- |
| public                    | public                           | protected                        | inaccesibil       |
| protected                 | protected                        | protected                        | inaccesibil       |
| private                   | private, dar accesibil din clasă | private, dar accesibil din clasă | inaccesibil       |
>[!info]
`Private` a fost adăugat pentru completitudine, dar este mai recomandată utilizarea compunerii în locul moștenirii private.

Dacă în clasa de bază o componentă era publică, iar moștenirea este privată, se poate reveni la public prin `using Base::nume_compontenta`.

>[!proof] Good practice 
>- **Variabilele de instanță ar trebui declarate `private`**, pentru a respecta principiul încapsulării.  
>- **Funcțiile care le accesează sau le modifică** (gettere, settere, logica internă) pot fi declarate `protected`, pentru a permite claselor derivate să le utilizeze fără a expune detalii în afara ierarhiei

### Compatibilitatea între bază și derivată. Conversie

Între tipul clasă derivată și tipul clasă de bază se admite compatibilitate. Este valabilă doar pentru clase **derivate cu acces public** și numai în sensul derivată --> bază.

Compatibilitatea se manifestă sub forma unor **conversii explicite de tip**:
- din obiect derivat în obiect de bază
- din pointer/referință la obiect derivat în pointer/referință la obiect de bază

