---
{"dg-publish":true,"permalink":"/courses/db/bd-l2/"}
---

# Funcții SQL

- Sunt **funcții predefinite** în sistemul Oracle și pot fi folosite în instrucțiuni SQL. A nu se confunda cu instrucțiunile definite de utilizator, scrise în PL/SQL.
- Dacă o funcție SQL este apelată cu un alt tip de date decât cel așteptat, sistemul convertește implicit argumentul înainte să evalueze funcția.
- *De obicei* dacă o funcție SQL e apelată cu argument `null`, atunci ea întoarce `null`.  

>[!tip] Excepție
>Funcțiile care nu urmează ultima regulă sunt `CONCAT`, `NVL`, `REPLACE`.

Pot fi clasificate în următoarele categorii:
- [[Courses/DB/bd.l2#Funcții single-row\|single-row]]
- [[Courses/DB/bd.l2#Funcțiile multiple-row\|multiple-row (funcții agregat)]]

## Funcții single-row

Returnează câte o singură *linie rezultat* pentru fiecare linie a tabelului sau vizualizării interogate. 

Ele pot apărea în:
- listele de expresii din clauza `SELECT`
- clauzele `WHERE`, `START WITH`, `CONNECT BY` și `HAVING`.

>[!tip]
>Pentru a testa funcțiile, cea mai ușoară metodă este:
>
>```sql
>SELECT functie(...) FROM DUAL;
>```
### Funcții de conversie

| Funcție     | Descriere                                                               | Exemplu                                                                   |
| ----------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `TO_CHAR`   | convertește/formatează un număr/dată calendaristică în șir de caractere | `TO_CHAR(7) = '7'`<br><br>`TO_CHAR(SYSDATE, 'DD-MM-YYYY') = '03-03-2025'` |
| `TO_DATE`   | convertește număr/șir de caractere la dată calendaristică               | `TO_DATE('22-04-2024','DD-MM-YYYY')`                                      |
| `TO_NUMBER` | convertește șir de caractere la număr                                   | `TO_NUMBER('-23.799', 99.999) = -23.799`                                  |
**Observație**

Există două tipuri conversie:
- implicită (realizată de sistem atunci când e necesar)
- explicită (realizată de utilizator prin intermediul funcțiilor de conversie)

*Conversii implicite*

- `VARCHAR2` sau `CHAR` la `NUMBER`
- `VARCHAR2` sau `CHAR` la `DATE`
- și reciprocele 

### Funcții pentru prelucrarea caracterelor 

Vezi [[Courses/DB/bd.l2#Exerciții funcții pe șiruri de caractere\|exerciții]].

| Funcție                                           | Descriere                                                                                                          | Exemplu                                                                                    |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| `LENGTH(STR)`                                     | lungimea unui string                                                                                               | `LENGTH('Informatica') = 11`                                                               |
| `SUBSTR(str, start [,n])`                         | subșirul unui string care începe la poziția start și are lungime n (opțional). `n` neprecizat -> până la final     | `SUBSTR('Informatica', 1, 4) = 'Info'`                                                     |
| `LTRIM(str [,chars])`                             | șterge din stânga unui string orice caracter care apare în `chars`. Implicit, se șterg spațiile libere.            | `LTRIM('    ab') = 'ab'`<br><br>`LTRIM('aabea', 'ab') = 'ea'`                              |
| `RTRIM(str [,chars])`                             | la fel, dar pe dreapta                                                                                             | -                                                                                          |
| `TRIM(TRAILING\LEADING\BOTH chars FROM expresie)` | elimină caracterele `chars` de la început/final/ambele dintr-un șir.                                               | `TRIM(BOTH 'a' FROM 'abXa') = 'bX')`                                                       |
| `LPAD(str, length [, chars]`                      | adaugă `chars` la stânga șirului până când lungimea ***noului șir*** devine `lenght`                               | `LPAD('info', 6) = '  info'`                                                               |
| `RPAD(str, length [, chars]`                      | la fel, pe dreapta                                                                                                 | -                                                                                          |
| `REPLACE(str1, str2 [, str3])`                    | întoarce `str1` cu toate aparițiile lui `str2` înlocuite cu `str3`. (Implicit se șterg)                            | `REPLACE('b$bb$$', '$', 'a') = 'babbaa')`                                                  |
| `UPPER(str)`                                      | transformă toate literele în majuscule                                                                             | `UPPER('aBna') = 'ABNA'`                                                                   |
| `LOWER(str)`                                      | transformă toate literele în minuscule                                                                             | -                                                                                          |
| `INITCAP(str)`                                    | prima litera majusculă, următoarele minuscule                                                                      | `INITCAP('aiE') = 'Aie' `                                                                  |
| `INSTR(str, substr [, start [, n]])`              | caută într-un string, de la `start` a n-a apariție a unui substring.                                               | `INSTR('abcab', 'ab', 1, 2) = '4'`                                                         |
| `ASCII(char)`                                     | codul ASCII al primului caracter al unui șir.                                                                      | `ASCII('abc') = ASCII('a') = 97`                                                           |
| `CHR(n)`                                          | caracterul corespunzător codului ASCII specificat.                                                                 | `CHR(97) = 'a'`                                                                            |
| `CONCAT(str1, str2)`                              | concatenează două șiruri de caractere                                                                              | `CONCAT('ab', 'cd') = 'abcd'`                                                              |
| `TRANSLATE(str, src, dest)`                       | fiecare caracter care apare în string-urile `str` și `src` este transformat în caracterul corespunzător din `dest` | `TRANSLATE('$a$aa', '$', 'b') = 'babaa'`<br><br>`TRANSLATE('$a$aa', '$a', 'bc') = 'bcbcc'` |

### Funcții aritmetice

Vezi [[Courses/DB/bd.l2#Exerciții cu funcții aritmetice\|exerciții]].

| Funcția                 | Descriere                            |
| ----------------------- | ------------------------------------ |
| `ABS`                   | modulul unui numar                   |
| `CEIL`                  | partea întreagă + 1                  |
| `FLOOR`                 | partea întreagă                      |
| `ROUND(n [, decimals])` | rotunjire cu n zecimale              |
| `TRUNC(n [, decimals])` | trunchiere cu n zecimale             |
| `EXP`                   | ridicarea la putere a lui $e$        |
| `LN`                    | logaritm natural                     |
| `LOG(base, n)`          | logaritm în baza `base`,             |
| `MOD(n1, n2)`           | restul împărțirii lui $n_1$ la $n_2$ |
| `POWER(n, pow)`         | $n^{pow}$                            |
| `SIGN`                  | semnul unui număr                    |
| `COS`                   | cosinus                              |
| `COSH`                  | cosinus hiperbolic                   |
| `SIN`                   | sinus                                |
| `SINH`                  | sinus hiperbolic                     |
| `SQRT`                  | $\sqrt{n}$                           |
| `TAN`                   | tangentă                             |
| `TANH`                  | tangentă hiperbolică                 |
| `LEAST(n1, ... , nk)`   | cel mai mic număr dintr-o listă      |
| `GREATEST(n1, ..., nk)` | cel mai mare număr dintr-o listă     |
### Funcții pentru prelucrarea datelor calendaristice 

Vezi [[Courses/DB/bd.l2#Exerciții cu funcții pe date calendaristice\|exerciții]].

| Funcție                        | Descriere                                                                                                                             | Exemplu                                                    |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| `SYSDATE`                      | întoarce data și timpul curent                                                                                                        | [[Courses/DB/bd.l1#Exercițiul 13\| ex. 13]]                           |
| `ADD_MONTHS(date, nr_months)`  | întoarce data care este după `nr_months` de la data `date`.                                                                           | `ADD_MONTHS('06-MAR-2025', 3) = '06-JUN-2025'`             |
| `NEXT_DAY(date, day)`          | întoarce următoarea dată după `date` în care ziua săptămânii este cea modificată prin șirul `day`                                     | `NEXT_DAY('03-JUN-2025', 'Monday') = '09-JUN-2025'`        |
| `LAST_DAY(date)`               | data corespunzătoare ultimei zile a lunii din care `date` face parte                                                                  | `LAST_DAY('03-JUN-2025') = 2025-06-30`                     |
| `MONTHS_BETWEEN(date1, date2)` | numărul de luni dintre două date                                                                                                      | `MONTHS_BETWEEN('10-FEB-2020', '31-MAR-2005') = -36.90322` |
| `TRUNC(date)`                  | data, dar cu timpul setat la 12:00 AM.                                                                                                | -                                                          |
| `ROUND(expr_date)`             | dacă este o dată înainte de miezul zilei, se întoarce ziua cu timpul setat la 12 AM, dacă nu, următoarea zi cu timpul setat la 12 AM. | -                                                          |
| `LEAST(date1, ... , datek)`    | prima dată în ordine cronologică                                                                                                      | -                                                          |
| `GREATEST(date1, ... , datek)` | ultima dată în ordine cronologică                                                                                                     | -                                                          |
#### Operațiile pe date calendaristice 

- `date +/- number` -> `date` scade/adună un număr de zile la o dată (nu trebuie să fie număr întreg)
- `date1 - date` -> `number` numărul de zile dintre două date 

### Funcții diverse

Vezi [[Courses/DB/bd.l2#Exerciții cu funcții diverse\|exerciții]].

| Funcție                                                       | Descriere                                                                                                                         | Exemplu                                                                                           |
| ------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| `DECODE(value, if1, then1, if2, then2, ... ifN, thenN, else)` | returnează `elseI` dacă valoarea este `idI`. Dacă nu este egală cu niciun `if`, returnează `else`                                 | `DECODE('a', 'a', 'b', 'c') = 'b'`<br><br>`DECODE('b', 'a', 'b', 'c') = 'c'`                      |
| `NVL(e1, e2)`                                                 | dacă `e1` e `NULL`, întoarce `e2`. Dacă nu, întoarce `e1`<br><br>!! Cele două tipuri trebuie să fie valide/să poată fi convertite | `NVL(NULL, 1) = 1`<br><br>`NVL(1, 2) = 2`<br><br>`NVL('a', 1) = a`<br><br>`NVL(1, 'a') nu merge!` |
| `NVL2(e1, e2, e2)`                                            | dacă `e1` este `NULL`, întoarce `e2`, altfel întoarce `e3`                                                                        | `NVL(1, 2, 3) = 2`<br><br>`NVL(NULL, 2, 3) = 3`                                                   |
| `NULLIF(e1, e2)`                                              | dacă `e1 = e2` întoarce `NULL`, altfel întoarce `e1`                                                                              | `NULLIF(1,1) = NULL`                                                                              |
| `COALESCE(e1, ... , en)`                                      | returnează prima expresie `NOT NULL` din listă                                                                                    | `COALESCE(NULL, NULL, 1, 2) = 1`                                                                  |
| `UID`, `USER`                                                 | întorc id-ul, respectiv username-ul utilizatorului Oracle curent                                                                  | `SELECT USER FROM DUAL;`                                                                          |
| `VSIZE(expr)`                                                 | întoarce numărul de bytes ai unei expresii de tipul `NUMBER`, `VARCHAR2`, `DATE`.                                                 | `SELECT VSIZE(SALARY) FROM EMPLOYEES WHERE EMPLOYEE_ID = 20;`                                     |
>[!tip] Case
>Utilizarea `DECODE` este echivalentă cu utilizarea clauzei `CASE`

Case:

```sql
CASE expr
WHEN e1 THEN val1
[WHEN e2 THEN val2]
...
[ELSE val]
END
```

- se întoarce prima valoare pentru care `expr = ei`, dacă nu, `else`
- toate valorile trebuie să aibă același tip
- nu se poate specifica `NULL` pentru toate valorile de returnat

## Funcțiile multiple-row

Sunt funcțiile care calculează o valoare pe baza mai multor rânduri dintr-un tabel.

- apar în clauzele `SELECT`, `ORDER_BY`, `HAVING`, dar doar în combinație cu `GROUP BY`

Exemple:
- `AVG`
- `SUM`
- `MAX`
- `MIN`
- `COUNT`
- `STDDEV`
- `VARIANCE`

Tipuri de date acceptate:
- `CHAR`, `VARCHAR2`
- `NUMBER` (numere)
- `DATE` (date calendaristice)

**Notă:**
- `AVG`, `SUM`, `STDDEV`, `VARIANCE` -> doar pe numere.
- `MAX`, `MIN` -> pot fi aplicate și pe text sau date.

**Toate funcțiile grup, cu excepția lui COUNT(*), ignoră valorile null.**

- `COUNT(*)` numără **toate rândurile**, indiferent dacă au valori nule.
- `COUNT(coloana)` numără **doar rândurile unde coloana NU e `NULL`**.

Când este utilizată clauza GROUP BY, server-ul sortează implicit mulțimea rezultată în
ordinea crescătoare a valorilor coloanelor după care se realizează gruparea.

# Exerciții

## Exerciții cu funcții pe șiruri de caractere 

Vezi [[Courses/DB/bd.l2#Funcții pentru prelucrarea caracterelor\|teorie]].
### Exercițiul 1

Scrieți o cerere care are următorul rezultat pentru fiecare angajat:
`<prenume angajat>` `<nume angajat>` câștigă `<salariu> `lunar dar dorește `<salariu de 3 ori mai mare>`. Etichetați coloana “Salariu ideal”. Pentru concatenare, utilizați atât funcția
`CONCAT` cât ți operatorul “||”.

*Soluție*

```sql
SELECT CONCAT(LAST_NAME || ' ', FIRST_NAME) || ' castiga ' ||
       SALARY || ' dar doreste ' || (SALARY * 3) "Salariu ideal"
FROM EMPLOYEES;
```

### Exercițiul 2

Scrieți o cerere prin care să se afișeze prenumele salariatului cu prima litera majusculă
și toate celelalte litere minuscule, numele acestuia cu majuscule și lungimea
numelui, pentru angajații al căror nume începe cu J sau M sau care au a treia literă din
nume A. Rezultatul va fi ordonat descrescător după lungimea numelui. Se vor eticheta
coloanele corespunzător. Se cer 2 soluţii (cu operatorul LIKE și funcția `SUBSTR`).

*Soluție cu `LIKE`*

```sql
SELECT INITCAP(FIRST_NAME),
       UPPER(LAST_NAME),
       LENGTH(LAST_NAME)
FROM EMPLOYEES
WHERE UPPER(LAST_NAME) LIKE 'J%' OR
      UPPER(LAST_NAME) LIKE 'M%' OR
      UPPER(LAST_NAME) LIKE '__A%'
ORDER BY LENGTH(LAST_NAME);
```

*Soluție cu `SUBSTR`*

```sql
SELECT INITCAP(FIRST_NAME),
       UPPER(LAST_NAME),
       LENGTH(LAST_NAME)
FROM EMPLOYEES
WHERE SUBSTR(UPPER(LAST_NAME), 1, 1) = 'M' OR
      SUBSTR(UPPER(LAST_NAME), 1, 1) = 'J' OR
      SUBSTR(UPPER(LAST_NAME), 3, 1) = 'A'
ORDER BY LENGTH(LAST_NAME);
```

### Exercițiul 3

Să se afișeze, pentru angajații cu prenumele „Steven”, codul și numele acestora, precum
și codul departamentului în care lucrează. Căutarea trebuie să nu fie case-sensitive, iar
eventualele blank-uri care preced sau urmează numelui trebuie ignorate.

*Soluție*

```sql
SELECT EMPLOYEE_ID, LAST_NAME, DEPARTMENT_ID
FROM EMPLOYEES
WHERE INITCAP(TRIM(BOTH FROM FIRST_NAME)) = 'Steven';
```

### Exercițiul 4

Să se afișeze pentru toți angajații al căror nume se termină cu litera 'e', codul, numele,
lungimea numelui și poziția din nume în care apare prima data litera 'A'. Utilizați alias-uri
corespunzătoare pentru coloane.

*Soluție*

```sql
SELECT EMPLOYEE_ID, LAST_NAME,
       LENGTH(LAST_NAME) "Lungimea numelui",
       INSTR(UPPER(LAST_NAME), 'A', 1, 1) "Prima aparitie 'A'"
FROM EMPLOYEES
WHERE SUBSTR(UPPER(LAST_NAME), -1) = 'E';
```

## Exerciții cu funcții aritmetice

Vezi [[Courses/DB/bd.l2#Funcții aritmetice\|teoria]].
### Exercițiul 5

Să se afișeze detalii despre salariații care au lucrat un număr întreg de săptămâni până
la data curentă.

```sql
SELECT *
FROM EMPLOYEES
WHERE (SYSDATE - HIRE_DATE) / 7  = ROUND((SYSDATE - HIRE_DATE)/7);
```

### Exercițiul 6

Să se afișeze codul salariatului, numele, salariul, salariul mărit cu 15%, exprimat cu
două zecimale și numărul de sute al salariului nou rotunjit la 2 zecimale. Etichetați
ultimele două coloane “Salariu nou”, respectiv “Numar sute”. Se vor lua în considerare
salariații al căror salariu nu este divizibil cu 1000.

```sql
SELECT EMPLOYEE_ID, LAST_NAME, SALARY,
       ROUND(SALARY * 1.15, 2) "Salariu Nou",
       ROUND((SALARY * 1.15)/100, 2) "Numar sute"
FROM EMPLOYEES
WHERE MOD(SALARY, 1000) != 0;
```

### Exercițiul 7

Să se listeze numele și data angajării salariaților care câștigă comision. Să se
eticheteze coloanele „Nume angajat”, „Data angajarii”. Utilizați funcția `RPAD` pentru a
determina ca data angajării să aibă lungimea de 20 de caractere.

```sql
SELECT LAST_NAME "Nume Angajat",
       RPAD(HIRE_DATE, 20) "Data Angajarii"
FROM EMPLOYEES
WHERE COMMISSION_PCT IS NOT NULL;
```

## Exerciții cu funcții pe date calendaristice 

Vezi [[Courses/DB/bd.l2#Funcții pentru prelucrarea datelor calendaristice\|teorie]].
### Exercițiul 8

Să se afișeze data (numele lunii, ziua, anul, ora, minutul și secunda) de peste 30 zile.

*Soluție*

```sql
SELECT TO_CHAR(SYSDATE + 30, 'Month DD, YYYY HH24:MI:SS') "Data viitoare"
FROM DUAL;
```

### Exercițiul 9

Să se afișeze numărul de zile rămase până la sfârșitul anului.

*Soluție*

```sql
SELECT TO_DATE('31-12-2025', 'DD-MM-YYYY') - SYSDATE
FROM DUAL;
```

### Exercițiul 10

a) Să se afișeze data de peste 12 ore.

*Soluție*

```sql
SELECT TO_CHAR(SYSDATE + 1/2, 'DD-MM-YYYY')
FROM DUAL;
```

b) Să se afișeze data de peste 5 minute

*Soluție*

```sql
SELECT TO_CHAR(SYSDATE + 5/(24*60), 'DD/MM HH24:MI:SS')
FROM DUAL;
```

Pentru că 
$$
\frac{5 \text{ min}}{1 \text{ zi}} = \frac{5 \text{ min}}{24 \text{ (h)} \;\cdot 60\; \text{ min}}
$$
### Exercițiul 11

Să se afișeze numele și prenumele angajatului (într-o singură coloană), data angajării și
data negocierii salariului, care este prima zi de Luni după 6 luni de serviciu. Etichetați
această coloană “Negociere”.

*Soluție*

```sql
SELECT LAST_NAME || ' ' || FIRST_NAME "Nume Complet",
       HIRE_DATE,
       NEXT_DAY(ADD_MONTHS(HIRE_DATE, 6), 'Monday') "Data Negocierii Salariului"
FROM EMPLOYEES;
```

### Exercițiul 12

Pentru fiecare angajat să se afișeze numele și numărul de luni de la data angajării.
Etichetați coloana “Luni lucrate”. Să se ordoneze rezultatul după numărul de luni lucrate.
Se va rotunji numărul de luni la cel mai apropiat număr întreg.

*Soluție*

```sql
SELECT LAST_NAME,
       ROUND(MONTHS_BETWEEN(SYSDATE, HIRE_DATE)) "Luni lucrate"
FROM EMPLOYEES
ORDER BY "Luni lucrate";
```

Alte moduri de a ordona:

```
ORDER BY ROUND(MONTHS_BETWEEN(SYSDATE, HIRE_DATE));
```

```SQL
ORDER BY 2; -- a doua coloana
```

## Exerciții cu funcții diverse

Vezi [[Courses/DB/bd.l2#Funcții diverse\|teoria]].
### Exercițiul 13

Să se afișeze numele angajaților și comisionul. Dacă un angajat nu câștigă comision, să
se scrie “Fara comision”. Etichetați coloana “Comision”

```sql
SELECT LAST_NAME,
       NVL(TO_CHAR(COMMISSION_PCT), 'Nu are comision') "Comision"
FROM EMPLOYEES;
```

### Exercițiul 14

Să se listeze numele, salariul și comisionul tuturor angajaților al căror venit lunar
(salariu + valoare comision) depășește 10 000.

```sql
SELECT LAST_NAME, SALARY, COMMISSION_PCT
FROM EMPLOYEES
WHERE SALARY * (1 + NVL(COMMISSION_PCT, 0)) > 10000;
```

Pentru că pot exista salarii $>$ 10000, dar comisionul să nu existe (i.e.,  e  `NULL`).

Dar `1 + NULL` -> `NULL`, și `Salary * NULL` -> `NULL`.

Deci trebuie `NVL` ca să seteze valoarea comisionului la $0$ atunci când e `NULL`.

### Exercițiul 15

Să se afișeze numele, codul funcției, salariul și o coloana care să arate salariul după
mărire. Se știe că pentru IT_PROG are loc o mărire de 10%, pentru ST_CLERK 15%, iar
pentru SA_REP o mărire de 20%. Pentru ceilalți angajați nu se acordă mărire. Să se
denumească coloana "Salariu renegociat".

*Soluție* 

Cu `DECODE`:

```sql
SELECT LAST_NAME, JOB_ID, SALARY,
       DECODE(JOB_ID,
           'IT_PROG', SALARY * 1.1,
           'ST_CLERK', SALARY * 1.15,
           'SA_REP', SALARY * 1.2,
            SALARY) "Salariu renegociat"
FROM EMPLOYEES;
```

Cu `CASE`:

```sql
SELECT LAST_NAME, JOB_ID, SALARY,
       CASE WHEN JOB_ID = 'IT_PROG' THEN SALARY * 1.1
            WHEN JOB_ID = 'ST_CLERK' THEN SALARY * 1.15
            WHEN JOB_ID = 'SA_REP' THEN SALARY * 1.2
            ELSE SALARY
        END "Salariu renegociat"
FROM EMPLOYEES;
```

