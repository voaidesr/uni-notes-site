---
{"dg-publish":true,"permalink":"/courses/db/bd-l1/"}
---

# Teorie 

## Baze de date

- ***O bază de date*** este un ansamblu structurat de date coerente, fără redundanță inutilă (pot exista aceleași date în mai locuri, dar trebuie să fie motivat), care pot fi accesate în mod *concurent* de mai mulți utilizatori.
- Un *sistem de gestiune a bazelor de date* (SGBD) este un produs software care asigură interacțiunea cu o bază de date, permițând definirea, consultarea și actualizarea datelor în baza de date.

## SQL

- SQL (*Structured query language*) este un limbaj *neprocedural* pentru interogarea și prelucrarea informațiilor din baza de date.
	- Compilatorul limbajului generează o procedură care accesează baza de date

SQL permite:
- definirea datelor (LDD)
- prelucrarea și interogarea datelor ([[Courses/DB/bd.l4#LMD\|LMD]]LMD)
- controlul accesului la date (LCD)

Comenzile din SQL pot fi integrate în programe scrise în alte limbaje.

# SQL \*Plus

- utilitar Oracle, cu comenzi specifice
- recunoaște instrucțiuni SQL și le trimite serverului Oracle pentru execuție 

| SQL                                                                        | SQL \*Plus                                                     |
| -------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Limbaj de comunicare cu serverul Oracle pentru accesarea datelor           | Recunoaște instrucțiuni SQL și le transferă server-ului Oracle |
| Se bazează pe standardul ANSI pentru SQL                                   | Interfață specifică Oracle pentru execuția instrucțiunilor SQL |
| Prelucrează date și definește obiecte din baza de date                     | Nu permite prelucrarea obiectelor din baza de date             |
| Utilizează funcții pentru a efectua formatări                              | Utilizează comenzi pentru a efectua formatări                  |
| Instrucțiunile nu pot fi abreviate                                         | Comenzile pot fi abreviate                                     |
| Nu are caracter de continuare al instrucțiunilor scrise pe mai multe linii | Are „-”                                                        |
| Caracter de terminare al unei comenzi este ;                               | Nu necesită caracter de terminare                              |
# Limbajele SQL

În funcție de tipul acțiunii, instrucțiunile SQL se împart în mai multe categorii:
- limbajul de definire al datelor (LDD)
	- `CREATE`, `ALTER`, `DROP`
- limbajul de prelucrare al datelor (LMD)
	- `INSERT`, `UPDATE`, `DELETE`, `SELECT`
- limbajul de control al datelor (LCD)
	- `COMMIT`, `ROLLBACK`, `SAVEPOINT`

# Exerciții 

## Exercițiul 2

Să se listeze **structura** tabelelor in schema HR.

*Soluție*

Exemplu pentru `EMPLOYEES`

```sql
DESC EMPLOYEES;
```

## Exercițiul 3

Să se listeze conținutul tabelelor din schema considerată, afișând valorile tuturor
câmpurilor.

*Soluție*

Exemplu pentru `EMPLOYEES`:

```sql
SELECT * FROM EMPLOYEES;
```

## Exercițiul 4

Să se afișeze codul angajatului, numele, codul job-ului, data angajării. 

*Soluție*

```SQL
SELECT EMPLOYEE_ID, LAST_NAME, JOB_ID, HIRE_DATE
FROM EMPLOYEES;
```

## Exercițiul 5

Să se listeze, cu și fără duplicate, codurile job-urilor din tabelul EMPLOYEES.

*Soluție*
- fără duplicate (merge și `UNIQUE` în loc de `DISTINCT`

```sql
SELECT DISTINCT JOB_ID
FROM EMPLOYEES;
```

- cu duplicate (`ALL` - implicit)

```sql
SELECT JOB_ID
FROM EMPLOYEE;
```

## Exercițiul 6

Să se afișeze numele concatenat cu prenumele și cu job_id-ul, separate prin virgula și
spațiu. Etichetați coloana “Detalii Angajat”.

### Observație : concatenare
- operatorul de concatenare este `||`
- șirurile de caractere se precizează între apostrofuri, NU ghilimele (astfel, ar fi considerat *alias*)

*Soluție*

```sql
SELECT LAST_NAME || ', ' || FIRST_NAME || ', ' ||JOB_ID "Detalii Angajat"
FROM EMPLOYEES;
```

## Exercițiul 7

Să se listeze numele și salariul angajaților care câștigă mai mult de 2850.

*Soluție*

```sql
SELECT LAST_NAME, SALARY
FROM EMPLOYEES
WHERE SALARY > 2850;
```

## Exercițiul 8

Să se creeze o cerere pentru a afișa numele angajatului și codul departamentului pentru
angajatul având codul 104.

*Soluție*

```sql
SELECT LAST_NAME, DEPARTMENT_ID
FROM EMPLOYEES
WHERE EMPLOYEE_ID = 104;
```

## Exercițiul 9 

Să se modifice cererea de la problema 7 pentru a afișa numele și salariul angajaților al
căror salariu nu se află în intervalul $[1400, 24000]$.

### Observație : `BETWEEN`

- pentru testarea apartenenței la un domeniu de valori se poate utiliza 

```sql
[NOT] BETWEEN val1 AND val2
```

*Soluție*

```sql
SELECT LAST_NAME, SALARY
FROM EMPLOYEES
WHERE SALARY BETWEEN 1400 AND 24000;
```

## Exercițiul 10

Să se afișeze numele, job-ul și data la care au început lucrul salariații angajați între 20
Februarie 1987 și 1 Mai 1989. Rezultatul va fi ordonat crescător după data de început.

*Soluție*

```sql
SELECT LAST_NAME, JOB_ID, HIRE_DATE
FROM EMPLOYEES
WHERE HIRE_DATE BETWEEN '20-Feb-1987' AND '01-May-1989'
ORDER BY HIRE_DATE ASC;
```

 `ORDER BY` ordonează output-ul. `ASC` - ascending, `DESC` - descending. Implicit este ascending.
## Exercițiul 11

Să se afișeze numele salariaților și codul departamentelor pentru toți angajații din
departamentele 10 și 30 în ordine alfabetică a numelor.

*Soluție*

```sql
SELECT LAST_NAME, DEPARTMENT_ID
FROM EMPLOYEES
WHERE DEPARTMENT_ID IN (10, 30)
ORDER BY LAST_NAME;
```

### Observație : `IN`

`IN` verifică dacă o valoare coincide cu un una dintre valorile dintr-o mulțime. 

## Exercițiul 12

Să se modifice cererea de la problema 11 pentru a lista numele și salariile angajaților care
câștigă mai mult de 1500 și lucrează în departamentul 10 sau 30. Se vor eticheta coloanele
drept Angajat și Salariu lunar.

*Soluție*

```sql
SELECT LAST_NAME "Angajat", SALARY "Salariu lunar"
FROM EMPLOYEES
WHERE DEPARTMENT_ID IN (10, 30) AND SALARY > 1500;
```

## Exercițiul 13

Care este data curentă? Afișați diferite formate ale acesteia.

```sql
SELECT SYSDATE FROM DUAL;
```

### Observație : date, `DUAL`, `TO_CHAR`

Funcția care returnează data curentă este `SYSDATE`. Pentru a completa sintaxa de `SELECT`, este necesară selectarea dintr-un tabel -> selectare din `DUAL`.

Datele calendaristice pot fi formatate cu ajutorul funcției `TO_CHAR(data, format)`

| Element       | Semnificație                                                    |
| ------------- | --------------------------------------------------------------- |
| D             | Numărul zilei din săptămână (Duminică = 1, Luni = 2, ...)       |
| DD            | Numărul zilei din lună                                          |
| DDD           | Numărul zilei din an                                            |
| DY            | Numărul zilei din săptămână -> abreviere de 3 litere (MON, THU) |
| DAY           | Numărul zilei din săptămână, scris în întregime                 |
| MM            | Numărul lunii din an                                            |
| MON           | Numele lunii din an -> abreviere de 3 litere (JAN, FEB)         |
| MONTH         | Numele lunii din an                                             |
| Y             | Ultima cifră din an                                             |
| YY, YYY, YYYY | Ultimele 2, 3, respectiv 4 cifre din an                         |
| YEAR          | Anul, scris în litere (two thousand four)                       |
| HH12, HH24    | Orele din zi, între 0-12, respectiv 0-24                        |
| MI            | Minutele din oră                                                |
| SS            | Secundele din minut                                             |
| SSSSS         | Secundele trecute de la miezul nopții                           |
## Exercițiul 14

Să se afișeze numele și data angajării pentru fiecare salariat care a fost angajat în 1987.
Se cer 2 soluții: una în care se lucrează cu formatul implicit al datei și alta prin care se
formatează data.

*Soluție 1*

```sql
SELECT LAST_NAME, HIRE_DATE
FROM EMPLOYEES
WHERE TO_CHAR(HIRE_DATE, 'YYYY') = '1987';
```

*Soluție 2*

```sql
SELECT LAST_NAME, HIRE_DATE
FROM EMPLOYEES
WHERE HIRE_DATE LIKE('%87%'); -- HIRE_DATE contine 87
```

### Observație : `LIKE`

Pentru compararea șirurilor de caractere cu operatorul `LIKE`, se utilizează caracterele **wildcard**:
- `%` - orice șir de caractere, inclusiv șirul vid 
- `_` - un singur caracter și numai unul

## Exercițiul 15

Să se listeze numele tuturor angajaților care au a treia literă din nume ‘A’.

```sql
SELECT LAST_NAME || ' ' || FIRST_NAME "Nume"
FROM EMPLOYEES
WHERE UPPER(LAST_NAME) LIKE ('__A%');
```

## Exercițiul 16

Să se listeze numele tuturor angajaților care au cel puțin 2 litere ‘L’ în nume și lucrează în
departamentul 30 sau managerul lor este 102.

```sql
SELECT LAST_NAME
FROM EMPLOYEES
WHERE UPPER(LAST_NAME) LIKE('%L%L%')
    OR DEPARTMENT_ID = 30
    OR MANAGER_ID = 102;
```

## Exercițiul 17

Să se afișeze numele, job-ul și salariul pentru toți salariații al căror job conține șirul
“CLERK” sau “REP” și salariul nu este egal cu 1000, 2000 sau 3000 $.

```sql
SELECT LAST_NAME, SALARY, JOB_ID
FROM EMPLOYEES
WHERE (JOB_ID LIKE('%CLERK%') OR JOB_ID LIKE('%REP%')) 
	AND SALARY NOT IN (1000, 2000, 3000);
```
## Exercițiul 18

Să se afișeze numele, salariul și comisionul pentru toți salariații care câștigă comision.
Să se sorteze datele în ordine descrescătoare a salariilor și comisioanelor.

```sql
SELECT LAST_NAME, SALARY, COMMISSION_PCT
FROM EMPLOYEES
WHERE COMMISSION_PCT IS NOT NULL
ORDER BY SALARY DESC, COMMISSION_PCT DESC;
```

## Exercițiul 19

Eliminați clauza WHERE din cererea anterioară. Unde sunt plasate valorile NULL în
ordinea descrescătoare?

*Soluție*

Ține de SGBD. La mine `NULL` se plasează la început în ordinea descrescătoare.

Se poate specifica `NULLS FIRST` sau `NULLS LAST`.

```sql
SELECT LAST_NAME, SALARY, COMMISSION_PCT
FROM EMPLOYEES
ORDER BY SALARY DESC, COMMISSION_PCT DESC NULLS LAST;
```

## Exercițiul 20

Să se afișeze angajații care au salariul între 5000 și 9000, iar prenumele (first_name) lor
începe cu litera a sau m. Verificarea se va face utilizând literă mica. De asemenea, se
afișează doar acei angajați care au fost angajați într-un an impar, iar luna lor de angajare
coincide cu luna curentă (adică luna în care ne aflăm în acest moment). Se vor afișa:
numele concatenat cu spațiu, concatenat cu prenumele, salariul și data angajării. Coloana
pe care se află numele și prenumele se va numi Nume Complet. Rezultatele se vor ordona
descrescător, în funcție de data angajării.

### Observație  : `MOD`

`MOD(param1, param2)` -> restul împărțirii `param1` la `param2`.

*Soluție*

```sql
SELECT LAST_NAME || ' ' || FIRST_NAME "Nume complet",
       SALARY, HIRE_DATE
FROM EMPLOYEES
WHERE (SALARY BETWEEN 5000 AND 9000) AND
      ((LOWER(FIRST_NAME) LIKE ('a%')) OR
    (LOWER(FIRST_NAME) LIKE ('m%'))) AND
    (MOD(TO_NUMBER(TO_CHAR(HIRE_DATE, 'YYYY')), 2) = 1) AND
    (TO_CHAR(HIRE_DATE, 'MM') = TO_CHAR(SYSDATE, 'MM'))
ORDER BY HIRE_DATE DESC;
```

## Exercițiul 21

*Soluție*

```sql
SELECT LAST_NAME, SALARY, JOB_ID,
       FLOOR(MONTHS_BETWEEN(SYSDATE, HIRE_DATE) / 12) "Ani Lucrati",
       TO_CHAR(HIRE_DATE, 'YYYY') "Anul angajarii"
FROM EMPLOYEES
WHERE UPPER(JOB_ID) LIKE '%CLERK';
```

## Exercițiul 22

```sql
SELECT first_name || ' ' || last_name AS NumeComplet,
       salary AS Salariu,
       hire_date,
       MOD(TO_NUMBER(TO_CHAR(hire_date, 'YYYY')), 2) AS ParitateAnAngajare,
       FLOOR(MONTHS_BETWEEN(SYSDATE, hire_date) / 12) AS AniLucrati
FROM employees
WHERE salary BETWEEN 40000 AND 90000
  AND first_name LIKE '_a%'
  AND department_id IN (10, 20, 30)
ORDER BY hire_date DESC;
```

