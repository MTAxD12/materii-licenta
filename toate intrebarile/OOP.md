# OOP — Toate întrebările de licență (2019–2025), ordonate după frecvență
> Întrebările reale din ultimii 6 ani (fără 2020), grupate pe teme și **ordonate de la cele mai frecvente la cele mai rare**. Apasă pe **„📖 Răspuns"** pentru a-l dezvălui.
> Legendă frecvență: 🔴 foarte frecvent · 🟠 frecvent · 🟡 de 2 ori · ⚪ o dată.

---

## 1. Suprascriere (overriding) și polimorfism dinamic 🔴 (~9 apariții)
> *„Suprascrierea metodelor și polimorfism dinamic. Cum face polimorfismul alegerea între funcții? Static vs dinamic."*

<details><summary>📖 Răspuns</summary>

**Polimorfism** = „multe forme": același apel produce comportamente diferite.

**Static (compile-time / early binding):**
- se rezolvă la **compilare**;
- prin **overloading** (supraîncărcarea metodelor/operatorilor) și **template-uri**.

**Dinamic (runtime / late binding):**
- se rezolvă la **execuție**;
- prin **metode virtuale** = **overriding** (suprascriere): redefinești în clasa derivată o metodă `virtual` din bază.

**Cum alege polimorfismul funcția (mecanismul vtable):**
- fiecare clasă cu metode virtuale are un **vtable** (tabel de pointeri la funcții virtuale);
- fiecare obiect are un **vfptr** (pointer ascuns) spre vtable-ul clasei sale **reale**;
- la apelul `p->metoda()` prin pointer/referință la bază, se caută în vtable adresa metodei corespunzătoare **tipului real** al obiectului → se execută versiunea corectă.

```cpp
struct Animal { virtual void sunet() { cout << "..."; } };
struct Pisica : Animal { void sunet() override { cout << "Miau"; } };
Animal* a = new Pisica();
a->sunet();   // "Miau" — decis la runtime prin vtable
```

**Overloading vs overriding** (confuzie frecventă): overloading = aceeași metodă cu **semnături diferite** (static); overriding = redefinirea unei **metode virtuale** (dinamic).
Cuvinte cheie: `virtual`, `override`, `final`, destructor virtual.
</details>

---

## 2. Clase abstracte, interfețe, metode virtuale 🔴 (~9 apariții)
> *„Clase abstracte. Interfețe. Definiții, asemănări, deosebiri, metode virtuale. Când folosești una sau cealaltă?"*

<details><summary>📖 Răspuns</summary>

**Metodă virtuală** = metodă care poate fi **suprascrisă** în derivată; apelul se rezolvă la runtime (vtable). **Metodă pur virtuală**: `virtual void f() = 0;` (fără implementare).

**Clasă abstractă** = conține **cel puțin o metodă pur virtuală** → **nu poate fi instanțiată**. Poate avea date membre, constructori, metode concrete.

**Interfață** (convenție în C++) = clasă cu **NUMAI metode pur virtuale**, fără date membre, fără constructori (doar un contract).

**Asemănări:** ambele nu se pot instanția; ambele definesc un contract prin metode virtuale; ambele se folosesc polimorfic (pointer/ref la bază).

**Deosebiri:**
| | Clasă abstractă | Interfață |
|--|-----------------|-----------|
| date membre | da | nu |
| metode concrete | da (implementare parțială) | nu |
| relația | „is-a" puternică | contract slab („respectă specificația") |

**Când folosești (întrebarea Rădulescu):**
- **interfață** → când **nu știi nimic** despre cum trebuie implementat (doar contractul);
- **clasă abstractă** → când ai deja o **idee de funcționalitate**/comportament comun.
> Nu se exclud: `List` = contractul (interfață), `AbstractList` = comportamentul comun (abstractă), `ArrayList`/`LinkedList` = comportament specific.

**Implementare:** C++ → clasă cu metode `= 0` + moștenire `:`; Java → `interface`+`implements`, `abstract class`+`extends`.
</details>

