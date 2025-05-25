---
{"dg-publish":true,"permalink":"/courses/oop/lesson-4/"}
---

### Index
[[Courses/OOP/Lesson 4#Variabile Statice\|Variabile Statice]]
[[Courses/OOP/Lesson 4#Supraîncărcarea operatorilor\|Supraîncărcarea operatorilor]]
### Variabile Statice 

Sunt variabile care își **păstrează valoarea** între apeluri și pentru care există o singură copie în memorie.

```cpp
void f();
	static int x = 10;
	cout << x << "\n";
	x++;
}

int main(){ 
	f(); // 10
	f(); // 11
	f(); // 12
}
```

### Obiecte statice 

Se inițializează o singură dată și își păstrează valoarea. (Generalizare la variabile). 

De 2 tipuri:
- locale: distrus la finalul programului, dar vizibil **doar** în scope-ul său
- globale: ultimele distruse

```cpp
class A{
    int a;
public:
    A(int x = 0): a(x) { cout << "constr " << a << endl; }
    ~A() { cout << "destr " << a << endl; }
};

void f(int i) {
    static A obiect(i);
    cout << &obiect << endl;
}

int main() {
    f(6); // constr 6 și adresa
    f(2); // de aici se afișează doar adresa, pentru că obiectul e deja creat
    f(3); // aceeeași adresa 
}
```

---
### `static` în clase 

#### Date membre 

Într-o clasă, datele membre pot fi:
- nestatice (distincte pentru fiecare obiect)
- **statice** (unice pentru toate obiectele, există doar o copie în memorie)

**Datele membre statice** sunt create, inițializate și accesate independent de obiectele clasei.

>[!warning]
>Datele membre statice `const` trebuie să fie inițializate în clasă 
>Datele membre care **nu** sunt constante  trebuie inițializate în afara clasei

Dă eroare de compilare: 

```cpp
class A{
public:
	static int x = 10;
};
```

**Referirea la membrii statici** se face fie prin `nume_clasa::membru` fie prin `obiect.membru` (la fel ca pentru membrii nestatici). 
#### Funcții 

Funcțiile statice:
- efectuează operații asupra întregii clase
- nu au cuvântul cheie `this` 
- se pot referi **doar** la membrii statici

```cpp
class A{
    static int x;
public:
    static void set_x(int a) {x = a;}
    static void show_x() { cout << x << endl; }
};

int A::x; // define x

int main() {
    A::set_x(10);
    A::show_x();
    return 0;
}
```

---
### Operatorul de rezoluție de scop `::` [[Lesson 2#Scope resolution operator ` ` Lesson 4 Operatorul de rezoluție de scop ` ` See here|see more]]

Poate fi folosit și astfel:

```cpp
int i = 10;

int main() {
    int i = 5;
    cout << i << endl; // 5, accesează local
    cout << ::i << endl; // 10, accesează global
}
```

### Clase locale 

- pot fi definite clase în clase/clase în funcții
-  `class` este o declarație (definește un scope) 

#### Nested classes (clase imbricate) 
```cpp
class X {
public:
    class Test {
        static int A;
        int x;
    public:
        void set(int i);     // declarat aici
        int get() { return A + x + global; } // definit aici → OK
    } ob2;
};

int X::Test::A = 90;
void X::Test::set(int i) { x = i; } // DEFINIRE în afara ambelor clase → OK
```

- funcțiile declarate în clasa interioară pot fi definite fie chiar în clasă, fie în afara clasei **exterioare**. **NU** merge definirea în afara clasei interioare dar în interiorul clasei exterioare. 
- pot fi accesate variabile **globale**
- se poate folosi `static`

#### Clase locale (în funcții) 
- **NU** pot exista date membre `static` dar funcțiile statice sunt permise;
- **NU** pot fi definite funcții membru în afara clasei;
- **NU** poți accesa variabile locale;
- Pot fi accesate **variabile locale statice** și globale; 

### Funcții care întorc obiecte 

Funcțiile pot întoarce obiecte:
```cpp
MyClass create() { 
	MyClass obj;
	return obj;
}
```

Obiectul întors este un obiect temporar creat automat. Se face prin **constructorul de copiere** *(sau prin move constructor în C++11+).* 

După ce valoarea a fost întoarsă, acest obiect este distrus.

>[!danger] Pericole
>Dacă obiectul returnat conține pointeri sau alocări dinamice trebuie:
> - să existe constructor de copiere bine definit (deepcopy)
> - să fie gestionat **corect polimorfismul** dacă ai clase de bază/derivate
>>[!tip] Soluții
>> implementare constructor de copiere și operator de atribuire (=) 

--- 

### Supraîncărcarea operatorilor 

- Majoritatea operatorilor pot fi supraîncărcați
- Supraîncărcarea se face definind o funcție operator (poate fi sau nu membră clasei)

>[!caution] Restricții
> - nu se poate redefini precedența operatorilor 
> - nu se poate redefini numărul de operanzi
> - nu putem avea valori implicite (*excepție pentru `()`*)
> - nu se poate face overload pentru:
> 	- `.` (acces de membru, e.g, ob.func())
> 	- `::` (scope resolution)
> 	- `.*` (acces membru prin pointeri) 
> 	- `?` (ternar)
> 	- `sizeof()`

#### Funcții operator membri ai clasei 

>[!abstract] Se pot defini astfel:
>```cpp
>ret_type class_name::operator#(args) {
>// operations
>} 
>```
>Unde `#` este operatorul pe care vrem să îl supraîncărcăm. 

- Pentru operatori unari: `args` este vid 
- Pentru operatori binari: `args` conține un singur element 

```cpp
class loc {
    int longitude;
    int latitude;
public:
    loc() {}
    loc(int, int);
    void show();
    // supraincarcare operator 
    loc operator+(const loc& ob2);
};

loc::loc(int lat, int lon) : longitude(lon), latitude(lat) {}
void loc::show() {
    cout << "latitude: " << latitude << " "
        << "longitude: " << longitude << endl;
}
loc loc::operator+(const loc& ob2) {
    loc temp;
    temp.latitude = ob2.latitude + latitude; // latitude este this->latitude 
    temp.longitude = ob2.longitude + longitude;
    return temp;
}

int main() {
    loc ob1(10, 20);
    loc ob2(20, 30);
    ob1 = ob1 + ob2; // putem avea expresii pentru ca se intoarce acelasi tip
    ob1.show();
    return 0;
}
```

>[!hint] Supraîncărcare `=`
>Apelul la funcția de operator se face de către obiectul din stânga. (în general la operatorii binari)
>- operatorul `=` copie variabilele, întoarce `*this`
>- se pot face atribuiri multiple (de la dreapta spre stânga)

#### Prefix și postfix pentru `++`

Pentru a avea `++ob` se definește

```cpp
type operator++(){
//
}
```

Iar pentru `ob++` se definește 

```cpp
type operator++(int){ // int - dummy parameter, pentru a face distinctia
//
}
``` 

Exemplu, pentru clasa `loc`:

```cpp
loc& loc::operator++(){ // notatie prefixata
    longitude++;
    latitude++;
    return *this; // increases, then returns
}

loc loc::operator++(int) {
    loc temp = *this;
    longitude++;
    latitude++;
    return temp; // returns value before increase
}
```

>[!important]
>Operatorii, mai puțin `=`, sunt moșteniți. Clasa derivată poate să redefinească operatorii. 

#### Supraîncărcarea operatorilor ca funcții `friend`

- operatorii pot fi definiți și ca funcție care nu e membră a clasei 
- o facem funcție `friend`
- nu mai există pointerul `this`, e nevoie de toți operanzii ca parametrii

Exemplu:

```cpp
class loc{
	int latitude, longitude;
public:
	loc friend operator-(const loc& ob1, const loc& ob2); 
	// declaration as friend
}

// definition
loc operator-(const loc &ob1, const loc &ob2) {
    loc temp;
    temp.latitude = ob1.latitude - ob2.longitude;
    temp.longitude = ob2.longitude - ob2.longitude;
    return temp;
}
```

>[!danger] Restricții supraîncărcare cu `friend`
>- nu se pot supraîncărca `=`, `()`, `[]`, `->` cu funcții prieten 
>- pentru `++` și `--` trebuie referințe 

>[!note] Membru vs `friend`
> - unari, compuși --> funcții membru 
> - când diferă ordinea operatorilor --> funcții prieten

#### Alți operatori 
##### 1. Spaceship

>[!tip] Spaceship operator 
> Operatorul `<=>` apare în `C++20`, se supraîncarcă toți operatorii de comparare „în același tip”

```cpp
#include <compare> 

class X{
	friend auto operator<=>(const X&, const X&) = default;
};
```

##### 2. New și delete

>[!tip] `new` și `delete`
>Pot fi supraîncărcați atât la nivel de clasă cât și la nivel global
>
>Pentru `new`
>```cpp
>void* operator new (size_t size){
>	// perform allocation, throw bad_alloc on failure 
>	// constructor called automatically
>	return pointer_to_memory
>}
>```
>Pentru delete: 
>```cpp
>void operator delete (void* p) {
>	// free memory pointed to by p
>	// destructor called automatically
>}
>```
>Iar pentru **array-uri**
>```cpp
>void* operator new[] (size_t size);
>void operator delete[] (void* p);
>```

##### 3. Operatorul `[]`

>[!warning] Supraîncărcarea `[]`
>- trebuie să fie funcție membru (nestatică)
>- nu poate fi `friend`
>- **operator binar**
>- `o[3]` <=> `o.operator[](3)`

Definire:

```cpp
type class_name::operator[](int i){
//
}
```

##### 4. Operatorul `()`

Permite ca obiectul să fie apelat precum o funcție. 

- `O(1, 10)` <=> `O.operator()(1, 10)`

```cpp
class A{
public:
	int operator()(int x) const {
		return x*x;
	}
} ob;

int result = A(5); // result = 25
```

##### 5. Operatorul `->`
- este un operator unar 
- `object->element`:
	- obiectul generează apelul <=> `object.operator->()->element`
	- elementul trebuie să fie accesibil prin pointer 
	- `operator->` întoarce un pointer către un obiect din clasă 
	
```cpp
class Inner {
public:
    void greet() { std::cout << "Hello from Inner\n"; }
};

class Wrapper {
    Inner inner;
public:
    Inner* operator->() { return &inner; }
};

int main() {
    Wrapper w;
    w->greet();  // works because w.operator->() returns a pointer to Inner
}
```

##### 6. Operatorul `,` 
- operator binar
- În mod implicit: evaluează operandul stâng, apoi operandul drept și returnează valoarea celui din dreapta
- Altfel spus, sunt ignorate toate valorile, cu excepția celui mai din dreapta operand
	- `ob1 = (ob2, ob3, ob4)` <=> `ob1` va fi egal cu `ob4`
- La suprascriere: se poate implementa logică personalizată între cei doi operanzi

```cpp
class_name operator,(const class_name& right_class){
///
}
```

