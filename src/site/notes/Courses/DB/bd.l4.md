---
{"dg-publish":true,"permalink":"/courses/db/bd-l4/"}
---

# Limbajul de manipulare a datelor (LMD) . Limbajul de control al datelor (LCD)

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

### Exercițiul 6 

Să se insereze un angajat corespunzător departamentului introdus anterior în tabelul EMP_pnu, precizând valoarea NULL pentru coloanele a căror valoare nu este cunoscută la inserare (metoda implicită de inserare). Determinați ca efectele instrucțiunii să devină permanente.

- Metoda implicită -> nu se scriu coloanele tabelului 

```sql
INSERT INTO EMP
VALUES(10, null, 'Nume',
       'email', null, SYSDATE,
       'SA_REP', null, null,
       null, 300);
       
COMMIT;
```

### Exercițiul 7

Să se mai introducă un angajat corespunzător departamentului 300, precizând după numele
tabelului lista coloanelor în care se introduc valori (metoda explicita de inserare). Se
presupune că data angajării acestuia este cea curentă (SYSDATE). Salvaţi înregistrarea.

```sql
INSERT INTO EMP(EMPLOYEE_ID, LAST_NAME, EMAIL,
                HIRE_DATE, JOB_ID, DEPARTMENT_ID)
VALUES(12, 'Albec', 'alb',
       SYSDATE, 'SA_CLERK', 300);
```

### Exercițiul 8 

Creaţi un nou tabel, numit EMP1_PNU, care va avea aceeaşi structură ca şi EMPLOYEES, dar
fara inregistrari (linii in tabel). Copiaţi în tabelul EMP1_PNU salariaţii (din tabelul EMPLOYEES)
al căror comision depăşeşte 25% din salariu.

```sql
CREATE TABLE EMP1 AS SELECT * FROM EMPLOYEES;

DELETE FROM EMP1; -- sterge totul din tabel

INSERT INTO EMP1
    SELECT * FROM EMPLOYEES WHERE COMMISSION_PCT > 0.25;
```

### Exercițiul 10

Creaţi 2 tabele emp2_pnu şi emp3_pnu cu aceeaşi structură ca tabelul EMPLOYEES, dar
fără înregistrări (acceptăm omiterea constrângerilor de integritate). Prin intermediul unei singure comenzi, copiaţi din tabelul EMPLOYEES:
- în tabelul EMP1_PNU salariaţii care au salariul mai mic decât 5000;
- în tabelul EMP2_PNU salariaţii care au salariul cuprins între 5000 şi 10000;
- în tabelul EMP3_PNU salariaţii care au salariul mai mare decât 10000.
Verificaţi rezultatele, apoi ştergeţi toate înregistrările din aceste tabele.

```sql
CREATE TABLE EMP2 AS SELECT * FROM EMPLOYEES;
DELETE FROM EMP2;

CREATE TABLE EMP3 AS SELECT * FROM EMPLOYEES;
DELETE FROM EMP3;

DELETE FROM EMP1;

INSERT ALL
    WHEN SALARY < 5000 THEN
        INTO EMP1
    WHEN SALARY BETWEEN 5000 AND 10000 THEN
        INTO EMP2
    WHEN SALARY > 10000 THEN
        INTO EMP3
SELECT * FROM EMPLOYEES;

SELECT * FROM EMP1;
SELECT * FROM EMP2;
SELECT * FROM EMP3;
```


### Exercițiul 11 

Să se creeze tabelul EMP0_PNU cu aceeaşi structură ca tabelul EMPLOYEES (fără
constrângeri), dar fără înregistrari. Copiaţi din tabelul EMPLOYEES:
- în tabelul EMP0_PNU salariaţii care lucrează în departamentul 80;
- în tabelul EMP1_PNU salariaţii care au salariul mai mic decât 5000;
- în tabelul EMP2_PNU salariaţii care au salariul cuprins între 5000 şi 10000;
- în tabelul EMP3_PNU salariaţii care au salariul mai mare decât 10000.
Dacă un salariat se încadrează în tabelul emp0_pnu atunci acesta nu va mai fi inserat şi în alt
tabel (tabelul corespunzător salariului său).

