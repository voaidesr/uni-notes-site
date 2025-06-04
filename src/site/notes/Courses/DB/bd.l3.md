---
{"dg-publish":true,"permalink":"/courses/db/bd-l3/"}
---

# Interogări multi-relație. Join. Operatori pe mulțimi

## Join 

**Join-ul** reprezintă operația de regăsire a datelor din două sau mai multe tabele, pe baza valorilor comune ale unor coloane. De obicei, aceste coloane reprezintă **cheia primară**, respectiv **cheia externă** a tabelelor.

>[!proof] 
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

