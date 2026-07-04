# SD — Toate întrebările de licență (2019–2025), ordonate după frecvență
> Întrebările reale din ultimii 6 ani (fără 2020), grupate pe teme și **ordonate de la cele mai frecvente la cele mai rare**. Apasă pe **„📖 Răspuns"**.
> Legendă: 🔴 foarte frecvent · 🟠 frecvent · 🟡 de 2 ori · ⚪ o dată.

---

## 1. Complexitate & analiza eficienței (caz fav/nefav, O/Ω/Θ, ordin de creștere) 🔴 (~11 apariții)
> *„Eficiență și complexitate în timp. Caz favorabil/nefavorabil. Exemple. Ordinul de creștere. Notații asimptotice O, Omega, Theta. Clasificarea algoritmilor folosind O."*

<details><summary>📖 Răspuns</summary>

**Analiza eficienței** estimează resursele (timp/spațiu) în funcție de **dimensiunea problemei** `n`, pe modelul **RAM** (fiecare operație elementară = 1 unitate de timp).

**Cele 3 cazuri** (timpul depinde și de datele de intrare):
- **favorabil** (best) — cel mai mic timp (margine inferioară);
- **nefavorabil** (worst) — cel mai mare timp (**margine superioară — cel mai important**);
- **mediu** (average) — media pe toate intrările.

**Exemplu (căutare secvențială a lui `v` în tablou de `n`):**
- favorabil: `v` pe prima poziție → **O(1)**;
- nefavorabil: `v` lipsește/e ultima → **O(n)**.