```sql
CREATE TABLE EMP0 AS SELECT * FROM EMPLOYEES;
DELETE FROM EMP0;

INSERT ALL
    WHEN DEPARTMENT_ID = 80 THEN
        INTO EMP0
    WHEN SALARY < 5000 THEN
        INTO EMP1
    WHEN SALARY BETWEEN 5000 AND 10000 THEN
        INTO EMP2
    WHEN SALARY > 10000 THEN
        INTO EMP3
SELECT * FROM EMPLOYEES;
```

## Update

Sintaxa simplificată este

```sql
UPDATE nume_tabel [alias]  
SET col1 = expr1[, col2 = expr2]  
[WHERE condiție];
```

Sau

```sql
UPDATE nume_tabel [alias]  
SET (col1, col2, ...) = (subcerere)  
[WHERE condiție];
```

Observații:
- de obicei, pentru identificarea unei linii se folosește o condiție ce implică cheia primară;
- dacă nu apare clauza `WHERE`, atunci sunt afectate toate liniile tabelului specificat;
- cazurile în care instrucțiunea `UPDATE` nu poate fi executată sunt similare celor în care eșuează instrucțiunea `INSERT`. Acestea au fost menționate anterior.

### Exercițiul 12 

Măriţi salariul tuturor angajaţilor din tabelul EMP_PNU cu 5%. Vizualizati, iar apoi anulaţi
modificările.

```sql
UPDATE EMP
SET SALARY = SALARY * 1.05;
```

### Exercițiul 13

Schimbaţi jobul tuturor salariaţilor din departamentul 80 care au comision, în 'SA_REP'.
Anulaţi modificările.

```sql
UPDATE EMP
SET JOB_ID = 'SA_REP'
WHERE COMMISSION_PCT IS NOT NULL AND DEPARTMENT_ID = 80;
```

### Exercițiul 14 

Să se promoveze Douglas Grant la manager în departamentul 20, având o creştere de salariu
cu 1000$.

```sql
UPDATE DEPT
SET MANAGER_ID = (
    SELECT EMPLOYEE_ID
    FROM EMP
    WHERE FIRST_NAME = 'Douglas' AND LAST_NAME = 'Grant'
)
WHERE DEPARTMENT_ID = 20;

UPDATE EMP
SET SALARY = SALARY + 1000
WHERE FIRST_NAME = 'Douglas' AND LAST_NAME = 'Grant';
```

## Delete 

Sintaxa simplificată

```sql
DELETE FROM nume_tabel
[WHERE conditie];
```

Dacă nu se specifică nicio condiție, atunci se șterg toate liniile 

### Exercițiul 15

Ştergeţi toate înregistrările din tabelul DEPT_PNU. Ce înregistrări se pot şterge? Anulaţi
modificările.

```sql
DELETE FROM DEPT;
```

Oracle dă eroarea

```
ORA-02292: integrity constraint (SYSTEM.FK_EMP_DEPT) violated - child record found
```

`EMP` are cheie externă care referă la `DEPARTMENT_ID`. Dacă sunt angajați care aparțin unui departament, acel departament nu poate fi șters.

Ștergeri posibile 

```sql
DELETE FROM DEPT_PNU
WHERE DEPARTMENT_ID NOT IN (SELECT DEPARTMENT_ID FROM EMP);
```

### Exercițiul 16 

Suprimaţi departamentele care nu au angajati. Anulaţi modificările.

(chiar ștergerile posibile de mai sus). 

## Exerciții generale 

### Exercițiul 17 

Să se mai introducă o linie in tabelul DEPT_PNU.

```sql
INSERT INTO DEPT(DEPARTMENT_ID, DEPARTMENT_NAME)
    VALUES(5, 'IT');
```

### Exercițiul 18 + 19 + 20

Să se şteargă din tabelul DEPT_PNU departamentele care au codul de departament cuprins
intre 160 si 200 . Listaţi conţinutul tabelului.

```sql
SAVEPOINT P;

DELETE FROM DEPT
WHERE DEPARTMENT_ID BETWEEN 160 AND 200;

SELECT * FROM DEPT;

ROLLBACK TO P;
```