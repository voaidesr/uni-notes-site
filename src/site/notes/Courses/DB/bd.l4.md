---
{"dg-publish":true,"permalink":"/courses/db/bd-l4/"}
---

# Limbajul de manipulare al datelor (LMD) . Limbajul de control al datelor (LCD)

## LMD 

Permit:
- regăsirea datelor (SELECT)
- adăugarea de noi inserări (INSERT)
- modificarea valorilor coloanelor (UPDATE)
- adăugarea sau modificarea condiționată de înregistrări (MERGE)
- suprimarea de înregistrări (DELETE)

O **tranzacție** este o unitate logică de lucru, compusă dintr-o secvență de comenzi SQL care trebuie executate **atomic**, pentru a menține consistența bazei de date.

**Server-ul Oracle** garantează consistența datelor prin mecanismul tranzacțiilor, chiar și în cazul apariției unor anomalii de sistem sau proces. Acestea oferă flexibilitate și control sporit asupra modificărilor datelor.

## LCD 

Comenzile sunt: 
- ROLLBACK - pentru a renunța la comenzile care sunt în așteptare
- COMMIT - determină încheierea tranzacției curente și permanentizarea modificărilor care au intervenit pe parcursul acesteia
**Obs:** o comandă LDD determină un `COMMIT` implicit 

- SAVEPOINT - marchează un punct intermediar în procesarea tranzacției 

## Insert

### Inserări mono-tabel

Are următoarea sintaxă simplificată:

```sql
INSERT INTO obiect [AS alias] [ (nume_coloană [, nume_coloană …] ) ]
{VALUES ( {expr | DEFAULT} [, {expr | DEFAULT} …] )
| subcerere}
```

Subcererea specificată în comanda INSERT returnează liniile care vor fi adăugate în tabel.

Dacă în tabel se introduc linii prin intermediul unei subcereri, coloanele din lista SELECT trebuie să corespundă, ca număr și tip, celor precizate în clauza INTO. În absența unei liste de coloane în clauza INTO, subcererea trebuie să furnizeze valori pentru fiecare atribut al obiectului destinație, respectând ordinea în care acestea au fost definite.

Observații (tipuri de date):

- Pentru claritate, este recomandată utilizarea unei liste de coloane în clauza INSERT.
- În clauza VALUES, valorile de tip caracter și dată calendaristică trebuie incluse între ' '. Nu se recomandă includerea între apostrofuri a valorilor numerice, întrucât aceasta ar determina conversii implicite la tipul NUMBER.
- Pentru introducerea de valori speciale în tabel, pot fi utilizate funcții.

Adăugarea unei linii care va conține valori null se poate realiza în mod:
- implicit, prin omiterea numelui coloanei din lista de coloane;
- explicit, prin specificarea în lista de valori a cuvântului cheie null  
  În cazul șirurilor de caractere sau al datelor calendaristice se poate preciza șirul vid ('').

Observații (erori):

Server-ul Oracle aplică automat toate tipurile de date, domeniile de valori și constrângerile de integritate. La introducerea sau actualizarea de înregistrări, pot apărea erori în următoarele situații:
- nu a fost specificată o valoare pentru o coloană NOT NULL;
- există valori duplicat care încalcă o constrângere de unicitate;
- a fost încălcată constrângerea de cheie externă sau o constrângere de tip CHECK;
- există o incompatibilitate în privința tipurilor de date;
- s-a încercat inserarea unei valori având o dimensiune mai mare decât a coloanei corespunzătoare.

### Inserări multi-tabel

Inserarea multi-tabel permite adăugarea de linii în una sau mai multe tabele simultan, pe baza rezultatelor unei subcereri (`SELECT`). Aceasta este utilă pentru evitarea execuției multiple a aceleași subcereri pentru fiecare tabel în parte.

Sintaxa standard `INSERT` suportă două forme:

 Inserare necondiționată:
 
```sql
INSERT ALL
  INTO tabel1 VALUES (...)
  INTO tabel2 VALUES (...)
SELECT ...
```

Pentru condiționate 

```sql
INSERT [ALL | FIRST]
  WHEN condiție1 THEN INTO tabel1 VALUES (...)
  WHEN condiție2 THEN INTO tabel2 VALUES (...)
  [ELSE INTO tabel3 VALUES (...)]
SELECT ...
```

## Exerciții

### Exercițiul 1 

Să se creeze tabelele EMP, DEPT prin copierea structurii și conținutului tabelelor EMPLOYEES, respectiv DEPARTMENTS.

```sql
CREATE TABLE EMP AS SELECT * FROM EMPLOYEES;

CREATE TABLE DEPT AS SELECT * FROM DEPARTMENTS;
```

### Exercițiul 4 

Pentru introducerea constrângerilor de integritate, executați instrucțiunile LDD indicate în
continuare.

```sql
ALTER TABLE EMP
ADD CONSTRAINT pk_emp PRIMARY KEY (EMPLOYEE_ID);

ALTER TABLE DEPT
ADD CONSTRAINT pk_dept PRIMARY KEY (DEPARTMENT_ID);

ALTER TABLE EMP
ADD CONSTRAINT fk_emp_dept
    FOREIGN KEY (DEPARTMENT_ID) REFERENCES DEPT(DEPARTMENT_ID);
```

### Exercițiul 5

 Să se insereze departamentul 300, cu numele Programare în DEPT

Corect:

```sql
INSERT INTO DEPT(DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES(300, 'Programare');
```

Dacă se repetă instrucțiunea, returnează eroare -> încalcă constraint-ul de cheie primară unică.