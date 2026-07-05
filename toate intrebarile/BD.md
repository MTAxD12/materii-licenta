# Baze de Date — Toate întrebările de licență (2019–2025), ordonate după frecvență
> Întrebările reale din ultimii ani, grupate pe teme și **ordonate de la cele mai frecvente la cele mai rare**. Apasă pe **„📖 Răspuns"**.
> Legendă: 🔴 foarte frecvent · 🟠 frecvent · 🟡 de 2 ori · ⚪ o dată.

---

## 1. Algebra relațională: operații + echivalent SQL 🔴 (~15 apariții — cea mai frecventă temă)
> *„Relații și operații cu relații în modelul relațional și corespondența în SQL (cel puțin 5 operații). Proiecție, intersecție, reuniune, selecție, join, agregare."*

<details><summary>📖 Răspuns</summary>

**Relație** = tabel; **tuplu** = linie; **atribut** = coloană; **domeniu** = valorile posibile ale unui atribut. Există **2 categorii** de operatori:

**A. Operatori din teoria mulțimilor** (relații peste **aceeași** mulțime de atribute, exceptând ×):
| Operator | Simbol | SQL |
|----------|--------|-----|
| Reuniune | ∪ | `UNION` |
| Diferență | − | `MINUS` / `EXCEPT` |
| Intersecție | ∩ | `INTERSECT` |
| Produs cartezian | × | `SELECT * FROM r1, r2` |
> `r₁ ∩ r₂ = r₁ − (r₁ − r₂)`.

**B. Operatori specifici algebrei relaționale:**
| Operator | Simbol | Ce face | SQL |
|----------|--------|---------|-----|
| **Proiecție** | π_X(r) | păstrează **coloanele** X | `SELECT nume, prenume FROM ...` |
| **Selecție** | σ_θ(r) | păstrează **liniile** ce satisfac θ | `SELECT * FROM ... WHERE ...` |
| Redenumire | ρ | schimbă numele unui atribut | `... AS "nume_nou"` |
| **Join natural** | ⋈ | îmbină după atributele comune | `NATURAL JOIN` / `JOIN..ON` |
| θ-join / equijoin | ⋈_θ | join cu condiție (egalitate) | `JOIN..ON a=b` |
| Join stânga/dreapta/extern | ⟕/⟖/⟗ | + tuplele fără corespondent (NULL) | `LEFT/RIGHT/FULL JOIN` |

**Join natural:** `r₁ ⋈_θ r₂ = σ_θ(r₁ × r₂)` (join = selecție pe produs cartezian).

**Agregarea:** funcții `avg, min, max, sum, count`; în SQL `GROUP BY`. ⚠️ **Agregarea NU se poate exprima** prin ceilalți operatori de bază (spre deosebire de join/intersecție).
</details>

---

## 2. Forme normale (1NF–4NF, BCNF) și relațiile dintre ele 🔴 (~10 apariții)
> *„Formele normale toate, pe scurt. 2NF și 3NF. BCNF și 4NF. Relația/legătura dintre ele."*

<details><summary>📖 Răspuns</summary>

**Normalizarea** = eliminarea **redundanțelor** + păstrarea **consistenței**; se face în faza de **proiectare** (asupra **schemei**).

- **Atribut prim** = în vreo cheie candidat; **neprim** = nu.
- **Dependență plină:** nicio submulțime a lui X nu determină A.
- **Tranzitiv dependent:** `X→Y`, `Y→A`, `Y→X ∉ Σ⁺`.

