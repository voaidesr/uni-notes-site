---
{"dg-publish":true,"permalink":"/courses/db/bd-l3/"}
---

# Interogări multi-relație. Join. Operatori pe mulțimi

## Join 

Vezi [[Courses/DB/bd.l3#Exerciții Join\|exerciții]].

**Join-ul** reprezintă operația de regăsire a datelor din două sau mai multe tabele, pe baza valorilor comune ale unor coloane. De obicei, aceste coloane reprezintă **cheia primară**, respectiv **cheia externă** a tabelelor.

>[!proof]  join
>Pentru a realiza un JOIN între $n$ tabele, e nevoie de cel puțin $n-1$ condiții de JOIN.

### Tipuri de Join

#### Inner join (equijoin, join simplu) 

Corespunde situației în care valorile de pe coloanele ce apar în condiția de join trebuie să fie **egale**.

```sql
SELECT e.EMPLOYEE_ID,
       e.LAST_NAME,
       d.DEPARTMENT_NAME
FROM EMPLOYEES e JOIN DEPARTMENTS d
    ON e.DEPARTMENT_ID = d.DEPARTMENT_ID;
```

*Sintaxa*

- Condiția poate fi scrisă în clauza `WHERE`

```sql
SELECT e.LAST_NAME, d.DEPARTMENT_NAME
FROM EMPLOYEES e, DEPARTMENTS d
WHERE e.DEPARTMENT_ID = d.DEPARTMENT_ID;
```

- Condiția poate fi în clauza `FROM` - **standardul SQL3**
	- utilizând `ON`. (vezi primul exemplu)
	- utilizând `USING` - efectuează equijoin pe baza coloanei cu nume specificat în sintaxă. 

```sql
SELECT e.LAST_NAME, d.DEPARTMENT_NAME
FROM EMPLOYEES e JOIN DEPARTMENTS d USING(DEPARTMENT_ID);
```
#### Nonequijoin 

Condiția de join conține alți operatori decât cel de egalitate.

```sql
SELECT LAST_NAME,
       SALARY,
       GRADE_LEVEL,
       LOWEST_SAL,
       HIGHEST_SAL
FROM EMPLOYEES, JOB_GRADES
WHERE SALARY BETWEEN LOWEST_SAL AND HIGHEST_SAL;
```

- creează produsul cartezian `SALARY` $\times$ `JOB_GRADES` 
- filtrează rezultatele pentru care  `LOWEST_SAL` < `SALARY` < `HIGHEST_SAL`

#### Left/right Outer join 

Un outer join este utilizat pentru a obține în rezultat și rândurile care nu satisfac condiția de join. 

- operatorul pentru outer join este `(+)` și se pune în partea **deficitară în informație**
- efect: unește liniile tabelului care nu este deficitar în informație, cărora nu le corespunde nicio linie în celălalt tabel, cu valori `NULL`.
- `(+)`, poate fi plasat în orice parte, dar nu în ambele părți!

**Observație**:

- O condiție care presupune *outer join* nu poate utiliza operatorul `IN` și nu poate fi legată de o altă condiție cu operatorul `OR`. 

```sql
SELECT e.last_name, d.department_name
FROM employees e LEFT OUTER JOIN departments d
    ON e.department_id = d.department_id; -- un angajat poate să nu aibă departament
```

Echivalent cu

```sql
SELECT e.last_name, d.department_name
FROM employees e, departments d
WHERE e.department_id = d.department_id(+);
```

- **Full outer join** = left outer join + right outer join
- **Self join** = join tabel cu sine însuși

*Self join, exemplu*

```sql
SELECT e.LAST_NAME "Angajat", 
       m.LAST_NAME "Manager"
FROM EMPLOYEES e LEFT JOIN EMPLOYEES m
    ON e.MANAGER_ID = m.EMPLOYEE_ID;
```

Într-o instrucțiune `SELECT` cu `JOIN`, se recomandă prefixarea numelor coloanelor cu numele sau alias-ul tabelului pentru claritate și performanță. Dacă o coloană apare în mai multe tabele, prefixarea devine obligatorie.

### Inner join vs. Outer join 

Exemplu 

```sql
SELECT employee_id, department_name
FROM employees e JOIN departments d ON (e.department_id = d.department_id);
```

Acest **inner join** afișează toți angajații care lucrează în departamente.

Pot exista angajați care nu au departamente! (i.e. `e.DEPARTMENT_ID = NULL`)

explicație 
- left join -> aduce toate rândurile din stânga, chiar dacă nu au corespondență în dreapta
- right join -> la fel, dar pentru dreapta

Știm că există angajați care nu au departamente, dar vrem să vedem toți angajații -> left join.

```sql
SELECT employee_id, department_name
FROM employees e LEFT JOIN departments d 
	ON (e.department_id = d.department_id);
```

Știm și că există departamente care nu au angajați, vrem să vedem toate departamentele:

```sql
SELECT employee_id, department_name
FROM employees e RIGHT JOIN departments d 
	ON (e.department_id = d.department_id);
```

(aici angajatul care nu face parte din niciun departament este ignorat);

Dacă vrem toate informațiile (și departamentele fără angajați, și angajații fără departament) -> `FULL JOIN`.

>[!warning] Notația cu `(+)`
>`(+)` se pune pe partea opusă a tipului de `JOIN`
>
>`LEFT JOIN` -> `(+)` pe dreapta

>[!danger]
>`FULL JOIN` nu are echivalent cu `(+)` -> operatorul nu poate fi scris în amândouă părțile

### Join în standardul SQL3 

Oracle oferă pentru JOIN și o sintaxă specifică, în conformitate cu standardul SQL3.
- nu aduce beneficii de performanță față de sintaxa anterior prezentată 
- cuvinte cheie 
	- `CROSS JOIN` -> produs cartezian
	- `NATURAL JOIN`
	- `FULL OUTER JOIN`
	- clauzele `USING`, `ON`

Sintaxa:

```sql
SELECT tabel_1.nume_coloana, tabel_2.nume_coloana
FROM   tabel_1
       [CROSS JOIN tabel_2]
     / [NATURAL JOIN tabel_2]
     / [JOIN tabel_2 USING (nume_coloană)]
     / [JOIN tabel_2 ON (condiție)]
     / [LEFT | RIGHT | FULL OUTER JOIN tabel_2
        ON (tabel_1.eume_coloana = tabel_2.nume_coloana)];
```

**Natural join** presupune existența unor coloane care au același nume în ambele tabele. Dacă tipurile de date ale coloanelor cu același nume sunt *diferite*, atunci va fi returnată o eroare.

Coloanele având același nume în cele două tabele trebuie să nu fie precedate de
numele sau alias-ul tabelului corespunzător.

Clauzele NATURAL JOIN și USING nu pot coexista în aceeași instrucțiune SQL.

```sql
SELECT LAST_NAME, JOB_ID, JOB_TITLE
FROM EMPLOYEES NATURAL JOIN JOBS;
```

### Exerciții Join

Vezi [[Courses/DB/bd.l3#Join\|teoria]].
#### Exercițiul 1

Să se listeze codurile și denumirile job-urilor care există în departamentul
30.

*Soluție*

```sql
SELECT J.JOB_ID, J.JOB_TITLE
FROM EMPLOYEES E JOIN JOBS J ON(E.JOB_ID = J.JOB_ID)
WHERE E.DEPARTMENT_ID = 30;
```

#### Exercițiul 2

Să se afișeze numele angajatului, numele departamentului și id-ul locației
pentru toți angajații care câștigă comision.

*Soluție*

```sql
SELECT LAST_NAME, d.DEPARTMENT_NAME, LOCATION_ID
FROM EMPLOYEES e JOIN DEPARTMENTS d ON (e.DEPARTMENT_ID = d.DEPARTMENT_ID)
WHERE COMMISSION_PCT IS NOT NULL;
```

#### Exercițiul 3

Să se afișeze numele angajaților, titlul job-ului și denumirea
departamentului pentru toți angajații care lucrează în Oxford (coloana - city).

*Soluție*

```sql
SELECT LAST_NAME, JOB_TITLE, DEPARTMENT_NAME
FROM EMPLOYEES e JOIN DEPARTMENTS d ON (e.DEPARTMENT_ID = d.DEPARTMENT_ID)
                JOIN LOCATIONS l ON (d.LOCATION_ID = l.LOCATION_ID)
                JOIN JOBS j ON (e.JOB_ID = j.JOB_ID)
WHERE LOWER(l.CITY) =  'oxford';
```

#### Exercițiul 4

Să se afișeze codul angajatului și numele acestuia, împreună cu numele și
codul șefului său direct. Se vor eticheta coloanele Cod Angajat, Nume
Angajat, Cod Manager, Nume Manager. 

*Soluție*

```sql
SELECT e.EMPLOYEE_ID "Cod angajat",
       e.LAST_NAME "Nume angajat",
       m.EMPLOYEE_ID "Cod manager",
       m.LAST_NAME "Nume Manager"
FROM EMPLOYEES e JOIN EMPLOYEES m
    ON(e.MANAGER_ID = m.EMPLOYEE_ID);
```

#### Exercițiul 5 

Să se modifice cererea anterioară pentru a afișa toți salariații, inclusiv cei care
nu au șef.

*Soluție*

```sql
SELECT e.EMPLOYEE_ID "Cod angajat",
       e.LAST_NAME "Nume angajat",
       m.EMPLOYEE_ID "Cod manager",
       m.LAST_NAME "Nume Manager"
FROM EMPLOYEES e LEFT JOIN EMPLOYEES m
    ON(e.MANAGER_ID = m.EMPLOYEE_ID);
```

#### Exercițiul 6

Scrieți o cerere care afișează numele angajatului, codul departamentului în
care acesta lucrează și numele colegilor săi de departament. Se vor eticheta
coloanele corespunzător.

*Soluție*

```sql
SELECT e.LAST_NAME, e.DEPARTMENT_ID,
       c.LAST_NAME
FROM EMPLOYEES e JOIN EMPLOYEES c
    ON(e.DEPARTMENT_ID = c.DEPARTMENT_ID)
WHERE e.EMPLOYEE_ID != c.EMPLOYEE_ID;
```

#### Exercițiul 7

Creați o cerere prin care să se afișeze numele angajaților, codul job-ului,
titlul job-ului, numele departamentului și salariul angajaților. Se vor include
și angajații al căror departament nu este cunoscut.

*Soluție*

```sql
SELECT LAST_NAME, e.JOB_ID, JOB_TITLE, DEPARTMENT_NAME, SALARY
FROM EMPLOYEES e LEFT JOIN DEPARTMENTS d ON (e.DEPARTMENT_ID = d.DEPARTMENT_ID)
                LEFT JOIN JOBS j ON(e.JOB_ID = j.JOB_ID);
```

#### Exercițiul 8

Să se afișeze numele și data angajării pentru salariații care au fost angajați
după salariatul cu numele (last_name) Gates.

*Soluție*

```sql
SELECT e.LAST_NAME,
       e.HIRE_DATE
FROM EMPLOYEES e JOIN EMPLOYEES p
    ON(e.HIRE_DATE > p.HIRE_DATE)
WHERE INITCAP(p.LAST_NAME) = 'Grant';
```

#### Exercițiul 9

Scrieți o cerere pentru a afișa numele salariatului, luna (în litere), anul
angajării și valoarea comisionului pentru toți salariații din același departament
cu Gates (last_name este Gates) – se verifică numele scris cu prima literă mare
și restul literelor mici, al căror nume conține litera “a”. Se va exclude Gates. Se
vor utiliza aliasuri pentru numele coloanelor din output. În cazul în care un
angajat nu câștigă comision, se va scrie în output, pe coloana respectivă,
mesajul “Nu câștigă comision”. Rezultatul se va ordona alfabetic după numele
salariaților. 

*Soluție*

```sql
SELECT e.LAST_NAME,
       TO_CHAR(e.HIRE_DATE, 'Month') "Luna",
       TO_CHAR(e.HIRE_DATE, 'YYYY') "An",
       NVL(TO_CHAR(e.SALARY * e.COMMISSION_PCT), 'Nu gastiga comision') "Valoare comision"
FROM EMPLOYEES e JOIN EMPLOYEES p
    ON(e.DEPARTMENT_ID = p.DEPARTMENT_ID)
WHERE INITCAP(p.LAST_NAME) = 'Gates'
        and e.EMPLOYEE_ID != p.EMPLOYEE_ID
        and LOWER(e.LAST_NAME) LIKE '%a%'
ORDER BY e.LAST_NAME;
```

#### Exercițiul 10 

Să se afișeze numele, salariul, titlul job-ului, orașul și țara în care lucrează
angajații conduși direct de King.

*Soluție*

```sql
SELECT e.LAST_NAME, e.SALARY, JOB_TITLE, CITY, COUNTRY_NAME
FROM EMPLOYEES e JOIN EMPLOYEES m ON (e.MANAGER_ID = m.EMPLOYEE_ID)
                JOIN JOBS j ON e.JOB_ID = j.JOB_ID
                JOIN DEPARTMENTS d ON e.DEPARTMENT_ID = d.DEPARTMENT_ID
                JOIN LOCATIONS l ON d.LOCATION_ID = l.LOCATION_ID
                JOIN COUNTRIES c ON c.COUNTRY_ID = l.COUNTRY_ID
WHERE INITCAP(m.LAST_NAME) = 'King';
```

#### Exercițiul 11

Să se afișeze codul departamentului, numele departamentului, numele și job-
ul tuturor angajaților din departamentele al căror nume conține șirul ‘ti’. De
asemenea, se va lista salariul angajaților, în formatul “$99,999.00”. Rezultatul
se va ordona alfabetic după numele departamentului, și în cadrul acestuia,
după numele angajaților.

*Soluție*

```sql
SELECT e.DEPARTMENT_ID, DEPARTMENT_NAME, LAST_NAME, JOB_TITLE,
       TO_CHAR(SALARY, '$99,999.00')
FROM EMPLOYEES e JOIN DEPARTMENTS d ON e.DEPARTMENT_ID = d.DEPARTMENT_ID
                JOIN JOBS j ON e.JOB_ID = j.JOB_ID
WHERE LOWER(DEPARTMENT_NAME) LIKE '%ti%'
ORDER BY DEPARTMENT_NAME, LAST_NAME;
```

#### Exercițiul 12 

Cum se poate implementa full outer join?

Full outer join se poate realiza fie prin reuniunea rezultatelor lui right outer
join și left outer join, fie utilizând sintaxa specifică standardului SQL3.

## Operatori pe mulțimi

Vezi [[Courses/DB/bd.l3#Exerciții cu operatori de mulțimi\|exerciții]].

Operațiile pe mulțimi combină rezultatele obținute din două (sau mai multe) interogări.
- cererile care conțin operatori pe mulțimi se numesc **cereri compuse**

Există patru operatori 
- union
- union all
- intersect 
- minus

Toți operatorii pe mulțimi au aceeași precedență. Dacă sunt mai mulți, de se evaluează de la stânga la dreapta (de sus în jos). Pentru a schimba ordinea, se utilizează paranteze. 

### Union 

Returnează toate liniile selectate de cele două cereri, *eliminând duplicatele*.

Nu ignoră `NULL` și are precedență mai mică decât `IN`.

### Union all

Returnează toate liniile selectate de cereri, **fără a elimina duplicatele**.

Precizările de la union, valabile și aici.

>[!warning] Distinct
>În cererile asupra cărora se aplică Union All nu se poate folosi Distinct.

### Intersect 

Returnează toate liniile **comune** cererilor asupra cărora se aplică. Nu ignoră valorile `NULL`.

### Minus

Determină liniile returnate de prima cerere care nu apar în rezultatul celei de-a doua cereri.

Pentru ca operatorul MINUS să funcționeze, este necesar ca toate coloanele din clauza WHERE să se afle și în clauza SELECT.

>[!warning]
>- Pentru o cerere care utilizează operatori pe mulțimi, cu excepția lui UNION ALL, server-ul Oracle elimină liniile duplicat.
>- În instrucțiunile SELECT asupra cărora se aplică operatori pe mulțimi, coloanele
selectate trebuie să corespundă ca număr și tip de date. Nu este necesar ca
numele coloanelor să fie identice. Numele coloanelor din rezultat sunt
determinate de numele care apar în clauza SELECT a primei cereri.


### Exerciții cu operatori de mulțimi

Vezi [[Courses/DB/bd.l3#Operatori pe mulțimi\|teoria]].

#### Exercițiul 1 

Se cer codurile departamentelor al căror nume conține șirul “re” sau în care
lucrează angajați având codul job-ului “SA_REP”.

*Soluție*

```SQL
SELECT DEPARTMENT_ID
FROM DEPARTMENTS
WHERE LOWER(DEPARTMENT_NAME) LIKE '%re%'

UNION

SELECT DEPARTMENT_ID
FROM EMPLOYEES
WHERE UPPER(JOB_ID) = 'SA_REP';
```

#### Exercițiul 2

dacă se folosește `UNION ALL` apar duplicate 

#### Exercițiul 3

Să se obțină codurile departamentelor în care nu lucrează nimeni (nu este
introdus niciun salariat în tabelul employees). Se cer două soluții.

*Soluție 1*

```sql
SELECT DEPARTMENT_ID
FROM DEPARTMENTS

MINUS

SELECT DEPARTMENT_ID
FROM EMPLOYEES;
```

*Soluție 2*

```sql
SELECT d.DEPARTMENT_ID
FROM DEPARTMENTS d LEFT JOIN EMPLOYEES e
    ON (d.DEPARTMENT_ID = e.DEPARTMENT_ID)
WHERE e.DEPARTMENT_ID IS NULL;
```

#### Exercițiul 4 

Se cer codurile departamentelor al căror nume conține șirul “re” și în care
lucrează angajați având codul job-ului “HR_REP”.

*Soluție*

```sql
SELECT DEPARTMENT_ID
FROM DEPARTMENTS
WHERE LOWER(DEPARTMENT_NAME) LIKE '%re%'

INTERSECT

SELECT DEPARTMENT_ID
FROM EMPLOYEES
WHERE JOB_ID = 'HR_REP';
```