**Notațiile asimptotice:**
- **O(g)** — margine **superioară** („cel mult"): `0 ≤ f(n) ≤ c·g(n)`.
- **Ω(g)** — margine **inferioară** („cel puțin"): `f(n) ≥ c·g(n)`.
- **Θ(g)** — margine **strânsă** (și sup, și inf): `Θ(g) = O(g) ∩ Ω(g)`.

**Ordin de creștere:** păstrezi termenul dominant, ignori constantele (`3n²+100n → Θ(n²)`).

**Clasificarea algoritmilor folosind O** (crescător):
```
O(1) ⊂ O(log n) ⊂ O(n) ⊂ O(n log n) ⊂ O(n²) ⊂ O(n³) ⊂ O(2ⁿ) ⊂ O(n!)
const  logaritmic liniar liniaritmic pătratic cubic  exponențial factorial
```
</details>

---

## 2. Complexitatea funcțiilor recursive (cele 4 metode + Teorema Master) 🔴 (~9 apariții)
> *„Funcții recursive și complexitatea timp. Cele 4 metode: substituție, iterație, arbori de recursie, Teorema Master. (le-au pus la TOATE)"*

<details><summary>📖 Răspuns</summary>

O funcție recursivă are **caz de bază** (oprire) + **caz general** (auto-apel pe o problemă mai mică). Timpul se descrie printr-o **relație de recurență** care se rezolvă prin **4 metode**:

**1. Metoda substituției** — **ghicești** soluția, apoi o demonstrezi prin **inducție matematică**.
*Ex: `T(n)=2T(n/2)+n` → ghicim `O(n log n)`, demonstrăm `T(n) ≤ cn log n`.*

**2. Metoda iterației** — **desfășori** recurența (înlocuiești termenii succesiv) până la cazul de bază, apoi **însumezi**.
*Ex: `T(n)=T(n-1)+1, T(1)=0` → `T(n)=n-1`.*

**3. Arborele de recursie** — desenezi un arbore în care fiecare nod = costul unei subprobleme; aduni costurile **pe niveluri**, apoi totul.
*Ex: `T(n)=3T(n/4)+cn²` → sumă de serie geometrică → O(n²).*

**4. Teorema Master** — formulă **directă** pentru `T(n) = a·T(n/b) + f(n)` (a≥1, b>1). Compari `f(n)` cu `n^(log_b a)`:
- `f` mai mic → `T(n) = Θ(n^(log_b a))`;
- `f` la fel → `T(n) = Θ(n^(log_b a) · log n)`;
- `f` mai mare (+ condiție) → `T(n) = Θ(f(n))`.
> NU se aplică dacă `f` diferă doar printr-un factor **logaritmic** (ex. `2T(n/2)+n log n`).

*Ex Fibonacci recursiv `fib(n)=fib(n-1)+fib(n-2)` → **O(φⁿ)** exponențial (recalculează); iterativ → O(n).*
</details>

---

## 3. Sortare prin numărare și prin distribuire 🔴 (~9 apariții)
> *„Sortarea prin numărare (counting sort). Sortarea prin distribuire (bucket sort). Ce fac (nu trebuie complexitățile pe de rost)."*

<details><summary>📖 Răspuns</summary>

Ambele sunt sortări **NEbazate pe comparații** → pot depăși limita `O(n log n)`.

**Counting sort (prin numărare):**
- **Ipoteză:** valori întregi într-un **interval mic** {1..k}.
- **Idee:** pentru fiecare valoare numeri **câte elemente sunt ≤ ea** → asta îți dă direct poziția în tabloul sortat.
- Pași: (1) numeri aparițiile fiecărei valori, (2) sume cumulate, (3) plasezi fiecare element pe poziția dată de cumulat.
- **Complexitate:** `O(n + k)`. Stabilă.

**Bucket sort (prin distribuire):**
- **Ipoteză:** valori distribuite **uniform** într-un interval (ex. [0,1)).
- **Idee:** împarți intervalul în `n` „găleți" egale, distribui elementele în găleți (`⌊n·a[i]⌋`), sortezi fiecare găleată, apoi concatenezi.
- **Complexitate medie:** `O(n)`.

**Diferența:** counting = numeri aparițiile (întregi în interval); bucket = distribui în găleți (date uniforme).
</details>

---

## 4. Arbori binari de căutare (ABC) — reprezentare și operații 🔴 (~8 apariții)
> *„Arbori binari de căutare. Reprezentare și operații. Cum arată nodul, cum sunt datele aranjate."*

<details><summary>📖 Răspuns</summary>

**Definiție:** arbore binar în care pentru **orice nod**: valorile din **subarborele stâng < nod < valorile din subarborele drept**.

**Reprezentare (nodul):** structură cu 3 câmpuri:
```cpp
struct Nod { int val; Nod* stg; Nod* drp; };
```
Datele sunt aranjate astfel încât mai mici mereu la stânga, mai mari la dreapta.

**Proprietate cheie:** parcurgerea **în inordine** (stânga → rădăcină → dreapta) dă valorile **sortate crescător**.

**Operații** (toate `O(h)`, h = înălțime):
- **Căutare** `x`: pornești din rădăcină; `x < nod` → stânga, `x > nod` → dreapta.
- **Inserare** `x`: cauți locul (ca la căutare) și adaugi ca **frunză**.
- **Ștergere** `x`: 3 cazuri — (1) fără fii → ștergi direct; (2) un fiu → îl înlocuiești cu fiul; (3) doi fii → înlocuiești cu **predecesorul** (max din subarborele stâng), apoi ștergi acel nod.

**Complexitate:** mediu `O(log n)`; **nefavorabil `O(n)`** (arbore **degenerat** = listă). De aceea folosim arbori **echilibrați** (AVL, roșu-negru) → garantat O(log n).
</details>

---

## 5. Structuri Union-Find (colecții de mulțimi disjuncte) 🔴 (~8 apariții, „x3" într-un an!)
> *„Structuri de tip union-find. ȘI la ce sunt folositoare (te întreabă explicit)."*

<details><summary>📖 Răspuns</summary>

**Union-Find** = TAD pentru gestionarea unei **partiții** (colecție de submulțimi **disjuncte**) a mulțimii {0..n-1}.

**Operații:**
- `find(i)` — întoarce **reprezentantul** submulțimii care conține `i`;
- `union(i, j)` — **reunește** submulțimile lui `i` și `j`;
- `singleton(i)` — creează o submulțime cu doar `i`.

**Reprezentare:** fiecare submulțime = un **arbore**; reprezentantul = rădăcina. Se stochează cu tabloul **`parinte[]`** (`parinte[rădăcină] = -1`).
```
find(i): urci din i prin parinte[] până la rădăcină (unde parinte = -1)
union(i,j): legi rădăcina unuia sub rădăcina celuilalt
```

**Optimizări** (aproape O(1) amortizat): **union ponderat** (arborele mic sub cel mare) + **aplatizarea drumului** (path compression). Complexitate: `O(n + m·log* n)`.

**⚠️ LA CE FOLOSEȘTE** (întrebarea capcană — Gațu):
- **componente conexe** ale unui graf;
- **algoritmul lui Kruskal** (arbore parțial minim);
- rețele de calculatoare (sunt conectate?);
- pixeli/regiuni într-o imagine.
</details>

---

## 6. Tipurile abstracte Stivă și Coadă 🔴 (~8 apariții)
> *„Tipurile abstracte de date Stiva și Coada. Ce e un tip abstract? (+ coada cu priorități, cum o implementezi)"*

<details><summary>📖 Răspuns</summary>

**Tip de date abstract (TAD)** = definește **obiectele** și **operațiile** permise (CE fac), separat de **implementare** (CUM). Aceeași Stivă poate fi implementată cu tablou SAU listă.

**STIVA — LIFO** (Last In, First Out):
- operații: `push(e)` (adaugă în vârf), `pop()` (scoate din vârf), `top()` (citește vârful), `esteVida()`;
- analogie: teanc de farfurii;
- aplicații: undo, istoric browser, apeluri recursive (stiva programului), DFS.

**COADA — FIFO** (First In, First Out):
- operații: `insereaza(e)` (la sfârșit), `elimina()` (scoate primul/cel mai vechi), `citeste()`, `esteVida()`;
- analogie: rândul la casă;
- aplicații: fire de așteptare, BFS.

**Implementare:** cu **tablou** (stiva: index vârf; coada: tablou **circular** cu `%Max`) sau cu **listă înlănțuită** (operații O(1) la capete).

**Coada cu priorități** (extra): se extrage mereu **cel mai prioritar** element (nu cel mai vechi); se implementează cu un **heap** (vezi tema Heap).
</details>

---

## 7. Tipuri de date. Definiție. Clasificare 🟠 (~6 apariții)
> *„Tipuri de date. Definiție. Clasificare."*

<details><summary>📖 Răspuns</summary>

**Tip de date** = un **domeniu** (colecția de valori posibile) + **operațiile** aplicabile asupra lor.

**Clasificare:**
- **Tipuri elementare:** întregi (+,-,*,/,%), reale, booleene (and/or/not/xor), caractere, **pointeri** (adrese + NULL, dereferențiere `*p`).
- **Tipuri structurate de nivel jos:** **tablouri** (ansamblu omogen, indexat), **structuri** (ansamblu eterogen de câmpuri), șiruri de caractere — operațiile lucrează la nivel de componentă.
- **Tipuri de nivel înalt (abstracte):** operațiile sunt implementate de algoritmi (ex. Stivă, Coadă, Listă, Arbore) — **TAD**-uri.

Costul operațiilor: model **cost uniform** (O(1) per operație) vs **cost logaritmic** (depinde de nr. de biți).
</details>

---

## 8. Liste liniare. Definiție. Moduri de implementare 🟠 (~6 apariții)
> *„Liste liniare. Definiție + moduri de implementare."*

<details><summary>📖 Răspuns</summary>

**Listă liniară** `L = (e₀, e₁, ..., e_{n-1})` — secvență de elemente în care fiecare (exceptând ultimul) are un **succesor** unic. Operații: `insereaza(k,e)`, `elimina(k)`, `alKlea(k)`, `poz(e)`, `lung()`, `parcurge()`.

**Două moduri de implementare:**

**1. Cu tablou:**
```
[e₀][e₁][e₂][e₃]   + un câmp „ultim"
```
- acces la al k-lea element `alKlea(k)`: **O(1)** (acces aleator);
- inserare/ștergere: **O(n)** (trebuie deplasate elementele).

**2. Cu listă înlănțuită** (noduri cu `elt` + pointer `succ`):
```
prim → [e₀|•] → [e₁|•] → [e₂|•] → NULL
```
- acces `alKlea(k)`: **O(n)** (parcurgi de la cap);
- inserare/ștergere (cu poziția dată): **O(1)** (refaci legături).

**Compromis:** tabloul e bun la **acces aleator**, lista e bună la **inserări/ștergeri**. (Variante: simplu/dublu înlănțuită, circulară, **listă ordonată** — căutare O(log n) cu tablou prin căutare binară.)
</details>

---

## 9. Coada cu priorități. Min/Max Heap 🟠 (~5 apariții)
> *„Tipul abstract coadă cu priorități. Structuri Min/Max Heap."*

<details><summary>📖 Răspuns</summary>

**Coadă cu priorități** (TAD): elementele au o **prioritate**; se extrage mereu **cel mai prioritar** (nu cel mai vechi). Operații: `insereaza(atom)`, `citeste()` (cel mai prioritar), `elimina()`.

**Max-Heap** (implementarea): arbore binar **complet** cu proprietatea: **cheia oricărui nod ≥ cheile fiilor** → **maximul e mereu în rădăcină**. (Min-heap: părinte ≤ fii → minimul în rădăcină.)

**Reprezentare cu tablou** (fără pointeri):
```
Pentru nodul de la indexul i:
   fiu stâng = 2i+1,   fiu drept = 2i+2,   părinte = (i-1)/2
```

**Inserare** (`O(log n)`): adaugi la sfârșit, apoi **urci** (sift-up) cât timp e mai mare decât părintele.
**Eliminare a maximului** (`O(log n)`): înlocuiești rădăcina cu ultimul nod, ștergi ultimul, apoi **cobori** (sift-down) cât timp e mai mic decât un fiu.

Înălțimea heap-ului = `O(log n)` (arbore complet). Stă și la baza **Heap Sort** (O(n log n), O(1) spațiu).
</details>

---

## 10. Arbori binari de căutare echilibrați „roșu-negru" 🟠 (~5 apariții)
> *„Arbori roșu-negru. Complexități, operații."*

<details><summary>📖 Răspuns</summary>

**Arbore roșu-negru** = ABC în care fiecare nod are o **culoare** (roșu/negru), respectând **4 proprietăți**:
1. fiecare nod e roșu sau negru;
2. rădăcina și frunzele (nil) sunt **negre**;
3. un nod **roșu** are ambii fii **negri** (fără doi roșii consecutivi);
4. orice drum de la un nod la frunze are **același număr de noduri negre**.

Aceste reguli garantează un echilibru „relaxat": **h ≤ 2·log₂(n+1)** → toate operațiile în **O(log n)**.

**Operații:**
- **Căutare/inordine** — ca la ABC normal, O(log n).
- **Inserare:** inserezi ca în ABC, **colorezi nodul nou ROȘU**, apoi restaurezi proprietățile prin **recolorări** și **rotații** (în funcție de culoarea „unchiului": caz 1 = unchi roșu → recolorare; caz 2/3 = unchi negru → rotații).
- **Ștergere:** similar, cu recolorări/rotații pentru a menține proprietățile.

**Utilizări reale:** C++ STL (`map`, `set`, `multimap`), Java `TreeMap`/`TreeSet`, kernel Linux (CFS scheduler).
</details>

---

## 11. Grafuri. Digrafuri. Reprezentare și algoritmi de parcurgere 🟠 (~5 apariții)
> *„Grafuri. Digrafuri. Definiție. Moduri de reprezentare (liste, matrice de adiacență). Algoritmi de parcurgere."*

<details><summary>📖 Răspuns</summary>

**Graf** `G=(V,E)` — vârfuri + **muchii** (perechi **neordonate** `{i,j}={j,i}`).
**Digraf** `D=(V,A)` — vârfuri + **arce** (perechi **ordonate** `(i,j)≠(j,i)`).

**Moduri de reprezentare:**
- **Matrice de adiacență** `a[i][j]=1` dacă există muchie/arc i→j: spațiu **O(n²)**, test „există i→j?" O(1); bună pentru grafuri **dense**. La graf neorientat = simetrică.
- **Liste de adiacență** (tablou de liste, `a[i]` = vecinii lui i): spațiu **O(n+m)**; bună pentru grafuri **rare**.

**Algoritmi de parcurgere** (ambii `O(n+m)`):
- **DFS (în adâncime)** — folosește o **stivă** (sau recursie); merge cât poate în adâncime, apoi backtrack.
- **BFS (în lățime)** — folosește o **coadă**; vizitează nivel cu nivel (toți vecinii direcți întâi).

Diferența cheie: **DFS = stivă**, **BFS = coadă**.
</details>

---

## 12. Tablouri și structuri. Definiții și exemple 🟠 (~4 apariții)
> *„Tablouri și structuri. Definiții, exemple, notații, caz favorabil/nefavorabil, metode de estimare a timpului."*

<details><summary>📖 Răspuns</summary>

**Tablou (array):** ansamblu **omogen** de componente de **același tip**, identificate prin **indici**, în memorie **contiguă**.
- 1-dimensional `a[0..n-1]`; 2-dimensional `a[m][n]` (memorat linie cu linie).
- Acces `a[i]`: **O(1)**. Folosit pentru mulțimi, secvențe, matrici.

**Structură (struct/record):** ansamblu **eterogen** de **câmpuri** (tipuri diferite), fiecare cu nume propriu.
```cpp
struct Punct { int x; int y; };        // câmpuri de tipuri (posibil) diferite
```
- acces `S.camp` sau `p->camp` (dacă p e pointer); memorie contiguă, în ordinea declarării.

**Diferența:** tablou = **omogen** (același tip); structură = **eterogen** (tipuri diferite).
Notații & estimarea timpului: identifici operația dominantă, numeri câte ori se execută, analizezi caz favorabil/nefavorabil (vezi tema Complexitate).
</details>

---

## 13. Arbori binari de căutare echilibrați AVL 🟠 (~3 apariții)
> *„Arbori AVL."*

<details><summary>📖 Răspuns</summary>

**Arbore AVL** (Adelson-Velskii & Landis) = ABC în care, pentru **orice nod**:
```
| înălțime(subarbore stâng) − înălțime(subarbore drept) | ≤ 1
```
Diferența = **factorul de echilibrare** (∈ {−1, 0, +1}).

Garantează înălțimea `Θ(log n)` → căutare/inserare/ștergere în **O(log n)**.

**Reechilibrare prin ROTAȚII** (după inserare/ștergere): 4 tipuri — stânga simplă, dreapta simplă, stânga-dreapta dublă, dreapta-stânga dublă. O rotație = O(1).
```cpp
// rotație simplă la stânga:
y = x->drp;  x->drp = y->stg;  y->stg = x;  return y;
```
**Algoritm inserare:** inserezi ca în ABC, reții drumul rădăcină→nod, apoi parcurgi invers și **reechilibrezi** nodurile dezechilibrate cu rotații.

**AVL vs roșu-negru:** AVL e **mai strict echilibrat** (căutări mai rapide, dar mai multe rotații la inserare/ștergere) → preferat când ai **multe căutări** și puține modificări.
</details>

---

## 14. Componente conexe și componente tare conexe 🟠 (~3 apariții)
> *„Determinarea componentelor conexe și tare conexe și algoritmi pentru a le afla."*

<details><summary>📖 Răspuns</summary>

**Componentă conexă** (graf **neorientat**) = submulțime maximală de vârfuri între care există **drumuri**. Un graf e **conex** dacă are o singură componentă.
**Algoritm** (DFS cu colorare): pornești un DFS din fiecare vârf necolorat; fiecare DFS colorează o componentă întreagă. Numărul de DFS-uri = numărul de componente. **O(n+m)**.

**Componentă tare conexă** (digraf) = mulțime maximală de vârfuri în care între **oricare** u, v există drum **u→v ȘI v→u** (în ambele sensuri).
**Algoritm** (Kosaraju, 2 DFS-uri):
```
1. DFS pe D → reține timpii finali de vizitare
2. Calculează Dᵀ (graful TRANSPUS — inversează toate arcele)
3. DFS pe Dᵀ luând vârfurile în ordinea DESCRESCĂTOARE a timpilor finali
4. Fiecare arbore DFS de la pasul 3 = o componentă tare conexă
```
Complexitate totală: **O(n+m)**.
</details>

---

## 15. Algoritmi de sortare bazați pe comparații 🟠 (~3 apariții)
> *„Sortări prin comparație. + Ce algoritm ai folosi pentru un set foarte mare de date?"*

<details><summary>📖 Răspuns</summary>

Sortări care compară elemente între ele (limita teoretică: **O(n log n)**).

| Algoritm | Favorabil | Mediu | Nefavorabil | Stabil? | Spațiu |
|----------|-----------|-------|-------------|---------|--------|
| Bubble/Insertion/Selection | n / n / n² | n² | n² | da/da/nu | O(1) |
| **Heap sort** | n log n | n log n | n log n | nu | O(1) |
| **Merge sort** | n log n | n log n | n log n | **da** | O(n) |
| **Quick sort** | n log n | n log n | **n²** | nu | O(log n) |

**Divide-et-impera** (merge & quick): divide → rezolvă recursiv → combină.
- **Merge:** divizarea banală (la mijloc), munca la **combinare** (interclasare).
- **Quick:** munca la **divizare** (partiționare în jurul pivotului); pivot prost → O(n²).

**Pentru un set FOARTE MARE cu garanție O(n log n) + stabilitate → MERGE SORT** (dar consumă O(n) spațiu). Dacă vrei O(1) spațiu și garanție → **Heap sort**. Quick sort e rapid în medie, dar nu garantează worst-case.
</details>

---

## 16. Tabele de dispersie. Ce se întâmplă în caz de coliziune 🟡 (2 apariții)
> *„Tabele de dispersie. Ce se întâmplă în caz de coliziune."*

<details><summary>📖 Răspuns</summary>

**Tabelă de dispersie (hash):** o **funcție hash** `h(k)` mapează cheia `k` la o poziție din tablou `{0..m-1}` → acces mediu **O(1)**.

**Coliziune** = două chei **diferite** produc **aceeași** valoare hash (aceeași poziție). Rezolvare — două metode:

**1. Înlănțuire (dispersie EXTERNĂ):** fiecare slot păstrează o **listă** cu toate elementele care au aceeași valoare hash.
```
T[1] → [k1] → [k7] → NULL   (k1, k7 au h=1)
```
Simplu, dar necesită spațiu suplimentar. Căutare medie `Θ(1+α)` (α = n/m = factor de încărcare).

**2. Adresare deschisă (dispersie INTERNĂ):** toate elementele în tablou; la coliziune **cauți altă poziție liberă** prin examinare:
- **liniară:** `h(k,i) = (h'(k)+i) mod m` (grupare primară);
- **pătratică:** `(h'(k)+c₁i+c₂i²) mod m`;
- **dublă:** `(h₁(k)+i·h₂(k)) mod m` (cele mai bune rezultate).

Funcții hash uzuale: diviziune `h(k)=k mod m` (m prim), înmulțire.
</details>

---

## 17. Arbori digitali (trie). Arbori digitali compactați ⚪ (1 apariție)
> *„Arbori digitali. Arbori digitali compactați."*

<details><summary>📖 Răspuns</summary>

**Arbore digital (trie)** = structură pentru **șiruri de caractere** (information retrieval). Cheile sunt secvențe de cifre/litere; **drumul de la rădăcină** descrie cheia (nu valoarea din nod).

- arbore **k-ar** (k = mărimea alfabetului); fiecare nod are până la k fii;
- **economie de memorie** când există multe **prefixe comune** (prefixul e stocat o singură dată).

**Operații** (m = lungimea cheii — **nu depinde de n!**):
- **Căutare** `a`: parcurgi drumul descris de literele `a[0..m-1]` → **O(m)**;
- **Inserare:** parcurgi drumul; unde nu există nod, adaugi unul nou;
- **Ștergere:** parcurgi drumul; la întoarcere ștergi nodurile cu toți succesorii nil.

**Compactat (Patricia/radix trie):** lanțurile de noduri cu un singur fiu se „comprimă" într-un singur nod → economie de spațiu.
</details>

---

> ⭐ **Strategie rapidă:** primele 6 teme (complexitate, recursivitate, sortare numărare/distribuire, ABC, union-find, stive/cozi) = miezul examenului SD. Union-Find apare foarte des ȘI te întreabă „la ce folosește" — nu o ignora.