---

## 3. Clase parametrizate (template-uri) 🔴 (~9 apariții)
> *„Clase parametrizate. Definiție. Exemple canonice. Implementare."*

<details><summary>📖 Răspuns</summary>

**Definiție:** o **clasă/funcție parametrizată cu tipuri** — scrii codul o singură dată, funcționează pentru orice tip. Mecanism de **polimorfism static** (compile-time): compilatorul generează cod separat pentru fiecare tip cu care e instanțiat.

**Funcție template:**
```cpp
template<typename T>
T maxim(T a, T b) { return a > b ? a : b; }
maxim(3, 5);      // T = int
maxim(2.1, 4.7);  // T = double
```

**Clasă template (exemplu canonic — un container):**
```cpp
template<typename T>
class Stiva {
    T elemente[100];
    int varf = -1;
public:
    void push(T x) { elemente[++varf] = x; }
    T pop() { return elemente[varf--]; }
};
Stiva<int> s;  Stiva<string> s2;
```

**Exemple canonice:** containere (vector, list, map din STL), `pair`, algoritmi generici (`sort`, `swap`).

**Detalii:** specializarea template (`template<> class Stiva<bool>`), parametri non-tip (`template<int N>`), `static_assert` pentru constrângeri. Tot codul e generat **la compilare** → verificare de tip la compilare, dar cod „umflat" (un exemplar per tip).
</details>

---

## 4. Relații de asociere între clase/obiecte 🔴 (~8 apariții)
> *„Relația de asociere. Definiție. Clasificări. Implementare. Exemple."*

<details><summary>📖 Răspuns</summary>

**Asociere** = o relație semantică/structurală între obiecte din clase diferite (un obiect „cunoaște"/„folosește" alt obiect). Se implementează prin **atribute care sunt referințe/pointeri** către alte obiecte.

**Clasificare (după tăria legăturii, de la slab la puternic):**

1. **Asociere simplă** — obiectele se cunosc, dar au cicluri de viață independente.
   *Ex: Student ↔ Disciplină (un student urmează discipline, disciplina există independent).*
