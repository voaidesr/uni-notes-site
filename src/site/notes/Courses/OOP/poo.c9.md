---
{"dg-publish":true,"permalink":"/courses/oop/poo-c9/"}
---

# Run-time type identification (RTTI) 

Permite identificarea tipului unui obiect în timpul execuției programului. 
- acesta nu există în limbaje nepolimorfice (e.g., C), pentru că tipul este cunoscut de la compilare
- C++ implementează polimorfismul prin [[Courses/OOP/poo.c5#Moștenirea\|moștenire]], [[Courses/OOP/poo.c6#Funcții virtuale\|funcții virtuale]] și pointeri la clasa de bază care pot fi utilizați pentru a arăta către obiecte derivate $\implies$ tipul obiectului nu poate fi cunoscut *a priori* $\implies$ determinarea se face la execuție, prin RTTI.  

## `type_id`

Avem header-ul `<typeinfo>`, care conține tipul `type_info`.

>[!important] 
>Funcția `typeid`
>```cpp
>typeid(object);
>```
>Returnează o referință către un obiect de tipul `type_info`, care descrie tipul obiectului.

>[!danger] eroare 
>```cpp
>typeid(NULL);
>```
>Generează eroare de tipul `bad_typeid`. 

### Clasa `type_info` 

Membri publici:
- operatorii `==` și `!=` -> pentru a verifica dacă două obiecte au sau nu același tip. 
- `bool before(const type_info& ob)` - verifică dacă un obiect precede altul
- `const char *name()` - depinde de compilator, dar conține și tipul obiectului

>[!proof]
>Cea mai importantă utilizare a `type_id` este pentru tipuri polimorfice. 

Exemplu:

```cpp
class B
{
public:
    virtual void f() {} // tip polimorfic;
};

class D1 : public B {};
class D2 : public B {};

int main()
{
    B *p, b;
    D1 d1;
    D2 d2;
    p = &b;
    cout << typeid(*p).name() << endl; // afis 1B
    p = &d1;
    cout << typeid(*p).name() << endl; // afis 2D1
    p = &d2;
    cout << typeid(*p).name() << endl; // AFIS 2D2
    return 0;
}
```

***Altă utilizare***: `typeid(type-name`

```cpp
if(typeid(ob) == typeid(int)){
// verifica daca ob e int
}
```

>[!warning]
>RTTI nu funcționează cu `void*`, pentru că nu au informație de tip. 

## Operatorii de cast 

- operatorul tradițional moștenit din C 
- `dynamic_cast`
- `const_cast`
- `reinterpret_cast`
- `static_cast`

### `dynamic_cast`

Schimbă tipul unui pointer/referință într-un alt tip de pointer/referință.
- dacă vrem să schimbăm tipul *la execuție*
- **se verifică dacă downcast e valid**, dacă nu e valid, *eroare*

Sintaxa:

```cpp
dynamic_cast<target-type> (expr);
```

Unde `target-type` trebuie să fie pointer sau referință. 

În general `dynamic_cast` reușește dacă pointer-ul/referința de transformat este un pointer/referință către `target_type` sau către un tip derivat. 

La eșuare: 
- se evaluează cu `null` la pointeri
- cu `bad_cast` exception la referințe

```cpp
class B
{
public:
    virtual void f() {} // tip polimorfic;
};

class D1 : public B {};
class D2 : public B {};
class D3 : public D1 {};


int main()
{
    B* b = new D1();
    B* b2 = new D2();
    B* b3 = new D3();
    cout << dynamic_cast<D1*>(b) << endl; // adresa
    cout << dynamic_cast<D1*>(b2) << endl; // 0 - nullptr
    cout << dynamic_cast<D1*>(b3) << endl; // adresa
    return 0;
}
```

Iar cu referințe 

```cpp
	D1 d1;
    D2 d2;
    D3 d3;

    B& b = d1;
    B& b2 = d2;
    B& b3 = d3;
    
    dynamic_cast<D1&>(b); // merge
    dynamic_cast<D1&>(b2); // generează std::bad_cast
    dynamic_cast<D1&>(b3); // merge 
```

`dynamic_cast` înlocuiește `typeid`. 

### `static_cast`

- substituent pentru operatorul de cast clasic 
- funcționează pe tipuri nepolimorfice 
- poate fi folosit pentru orice conversie standard 
- nu se fac verificări la execuție 

Sintaxa 

```cpp
static_cast <type> (expr);
```

### `const_cast`

- elimină proprietatea de a fi constant 
- tipul destinație trebuie să fie același ca tipul sursă 

```cpp
void sqrvalue(const int* value) {
	int *p;
	p = const_cast <int*> (value);
	*p = *val * *val;
}

// cu referinte 
void sqrvalue(const inst& value) {
	const_cast <int&> (value) = value * value;
}
```

### `reinterpret_cast`

- convertește un tip într-un alt tip fundamental diferit 

Sintaxa:

```cpp
reinterpret_cast<type>(p);
```

# Șabloane 

## Funcții generice 

- implementarea aceleiași logici, pentru mai multe tipuri de date, printr-un șablon 

Sintaxa:

```cpp
template <class Ttype> tip_returnat nume_functie(args){
// corpul functiei
} 

sau 

template <typename Ttype> // same
```

`Ttype` - nume pentru tipul de date folosit, compilatorul îl va înlocui cu tipul de date folosit. 

>[!danger] Eroare
>Specificația de template trebuie să fie fix înaintea declarării funcției, dacă nu, eroare. 

Pot exista funcții cu mai mult de un tip generic:

```cpp
template <typename T1, typename T2>
```

### Overload pe templates

- șablon -> overload implicit 
- se poate face și overload explicit -> ***specializare explicită*** 

```cpp
template <typename T> 
T maxim (T a, T b) {}

template <> 
string maxim (string a, string b) {} // specializare pe string
```

>[!warning] 
>Între pointer și pointer constant nu se face conversie automată.

```cpp
template <typename T> 
T maxim(T a, T b) {}

template<>
char* maxim(char* a, char* b) {}

template<>
const char* maxim(const char* a, const char* b) {}

int main() {
	char v1[] = "abc";
	char v2[] = "cde"
	maxim(v1, v2); // daca nu exista template<> pt char* se apela template general
	maxim("abc", "cde"); // daca nu exista template pt const chat* se apela general

// NU se face conversia de la const la const char*
}
```

*Overload cu mai mulți parametri* -> la fel ca la funcții normale 

```cpp
template <typename X> 
void f(X a) {}

template <typename X, typename Y> 
void f(X x, Y y) {}
```

### Ordinea de alegere a funcțiilor 

*Pasul 1* Potrivire fără conversie 
Se caută o funcție care să se potrivească fără să se facă conversie de tip
- varianta non-template 
- template fără parametri 
- template cu un singur parametru
- template cu doi parametri 

*Pasul 2* Dacă nu există potrivire exactă, se încearcă găsirea unei funcții prin conversie de tip. (Doar la varianta **non-template**).

Exemplu:

```cpp
template <typename T>
void f(T a)
{
    std::cout << "general" << std::endl;
}

template <>
void f(float a)
{
    std::cout << "template float" << std::endl;
}

void f(float a)
{
    std::cout << "float" << std::endl;
}

int main()
{
    f(1); // int -> general
    f(2.5); // double -> general
    float a = 1.5; 
    f(a); // float -> va afisa float non-template
    f<>(a); // template<> prioritar față de template general cu T = float
    f<float>(a); // same
    return 0;
}
```

## Clase generice 

- șabloane pentru clase, nu pentru funcții

exemple: cozi, stive, liste înlănțuite, arbori de sortare 

```cpp
template <class Ttype> 
class ClassName {
//
};

// declararea unei clase
ClassName <type> ob;
```

pot fi și mai multe tipuri:

```cpp
template <typename T1, T2> 
class A{
	T1 a;
	T2 b;
//
};

int main(){
	A <int, double> ob;
}
```

În definirea șabloanelor, se pot specifica și argumente **valori**, nu numai argumente tip.

```cpp
template <typename T1, typename T2, int N>
```

*Specializare* la fel ca la funcții, cu `template<>`

```cpp
template <typename T> 
class X{
	T x;
}

template <>
class X<int> {
	int x;
}
```

>[!tip] Tip default
>Putem avea valori default pentru tipurile parametrizate. 

```cpp
template <typename T = int> 
class myclass{
//
}
```

### Moștenire

Putem avea moștenire

```cpp
template <typename T> 
class B {};

template <typename T>
class D : public B<T> {};
```
### `typeid` și clase template 

- fiecare combinație de tipuri duce la clase diferite generate de compilator. 

```cpp
template <typename T>
class A {};

int main()
{
    A<int> a; 
    A<double> b;
    A<float> c;
    A<A<float>> d;
    std::cout << typeid(a).name() << std::endl; 
    std::cout << typeid(b).name() << std::endl;
    std::cout << typeid(c).name() << std::endl;
    std::cout << typeid(d).name() << std::endl;
}
```

Output-ul depinde de compilator, dar duce la tipuri diferite:

```
1AIiE
1AIdE
1AIfE
1AIS_IfEE
```

### `dynamic_cast` și clase template 

2 instanțe de tipuri diferite au fost create cu date diferite. 

>[!warning]
>Nu se poate folosi `dynamic_cast` pentru a schimba tipul unui pointer dintr-o instanță în tipul unui pointer dintr-o instanță diferită.

