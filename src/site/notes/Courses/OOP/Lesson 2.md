---
{"dg-publish":true,"permalink":"/courses/oop/lesson-2/"}
---


## Alte completări aduse de C++

```ad-index
```
### [[Lesson 1#Lesson 2 Principiile POO Principiile POO|Clase]]

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

#### Scope resolution operator `::`

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

| Avem acces? | Public | Private | Protected |
| ----------- | ------ | ------- | --------- |
|             |        |         |           |

		




