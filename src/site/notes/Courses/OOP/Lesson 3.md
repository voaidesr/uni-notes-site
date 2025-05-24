---
{"dg-publish":true,"permalink":"/courses/oop/lesson-3/"}
---

[[Courses/OOP/Lesson 3#Struct, Class, Union\|Struct, Class, Union]]
[[Courses/OOP/Lesson 3#Funcții prieten\|Funcții prieten]]
[[Courses/OOP/Lesson 3#Clase prieten\|Clase Prieten]]
[[Courses/OOP/Lesson 3#Funcții Inline\|Funcții Inline]]
[[Courses/OOP/Lesson 3#Constructori și Destructori\|Constructori și destructori]]
[[Courses/OOP/Lesson 3#Constructor de copiere\|Constructor de copiere]]
### Struct, Class, Union 

#### Struct 

Singura diferență între `struct` și `class` este modificatorul de acces, struct fiind **public** by default, iar class fiind **private**.

În C++, struct-urile pot avea funcții membru. Existența `struct asigură compatibilitate cu cod mai vechi.

#### Union 

Membrii sunt publici, dar datele membre folosesc aceeași locație de memorie (se suprascriu).

```cpp
union Value {
    int i;
    float f;
    char c;
};

int main() { 
	Value v; 

	v.i = 42; 
	std::cout << v.i << "\n"; // show 42

	v.f = 2.34;
	std::cout << v.i << "\n"; // garbage, being overwritten
}
```

>[!important] Observații
> - union nu poate moșteni / nu se poate moșteni din union 
> - nu poate avea funcții virtuale, variabile statice, referințe
> - pot avea constructori/destructori doar după c++11

##### Union anonime 

- nu au nume pentru timp și nu pot fi declarate obiecte de respectivul tip 
- folosite pentru a spune compilatorului cum să aloce/proceseze variabilele în memorie 

```cpp
union {
    long l;
    double d;
    char s[4]; // all members share the same memory
};

l = 100000;
cout << l << " ";

d = 123.2342;
cout << d << " "; // overwrites l

strcpy(s, "hi");
cout << s; // overwrites both l and d

```

>[!important] Pentru union anonim
>- nu pot exista funcții
>- nu există specificatori de acces, standard public
>- dacă sunt globale, trebuie menționate ca statice 

### Funcții prieten 

- declarate cu cuvântul cheie `friend`
- sunt utilizate pentru a accesa câmpurile `protected` și `private` ale altor clase
	- overloading operatori
	- funcții I/O
	- porțiuni interconectate 
	
- Funcție prieten pentru **o clasă:**

```cpp
class obj{
	int a, b;
public:
	friend int sum(obj) x);
};

int sum(obj x) {
	return x.a + x.b; // poate accesa, chiar daca sunt private 
}
```

- Funcție prieten pentru **mai multe clase**:

```cpp
class ob2; // trebuie declarat inainte

class ob1{ 
	int x;
public: 
	friend void f(ob1, ob2);
};

class ob2{ 
	int y;
public:
	friend void f(ob1, ob2);
};

void f(ob1 A, ob2 B) { 
	cout << A.x + B.y << "\n";
}
```

- Funcție prieten din **din altă clasă**:

```cpp
class ob2;

class ob1{ 
	int x;
public: 
	void f(ob2);
};

class ob2{ 
	int y;
public:
	friend void ob1::f(ob2);
}

ob1::f(ob2) { 
	cout << this->x + ob2.y << "\n";
}
```

### Clase prieten 

Dacă Y este clasă `friend` a clasei X, atunci toate funcțiile membre ale lui Y au acces la membrii privați ai clasei X.

```cpp
class ob1{
	int x;
public:
	friend class ob2;
}

class ob2{
public:
	void set_x(int a, ob1& ob) { ob.x = a};
}

int main() {
	ob1 A;
	ob2 B;
	B.set_x(5, A); // funcționează
}
```

### Funcții Inline

`inline` este o _sugestie_ pentru compilator de a înlocui apelul funcției cu corpul funcției. 

- poate aduce execuție mai rapidă 
- utilă pentru funcții mici 
- pot fi *implicite* și *explicite*

**Implicite**

- orice funcție definită într-o clasă e automat `inline`

```cpp 
class myclass{
	int a;
public:
	void show() { cout << a << "\n"; } // inline automatic
};
```

**Explicite**

- cu keyword `inline`

```cpp
class myclass{
	int a;
public:
	void show();
}

inline myclass::show() { 
	cout << a << "\n";	
}
```

### Constructori și Destructori

#### **Constructorii**
- inițializează obiecte și efectuează operații prealabile
- are numele clasei 
- **nu pot întoarce valori** 
- obiectele **nu sunt** statice 
- poate avea parametri 

#### Destructorii
- numele clasei precedat de ~
- unic și fără parametri

##### Caracteristici comune 
- nu se specifică tipul returnat la declarare 
- **nu pot fi moșteniți, doar apelați de clase derivate** 
- nu se pot utiliza pointeri către constructori/destructori

>[!caution] Moștenire constructori
> Este posibilă de la C++11
		
### Constructor de copiere 
- creează un obiect preluând valorile corespunzătoare altui obiect
- există **implicit**: bit cu bit, trebui redefinit la date alocate dinamic 

>[!iimportant] Orice clasă are
>- constructor de inițializare
>- constructor de copiere
>- destructor
>- operator de atribuire 

```cpp 
class A{ 
	int a;
	string s;
};

int main(){
	A a; // inițializare
	A b = a; // apel constructor de copiere
	A e(a); // apel constructor de copiere 
	A c; // apel constructor de inițializare
	c = a; // apel constructor de atribuire
}
```

>[!warning]
>Apelul de funcție prin valoare duce la copierea argumentului.
>```cpp
>class A{
>	int* v;
>public:
>	A() { 
>		v = new int[3];
>		v[0] = v[1] = v[2] = 123;
>		cout << "C\n";
>	}
>	~A() {
>		delete[] v;
>		cout << "D\n";
>	}
>	void afis() {
>		cout << v[2] << "\n";
>	}
>};
>void func(A ob) {
>	ob.afis();
>}
>int main() { 
>	A ob1; // a fost apelat constructorul, se afișează C
>	 functie(ob1); // nu avem constructor de copiere definit, se face shallow copy (se copiaza pointer-ul), afiseaza 123, dupa care se apeleaza destructorul copiei (afiseaza D). Este sters v atat in copie cat si in ob1
>	 ob1.afis(); //garbage
>}
>```

Output:

```bash
C
123
D
15925584   // ← garbage (undefined memory)
```

>[!definition] Caz particular: constructor cu un parametru 
> ```cpp
> class X{
> 	int x;
> public:
> 	X(int j) { x = j; }
> 	int get { return x; }
> };
> 
> int main() {
> 	X ob = 99;
> 	cout << ob.get() << endl;
> 	return 0;
> }
> ```
> Constructorul nu este marcat ca `explicit` deci se face conversia implicită
> Se poate evita menționând `explicit` înaintea constructorului

##### Tablouri de obiecte 
- constructor parametrizat cu mai mulți parametri, pot fi inițializați astfel:

```cpp
class X{
int a, b, c;
//
};

X v[3] = {X(0,1,2), X(0, 3, 4), X(8, 9, 10)};
```

- cu un singur parametru, poate fi dată direct valoarea: 
```cpp
class X{
int a;
//
};

X v[3] = {1, 8, 10};
```

##### Ordine de apel 
- (în același scope) constructorii: în ordinea declarării obiectelor, destructorii: în ordine inversă
- constructorii variabilelor globale se declară primii
- dacă o funcție apelează un obiect prin valoare, se apelează **constructorul de copiere** și creează un nou obiect în scope-ul funcției
**Exemple:**

```cpp 
class A {
public: 
	A() { cout << "A "; }
};

class B {
	A a;
public: 
	B() {cout << "B "; }
};

void foo(B ob1) {
	A ob2;
}

int main() { 
	B ob; // afișează A B
	// constructorul lui B se execută doar după ce au fost construiți toți membrii lui B (i.e., A)
}
```

**Constructorul de copiere poate fi rescris**:

```cpp
classname(const classname& o) { 
	// se copiază efectiv datele membre
	// la pointer, deepcopy
}
```

Pot exista și parametri, dar trebuie să fie impliciți.

 >[!info]
 >Pot fi folosiți doar la inițializări 
 

>[!danger] La funcții
>```cpp
>MyClass f() { MyClass obiect; … return obiect; }
>MyClass x = f();
>```
>- În standardul `C++98` - se returnează valoare, implică copiere atât la return, cât și la x = f()
>- Începând cu `C++11` - Se încearcă **Return value optimization (RVO)**: se elimină complet copierea și construiește `x` direct în locul unde `obiect` ar fi fost.
>- Începând cu `C++17`: **fără apeluri de copiere/mutare** (RVO garantat)
>

Exemplificare:

```cpp
#include <iostream>
using namespace std;

class A{
	int x;
public:
	A() {
		cout << "Constructor void" << endl;
	}

	A(int x) {
		this->x = x;
		cout << "Constructor " << x << endl;
	}

	A(const A& ob) { // constructor de copiere
		this->x = ob.x;
		cout << "Copy " << this->x << endl;
	}

};

A procedure() {
	A ob(1);
	return ob;
}

int main() {
	A x = procedure();
	return 0;
}
```

Compilând cu `g++ -std=c++98 -fno-elide-constructors` avem următorul output:

```
Constructor 1
Copy 1
Copy 1
```
	
Compilând cu `g++ -std=c++20 -fno-elide-constructors` 

```
Constructor 1
Copy 1
```

Compilând fără `-fno-elide-constructors` (RVO asigurat):

```
Constructor 1
```

