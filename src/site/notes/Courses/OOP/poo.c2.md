---
{"dg-publish":true,"permalink":"/courses/oop/poo-c2/"}
---

[[Courses/OOP/poo.c2#Lesson 1 Lesson 2 Principiile POO Principiile POO Clase]\|Clase]]
[[Courses/OOP/poo.c2#Scope resolution operator\|Scope resolution operator]]
[[Courses/OOP/poo.c2#Principiile POO\|Principiile POO]]

## Alte completări aduse de C++

### [[Courses/OOP/poo.c1#Lesson 2 Principiile POO Principiile POO\|Clase]]

Specificatorii de acces pot fi alternați

```cpp
class employee { 
// private din oficiu
	char name[80]; 
public: 
// acestea sunt  publice
	void putname(char *n); 
	void getname(char *n); 
private: 
// acum din nou  private 
	double wage; 
public: 
// înapoi la public 
	void putwage(double w); 
	double getwage(); 
};
```

```ad-caution
1) Nu putem avea ca membri într-o clasă obiecte de tipul clasei (putem avea pointeri la tipul clasei) 
2) nu se folosește auto, extern, register
```

#### Scope resolution operator `::` [[poo.c4#Operatorul de rezoluție de scop ` `|See here]]

`::` Este folosit pentru a accesa un membru care aparține unui anumit namespace, clase, structură sau pentru a defini funcții în afara clasei.

```cpp
class A{
public:
	void show();
};

class B{
public:
	void show();

void A::show() { // defines the method in A
	cout << "A" << endl;
}

void B::show() { // defines the method in B
	cout << "B" << endl;
}
```

### Principiile POO

##### Încapsularea 

- Data hiding 
- sunt separate aspectele externe ale unui obiect (care pot fi accesate de alte obiecte) de cele interne 
- doar metodele proprii pot accesa starea curenta a obiectului

| Avem acces?    | Public | Protected | Private |
| -------------- | ------ | --------- | ------- |
| Aceeași clasă  | da     | da        | da      |
| Clase derivate | da     | da        | nu      |
| Alte clase     | da     | nu        | nu      |
##### Agregarea (ierarhia de obiecte) 
- un obiect este compus din mai multe obiecte mai simple 

```cpp
class Profesor {
	string nume;
	int vechime;
};

class Curs { 
	string denumire;
	Profesor p;
};
```

##### Moștenirea (ierarhia de clase) [[Courses/OOP/poo.c5\|See more]] 
- reprezintă o relație între clase, în care o clasă moștenește structura și comportarea definită în una sau mai multe clase
- utilă pentru **reutilizare de cod**

##### Polimorfisme 
- claritate/cod mai sigur
- poate fi la compilare `max(int)` vs `max(float)`
- la execuție (runtime): **RTTI** (see [[Courses/OOP/poo.c9\|RTTI]])

##### Șabloane (templates) 
- cod mai sigur, reutilizare de cod 
 