**1NF:** valori **atomice** (fără grupuri repetitive).
**2NF:** e în 1NF + orice atribut **neprim** e **dependent plin** de orice cheie.
**3NF:** e în 2NF + niciun atribut neprim **NU e tranzitiv dependent** de vreo cheie. *(„a fact about the key, the whole key, and nothing but the key")*
**BCNF (≡ 3.5NF):** pentru orice FD netrivială `X→A`, **X este supercheie**.
**4NF:** pentru orice **dependență multivaluată** netrivială `X↠A`, **X este supercheie**.

**RELAȚIA DINTRE ELE (întrebare cheie!):**
```
1NF ⊃ 2NF ⊃ 3NF ⊃ BCNF ⊃ 4NF   (fiecare o rafinează pe precedenta)
4NF ⟹ BCNF ⟹ 3NF ⟹ 2NF ⟹ 1NF  (mai tare implică mai slabă)
```
4NF e cea mai strictă. **BCNF ⟹ 3NF**, **4NF ⟹ BCNF** (se demonstrează).

**Exemple:** 2NF violat: `OS[nume,versiune,an,companie]` cu `nume→companie` (companie nu e dep. plin). 3NF violat: `[materie,an,castigator,IQ]` cu `castigator→IQ` (IQ tranzitiv). 4NF violat: `Student[cnp,nume,facultate,pasiune]` cu `cnp,nume ↠ facultate`.
</details>

---

## 3. Indexare (structuri de date, tipuri, eficiență) 🔴 (~8 apariții)
> *„Indexare — rol, structuri, tipuri, eficiență. Când/cum/de ce se folosesc. Hash și bitmap: structuri și eficiență."*

<details><summary>📖 Răspuns</summary>

**Rol:** SGBD-ul petrece majoritatea timpului **căutând**. Un **fișier index** (perechi `valoare cheie → pointer`, mai mic decât datele) accelerează căutarea după un atribut care nu e cheie de sortare.

**Când/cum:** pe coloane cu valori **distincte**, selectivitate ridicată, folosite în JOIN. NU pe tabele mici sau coloane modificate des.

**Tipuri (structuri de date + eficiență):**
- **B+ arbori** (cei mai folosiți): arbore **echilibrat**, doar frunzele trimit spre date (legate în listă → bune pentru **interval** și `ORDER BY`). **Complexitate O(log n)** la căutare/inserare/ștergere. La inserare, nod plin → **split** propagat în sus. *(Interogare punctuală + interval.)*
- **B-arbori:** o singură apariție a valorii (și în noduri interne) → mai adânci, implementare grea → **B+ preferat**.
- **Indecși hash:** funcție hash → **bucket-uri**; **coliziune** = chei diferite în același bucket. Eficiență: **O(1) pentru căutare punctuală**, dar **INEFICIENT pentru interval**. Static vs dinamic/extensibil.
- **Indecși bitmap:** pentru atribute cu **puține valori distincte**; un **șir binar** per valoare (1 = înregistrarea are acea valoare). Selecții multiple → operatori pe biți **AND/OR/NOT** (foarte rapizi).

- **Index dens** (o intrare per valoare) vs **rar** (doar unele, când cheia de căutare = cheia de sortare); **primar** (cheia de căutare = cheia de sortare, max 1/tabel) vs **secundar**.

**SQL:** `CREATE INDEX i ON tabel(col);` / `DROP INDEX i;`. Oracle: implicit B+ arbori, suportă bitmap, NU hash.
</details>

---

## 4. Dependențe funcționale 🔴 (~5 apariții)
> *„Dependențe funcționale: definiție, exemple, rol în modelare. Axiomele lui Armstrong."*

<details><summary>📖 Răspuns</summary>

**Definiție:** relația `r` satisface `X → Y` dacă `∀ t₁,t₂ ∈ r: t₁[X]=t₂[X] ⟹ t₁[Y]=t₂[Y]` (dacă coincid pe X, coincid pe Y). Spunem că **X determină funcțional Y**.

**Exemplu:** `nume → l(nume)`, `data_nastere → varsta`, `data_nastere → zodie`.

**Rol în proiectare:** îmi arată la ce **COLOANE pot renunța** ca să le pot reface (elimină redundanța); se folosesc la **găsirea cheilor candidat** și la **normalizare** (2NF, 3NF, BCNF).

**Axiomele lui Armstrong** (reguli de inferență, complete și corecte):
```
A1 (reflexivitate):  A₁..Aₙ → Aᵢ
A2 (augmentare):     X→Y ⟹ XZ→YZ
A3 (tranzitivitate): X→Y, Y→Z ⟹ X→Z
```
Alte proprietăți: uniune, descompunere, pseudotranzitivitate. **Închiderea** `X⁺` = atributele determinate de X; `X→Y` e adevărată ⟺ `Y ⊆ X⁺`.
</details>

---

## 5. Dependențe multivaluate 🔴 (~5 apariții)
> *„Ce sunt dependențele multivaluate, unde se folosesc, exemple. Unde intervin în proiectare."*

<details><summary>📖 Răspuns</summary>

**Definiție:** cu Z = U−XY („rest"), `r` satisface `X ↠ Y` dacă pentru orice t₁,t₂ cu t₁[X]=t₂[X] există tuplele care combină Y-ul unuia cu Z-ul celuilalt. Intuitiv: pentru un X fixat, **Y e independent de Z** (apar toate combinațiile).

**Exemplu:** persoana CNP=1 admisă la 2 facultăți (Info, Mate) ȘI cu permis pentru 2 categorii (A, B) — facultatea și permisul sunt **independente** → trebuie toate 4 combinațiile:
```
1 Info A  |  1 Mate B  |  1 Info B (deductibil)  |  1 Mate A (deductibil)
```
Deci `CNP ↠ Facultate`.

**Unde intervin (rol):**
- FD-urile îmi spun la ce **coloane** renunț; MVD-urile îmi spun la ce **LINII** (redundante) renunț ca să le pot reface;
- stau la baza formei normale **4NF**;
- **legătura cu FD:** dacă `X → Y`, atunci și `X ↠ Y` (orice FD e și MVD).
</details>

---

## 6. Chei candidat și constrângeri 🟠 (~6 apariții)
> *„Cheie candidat — cum și unde se folosește, cum se obține. Constrângeri (chei, tipuri de chei)."*

<details><summary>📖 Răspuns</summary>

**Tipuri de chei:**
- **Supercheie** — identifică **unic** un tuplu;
- **Cheie candidat** — supercheie **minimală** (`X⁺=U` și nicio submulțime proprie nu e supercheie);
- **Cheie primară** — cheia candidat aleasă;
- **Cheie alternativă** — candidate neselectate;
- **Cheie străină** — referă o cheie candidat a **altei** relații.

**Cum obții cheile candidat** (din dependențe): organizezi atributele — cele **doar în stânga** dependențelor sunt **prime** (obligatoriu în cheie), cele **doar în dreapta** sunt **neprime**; calculezi `X⁺` și verifici minimalitatea.
*Ex: `Σ={A→BD, B→C, DE→F}` → stânga {A,E} → `AE⁺=U` → **AE e cheie**.*

**Constrângeri de integritate (SQL):** `NOT NULL`, `UNIQUE` (cheie candidat 1 atribut), `PRIMARY KEY` (implicit NOT NULL+UNIQUE), `FOREIGN KEY ... REFERENCES` (integritate referențială), `CHECK(condiție)`. Integritate referențială: valoarea cheii străine trebuie să existe în cheia primară referită; `ON DELETE/UPDATE RESTRICT | SET NULL | CASCADE`.
</details>

---

## 7. Subinterogări (corelate / necorelate) 🟠 (~5 apariții)
> *„Subinterogări în SQL. Necorelate vs corelate. Exemple. Planul de execuție în cele două cazuri."*

<details><summary>📖 Răspuns</summary>

**Subinterogare** = interogare imbricată în alta (în `WHERE`/`FROM`/`SELECT`).

**NECORELATĂ** — interioara **NU depinde** de exterioară → se execută **o singură dată**:
```sql
SELECT nume FROM studenti WHERE an = (SELECT MAX(an) FROM studenti);
```
*Plan:* subinterogarea rulează **întâi și o dată**, rezultatul e reutilizat.

**CORELATĂ** — interioara **depinde** de un atribut al exterioarei → se **re-execută pentru fiecare tuplu**:
```sql
SELECT s.nume FROM studenti s
WHERE s.bursa > (SELECT AVG(b.bursa) FROM studenti b WHERE b.an = s.an);
```
*Plan:* subinterogarea rulează **per tuplu** al exterioarei → în general **mai costisitoare** (poate fi rescrisă ca join / cu `EXISTS`).
</details>

---

## 8. Tranzacții și ACID 🟠 (~4 apariții)
> *„Ce este o tranzacție? ACID. Comenzi SQL. Ce marchează începutul și încheierea unei tranzacții?"*

<details><summary>📖 Răspuns</summary>

**Tranzacție** = secvență de operații executată ca o **unitate logică** (totul sau nimic).

**ACID:**
- **A — Atomicitate:** toate operațiile sau niciuna (revenire la starea inițială la eșec);
- **C — Consistență:** la final, datele sunt corecte conform logicii;
- **I — Izolare:** tranzacțiile nu se influențează, chiar simultane;
- **D — Durabilitate:** după `COMMIT`, efectele sunt permanente.

**Comenzi SQL (TCL):**
```sql
BEGIN TRANSACTION;   -- ÎNCEPUTUL tranzacției
   ... operații ...
SAVEPOINT s1;        -- punct de restaurare
COMMIT;              -- ÎNCHEIEREA cu succes (permanent)
ROLLBACK;            -- anulare (revenire la starea inițială / savepoint)
```
- **Începutul:** `BEGIN TRANSACTION` (implicit la prima comandă).
- **Încheierea:** `COMMIT` (salvează) sau `ROLLBACK` (anulează).

> BD nerelaționale folosesc **BASE** (Basic Availability, Soft-state, Eventual consistency). **CAP:** un sistem distribuit satisface doar 2 din Consistency/Availability/Partition tolerance.
</details>

---

## 9. Tabele virtuale (view-uri) 🟠 (~4 apariții)
> *„View-uri: ce sunt, rol, tipuri, comportament la interogare și la actualizare. Când se pot actualiza?"*

<details><summary>📖 Răspuns</summary>

**View (tabel virtual)** = o **interogare stocată** peste tabele/alte view-uri. `CREATE VIEW nume AS <select>;`

**Rol:** acces **modular**; **ascunderea** datelor față de anumiți utilizatori; **ușurarea** interogărilor complexe.

**La interogare:** se comportă ca un tabel; interogarea e **rescrisă** inserând definiția view-ului, apoi optimizată.

**La actualizare:** modificările trebuie **rescrise** pe tabelele de bază — nu e mereu posibil. **View actualizabil (updatable)** dacă: `SELECT` **fără `DISTINCT`** pe **o singură tabelă**, atributele lipsă pot fi NULL/default, subinterogările nu referă tabela, **fără `GROUP BY`/agregare**. Alternativă: trigger **`INSTEAD OF`**.

**View materializat:** rezultatul e **stocat efectiv** → interogări rapide, dar ocupă spațiu și trebuie reîmprospătat la modificarea tabelelor de bază.
</details>

---

## 10. Tipuri de join și algoritmi de join 🟠 (~4 apariții)
> *„Diferite tipuri de join. Join natural, intern, extern (stânga/dreapta), self join. Algoritmi de join."*

<details><summary>📖 Răspuns</summary>

**Tipuri de join (+ SQL):**
- **Join natural** (`⋈` / `NATURAL JOIN`) — după atributele comune, egale;
- **Join intern / θ-join / equijoin** (`JOIN..ON`) — după o condiție;
- **Join extern stânga** (`LEFT JOIN`) — tot din stânga + potriviri (NULL unde lipsesc);
- **Join extern dreapta** (`RIGHT JOIN`) — tot din dreapta;
- **Join extern plin** (`FULL JOIN`) — stânga ∪ dreapta;
- **Self join** — o tabelă cu ea însăși (alias-uri).

**Algoritmi de join (alegerea depinde de costul estimat):**
- **Nested-loop join** — pentru fiecare tuplu din r, parcurgi tot s (cost mare);
- **Indexed nested-loop** — folosești un **index** pe atributul de join al interioarei (echi/natural join);
- **Merge join** — **sortezi** ambele relații după atributul de join, apoi fuzionezi (doar echi-join);
- **Hash join** — o funcție hash partiționează ambele relații; compari doar tuplele din partiții corespondente (doar echi-join).
</details>

---

## 11. B+ arbori (structură, complexitate) 🟡 (2 apariții)
> *„Ce sunt arborii B+ și unde sunt folosiți? Complexitatea inserare/căutare/ștergere. (posibil: desenează un B+)"*

<details><summary>📖 Răspuns</summary>

**B+ arbore** = structură de **index ordonat**, cel mai folosit în SGBD-uri relaționale.
- arbore **echilibrat** (toate frunzele pe același nivel);
- nod cu constanta **m** (max m valori, m+1 pointeri), calibrat = un bloc de disc;
- **doar frunzele** trimit spre fișierul de date; nivelurile superioare = index rar; **frunzele legate** (listă dublu înlănțuită) → bune pentru interogări de **interval** și `ORDER BY`;
- **Complexitate: O(log n)** la căutare, inserare, ștergere (căutare ≤ `log⌈(m+1)/2⌉(K)` blocuri).
- Inserare: găsești frunza; dacă e plină → **divizare (split)**, propagată în sus (poate crește adâncimea). Ștergere: dacă frunza rămâne cu prea puține → **contopire/redistribuire**.

*De desenat:* rădăcină → noduri interne (chei + pointeri) → frunze (valori sortate + pointeri spre date + pointer spre frunza următoare).
</details>

---

## 12. Pașii procesării interogărilor + plan de execuție 🟡 (2 apariții)
> *„Pașii unei interogări. Plan de execuție (cum e procesat un query)."*

<details><summary>📖 Răspuns</summary>

```
COMPILARE:
  1. Analiză sintactică (parsare) → arbore de parsare
  2. Analiză semantică / preprocesare: rescrierea view-urilor, verificarea
     existenței tabelelor/atributelor și a tipurilor
  3. Rescriere în algebra relațională → PLAN LOGIC
OPTIMIZARE:
  4. Optimizarea planului logic (rescrieri echivalente: împinge selecțiile/
     proiecțiile devreme, ordonează join-urile)
  5. Selecția algoritmilor + ordinii → PLAN FIZIC (primitive de execuție)
EXECUȚIE:
  6. Execuția planului fizic
```
**Optimizare cheie:** aplici **selecțiile și proiecțiile cât mai devreme** (reduci dimensiunea relațiilor înainte de join); ordonarea join-urilor (multe variante → programare dinamică). **Plan de execuție** = arborele de operatori ales; în Oracle `EXPLAIN PLAN FOR <sql>`. Costul dominant = **accesul la disc**.
</details>

---

## 13. Descompunere join cu / fără pierdere 🟡 (2 apariții)
> *„Descompunere join cu și fără pierdere."*

<details><summary>📖 Răspuns</summary>

O descompunere ρ={R₁,R₂,...} e **fără pierdere** (lossless) dacă orice relație se reconstruiește exact prin join: `r = r[R₁] ⋈ r[R₂] ⋈ ...`. Altfel e **cu pierdere** (apar tuple false).

**Teoremă (2 scheme):** ρ={R₁,R₂} e fără pierdere ⟺
```
R₁∩R₂ → R₁−R₂ ∈ Σ⁺   SAU   R₁∩R₂ → R₂−R₁ ∈ Σ⁺
```
(atributul comun determină restul uneia dintre scheme).

**Exemplu** — R[A,B,C], Σ={A→B}:
- ρ₁={[A,B],[A,C]} — **fără pierdere** (A→B ∈ Σ⁺). ✓
- ρ₂={[A,B],[B,C]} — **cu pierdere** (B→A, B→C ∉ Σ⁺). ✗

Se folosește în descompunerea în **BCNF/4NF** (descompui păstrând proprietatea fără pierdere).
</details>

---

## 14. Declanșatoare (triggere) ⚪ (1 apariție)
> *„Triggere."*

<details><summary>📖 Răspuns</summary>

**Trigger (declanșator)** = constrângere **dinamică**; monitorizează schimbări în BD, verifică o condiție și execută o acțiune. Regulă **eveniment–condiție–acțiune**; introduce logica aplicației în SGBD.
```sql
CREATE TRIGGER nume
  BEFORE | AFTER | INSTEAD OF  {INSERT | DELETE | UPDATE} ON tabel
  [REFERENCING OLD/NEW ROW/TABLE AS var]
  [FOR EACH ROW]              -- tip row (per linie) vs statement (per comandă)
  [WHEN (condiție)]
  acțiune;
```
*Ex (ștergere cascadă):* `AFTER DELETE ON S ... DELETE FROM R WHERE A = O.B`.
**Probleme:** ordinea când mai multe triggere se activează; **înlănțuire/ciclare** (un trigger activează altul).
</details>

---

> ⭐ **Strategie rapidă:** primele 3 teme (**algebra relațională + SQL**, **forme normale**, **indexare**) apar la aproape fiecare examen. Adaugă **dependențele** (funcționale + multivaluate) și **cheile** → ai acoperit miezul BD. Exersează să **desenezi un B+ arbore** (te pot pune la tablă).