2. **Agregare** (part-whole slab, „has-a") — întregul conține părți, dar **partea poate exista independent** de întreg.
   *Ex: Echipă — Jucători (jucătorul supraviețuiește dizolvării echipei).*
3. **Compoziție** (part-whole puternic) — partea **nu poate exista fără** întreg; ciclul de viață e legat (partea moare cu întregul).
   *Ex: Casă — Camere.*

**Multiplicitate:** 1..1, 0..*, 1..* (câte instanțe ale unei clase corespund uneia din cealaltă).

**Implementare (C++):**
```cpp
class Disciplina { /* ... */ };
class Student {
    vector<Disciplina*> discipline;   // asociere/agregare (pointeri — nu deține)
};
class Casa {
    Camera camere[5];                 // compoziție (obiecte membre — le deține)
};
```
Regulă practică: **agregare → pointer/referință** (nu deții); **compoziție → obiect membru** (deții, se distruge odată cu tine).
</details>

---

## 5. Principiul Dependențelor Inversate (D din SOLID) 🔴 (~8 apariții)
> *„Dependency Inversion. Definiție, exemplu unde NU e respectat, soluție."*

<details><summary>📖 Răspuns</summary>

**Definiție:** „Modulele de nivel **înalt** nu trebuie să depindă de cele de nivel **jos**. Ambele trebuie să depindă de **abstracții**." + „Abstracțiile nu depind de detalii; detaliile depind de abstracții."

**Exemplu unde NU e respectat** (cuplaj rigid cu o clasă concretă):
```cpp
class EmployeeService {
    MySqlEmployeeFinder finder;   // depinde DIRECT de o clasă concretă (SQL)
    Employee find(int id) { return finder.find(id); }
};
// Problemă: nu poți schimba sursa (XML, mock pt teste) fără a modifica EmployeeService.
```

**Soluția** (depinzi de o abstracție + Dependency Injection):
```cpp
struct IEmployeeFinder { virtual Employee find(int id) = 0; };  // abstracție
class EmployeeService {
    IEmployeeFinder& finder;                    // depinde de abstracție
public:
    EmployeeService(IEmployeeFinder& f) : finder(f) {}  // injectat prin constructor
    Employee find(int id) { return finder.find(id); }
};
// Acum poți da SqlFinder, XmlFinder, MockFinder — fără a modifica EmployeeService.
```
Semnal de violare: folosirea directă a lui `new`/clase concrete, metode statice. Soluție: **Dependency Injection**, principiul Hollywood („Don't call us, we'll call you").
</details>

---

## 6. Constructori și destructori 🔴 (~7 apariții)
> *„Constructor și destructor. Scop, responsabilități, cum se inițializează la moștenire, apelare în contextul derivării."*

<details><summary>📖 Răspuns</summary>

**Constructorul** — metodă specială, cu **numele clasei**, fără tip de return, apelată automat la crearea obiectului. Scop: **inițializarea** datelor membre, alocarea resurselor, validarea stării.
**Destructorul** — `~Clasa()`, fără parametri, fără return; apelat automat la distrugere. Scop: **eliberarea resurselor** (memorie, fișiere).

**Tipuri de constructori:** default, parametrizat, de **copiere** (`Clasa(const Clasa&)`), de **mutare** (`Clasa(Clasa&&)`), delegating. Inițializarea membrilor se face în **lista de inițializare** (`: a(x), b(y)`) — obligatorie pentru membri `const`/referință.

**Apelarea la derivare (ordinea):**
```
CONSTRUCTORI:  Bază → membri (în ordinea declarării) → corpul constructorului derivat
DESTRUCTORI:   exact INVERS: derivat → membri → bază
```
```cpp
class Baza { public: Baza() { cout << "Baza\n"; } };
class Derivata : public Baza {
    Membru m;
public:
    Derivata() : Baza(), m() { cout << "Derivata\n"; }  // Baza se apelează întâi
};
// Output la construire: Baza, (m), Derivata
```
Constructorul bazei se poate apela explicit în lista de inițializare a derivatei (`: Baza(args)`); altfel se apelează cel default.
⚠️ **Destructorul bazei trebuie `virtual`** dacă ștergi obiecte derivate prin pointer la bază — altfel partea derivată nu se distruge (leak).
</details>

---

## 7. Principiul Open-Closed (O din SOLID) 🟠 (~5 apariții)
> *„Open-Closed. Descriere, exemplu în care se încalcă, soluția. Când e folosit, când e încălcat."*

<details><summary>📖 Răspuns</summary>

**Definiție:** entitățile software (clase, module, funcții) trebuie să fie **deschise pentru extensie, dar închise pentru modificare**. Adaugi comportament nou **fără a modifica** codul existent. Mecanisme: **abstractizare + polimorfism**.

**Exemplu unde se ÎNCALCĂ** (fiecare formă nouă cere modificarea clasei):
```cpp
class GraphicEditor {
    void drawShape(Shape s) {
        if (s.type == CIRCLE) drawCircle(s);
        else if (s.type == RECT) drawRect(s);   // la fiecare formă nouă → modifici aici!
    }
};
```
**Soluția** (extensie prin derivare):
```cpp
struct Shape { virtual void draw() = 0; };            // închis la modificare
struct Circle : Shape { void draw() override { ... } };
struct Rectangle : Shape { void draw() override { ... } };
struct Triangle : Shape { void draw() override { ... } };  // formă nouă — nu modifici nimic
class GraphicEditor { void drawShape(Shape& s) { s.draw(); } };
```
**Când e încălcat:** când adăugarea unui caz nou cere un `switch`/`if` pe tip într-o clasă existentă. **Observație:** OCP adaugă complexitate — niciun design nu poate fi închis la TOATE schimbările.
</details>

---

## 8. Design pattern: Visitor 🟠 (~5 apariții)
> *„Șablon de proiectare Visitor."*

<details><summary>📖 Răspuns</summary>

**Tip:** comportamental (GoF).
**Intent:** reprezintă o **operație de efectuat pe elementele unei structuri de obiecte**; permite definirea unei **operații NOI fără a modifica clasele** elementelor.

**Problema:** ai o structură cu obiecte de tipuri diferite (ex. CustomerGroup, Customer, Order, Item) și vrei să adaugi operații (statistici, export, validare) fără să „umfli" fiecare clasă cu tot mai multe metode.

**Soluția (double dispatch):** fiecare element are metoda `accept(Visitor v)` care apelează `v.visit(this)`. Vizitatorul implementează `visit()` pentru fiecare tip de element.
```cpp
struct Vizitator { virtual void viziteaza(Cerc&) = 0; virtual void viziteaza(Patrat&) = 0; };
struct Forma { virtual void accepta(Vizitator& v) = 0; };
struct Cerc : Forma { void accepta(Vizitator& v) override { v.viziteaza(*this); } };
// Operație nouă = un Vizitator nou (ex. CalculArie, Desenare), FĂRĂ a modifica Cerc/Patrat.
```
**Avantaje:** adaugi operații ușor; păstrează S și O din SOLID.
**Dezavantaj:** dacă adaugi o **clasă nouă** în structură, **toți vizitatorii** trebuie actualizați; vizitatorul nu accesează membrii privați.
</details>

---

## 9. Excepții — mecanismul de administrare în POO 🟠 (~4 apariții)
> *„Excepții. Definiție și mecanismul de management al excepțiilor în POO."*

<details><summary>📖 Răspuns</summary>

**Excepție** = un obiect care semnalează o **situație anormală** (eroare) apărută la execuție, întrerupând fluxul normal.

**Mecanism (C++):**
- `throw obiect;` — aruncă excepția;
- `try { ... }` — bloc monitorizat;
- `catch (Tip& e) { ... }` — prinde excepția; potrivirea se face **pe tip** (fără promovare implicită);
- `catch (...)` — prinde orice, trebuie plasat **ultimul**;
- re-aruncare cu `throw;` (bare).

```cpp
try {
    if (div == 0) throw runtime_error("împărțire la zero");
} catch (const runtime_error& e) {
    cout << e.what();
} catch (...) { /* orice altceva */ }
```
Ierarhia standard: `std::exception` → `logic_error`, `runtime_error`, `bad_alloc`, `out_of_range`... Excepții custom: moștenești din `std::exception` și suprascrii `what()`.
**RAII**: constructor achiziționează resursa, destructorul o eliberează → resursele se curăță automat chiar dacă apare o excepție. Nu arunca niciodată din destructor.

**Legătura cu un design pattern (întrebare frecventă):** tratarea excepțiilor seamănă cu **Chain of Responsibility** — excepția e pasată din `catch` în `catch` (din context în context) până când unul „se potrivește" și o tratează.
</details>

---

## 10. Clase și obiecte, gruparea obiectelor, modificatori de acces 🟠 (~4 apariții)
> *„Clase și obiecte. Ce e un obiect. Gruparea obiectelor în clase. Clase abstracte. Modificatori de acces."*

<details><summary>📖 Răspuns</summary>

**Clasă** = un **șablon** care descrie trăsăturile comune (atribute + metode) ale unui **grup de obiecte**.
**Obiect** = o **instanță** concretă a unei clase (are stare proprie = valori pentru atribute).
> „O clasă reprezintă trăsăturile comune ale unui grup de obiecte."

**Gruparea:** identifici conceptele din problemă → obiectele cu aceleași atribute/comportament se descriu printr-o clasă. Obiectele se creează din clasă (`new`/pe stivă) și se dispun în memorie având fiecare copie proprie a datelor (dar partajează metodele).

**Modificatori de acces (encapsulare):**
| Modificator | Accesibil din |
|-------------|---------------|
| `private` | doar clasa însăși |
| `protected` | clasa + clasele derivate |
| `public` | de oriunde |

De obicei: date `private` (ascunse), acces controlat prin metode `public` (getters/setters) → **încapsulare** (data hiding). O **clasă abstractă** are ≥1 metodă pur virtuală și nu se poate instanția.
</details>

---

## 11. Design pattern: Composite 🟠 (~4 apariții)
> *„Șablon de proiectare Composite."*

<details><summary>📖 Răspuns</summary>

**Tip:** structural (GoF).
**Intent:** compune obiecte în **structuri arborescente parte-întreg** (part-whole) astfel încât clienții tratează **uniform** obiectele individuale (frunze) și compozițiile (noduri).

**Problema:** vrei să tratezi la fel un obiect simplu și un grup de obiecte (ex. un fișier și un folder; un angajat și un manager cu subordonați).

**Soluția:** o interfață comună `Component`; frunza o implementează direct; compozitul ține o listă de `Component` și delegă operațiile către copii.
```cpp
struct Component { virtual void afiseaza() = 0; };
struct Frunza : Component { void afiseaza() override { cout << "frunză"; } };
struct Compozit : Component {
    vector<Component*> copii;
    void adauga(Component* c) { copii.push_back(c); }
    void afiseaza() override { for (auto c : copii) c->afiseaza(); }  // delegă recursiv
};
```
*Ex. din curs: ierarhia de angajați (CFO → head finance → contabili); meniuri.*
**Avantaj:** păstrează O (adaugi tipuri noi ușor), polimorfism pe arbori. **Dezavantaj:** predispus la supra-generalizare.
</details>

---

## 12. Relații de agregare 🟠 (~3 apariții)
> *„Relații de agregare între clase/obiecte. Definiție. Clasificare. Implementare. Exemple."*

<details><summary>📖 Răspuns</summary>

**Agregarea** = **caz particular de asociere**, o relație **parte-întreg** („has-a") în care **partea poate exista independent** de întreg (ciclu de viață independent).

**Față de compoziție:**
- **Agregare** (slabă): partea supraviețuiește distrugerii întregului. *Ex: Echipă — Jucători; Bibliotecă — Cărți.*
- **Compoziție** (puternică): partea **moare** cu întregul. *Ex: Casă — Camere; Om — Inimă.*

**Implementare (C++):** de obicei se specifică doar **multiplicitatea**; agregarea se face prin **pointer/referință** (nu deții obiectul), compoziția prin **obiect membru** (îl deții):
```cpp
class Jucator { /* ... */ };
class Echipa {
    vector<Jucator*> jucatori;   // AGREGARE — jucătorii există independent
};
class Casa {
    Camera camere[4];            // COMPOZIȚIE — camerele se distrug cu casa
};
```
Grafic UML: agregare = **romb gol** (◇) la capătul „întreg"; compoziție = **romb plin** (◆).
</details>

---

## 13. Design pattern: Object Factory / Factory 🟠 (~3 apariții)
> *„Design pattern-ul Object-Factory / Factory."*

<details><summary>📖 Răspuns</summary>

**Tip:** creațional (GoF).
**Intent:** creează obiecte **fără a expune/specifica clasa concretă** instanțiată; centralizezi logica de creare într-un singur loc.

**Problema:** vrei să creezi obiecte de tipuri diferite în funcție de un parametru, fără ca clientul să folosească direct `new ClasaConcreta`.

**Soluția:**
```cpp
struct Animal { virtual void sunet() = 0; };
struct Caine : Animal { void sunet() override { cout << "Ham"; } };
struct Pisica : Animal { void sunet() override { cout << "Miau"; } };

struct AnimalFactory {
    static Animal* creeaza(const string& tip) {
        if (tip == "caine") return new Caine();
        if (tip == "pisica") return new Pisica();
        return nullptr;
    }
};
Animal* a = AnimalFactory::creeaza("caine");   // clientul nu știe de clasele concrete
```
**Avantaj:** detașează clientul de clasele concrete; păstrează S și O din SOLID. **Factory Method** (varianta GoF) lasă **subclasele** să decidă ce clasă instanțiază.
</details>

---

## 14. Reguli de conversie (cast) 🟡 (2 apariții)
> *„Reguli de conversie / casting în C++."*

<details><summary>📖 Răspuns</summary>

C++ are **4 operatori de cast**:

| Cast | Ce face | Verificare |
|------|---------|------------|
| `static_cast<T>` | conversii „normale": numerice, upcast, downcast fără verificare | la compilare |
| `dynamic_cast<T>` | downcast **sigur** în ierarhii polimorfice; dă `nullptr` (pointer) sau aruncă `bad_cast` (referință) la eșec | **la runtime** (RTTI) |
| `const_cast<T>` | adaugă/elimină `const`/`volatile` | — |
| `reinterpret_cast<T>` | reinterpretează biții (pointer↔int, pointer↔pointer neînrudit) — periculos | fără verificare |

```cpp
Baza* b = new Derivata();
Derivata* d = dynamic_cast<Derivata*>(b);   // OK, verificat la runtime
if (d) { /* conversie reușită */ }
```
**Upcast** (derivat→bază) e mereu sigur (implicit sau `static_cast`). **Downcast** (bază→derivat) e sigur doar cu `dynamic_cast`. `const_cast` — doar pentru const/volatile. **Object slicing**: dacă atribui un obiect derivat prin valoare unui obiect bază, se pierde partea derivată.
</details>

---

## 15. Derivare și moștenire (clasificarea derivării) 🟡 (2 apariții)
> *„Derivare și moștenire (diferențe, implementare). Clasificarea relației de derivare, când derivarea definește o moștenire."*

<details><summary>📖 Răspuns</summary>

**Moștenire** = relația conceptuală **is-a** („este un/o") — clasa derivată **este un** tip de clasă de bază. **Derivarea** = mecanismul C++ prin care implementezi moștenirea (`class D : public B`).

**Clasificarea derivării** (după modificatorul de acces) — cum devin membrii bazei în derivată:
| Derivare | public din bază → | protected din bază → | private din bază → |
|----------|-------------------|----------------------|---------------------|
| `public` | public | protected | inaccesibil |
| `protected` | protected | protected | inaccesibil |
| `private` | private | private | inaccesibil |

**Derivarea definește o MOȘTENIRE (is-a)** doar când e **`public`** — atunci un `D*` poate fi tratat ca `B*` (substituibilitate). Derivarea `private`/`protected` exprimă mai degrabă „implemented-in-terms-of" (compoziție), NU is-a.

```cpp
class Vehicul { /* ... */ };
class Masina : public Vehicul { /* Masina IS-A Vehicul */ };
```
Moștenirea multiplă poate crea **problema diamantului** → rezolvată cu **moștenire virtuală** (`class B : virtual public A`).
</details>

---

## 16. Principiul lui Liskov (L din SOLID) 🟡 (2 apariții)
> *„Principiul lui Liskov. Definiție, exemplu."*

<details><summary>📖 Răspuns</summary>

**Definiție:** „Subtipurile trebuie să fie **substituibile** pentru tipurile lor de bază." — oriunde folosești un obiect de tip bază, trebuie să poți pune un obiect derivat **fără a strica** comportamentul așteptat. Moștenirea normală e „is-a"; LSP cere „is-**substitutable**-for".

Subclasa **nu trebuie** să: elimine comportamentul bazei, întărească precondițiile, slăbească postcondițiile, violeze invarianții.

**Exemplu clasic de ÎNCĂLCARE (Square/Rectangle):**
```cpp
class Rectangle { public: virtual void setW(int w); virtual void setH(int h); int arie(); };
class Square : public Rectangle {
    void setW(int w) override { width = height = w; }   // schimbă AMBELE
    void setH(int h) override { width = height = h; }
};
void test(Rectangle& r) { r.setW(5); r.setH(4); assert(r.arie() == 20); }
// Pentru un Square, aria = 16, nu 20 → Square NU e substituibil pentru Rectangle → LSP încălcat.
```
Soluție: nu forța o relație de moștenire care nu respectă substituibilitatea (Square nu ar trebui să moștenească Rectangle).
</details>

---

## 17. Subiecte apărute o dată (2019) ⚪
> Din 2019 — mai puțin probabile, dar utile.

<details><summary>📖 Funcții pur virtuale în C++</summary>

`virtual void f() = 0;` — metodă **fără implementare** în clasa de bază, care **trebuie** implementată în derivată. O clasă cu ≥1 metodă pur virtuală devine **abstractă** (nu se poate instanția). Servește la definirea unui **contract/interfață**. Poate coexista cu metode concrete.
</details>

<details><summary>📖 Funcții friend</summary>

O funcție/clasă `friend` poate accesa membrii **private și protected** ai unei clase, deși **nu e membru** al ei.
```cpp
class Vector2D { int x, y;
    friend Vector2D operator+(const Vector2D&, const Vector2D&);  // acces la x,y private
};
```
Proprietăți: prietenia **nu se moștenește**, **nu e reciprocă**, nu e tranzitivă. Sparge encapsularea controlat — folosită des la supraîncărcarea operatorilor.
</details>

<details><summary>📖 Supraîncărcarea metodelor (overloading)</summary>

Mai multe metode cu **același nume** dar **semnături diferite** (nr./tip parametri, `const`). NU se poate supraîncărca după tipul de return. Rezolvat la **compilare** (polimorfism static). Rezoluția: potrivire exactă → promovare → conversie standard → conversie user → variadic.
```cpp
void print(int);  void print(double);  void print(const char*);
```
</details>

<details><summary>📖 Containere STL</summary>

Structuri generice (template) din biblioteca standard:
- **Secvențiale:** `vector` (tablou dinamic), `deque`, `list` (dublu înlănțuită), `array`, `forward_list`.
- **Adaptoare:** `stack` (LIFO), `queue` (FIFO), `priority_queue` (heap).
- **Asociative** (ordonate, arbore roșu-negru): `map`, `multimap`, `set`, `multiset`.
- **Neordonate** (hash): `unordered_map`, `unordered_set`.
Se parcurg cu **iteratori**; algoritmi generici (`sort`, `find`, `for_each`).
</details>

<details><summary>📖 Pointeri și referințe (+ diferența)</summary>

**Pointer** = variabilă care stochează o **adresă**; poate fi `nullptr`, poate fi **reasignat**, aritmetică de pointeri, dereferențiere cu `*`.
**Referință** = **alias** pentru o variabilă existentă; trebuie **inițializată** la declarare, **nu poate fi reasignată**, nu poate fi null.
```cpp
int x = 5;
int* p = &x;   // pointer
int& r = x;    // referință (alias pentru x)
```
Referințele sunt mai sigure (fără null, fără aritmetică) → folosite la parametri; pointerii — pentru structuri dinamice și opționalitate (null).
</details>

<details><summary>📖 Operatori C++ (supraîncărcare)</summary>

Poți **supraîncărca operatori** pentru tipuri proprii: `+ - * / == != < [] () << >> = ++`, etc.
```cpp
struct Complex { double re, im;
    Complex operator+(const Complex& c) const { return {re+c.re, im+c.im}; }
    bool operator==(const Complex& c) const { return re==c.re && im==c.im; }
};
```
`operator<<` se supraîncarcă de obicei ca funcție `friend`. Nu se pot supraîncărca `::`, `.`, `.*`, `?:`, `sizeof`.
</details>

---

> ⭐ **Strategie de învățare rapidă:** primele 6 teme (polimorfism, abstracte/interfețe, template-uri, asociere, DIP, constructori/destructori) acoperă majoritatea examenelor. Dacă înveți temeinic primele 10, ai șanse foarte mari la ambele întrebări.
