# SINTEZĂ COMPLETĂ — INGINERIA PROGRAMĂRII (IP)
> Acoperă toate cele 12 cursuri. Organizat după frecvența la examen (⭐).
> Lângă fiecare capitol, întrebările din anul trecut.

> **Notă:** Cursurile sunt slide-uri cu imagini. Tot ce e mai jos provine din cursuri.
> Două teme au slide-uri bazate pe imagini, deci în txt lipsesc detalii: **Scrum** (rolurile/artefactele/
> evenimentele — IP05 le listează ca temă dar nu le dezvoltă în text) și **formula de code coverage**
> (IP06). Acolo am completat cu cunoștințe standard, **marcate ca atare**.

---

## CUPRINS (după prioritatea la examen)
> **Prioritate actualizată cu întrebări din 2021, 2022, 2023, 2024.** Aproape toate temele apar în fiecare an.

| # | Temă | Recurență 2021–2024 | Prioritate |
|---|------|---------------------|------------|
| 1 | [Design Patterns GoF (definiție, elemente, tipuri + toate pattern-urile)](#1-design-patterns-gof-) | în fiecare an, multiple (Adapter, Observer, Proxy, Decorator, Mediator, Prototype, Chain of Resp., Composite) | ⭐⭐⭐⭐⭐ |
| 2 | [Principiile SOLID (S, O, L, I, D)](#2-principiile-solid-) | **în fiecare an, fiecare principiu cerut** (D, L, O, S, I) | ⭐⭐⭐⭐⭐ |
| 3 | [Quality Assurance & Testare](#3-quality-assurance--testare-) | în fiecare an (manual vs auto, code coverage, nefuncțională, unit) | ⭐⭐⭐⭐ |
| 4 | [Reverse Engineering](#4-reverse-engineering-) | în fiecare an | ⭐⭐⭐⭐ |
| 5 | [Ce e IP, importanță, etape, modele de dezvoltare](#5-ce-este-ip-) | **în fiecare an (×3-4)** | ⭐⭐⭐⭐ |
| 6 | [Metodologii Agile (Scrum, Kanban, XP)](#6-metodologii-agile-) | **Scrum în fiecare an** (roluri/artefacte/evenimente) | ⭐⭐⭐⭐ |
| 7 | [Modelare & UML (Use Case, Diagrama de clase)](#7-modelare--uml-) | recurent (use case, diagrama de clase, modelare) | ⭐⭐⭐ |
| A,E | [Anexe: **GRASP** (coeziune/cuplaj), **Pachete + principii** (stabilitate), DRY/YAGNI/KISS, Refactoring](#anexe) | **GRASP și Pachete în fiecare an** | ⭐⭐⭐ |

> **Top recurente IP (toți anii 2021–2024):**
> - **SOLID** — în fiecare an se cere "care sunt + detaliază X"; cele mai cerute: **D (DIP)**, **L (Liskov)**, **O (Open-Closed)**, **S (SRP)**, **I (ISP)** — știi-le pe toate cu exemplu.
> - **Design Patterns GoF** — definiție + elemente + tipuri ȘI un pattern de detaliat; cele mai cerute: Adapter, Decorator, Proxy, Mediator, Observer, Prototype, Chain of Responsibility, Composite.
> - **Scrum** — în fiecare an: **roluri, artefacte, evenimente** (vezi cap. 6 — detaliate).
> - **Ce e IP / etape / cel mai important pas (= analiza cerințelor)** + **importanță/statistici/exemple** + **când se folosește IP (și când NU)** — în fiecare an.
> - **Reverse Engineering** (definiție + tipuri: cu/fără cod sursă) — în fiecare an.
> - **QA** (manual vs automat, code coverage, testare nefuncțională, unit testing) — în fiecare an.
> - **GRASP** (coeziune și cuplaj) și **Pachete** (principii OOP, principiul stabilității) — în fiecare an (Anexele A și E).
> - **Modele de dezvoltare** (Cascadă, XP, Spirală), **Agile/Kanban**, **Use Case**, **Diagrame de clase**, **Modelare** — recurente.

> **Notă cross-materie:** *"Arhitectura pe N straturi (N-tier/layered), beneficii"* a apărut la IP (2023) — e tratată complet în **sinteza Web, cap. 1** (avantaje/dezavantaje straturi).

---

## 1. Design Patterns GoF ⭐⭐⭐⭐⭐
> **13 din 40 de întrebări!** De detaliat: Flyweight, Mediator, Decorator, Adapter, Visitor, Prototype, Chain of Responsibility, Observer + definiție/elemente/tipuri.

### Ce este un design pattern (definiție)

> Un **design pattern** este o **soluție generală, reutilizabilă, la o problemă care apare frecvent** în proiectarea software. "Dacă o problemă apare iar și iar, o soluție la acea problemă a fost folosită eficient (soluție = pattern)."

- Captează soluții dezvoltate și rafinate în timp (GoF).
- Sunt **strategii independente de limbaj** pentru probleme comune de proiectare OO.
- Învățarea lor = un **vocabular comun** → comunicăm eficient și proiectăm la nivel mai înalt de abstractizare.

**GoF (Gang of Four)** = Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides — autorii cărții *"Design Patterns: Elements of Reusable Object-Oriented Software"*.

### Elementele unui design pattern (întrebare Q20!)

Forma esențială are **4 elemente**:
```
1. Pattern name (numele)  — un "mâner" pentru a descrie problema/soluția în 1-2 cuvinte;
                            crește vocabularul de design
2. Problem (problema)     — când se aplică pattern-ul, contextul
3. Solution (soluția)     — elementele design-ului, relațiile, responsabilitățile
                            (e un șablon abstract, nu o implementare concretă)
4. Consequences (consecințe) — rezultate și compromisuri (trade-offs): spațiu/timp,
                            flexibilitate, extensibilitate, portabilitate
```

**Forma extinsă GoF** (mai multe câmpuri): Pattern Name and Classification, Intent, Also Known As, Motivation, Applicability, Structure, Participants, Collaborations, Consequences, Implementation, Sample Code, Known Uses, Related Patterns.

### Tipuri (clasificare) de design patterns (întrebare!)

```
┌──────────────────────────────────────────────────────────┐
│ CREAȚIONALE (Creational) — despre CREAREA obiectelor      │
│   Abstract Factory, Builder, Factory Method, Prototype,   │
│   Singleton                                                │
├──────────────────────────────────────────────────────────┤
│ STRUCTURALE (Structural) — despre COMPUNEREA claselor/obj │
│   Adapter, Bridge, Composite, Decorator, Facade,          │
│   Flyweight, Proxy                                         │
├──────────────────────────────────────────────────────────┤
│ COMPORTAMENTALE (Behavioral) — despre ALGORITMI și        │
│   atribuirea responsabilităților între obiecte            │
│   Chain of Responsibility, Command, Interpreter, Iterator,│
│   Mediator, Memento, Observer, State, Strategy,           │
│   Template Method, Visitor                                 │
└──────────────────────────────────────────────────────────┘
```

---

### 1.A. Pattern-uri CREAȚIONALE

Abstractizează procesul de instanțiere; fac sistemul independent de cum sunt create/compuse/reprezentate obiectele. Încapsulează ce clase concrete folosește sistemul și ascund cum sunt create instanțele.

**Abstract Factory** (AKA Kit) — *exemplu curs: componente de computer, maze.*
- **Intent:** oferă o interfață pentru crearea unor **familii** de obiecte înrudite/dependente, fără a specifica clasele concrete.
- **Consecințe:** detașează creatorii de obiectele create; garantează compatibilitatea obiectelor create; **păstrează S și O** (SOLID); codul devine complicat (multe clase extra).

**Builder** — *exemplu curs: RTF Reader, Happy Meal.*
- **Intent:** separă **construcția** unui obiect complex de **reprezentarea** lui, astfel încât același proces de construcție poate crea reprezentări diferite.
- **Aplicabilitate:** când parametrii de construcție sunt numeroși dar doar unii necesari (tipuri de pizza); construire pas cu pas, pași care pot fi săriți/rulați recursiv.
- **Consecințe:** păstrează **S**; cod complicat (multe clase noi).

**Factory Method** (AKA Virtual Constructor) — *exemplu curs: Open/New Project, Hello Mr/Ms.*
- **Intent:** definește o interfață pentru crearea unui obiect, dar lasă **subclasele să decidă** ce clasă instanțiază.
- **Consecințe:** detașează creatorii de obiecte; păstrează **S și O**; cod complicat (clase derivate extra).

**Prototype** (întrebare Q17!) — *exemplu curs: editor muzical, diviziune celulară, Cloneable.*
- **Intent:** specifică tipurile de obiecte de creat folosind o **instanță prototip** și creează obiecte noi **prin clonarea** acestui prototip.
- **Motivație:** într-un editor de partituri, în loc de o ierarhie mare de clase pentru note/pauze, clonezi un prototip → reduci numărul de clase.
- **Aplicabilitate:** când clasele de instanțiat sunt specificate la runtime (dynamic loading); pentru a evita o ierarhie de factory-uri paralelă cu cea de produse; când instanțele au puține combinații de stare.
- **Consecințe:** copiezi obiecte fără a folosi clasa concretă; sari peste inițializări repetate; creezi rapid obiecte complexe; ✗ greu de clonat clase complexe cu multe referințe.

**Singleton** — *exemplu curs: Logger, fișiere log server.*
- **Intent:** garantează că o clasă are **o singură instanță** și oferă un punct global de acces la ea.
- **Consecințe:** garantează o singură instanță + acces global; ✗ **încalcă S** (are DOUĂ responsabilități); poate masca cuplaj ridicat; greu de folosit în sisteme multi-thread.

> **Nu sunt în GoF (creaționale):** Lazy initialization (amână crearea până la prima folosire, cu un flag), Object pool (reutilizează obiecte costisitoare de creat — ex. conexiuni la BD), Multiton, Resource acquisition.

---

### 1.B. Pattern-uri STRUCTURALE

Se ocupă de cum sunt **compuse** clasele și obiectele în structuri mai mari. Compoziția de obiecte permite schimbarea compoziției la runtime.

**Adapter** (AKA Wrapper) — *exemplu curs: priză-ștecher (socket 15A / plug 5A).* **(întrebare Q11, Q38!)**
- **Intent:** **convertește interfața** unei clase în altă interfață așteptată de client. Permite claselor cu interfețe **incompatibile** să lucreze împreună.
- **Aplicabilitate:** vrei să folosești o clasă existentă a cărei interfață nu se potrivește cu ce ai nevoie; vrei o clasă reutilizabilă care cooperează cu clase neînrudite.
```
Client așteaptă interfața Socket (getOutput).
Avem Plug (getInput, "5 AMP") cu interfață incompatibilă.
ConnectorAdapter implements Socket → în getOutput() apelează plug.getInput()
   → "traduce" interfața.
```
- **Consecințe:** păstrează **S și O**; cod complicat (clase extra).

**Bridge** (AKA Handle/Body) — *exemplu curs: drawing API.*
- **Intent:** **decuplează o abstractizare de implementarea ei** astfel încât cele două pot varia independent.
- **Exemplu:** `Shape` (abstracție) cu `DrawingAPI` (implementor) separat → forme desenate pe diferite OS/API-uri.
- **Consecințe:** păstrează S și O; sisteme independente de platformă; ✗ cod complicat, separă clase coezive.

**Composite** — *exemplu curs: ierarhie de angajați (CFO → head finance → contabili), meniuri.*
- **Intent:** compune obiecte în **structuri arborescente** (part-whole) astfel încât clienții tratează **uniform** obiectele individuale și compozițiile.
- **Aplicabilitate:** ierarhii parte-întreg; clienții să poată ignora diferența între obiecte simple și compuse.
- **Consecințe:** păstrează **O** (clase noi ușor de adăugat); polimorfism + moștenire pentru arbori complecși; ✗ predispus la supra-generalizare.

**Decorator** (AKA Wrapper) — *exemplu curs: brad de Crăciun (BallDecorator, StarDecorator).* **(întrebare Q9!)**
- **Intent:** **atașează responsabilități suplimentare unui obiect dinamic**. Alternativă flexibilă la subclasare pentru extinderea funcționalității.
- **Motivație:** vrei să adaugi responsabilități **obiectelor individuale**, nu întregii clase (ex. borduri/scrolling la o componentă GUI). Înconjori componenta în alt obiect (decoratorul) care adaugă comportamentul.
```
Component → Decorator înfășoară componenta și adaugă comportament,
apoi delegă mai departe. Poți pune mai multe decoratoare (stratificat).
```
- **Aplicabilitate:** adaugi responsabilități **dinamic și transparent**; responsabilități ce pot fi **retrase**; când subclasarea e impracticabilă.
- **Consecințe:** schimbi comportamentul fără moștenire; păstrează **S** (o clasă mare se sparge în altele mici); adaugi multiple comportamente; schimbi responsabilități la runtime; ✗ **ordinea aplicării decoratorilor contează**; greu de scos un decorator din mijloc; cod greu de gestionat.

**Facade** — *exemplu curs: store keeper (magazioner), JDBC.*
- **Intent:** oferă o **interfață unificată/simplificată** la un set de interfețe dintr-un subsistem.
- **Motivație:** minimizează comunicarea și dependențele între subsisteme; clientul vede o "față" prietenoasă care ascunde complexitatea.
- **Consecințe:** izolează și maschează complexitatea sistemului; ✗ clasa facade riscă să fie cuplată cu tot.

**Flyweight** — *exemplu curs: FontData (caractere într-un editor de text).* **(întrebare Q2, Q35!)**
- **Intent:** folosește **partajarea (sharing)** pentru a suporta eficient un **număr mare de obiecte fine-grained**.
- **Motivație:** un editor de documente ar avea mii de obiecte-caracter, fiecare cu font/dimensiune/culoare → prohibitiv de costisitor. În loc, partajezi datele comune.
- **Aplicabilitate:** un număr mare de obiecte care: sunt similare, partajează cel puțin câteva atribute, sunt prea numeroase pentru a fi stocate întregi în memorie.
```
Un Flyweight = obiect care minimizează memoria PARTAJÂND cât mai multe date
cu alte obiecte similare. Ex: FontData (pointSize, fontFace, color, effects)
e partajat între toate caracterele identice (pool de flyweight-uri).
Starea INTRINSECĂ (partajată) e în flyweight; starea EXTRINSECĂ (variabilă)
e ținută de client și pasată la nevoie.
```
- **Consecințe:** economisește **memorie** când sunt multe obiecte; ✗ costisitor ca timp de procesare; cod complicat și neintuitiv.

**Proxy** (AKA Surrogate) — *exemplu curs: acces la ATM (proxy spre Bank).*
- **Intent:** oferă un **substitut/placeholder** pentru alt obiect, pentru a **controla accesul** la el.
- **Motivație:** obiecte grele (ex. imagini raster mari) — un image proxy ține locul imaginii reale și o instanțiază doar când e nevoie.
- **Aplicabilitate:** serviciu interpus între logica aplicației și client; versiune lightweight a unui serviciu; restricționarea accesului.
- **Consecințe:** serviciul poate fi schimbat fără a afecta clientul; proxy disponibil chiar dacă serviciul de bază nu e; păstrează **O**; ✗ întârzie răspunsul; cod complicat (mai multe clase).

---

### 1.C. Pattern-uri COMPORTAMENTALE

Se ocupă de **algoritmi** și **atribuirea responsabilităților** între obiecte. Caracterizează fluxuri de control complexe; tema: **încapsularea variației**.

**Chain of Responsibility** — *exemplu curs: sistem de help context-sensitive, filtru de pietriș multi-nivel.* **(întrebare Q23!)**
- **Intent:** **înlănțuie** obiectele receptoare și **pasează cererea** de-a lungul lanțului până când un obiect o tratează.
- **Aplicabilitate:** mai multe obiecte pot trata o cerere și handler-ul nu e cunoscut dinainte; vrei să emiți o cerere către unul din mai multe obiecte fără a specifica explicit receptorul; setul de obiecte se specifică dinamic.
```
Cerere → Handler1 → (nu pot) → Handler2 → (nu pot) → Handler3 → tratează
Fiecare handler ori tratează cererea, ori o pasează mai departe.
```
- **Consecințe:** controlezi secvența apelurilor de handlere; păstrează **S și O**; ✗ unele cereri pot rămâne netratate.

**Command** (AKA Action, Transaction) — *exemplu curs: comandă la restaurant (client→chelner→bucătar).*
- **Intent:** **încapsulează o cerere ca obiect** → poți parametriza clienți cu cereri diferite, pune cereri în coadă/log, suporta operații **undo**.
- **Consecințe:** suportă undo/redo; păstrează **S și O**; combini comenzi simple într-una complexă; permite întârzierea execuției; ✗ cod complicat (strat extra între caller și serviciu).

**Interpreter** — *exemplu curs: note muzicale (Sa/Re/Ga = frecvențe), expresii regulate.*
- **Intent:** dată o limbă, definește o **reprezentare a gramaticii** + un **interpretor** care procesează propozițiile.
- **Consecințe:** detașează clasele utilizator de clasele model; păstrează S și O; ✗ multe clase extra.

**Iterator** (AKA Cursor) — *exemplu curs: telecomandă TV (parcurgere canale).*
- **Intent:** oferă o cale de a **accesa secvențial** elementele unui agregat **fără a expune** structura internă.
- **Consecințe:** mai mulți iteratori simultan pe aceeași colecție; iterația poate fi oprită/reluată (fiecare iterator își ține starea); păstrează S și O; ✗ complexitate inutilă pentru colecții simple.

**Mediator** — *exemplu curs: turn de control / avioane, chat, bursă (stock exchange).* **(întrebare Q8, Q18, Q28 — de 3 ori!)**
- **Intent:** definește un obiect care **încapsulează modul în care un set de obiecte interacționează**.
- **Motivație:** designul OO distribuie comportamentul între obiecte → multe conexiuni; în cel mai rău caz, fiecare obiect cunoaște pe fiecare. Mediatorul centralizează aceste interacțiuni.
```
FĂRĂ Mediator: obiectele se cunosc reciproc (n×n legături, "spaghetti")
   A ── B
   │ ╳ │       fiecare vorbește cu fiecare
   C ── D

CU Mediator: obiectele vorbesc DOAR cu mediatorul
   A   B
    ╲ ╱
   MEDIATOR     mediatorul știe despre toate, ele nu se cunosc între ele
    ╱ ╲
   C   D
```
- **Aplicabilitate (Gamma):** un set de obiecte comunică în moduri complexe, cu interdependențe neструктurate, greu de înțeles; reutilizarea unui obiect e dificilă pentru că comunică cu multe altele; un comportament distribuit între clase trebuie personalizabil fără multă subclasare.
- **Exemple curs:** avioanele interacționează cu **turnul de control**, nu între ele; bursa; aplicația de chat.
- **Consecințe:** **reduce cuplajul**; permite reutilizarea ușoară a claselor; păstrează **S și O**; ✗ Mediatorul poate deveni un **God Object** (știe prea mult, face prea multe).

**Memento** (AKA Token) — *exemplu curs: calculator cu undo, tranzacții БД (rollback).*
- **Intent:** **fără a viola încapsularea**, capturează și externalizează starea internă a unui obiect, astfel încât obiectul poate fi **restaurat** ulterior la acea stare.
- **Participanți:** **Memento** (stochează starea), **Originator** (creează/folosește memento), **Caretaker** (păstrează memento, dar nu operează pe el).
- **Consecințe:** salvează stări fără a rupe încapsularea; ✗ multe memento-uri consumă memorie.

**Observer** (AKA Dependents, Publish-Subscribe) — *exemplu curs: input de la tastatură ca eveniment, Java Swing.* **(întrebare Q25!)**
- **Intent:** definește o dependență **one-to-many** între obiecte, astfel încât când un obiect își schimbă starea, **toți dependenții sunt notificați și actualizați automat**.
- **Aplicabilitate:** când o abstractizare are două aspecte, unul dependent de celălalt; când o schimbare la un obiect cere modificarea altora și nu știi câte; când un obiect trebuie să notifice altele fără a presupune cine sunt.
```
       Subject (Observable)
         │  setChanged(); notifyObservers()
         ▼
   ┌─────┼─────┐
   ▼     ▼     ▼
 Obs1  Obs2  Obs3     fiecare implementează update()
 → la o schimbare în Subject, toți observatorii primesc update() automat
```
- **Consecințe:** stabilește relații între **obiecte** (nu clase) la runtime; păstrează **S și O**; ✗ ordinea notificării observatorilor e aleatoare.

**State** (AKA Objects for States) — *exemplu curs: TCPConnection (Established/Listening/Closed).*
- **Intent:** permite unui obiect să-și **altereze comportamentul când starea internă se schimbă** (obiectul pare să-și schimbe clasa).
- **Consecințe:** elimină multe instrucțiuni `if`; păstrează S și O; ✗ overhead pentru obiecte cu puține stări.

**Strategy** (AKA Policy) — *exemplu curs: calculator cu operații (+,-,*), algoritmi de împărțire text în linii.*
- **Intent:** definește o **familie de algoritmi**, încapsulează fiecare și îi face **interschimbabili** (selectabili la runtime).
```
Context are o referință Strategy; algoritmul concret (Add/Sub/Mul)
e injectat și apelat prin context.execute() → schimbabil la runtime.
```
- **Consecințe:** schimbi algoritmi la runtime; **înlocuiește moștenirea cu compoziție**; izolează implementarea algoritmului; ✗ pentru puțini algoritmi, complicație inutilă.

**Template Method** — *exemplu curs: Game (Monopoly, Chess), Hollywood Principle.*
- **Intent:** definește **scheletul unui algoritm** într-o operație, **amânând unii pași către subclase**.
- **Hollywood Principle:** "Don't call us, we'll call you" — metoda template din clasa părinte controlează procesul, apelând metodele subclaselor la nevoie.
```
abstract class Game {
   final void playOneGame() {     // SCHELETUL (fix)
     initializeGame();            // pași abstracți, definiți de subclase
     while(!endOfGame()) makePlay(j);
     printWinner();
   }
}
Monopoly/Chess extind Game și implementează pașii.
```
- **Consecințe:** clienții suprascriu doar unele părți; cod duplicat mutat în superclasă; ✗ suprascrierea poate rupe Liskov.

**Visitor** — *exemplu curs: modul de raportare/statistici despre un grup de clienți.* **(întrebare Q14!)**
- **Intent:** reprezintă o **operație de efectuat pe elementele unei structuri de obiecte**. Permite definirea unei **operații noi fără a schimba clasele** elementelor pe care operează.
- **Motivație:** colecțiile conțin obiecte de tipuri diferite; vrei operații pe toate elementele fără a cunoaște tipul. Toate entitățile (CustomerGroup, Customer, Order, Item) **acceptă** un vizitator (sunt "visitable").
```
Element.accept(Visitor v) → v.visit(this)   // double dispatch
Vizitatorul implementează visit() pentru fiecare tip de element.
Adaugi o operație nouă = scrii un Visitor nou, FĂRĂ a modifica elementele.
```
- **Aplicabilitate:** operații similare pe obiecte de tipuri diferite dintr-o structură; multe operații distincte; structura de obiecte e stabilă dar se adaugă des operații noi.
- **Consecințe:** vizitatorul acumulează informație pe măsură ce parcurge; păstrează **S și O**; ✗ nu poate accesa câmpuri private/protected; **la adăugarea unei clase noi în colecție, toți vizitatorii trebuie actualizați**.

> **Nu sunt în GoF:** Concurrency patterns (Single Threaded Execution, Scheduler, Producer-Consumer), Testing patterns, Distributed Architecture, Database, Temporal patterns.

---

## 2. Principiile SOLID ⭐⭐⭐⭐
> Întrebări: S (Q1,16), O (Q15), L (Q31), I (Q19,32), D (Q10). **5 principii** introduse de **Robert C. Martin** (~2000).

```
S — Single Responsibility Principle (SRP)
O — Open/Closed Principle (OCP)
L — Liskov Substitution Principle (LSP)
I — Interface Segregation Principle (ISP)
D — Dependency Inversion Principle (DIP)
```

### S — Single Responsibility Principle (Q1, Q16)
> "O clasă trebuie să aibă **o singură responsabilitate**, și aceasta să fie complet încapsulată de clasă." / "Nu ar trebui să existe niciodată mai mult de **un motiv** pentru ca o clasă să se schimbe." (R. Martin)

- O responsabilitate = **un motiv de schimbare** = o axă de schimbare.
- Legat de **low coupling & strong cohesion**.
- **Violări clasice:** obiecte care se pot și desena/printa singure, ȘI salva/restaura singure.
- **Soluție:** separi responsabilitățile (ex. interfața `Modem` cu dial/hangup ȘI send/recv → o separi în `Connection` + `DataChannel`); interfețe mici, clase mici, responsabilități distincte.
- **Corolar:** o axă de schimbare e axă doar dacă schimbările chiar au loc (nu separa dacă implementările se schimbă mereu împreună).

### O — Open/Closed Principle (Q15)
> "Entitățile software (clase, module, funcții) trebuie să fie **deschise pentru extensie, dar închise pentru modificare**." Termenul: Bertrand Meyer (1988).

- Mecanismele principale: **abstractizare și polimorfism**. Te bazezi pe abstracții, nu pe implementări.
- Abordări: parametri (delegates/callbacks), moștenire/Template Method, compoziție/Strategy.

**Exemplu care ÎNCALCĂ OCP (din curs):**
```java
// RĂU: adăugarea unei forme noi cere modificarea clasei
class GraphicEditor {
   void drawShape(Shape s) {
      if (s.m_type==1) drawRectangle(s);
      else if (s.m_type==2) drawCircle(s);   // trebuie modificat la fiecare formă nouă!
   }
}
class Shape { int m_type; }
```
**Soluția corectă (extensie prin derivare):**
```java
// BINE: forme noi NU modifică nimic existent
class GraphicEditor {
   void drawShape(Shape s) { s.draw(); }   // închis la modificare
}
abstract class Shape { abstract void draw(); }   // deschis la extensie
class Rectangle extends Shape { void draw() { /*...*/ } }
class Circle extends Shape { void draw() { /*...*/ } }   // adaugi forme noi liber
```
- **Când aplicăm OCP?** experiența îți spune; OCP adaugă complexitate (TANSTAAFL); niciun design nu poate fi închis la TOATE schimbările.

### L — Liskov Substitution Principle (Q31)
> "**Subtipurile trebuie să fie substituibile pentru tipurile lor de bază.**" (Barbara Liskov, 1988)

- Moștenirea OO normală = relație **IS-A**; LSP = **IS-SUBSTITUTABLE-FOR**.
- Subclasele **NU trebuie** să: elimine comportamentul clasei de bază, violeze invarianții bazei.
- O funcție care nu respectă LSP folosește o referință la baza, dar trebuie să cunoască toate derivatele → încalcă și OCP.

**Exemplu clasic Rectangle/Square (din curs):**
```java
class Rectangle { setWidth(w); setHeight(h); getArea(); }
class Square extends Rectangle {
   setWidth(w)  { m_width = m_height = w; }   // schimbă AMBELE
   setHeight(h) { m_width = m_height = h; }
}
// Cod client: Rectangle r = new Square(); r.setWidth(5); r.setHeight(10);
// Așteaptă area = 50, dar primește 100 → Square NU e substituibil pentru Rectangle!
```

### I — Interface Segregation Principle (Q19, Q32)
> "**Clienții nu trebuie forțați să depindă de metode pe care nu le folosesc.**"

- Preferă interfețe **mici, coezive, focalizate**. Sparge interfețele "grase" (fat interfaces) în altele mai mici.
- Interfețe "grase" = clase cu metode inutile, cuplaj crescut, flexibilitate/mentenabilitate reduse.

**Exemplu (din curs):**
```java
// RĂU: interfață poluată
interface Worker { void work(); void eat(); }
class RobotWorker implements Worker {
   void work() {...}
   void eat() {} // N/A pentru un robot!
}
// BINE: împărțită în două interfețe
interface Workable { void work(); }
interface Feedable { void eat(); }
```
- **Când repari?** doar când "doare" (don't fix if it's not broken); dacă interfața grasă e a ta, separ-o; dacă nu e a ta, folosește **Adapter**.

### D — Dependency Inversion Principle (Q10)
> "Modulele de nivel **înalt** nu trebuie să depindă de cele de nivel **jos**. Ambele trebuie să depindă de **abstracții**." / "Abstracțiile nu trebuie să depindă de detalii. Detaliile trebuie să depindă de abstracții."

- **Cum?** **Dependency Injection**; principiul Hollywood "Don't call us, we'll call you"; clasele declară ce au nevoie (în constructor), dependențele sunt abstracții.
- **Violări clasice:** folosirea `new`, metode/proprietăți statice.

**Exemplu (din curs):**
```java
// RĂU: depinde de o clasă concretă
class EmployeeService { private EmployeeFinder emFinder; }   // ex. acces direct la SQL DB
// BINE: depinde de o abstracție
class EmployeeService { private IEmployeeFinder emFinder; }
// Acum poți schimba: XmlEmployeeFinder, DBEmployeeFinder, MockEmployeeFinder...
```

> **Semnele unui design rău (din curs):** **Rigiditate** (greu de schimbat), **Fragilitate** (se rupe în locuri neașteptate), **Imobilitate** (greu de reutilizat), **Viscozitate**.
> **Alte principii:** **DRY** (Don't Repeat Yourself), **YAGNI** (You Aren't Gonna Need It), **KISS** (Keep It Simple, Stupid) — vezi Anexa.

---

## 3. Quality Assurance & Testare ⭐⭐⭐
> Întrebări: code coverage (Q3), manual vs automat (Q5,37), unit testing (Q21), testare nefuncțională (Q24,29), definiție QA (Q37).

### Ce este Quality Assurance (QA)

> Procese de producție **planificate și sistematice** care oferă încredere în adecvarea unui produs la scopul propus. Set de activități menite să asigure că produsele satisfac cerințele clientului.

- **Două principii cheie:** **"fit for purpose"** (potrivit scopului) și **"right first time"** (corect din prima).
- QA nu garantează absolut calitatea, dar o face mai probabilă.

**QA vs Quality Control:**
```
QA (Quality Assurance) = PREVENȚIE
   → previne defecte, se ocupă de PROCESE, îmbunătățire continuă a proceselor
Quality Control = CORECȚIE
   → corectează; TESTAREA software e o SUBMULȚIME a Quality Control
```
- Standarde: **ISO 9000/9001**, CMMI.

### Testarea software

> "Procesul de a exercita sau evalua un sistem, manual sau automat, pentru a verifica dacă satisface cerințele specificate sau pentru a identifica diferențe între rezultatele așteptate și cele actuale." (IEEE)

- **Testarea NU e o fază** — e integrată în toate fazele dezvoltării.
- **Testare vs Debugging:** testarea verifică conformitatea cu cerințele (parte externă, planificată, controlată); debugging verifică validitatea secțiunilor de cod (de către dezvoltator, proces aleator).
- **Când ne oprim?** când nr. de erori găsite e sub un prag / nu se mai găsesc defecte critice / s-a terminat timpul.
- "Testarea profesională = a găsi cel mai mic număr de cazuri de test care verifică cele mai multe funcționalități."

### Niveluri (layers) de testare
```
Unit (unitate) → Module/Sub-sistem → Integration → System → Acceptance
```

**Unit testing (Q21):**
- Testarea unei funcții, ecran, funcționalități. Rulat de **programatori**. Predefinit. Rezultate documentate. Se folosesc **simulatoare** de Input/Output.
- *Exemplu (standard):* test JUnit care verifică o metodă — ex. `add(2,3)` returnează `5`; pentru un calculator: apeși 2, +, 3, = și verifici cu `assertEquals("5", rezultat)`.

**Integration:** testarea mai multor module împreună (coexistență). **System:** sistem complet integrat, intră în **black box testing**. **Acceptance:** testare black-box înainte de livrare; cea făcută de client = **UAT (User Acceptance Testing)**. **Regression:** caută **regresii** (funcționalitate care mergea și s-a stricat), prin re-rularea testelor anterioare.

### Metode de testare (după acces la cod)
```
WHITE BOX  — testerul are acces la structurile interne și algoritmi
             tipuri: API testing, CODE COVERAGE, fault injection,
             mutation testing, static testing
BLACK BOX  — bazată pe specificații, fără acces la cod intern
             metode: equivalence partitioning, boundary value analysis,
             all-pairs, fuzzing, exploratory testing
GRAY BOX   — acces la structurile interne pentru a proiecta testele,
             dar testare la nivel de utilizator (black-box)
```

### Code coverage (Q3) — ce este și cum se calculează

> **Code coverage** este o **tehnică de white box testing**: creezi teste care să satisfacă un anumit criteriu de **acoperire a codului** (cât din cod e executat de teste).

**⚠️ Formula de calcul** (slide-uri imagine în IP06; completare standard):
```
Code Coverage (%) = (linii/ramuri/căi executate de teste / total) × 100

Criterii uzuale:
• Statement (line) coverage  = instrucțiuni executate / total instrucțiuni
• Branch (decision) coverage = ramuri (if/else) executate / total ramuri
• Path coverage              = căi de execuție parcurse / total căi
• Function coverage          = funcții apelate / total funcții
```
Exemplu: dacă un program are 100 de linii și testele execută 80 → **statement coverage = 80%**.
Unelte (din curs): JaCoCo (NetBeans/EclEmma pentru Eclipse), IntelliJ "Running with coverage".

### Manual vs Automat (Q5, Q37)

| | **Testare Manuală** | **Testare Automată** |
|--|---------------------|----------------------|
| Definiție | testerul joacă rolul de end-user, parcurge manual cazurile de test | program care controlează execuția testelor, compară rezultate actuale cu cele prezise |
| Cine/cum | tester urmează un test plan scris | cod care automatizează un proces manual existent |
| Avantaje | bună pt UI (text, layout, ordinea elementelor, vizibilitate) | probleme găsite rapid, ieftin de repetat, dezvoltare predictibilă/planificabilă, mai puțină testare manuală |
| Dezavantaje | lent, greu de repetat, costisitor | scrierea scenariilor e dificilă, necesită cunoștințe tehnice ale întregului sistem |

Pași testare manuală: (1) plan de test high-level → (2) scrii cazuri de test detaliate → (3) le aloci testerilor care urmează pașii și înregistrează rezultate → (4) raport de test (folosit de manageri pt decizia de release).

**Documente:** Test Strategy (Project Manager), Test Plan (Test Lead — ce/cum/când/cine), Test Scenario, Test Case (condiție testabilă; minim un test case per cerință).

### Testare nefuncțională (Q24, Q29)

> Verifică faptul că software-ul funcționează corect chiar și la input invalid/neașteptat — aspecte de **calitate**, nu de funcționalitate.

```
• PERFORMANCE / LOAD testing — poate gestiona cantități mari de date/utilizatori?
   - load testing    = nr. concurent așteptat de utilizatori (BD monitorizată)
   - stress testing  = "spargi" aplicația (2× utilizatori, încărcare extremă) → robustețe
   - endurance testing = încărcare continuă susținută (memory leaks)
   - spike testing   = vârf brusc de utilizatori
• USABILITY testing — interfața e ușor de folosit/înțeles?
   - obiective: performance, accuracy, recall, emotional response
• SECURITY testing — protejează datele confidențiale, previne intruziuni
   - 6 concepte: Confidentiality, Integrity, Authentication, Authorization,
     Availability, Non-repudiation
• INTERNATIONALIZATION (i18n) & LOCALIZATION (l10n)
   - i18n = proiectarea ca să poată fi adaptat la limbi/regiuni fără schimbări de cod
   - l10n = adaptarea efectivă pentru o regiune/limbă (traducere, componente locale)
```

---

## 4. Reverse Engineering ⭐⭐⭐
> Întrebări: ce este + tipuri (Q4,36), definiție/metode/exemple (Q27,39).

### Forward vs Reverse Engineering

```
FORWARD ENGINEERING (FE):
   proces tradițional de la abstracții high-level și design logic
   → implementare fizică (cerințe → design → implementare)

REVERSE ENGINEERING (RE):
   procesul de a DESCOPERI principiile tehnologice ale unui dispozitiv/obiect/sistem
   prin ANALIZA structurii, funcției și operării sale
   → a face un dispozitiv/program nou care face același lucru, fără a copia originalul
   (analiza unui sistem pentru a crea reprezentări la un nivel mai înalt de abstractizare)
```
RE își are originea în analiza hardware pentru avantaj comercial/militar.

### Motive pentru RE
```
Interoperabilitate · Documentație pierdută · Analiza produsului ·
Audit de securitate · Eliminarea protecției la copiere · Scopuri academice/învățare ·
Curiozitate · Inteligență tehnică competitivă (ce face concurentul vs ce spune)
```

### Tipuri de Reverse Engineering (întrebare!)
```
RE1: RE al dispozitivelor MECANICE        (ex. rapid prototyping, FullCure)
RE2: RE al CIRCUITELOR INTEGRATE / SMART CARDS
     (invaziv/distructiv — se șlefuiește strat cu strat, poze cu microscop electronic)
     ex.: Satellite TV, security card, phone card, ticket card, bank card
RE3: RE pentru aplicații MILITARE
     (copierea tehnologiilor altor națiuni — WWII, Război Rece)
RE4: RE al SOFTWARE-ului
```

### RE software (RE4) — două tipuri principale
```
1. Codul sursă ESTE disponibil (dar slab documentat)
2. NU există cod sursă pentru software
Black box testing în IP are multe în comun cu reverse engineering.
```
Unelte (din curs): decompilatoare Java (jad, DJ Java Decompiler — `.class` → `.jad`), ArgoUML (File → Import Sources pentru a obține diagrame din cod), asistenți AI (Copilot, ChatGPT, Claude).

### Exemple celebre (din curs)
```
• US B-29  →  URSS Tupolev Tu-4 (copie)
• US AIM-9 Sidewinder  →  Soviet Vympel K-13
• US F-22 / Russian Sukhoi T-50  →  Chinese J-20
• China a copiat multe exemple de hardware US și rusesc (avioane, rachete, HMMWV)
```
> **Etic?** Întrebare deschisă din curs: e etic ca o firmă mare să facă RE pe un produs al concurenței? De ce nu trebuie încurajat RE? (efecte: copiere neautorizată, încălcare copyright).

---

## 5. Ce este IP ⭐⭐
> Întrebări: scop + pași + cel mai important pas (Q13,33), importanță/statistici (Q30), modele de dezvoltare (Q40).

### Definiția Ingineriei Programării
> **NATO (1968):** "stabilirea și utilizarea de **principii inginerești solide** pentru a obține în mod **economic** programe care sunt **sigure** și **funcționează eficient** pe mașini de calcul concrete."
> **IEEE (1983):** "abordarea **sistematică** a dezvoltării, funcționării, întreținerii și retragerii din funcțiune a programelor."

- Disciplină inginerească pentru **toate aspectele** dezvoltării unui program **de dimensiuni mari**, de către o **echipă**.
- **IP vs informatică:** informatica = aspectele **teoretice**; IP = aspectele **practice**.
- **De ce contează (Q30):** economiile statelor depind de software; proiectele mari sunt printre cele mai complicate produse. **Statistici:** majoritatea proiectelor mari eșuează parțial/total (ex. studii: succes ~17-33%). **Erori celebre:** Ariane 5 (explozie, 500M$), Mars Climate Orbiter (English vs metric), IBM OS360 (1000 greșeli/relansare).

### Etapele dezvoltării programelor (Q13, Q33)
```
1. Ingineria cerințelor (Requirements engineering)
2. Proiectarea arhitecturală (Architectural design)
3. Proiectarea detaliată (Detailed design)
4. Scrierea codului (Implementation)
5. Integrarea componentelor (Integration)
6. Validare (Validation)
7. Verificare (Verification)
8. Întreținere (Maintenance)
```
- **Validare:** "Construim produsul **corect**?" (îndeplinește cerințele utilizatorului)
- **Verificare:** "Construim **corect** produsul?" (funcționează corect din punct de vedere tehnic)
- **Integrare:** modelul big-bang vs incremental.

> **Cel mai important pas (răspuns pentru Q13/Q33): Ingineria/Analiza cerințelor.**
> Din curs: "atenția insuficientă acordată **analizei cerințelor** este cea mai des întâlnită cauză a proiectelor vulnerabile"; 50% dintre erori provin din cerințe incorecte/incomplete. Cu cât o eroare e descoperită mai târziu, cu atât costul reparării e mai mare.

**Pașii ingineriei cerințelor:** stabilirea limitelor aplicației → găsirea clientului → identificarea cerințelor → analiza → specificarea → gestionarea. **Tipuri de cerințe:** utilizator, funcționale, de performanță, constrângeri. Calitatea cerințelor: atomice, trasabile, unic identificate, complete, consistente, neambigue, prioritizate, testabile.

### Modele de dezvoltare (Q40)

**Model în CASCADĂ (Waterfall)** — Winston Royce, 1970:
```
Cerințe → Proiectare arhit. → Proiectare detaliată → Implementare →
Testare unități → Testare sistem → Acceptare
```
- **+** împarte sarcina complexă în pași mici, ușor de administrat, fiecare pas dă un produs bine definit, mereu știi unde ești.
- **−** erorile se propagă între pași; fără mecanisme de reparare; **cu feedback** = oferă cadru de remediere a erorilor din pasul precedent, dar erorile descoperite târziu (pasul i+2) nu se remediază; clientul vede produsul abia la final.

**Model în SPIRALĂ:**
```
Pentru fiecare pas, 4 activități: 1.pregătire → 2.gestiunea riscului →
3.dezvoltare → 4.planificarea următorului stagiu
```
- **+** păstrează avantajele cascadei + ia în calcul **noțiunea de risc** (ex. concurent lansează produs similar, arhitect pleacă, client schimbă cerințe).

**Extreme Programming (XP)** — model modern, lightweight, inspirat din RUP:
- Dezvoltarea = colaborarea oamenilor, nu ierarhii/documentații; proiectul e în mintea programatorilor; un reprezentant al clientului mereu disponibil; cod simplu, **test întâi** (TDD); **programare în perechi** (pair programming); integrare continuă (de câteva ori pe zi); 40 ore/săptămână fără ore suplimentare.
- **+** productivitate; **−** documentație insuficientă, doar dezvoltatori senior, structurare slabă a modelării.

> Alte modele (din curs): Ad-hoc, Prototipizare (throw-away / evoluționar), **RUP** (4 etape: Inception, Elaboration, Construction, Transition), **V-Model** (V de la Verificare/Validare), MDD/AMDD, TDD, Agile/Lean/Scrum/Kanban (vezi cap. 6).

---

## 6. Metodologii Agile ⭐⭐
> Întrebări: Scrum (Q6), Kanban (Q7), Agile (Q12).

### Agile (Q12)

> Manifestul Agile — valori și principii pentru dezvoltarea iterativă, centrată pe colaborare și adaptare.

Caracteristici (din curs):
```
• Satisfacerea rapidă a clientului prin livrare continuă de software util (săptămânal)
• Progresul se măsoară prin partea FUNCȚIONALĂ a proiectului
• Chiar și modificările târzii în cerințe sunt binevenite
• Cooperare apropiată client ↔ programatori
• Discuțiile face-to-face = cea mai bună formă de comunicare
• Adaptare continuă la modificări
• Evidențierea și rezolvarea problemelor (nu ascunderea lor)
```

### Scrum (Q6 — roluri, artefacte, evenimente)

Din curs (IP02): clientul devine **parte din echipă**; distribuiri intermediare frecvente cu verificări/validări imediate; transparență în planificare; întâlniri frecvente; **discuții zilnice** cu 3 întrebări:
```
• Ce ai făcut ieri? (realizări)
• Ce ai de gând să faci până mâine? (de realizat)
• Care sunt problemele care te-ar putea încurca? (probleme/riscuri)
```

**⚠️ Detaliere standard** (IP05 listează "Scrum – roles, values, artifacts, events, rules" ca temă, dar slide-urile sunt imagini; completare cu cunoștințe standard pentru Q6):
```
ROLURI (Roles):
• Product Owner  — gestionează Product Backlog, prioritizează, reprezintă clientul
• Scrum Master   — facilitează procesul, înlătură impedimente, apără echipa
• Development Team — auto-organizat, livrează incrementul

ARTEFACTE (Artifacts):
• Product Backlog — lista prioritizată a tuturor cerințelor (user stories)
• Sprint Backlog  — sarcinile selectate pentru sprintul curent
• Increment       — produsul potențial livrabil la finalul sprintului

EVENIMENTE (Events):
• Sprint          — iterație fixă (1-4 săptămâni)
• Sprint Planning — planificarea sprintului
• Daily Scrum     — întâlnirea zilnică (cele 3 întrebări de mai sus)
• Sprint Review   — demonstrarea incrementului către stakeholderi
• Sprint Retrospective — îmbunătățirea procesului
```

### Lean & Kanban (Q7)

**Lean Software Development** — 7 principii: elimină ce e nefolositor, amplifică învățarea, decide cât mai târziu, termină cât mai curând, oferă responsabilități echipei, construiește integritate, vezi proiectul în ansamblu.

**Kanban** (Taiichi Ohno, Toyota, 1953; pentru software: Corey Ladas/David Anderson):
```
• Sistem de planificare pentru producție lean & just-in-time (JIT)
• Metodă de management al "knowledge work" cu accent pe livrare JIT,
  fără a supraîncărca membrii echipei
• Sistem VIZUAL de management al procesului — ce să produci, când, cât
• Vizualizarea = aspect important pentru a înțelege munca și fluxul (workflow)

Principii (caracteristici):
• Start with existing process (nu prescrie roluri/pași specifici)
• Pursue incremental, evolutionary change (schimbare continuă, incrementală)
• Respect current process, roles, responsibilities (respectă rolurile actuale)
• Leadership at all levels (acte de leadership la toate nivelurile)
Reguli din producție: procesul ulterior ia piese conform kanban-ului;
   niciun produs fără kanban; produse defecte nu trec mai departe (100% defect-free).
```

> Comparativ (din curs): comunicare cu clientul = **Scrum**; produs finit per etapă = **Cascadă**; sprijin pentru membri = **Lean**; fiecare face ce-i place = **XP**; studiu de risc = **Spirală**; realocarea resurselor la dificultăți = **Kanban**.

---

## 7. Modelare & UML ⭐⭐
> Întrebări: use case (Q22), modelare/limbaje (Q26), diagrama de clase + relații (Q34).

### Modelare (Q26)

> **Model** = simplificarea realității; planul detaliat (blueprint) al unui sistem.

**De ce modelăm?** pentru a înțelege mai bine ce avem de făcut; pentru a ne concentra pe un aspect la un moment dat; pentru a vizualiza, specifica structura/comportamentul, a oferi un șablon de construcție, a documenta deciziile.

**Principii ale modelării:** modelele influențează soluția finală; se pot folosi diferite niveluri de precizie; modelele bune au corespondent în realitate; **nu e suficient un singur model**.

**Limbaje de modelare (Q26):**
```
GRAFICE: arbori comportamentali, modelarea proceselor de business,
   EXPRESS (date), flowchart, ORM (roluri), rețele Petri, diagrame UML
SPECIFICE: AML (algebric), DSL (domenii specifice), VRML (realitate virtuală)
```

### UML

> **UML (Unified Modeling Language)** = limbaj **grafic** pentru vizualizarea, specificarea, construirea și documentarea artefactelor unui sistem software. Succesorul a trei limbaje OO: **Booch + OMT + OOSE**. Standard OMG; ultima versiune UML 2.5.1.

**Tipuri de diagrame UML:**
```
STRUCTURALE:  Clase, Componente, Deployment, Obiecte, Pachete, Structură compozită
COMPORTAMENTALE: Use Case, Activitate, Stare
DE INTERACȚIUNE: Secvență, Comunicare
```

### Diagrame Use Case (Q22)

> Diagramă **comportamentală** care captează cerințele sistemului și delimitează **granițele** sistemului.

**Use case (caz de utilizare):** descrierea unei mulțimi de secvențe de acțiuni pe care un program le execută când interacționează cu **actori** și care conduc la un **rezultat observabil**. Precizează **CE** face programul, nu **CUM**.

**Conține:**
```
• Use Case-uri = funcționalități ale sistemului (oval cu nume = frază verbală)
• Actori = entități externe (utilizator uman, sistem software, sistem hardware)
• Relații
```
**Tipuri de relații:**
```
• Asociere:     Actor–UseCase, UseCase–UseCase (comunicare)
• Generalizare: Actor–Actor, UseCase–UseCase (ierarhie, caz particular, moștenește relațiile)
• Dependență:   UseCase–UseCase:
    <<include>> — un UseCase FOLOSEȘTE comportamentul altuia (obligatoriu)
    <<extend>>  — comportamentul unui UseCase poate fi EXTINS de altul (opțional)
```
**Tipuri de conținut use case:** pe scurt (cazul principal de succes), cazual (ce se face dacă apare ceva), detaliat (toate situațiile).

### Diagrame de Clase (Q34)

> Diagramă **structurală** care modelează **vocabularul** sistemului — conceptele folosite pentru a descrie soluția. Folosită pentru a modela structura unui program.

**Conține:** Clase/Interfețe, Obiecte, Relații.

**Elementele unei clase:**
```
┌─────────────────┐
│   Nume          │  identifică clasa
├─────────────────┤
│ Atribute        │  proprietăți (private)
├─────────────────┤
│ Metode          │  servicii cerute instanțelor (private/protected)
└─────────────────┘
```

**Tipuri de relații într-o diagramă de clase (Q34):**
```
• GENERALIZARE (moștenire) — relație "is-a" (este un/o); elementul particular
  moștenește relațiile elementului general.            ───────▷ (triunghi gol)

• ASOCIERE — conexiune semantică/interacțiune între obiecte din clase diferite.
  Elemente: nume (rolul), capete de asociere, MULTIPLICITATE (câte instanțe
  ale unei clase corespund unei instanțe a celeilalte).    ─────── (linie)
  Ex: Student — Disciplină (un student urmează 0..* discipline).

• AGREGARE — caz particular de asociere; relație "parte-întreg"; de obicei se
  specifică doar multiplicitatea.                          ◇─────── (diamant gol)

• DEPENDENȚĂ — o clasă depinde/folosește alta.            ┄┄┄┄┄> (săgeată punctată)
```

**Exemplu diagramă de clase (pentru desenat pe tablă — Q34):**
```
        ┌───────────┐  1      0..*  ┌───────────┐
        │  Profesor │───────────────│ Disciplină│
        └───────────┘  predă         └───────────┘
                                          │ 0..*
                                          │ urmată de
                                          │ 0..*
                                     ┌───────────┐
                                     │  Student  │
                                     └───────────┘
Relații: Profesor 1—0..* Disciplină (asociere "predă");
         Disciplină 0..*—0..* Student (asociere "urmează").
```

> **Legătura use case ↔ diagrame de clase (întrebare din curs):** use case-urile captează **cerințele** (ce face sistemul, din perspectiva utilizatorului); din ele și din descrierea problemei se identifică **clasele** (vocabularul). Use case = punctul de plecare; diagrama de clase = structura care implementează acele funcționalități.

> **Alte diagrame (din curs):** Secvență (mesaje ordonate în timp, bidimensională: viața obiectului vertical, mesaje orizontal), Comunicare/Colaborare (organizarea structurală), Stare (stări + tranziții), Activitate (flux de activități), Deployment (hardware), Pachete (`<<import>>` public, `<<access>>` privat). **C4 Model:** Context, Containers, Components, Code (niveluri de abstractizare).

---

# ANEXE

## Anexă A — GRASP (IP05)
**GRASP** = General Responsibility Assignment Software Patterns (Craig Larman) — alocarea responsabilităților claselor/obiectelor.
- **Information Expert:** asignează responsabilitatea clasei care are informațiile necesare.
- **Creator:** B creează A dacă B agregă/conține/folosește A sau are datele de inițializare.
- **Low Coupling:** cuplaj redus = o clasă nu depinde de multe altele (probleme: schimbări forțate, greu de înțeles/refolosit).
- **High Cohesion:** coeziune mare = responsabilitățile clasei sunt strâns legate (clasa nu face prea multe lucruri nerelaționate).
- **Controller:** obiect non-UI responsabil cu tratarea evenimentelor sistemului; deleagă munca, nu o face singur.
- **Law of Demeter** ("don't talk to strangers"): o metodă apelează doar metode ale: lui însuși, parametrilor, obiectelor create, obiectelor conținute.

## Anexă B — DRY / YAGNI / KISS (IP05)
- **DRY (Don't Repeat Yourself):** "fiecare cunoaștere trebuie să aibă o reprezentare unică, neambiguă în sistem". Repetiția în logică → abstractizare; în proces → automatizare. (Variante: Once and Only Once, DIE.)
- **YAGNI (You Aren't Gonna Need It):** nu adăuga funcționalitate până nu e necesară; implementează lucrurile când chiar ai nevoie, nu când prevezi că ai putea.
- **KISS (Keep It Simple, Stupid):** sistemele merg cel mai bine dacă sunt simple; evită complexitatea inutilă.

## Anexă C — Refactoring (IP11)
> "Procesul de a schimba un sistem software astfel încât **nu alterează comportamentul extern** al codului, dar **îi îmbunătățește structura internă**." (Martin Fowler)

- Cod mai curat/simplu/elegant; programul refactorizat = **funcțional echivalent** cu cel inițial. **Refactoring ≠ rewriting** (rewriting schimbă funcționalitatea).
- **Degradarea codului:** cod inițial curat → se degradează prin schimbări incrementale → Rigid, Fragil, Imobil, Vâscos.
- **Code smells:** cod duplicat, metode lungi, clase mari, liste lungi de parametri, switch-uri, generalitate speculativă, comunicare intensă între obiecte, chaining de mesaje.
- **Procedeu:** pregătești teste automate → schimbi în iterații mici → testezi la fiecare iterație (echivalență semantică) → dacă un test pică, anulezi.
- **Tehnici:** Encapsulate Field, Generalize Type, Replace conditional with polymorphism, Extract Method/Class, Move Method/Field, Rename, Pull Up / Push Down.

## Anexă D — Estimare software & COCOMO (IP11)
- Estimăm: dimensiune, complexitate, timp, resurse, costuri, productivitate. **WBS (Work Breakdown Structure)** — listă de 10-20 task-uri.
- **KLOC** (Kilo Lines Of Code), **PM** (Person-Month). **Legea lui Brooks:** "a adăuga oameni la un proiect întârziat îl întârzie și mai mult" (overhead de comunicare; P membri → între P-1 și P(P-1)/2 canale).
- **COCOMO** (Boehm) — model de estimare a efortului; ia în calcul atribute de produs, platformă, personal, proiect.
- **Planning Game** (din XP) — planificare ca joc cu "user stories" pe cartonașe, estimate de 1/2/3 săptămâni.

## Anexă E — Pachete & principii (IP09)
- **Pachet** = container logic pentru elemente; definește un spațiu de nume; grupează clase; poate conține subpachete (structură arborescentă). Relații: `<<import>>` (public), `<<access>>` (privat).
- Principii de coeziune a pachetelor: Release Reuse Equivalency, Common Closure, Common Reuse. De cuplaj: Acyclic Dependencies (DAG, fără cicluri), Stable Dependencies, Stable Abstractions.
- Metrici: Ca (afferent couplings), Ce (efferent couplings), **I = Ce/(Ca+Ce)** (instabilitate; I=0 maxim stabil, I=1 maxim instabil), A (abstractness = AC/TC).

## Anexă F — AI Code Generation (IP12)
- IDE-uri moderne + **GenAI / LLM** (ChatGPT, GitHub Copilot, Claude) → autocomplete de snippet-uri întregi, generare cod din comentarii/specificații, generare teste unitare/code coverage.
- **+** productivitate (mai ales pt dezvoltatori experimentați), stres redus, mai puțin context switching. **−** creativitate limitată (imită, nu inovează), dificultăți cu contexte mari/complexe.
- Codul generat e aproape de soluție, dar **code review-urile rămân importante** (codul AI are bug-uri). Atenție la securitate, privacy, conformitate.

---

# CHEAT SHEET FINAL

### Design Patterns GoF
```
Definiție: soluție generală reutilizabilă la o problemă frecventă de design
4 ELEMENTE: Name, Problem, Solution, Consequences
3 TIPURI:
  CREAȚIONALE: AbstractFactory, Builder, FactoryMethod, Prototype, Singleton
  STRUCTURALE: Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy
  COMPORTAM.: ChainOfResp, Command, Interpreter, Iterator, Mediator, Memento,
              Observer, State, Strategy, TemplateMethod, Visitor

Adapter  = convertește interfața incompatibilă (priză-ștecher)
Decorator= adaugă responsabilități dinamic, alt. la subclasare (brad)
Flyweight= partajare pt multe obiecte fine-grained, economie memorie (FontData)
Mediator = încapsulează interacțiunea obiectelor, reduce cuplaj (turn de control)
Observer = one-to-many, notifică dependenții automat (publish-subscribe)
Visitor  = operație nouă fără a schimba clasele elementelor
Prototype= creează prin clonare
ChainOfResp = pasează cererea pe lanț până e tratată
```

### SOLID
```
S: o singură responsabilitate (un motiv de schimbare)
O: deschis extensiei, închis modificării (abstracție+polimorfism)
L: subtipurile substituibile pt tipul de bază (Rectangle/Square)
I: interfețe mici, clientul nu depinde de metode nefolosite (Worker→Workable+Feedable)
D: depinde de abstracții, nu de concrete (Dependency Injection)
+ DRY, YAGNI, KISS, GRASP
```

### QA / Testare
```
QA = prevenție (procese); Quality Control = corecție (testarea e subset)
Niveluri: Unit → Integration → System → Acceptance (+Regression)
Metode: White box (code coverage) / Black box / Gray box
Code coverage = % cod executat de teste (statement/branch/path)
Nefuncțională: performance(load/stress/endurance/spike), usability, security, i18n
Manual (UI, lent) vs Automat (rapid, repetabil, necesită cod)
```

### Reverse Engineering
```
RE = descoperă principiile prin analiza structurii/funcției/operării
Tipuri: RE1 mecanic, RE2 circuite/smartcard, RE3 militar, RE4 software
RE4: cu sursă (slab documentată) / fără sursă; ex: B-29→Tu-4
```

### IP & modele
```
Etape: Cerințe→Arhitectură→Detaliat→Implementare→Integrare→Validare→Verificare→Întreținere
Cel mai important pas: ANALIZA CERINȚELOR (50% din erori vin de aici)
Validare="produsul corect?"; Verificare="corect produsul?"
Modele: Cascadă (pași, dar erori se propagă), Spirală (+risc), XP (pair, TDD)
Agile: livrare continuă, modificări binevenite; Scrum (PO/SM/Team, backlog, sprint);
Kanban (vizual, JIT, schimbare evolutivă)
```

### UML
```
Use Case: actori + use case-uri + relații (asociere, generalizare, <<include>>/<<extend>>)
Clase: Nume/Atribute/Metode; relații: generalizare(▷), asociere(─),
       agregare(◇), dependență(┄>); multiplicitate
Diagrame: structurale (clase), comportamentale (use case), interacțiune (secvență)
```

---

# 🎯 TEST GRILĂ — Ingineria Programării (subiecte reale de licență 2021–2025)
> Fiecare grilă pornește de la un **subiect dat efectiv la examen**. Apasă pe **„Răspuns"**. Răspunsul corect e distribuit uniform pe A/B/C/D.

**1.** *(«Design patterns. Definiție»)* — Un design pattern este:
- A) un algoritm de sortare
- B) o soluție generală, reutilizabilă, la o problemă care apare frecvent în design-ul software
- C) o bibliotecă de cod gata scrisă, gata de importat
- D) un limbaj de programare

<details><summary>✅ Răspuns</summary>

**B)** — C e o confuzie clasică: pattern-ul e o descriere/șablon, NU cod concret sau bibliotecă.
</details>

**2.** *(«Design patterns. Elemente»)* — Cele 4 elemente esențiale (GoF):
- A) cod, teste, documentație, deployment
- B) model, view, controller, service
- C) Nume, Problemă, Soluție, Consecințe
- D) clase, obiecte, metode, atribute

<details><summary>✅ Răspuns</summary>

**C)** — Numele crește vocabularul de design; consecințele = trade-off-urile.
</details>

**3.** *(«Design patterns. Clasificare»)* — Cele 3 categorii GoF:
- A) Creaționale, Structurale, Comportamentale
- B) publice, private, protejate
- C) simple, medii, complexe
- D) front-end, back-end, database

<details><summary>✅ Răspuns</summary>

**A)** — Creaționale (creare), Structurale (compunere), Comportamentale (algoritmi/responsabilități).
</details>

**4.** *(«DP structurale. Exemplificare Adapter»)* — Adapter se folosește pentru a:
- A) adăuga responsabilități dinamic unui obiect
- B) garanta o singură instanță
- C) parcurge secvențial o colecție
- D) converti interfața unei clase în alta așteptată de client (interfețe incompatibile)

<details><summary>✅ Răspuns</summary>

**D)** — A = Decorator, B = Singleton, C = Iterator. Exemplu: priză-ștecher.
</details>

**5.** *(«DP structurale. Decorator»)* — Decorator permite:
- A) convertirea unei interfețe incompatibile
- B) adăugarea dinamică de responsabilități unui obiect (alternativă la subclasare)
- C) definirea unei familii de algoritmi
- D) restaurarea unei stări anterioare

<details><summary>✅ Răspuns</summary>

**B)** — A = Adapter, C = Strategy, D = Memento. Atenție: ordinea aplicării decoratorilor contează.
</details>

**6.** *(«GoF. Proxy»)* — Proxy oferă:
- A) o interfață simplificată unui subsistem complex
- B) un mecanism de clonare a obiectelor
- C) un substitut/placeholder pentru alt obiect, pentru a controla accesul la el
- D) notificarea automată a observatorilor

<details><summary>✅ Răspuns</summary>

**C)** — A = Facade, B = Prototype, D = Observer. Exemplu: ATM ca proxy spre bancă.
</details>

**7.** *(«DP structurale. Flyweight»)* — Flyweight rezolvă problema:
- A) suportării eficiente a unui număr mare de obiecte fine-grained prin partajarea datelor comune (economie de memorie)
- B) prea multor instanțe de servicii
- C) interfețelor incompatibile
- D) ordinii de execuție a firelor

<details><summary>✅ Răspuns</summary>

**A)** — Exemplu: FontData partajat între mii de caractere identice. C = Adapter.
</details>

**8.** *(«DP comportamentale. Mediator»)* — Mediator:
- A) adaugă responsabilități dinamic
- B) clonează obiecte
- C) definește scheletul unui algoritm
- D) încapsulează cum interacționează un set de obiecte → reduce cuplajul (obiectele nu se mai cunosc direct)

<details><summary>✅ Răspuns</summary>

**D)** — Exemplu: turnul de control (avioanele nu comunică între ele). Risc: „God Object".
</details>

**9.** *(«GoF. Observer»)* — Observer definește:
- A) o operație nouă fără modificarea claselor
- B) o dependență one-to-many: când un obiect se schimbă, toți dependenții sunt notificați automat (publish-subscribe)
- C) un lanț de handlere
- D) încapsularea unei cereri ca obiect

<details><summary>✅ Răspuns</summary>

**B)** — A = Visitor, C = Chain of Responsibility, D = Command.
</details>

**10.** *(«DP comportamentale. Chain of Responsibility»)* — Chain of Responsibility:
- A) pasează cererea de-a lungul unui lanț de handlere până când unul o tratează
- B) notifică automat mai multe obiecte
- C) convertește o interfață
- D) oferă un punct global de acces

<details><summary>✅ Răspuns</summary>

**A)** — B = Observer, C = Adapter, D = Singleton.
</details>

**11.** *(«GoF. DP creaționale. Prototype»)* — Prototype creează obiecte prin:
- A) instanțierea directă cu `new`
- B) un lanț de fabrici
- C) clonarea unui obiect prototip existent
- D) o interfață unificată

<details><summary>✅ Răspuns</summary>

**C)** — Evită reinițializări repetate; util când clasele se specifică la runtime.
</details>

**12.** *(«Care sunt principiile SOLID»)* — SOLID înseamnă:
- A) Structure, Object, Logic, Interface, Data
- B) Single Responsibility, Open-Closed, Liskov Substitution, Interface Segregation, Dependency Inversion
- C) Simple, Optimized, Layered, Independent, Documented
- D) Static, Overloaded, Linked, Inherited, Dynamic

<details><summary>✅ Răspuns</summary>

**B)** — Cele 5 principii de design OO (Robert C. Martin).
</details>

**13.** *(«SOLID: detaliere S»)* — Principiul S (Single Responsibility):
- A) deschis extensiei, închis modificării
- B) subtipurile substituibile pentru bază
- C) depinde de abstracții
- D) o clasă trebuie să aibă un singur motiv de schimbare (o singură responsabilitate)

<details><summary>✅ Răspuns</summary>

**D)** — A = O, B = L, C = D. Legat de low coupling & high cohesion.
</details>

**14.** *(«SOLID: detaliere O»)* — Principiul O (Open-Closed): entitățile trebuie să fie:
- A) deschise extensiei, închise modificării
- B) deschise modificării, închise extensiei
- C) cu o singură responsabilitate
- D) substituibile prin subtipuri

<details><summary>✅ Răspuns</summary>

**A)** — B e inversul greșit. Se obține prin abstractizare + polimorfism.
</details>

**15.** *(«SOLID: detaliere L»)* — Principiul L (Liskov):
- A) clientul nu depinde de metode nefolosite
- B) o clasă = o singură responsabilitate
- C) subtipurile trebuie să fie substituibile pentru tipurile de bază, fără a strica comportamentul așteptat
- D) se depinde de abstracții, nu de detalii

<details><summary>✅ Răspuns</summary>

**C)** — A = I, B = S, D = D. Exemplu de încălcare: Square vs Rectangle (setWidth/setHeight).
</details>

**16.** *(«SOLID și principiul I»)* — Principiul I (Interface Segregation):
- A) o clasă = un motiv de schimbare
- B) clienții nu trebuie forțați să depindă de metode pe care nu le folosesc (interfețe mici, specifice)
- C) deschis extensiei, închis modificării
- D) substituibilitatea subtipurilor

<details><summary>✅ Răspuns</summary>

**B)** — Sparge interfețele „grase". Ex: `Worker(work+eat)` → `Workable` + `Feedable`.
</details>

**17.** *(«D din SOLID»)* — Principiul D (Dependency Inversion):
- A) o clasă = o responsabilitate
- B) deschis extensiei, închis modificării
- C) interfețe mici, specifice clientului
- D) modulele de nivel înalt și cele de nivel jos depind ambele de abstracții (nu de concrete)

<details><summary>✅ Răspuns</summary>

**D)** — A = S, B = O, C = I. Realizat prin Dependency Injection.
</details>

**18.** *(«Principiile GRASP. Coeziune»)* — Coeziunea (cohesion) măsoară:
- A) cât de strâns legate/focalizate sunt responsabilitățile unei clase
- B) câte alte clase depinde o clasă
- C) numărul de linii de cod
- D) viteza de execuție

<details><summary>✅ Răspuns</summary>

**A)** — B descrie **cuplajul**. Coeziune mare = bine; cuplaj mare = rău.
</details>

**19.** *(«Principiile GRASP. Cuplaj»)* — Cuplajul redus (low coupling) e de dorit pentru că:
- A) face clasele să depindă de multe altele
- B) crește numărul de bug-uri
- C) o clasă cu cuplaj mic nu depinde de multe alte clase → mai ușor de înțeles, refolosit, întreținut
- D) elimină nevoia de interfețe

<details><summary>✅ Răspuns</summary>

**C)** — Cuplaj mare = schimbări în lanț, clase greu de refolosit în izolare.
</details>

**20.** *(«Metodologia SCRUM. Roluri»)* — Rolurile în Scrum:
- A) Manager, Analist, Tester
- B) Product Owner, Scrum Master, Development Team
- C) Client, Server, Database
- D) Frontend, Backend, DevOps

<details><summary>✅ Răspuns</summary>

**B)** — PO = prioritizează backlog-ul; Scrum Master = facilitează; Team = livrează.
</details>

**21.** *(«SCRUM. Artefacte»)* — Artefactele în Scrum:
- A) Sprint, Daily, Retrospective
- B) Model, View, Controller
- C) roluri, valori, reguli
- D) Product Backlog, Sprint Backlog, Increment

<details><summary>✅ Răspuns</summary>

**D)** — A sunt **evenimente** (confuzie frecventă artefacte ↔ evenimente!).
</details>

**22.** *(«SCRUM. Evenimente»)* — Care este un eveniment (ceremonie) Scrum?
- A) Sprint Planning / Daily Scrum / Sprint Review / Retrospective
- B) Product Backlog
- C) Product Owner
- D) Increment

<details><summary>✅ Răspuns</summary>

**A)** — B și D = artefacte; C = rol.
</details>

**23.** *(«Metodologia Kanban»)* — Kanban se caracterizează prin:
- A) iterații fixe (sprinturi) cu roluri stricte
- B) documentație extinsă înainte de a scrie cod
- C) vizualizarea fluxului de lucru, livrare just-in-time, schimbare evolutivă, fără roluri prescrise
- D) programare în perechi obligatorie

<details><summary>✅ Răspuns</summary>

**C)** — A e mai degrabă Scrum; B e cascada; D e XP.
</details>

**24.** *(«Metodologia Agile»)* — Un principiu Agile:
- A) modificările târzii în cerințe trebuie respinse
- B) livrare continuă de software funcțional; modificările (chiar târzii) sunt binevenite; colaborare strânsă cu clientul
- C) documentația e mai importantă decât software-ul funcțional
- D) comunicarea se face doar prin rapoarte scrise

<details><summary>✅ Răspuns</summary>

**B)** — A, C, D sunt exact opusul valorilor Agile.
</details>

**25.** *(«Reverse Engineering. Definiție»)* — Reverse engineering este procesul de a:
- A) scrie cod pornind de la cerințe spre implementare
- B) compila codul sursă
- C) sorta datele
- D) descoperi principiile unui sistem prin analiza structurii, funcției și operării sale

<details><summary>✅ Răspuns</summary>

**D)** — A este **forward engineering** (opusul).
</details>

**26.** *(«Reverse Engineering. Tipuri»)* — La RE **software**, cele două situații principale sunt:
- A) cod sursă disponibil (dar slab documentat) / fără cod sursă
- B) cu compilator / fără compilator
- C) static / dinamic
- D) manual / automat

<details><summary>✅ Răspuns</summary>

**A)** — Tipuri generale RE: mecanic, circuite/smartcard, militar, software.
</details>

**27.** *(«Ce este IP, etapele, care e cea mai importantă»)* — Cea mai importantă etapă:
- A) scrierea codului (implementarea)
- B) deployment-ul
- C) ingineria/analiza cerințelor (atenția insuficientă aici e cea mai frecventă cauză de eșec)
- D) întreținerea

<details><summary>✅ Răspuns</summary>

**C)** — ~50% din erori provin din cerințe incorecte/incomplete; erorile târzii costă cel mai mult.
</details>

**28.** *(«Etapele dezvoltării»)* — Diferența validare – verificare:
- A) validare = „construim corect produsul?"; verificare = „construim produsul corect?"
- B) validare = „construim produsul corect?" (îndeplinește cerințele); verificare = „construim corect produsul?" (corect tehnic)
- C) sunt sinonime
- D) validarea doar de client, verificarea doar de manager

<details><summary>✅ Răspuns</summary>

**B)** — A e inversat. Validare = produsul potrivit; Verificare = produs făcut corect.
</details>

**29.** *(«Unde/când se folosește IP; când NU»)* — Când NU e neapărat necesar să aplici riguros ingineria programării?
- A) la un proiect mic, personal (ex. un scraper de uz propriu) — nu aplici SCRUM etc., ai pierde timp aiurea
- B) la un sistem bancar de milioane de linii
- C) la un sistem de control al traficului aerian
- D) la o aplicație folosită de mii de utilizatori

<details><summary>✅ Răspuns</summary>

**A)** — IP e pentru proiecte mari, în echipă. La un proiect trivial personal, overhead-ul metodologic nu se justifică.
</details>

**30.** *(«Modele de dezvoltare. Cascada»)* — Un dezavantaj al modelului în cascadă:
- A) nu împarte munca în pași
- B) ia în calcul riscul la fiecare pas
- C) nu produce niciun rezultat intermediar
- D) erorile se propagă între pași, iar clientul vede produsul abia la final

<details><summary>✅ Răspuns</summary>

**D)** — B e specific modelului în **spirală**. Avantaj cascadă: simplu, ușor de controlat.
</details>

**31.** *(«Modele de dezvoltare. Spirala»)* — Ce aduce în plus modelul în spirală față de cascadă?
- A) programarea în perechi
- B) analiza/gestiunea riscului la fiecare iterație
- C) livrarea zilnică de software
- D) eliminarea etapei de testare

<details><summary>✅ Răspuns</summary>

**B)** — Ex. riscuri: concurent lansează produs similar, arhitect pleacă, client schimbă cerințe.
</details>

**32.** *(«Quality Assurance. Definiție»)* — Relația între QA și testare:
- A) QA = corecția defectelor; testarea = prevenție
- B) sunt exact același lucru
- C) QA = prevenție (procese, îmbunătățire continuă); testarea = parte din Quality Control (corecție)
- D) testarea înlocuiește complet QA

<details><summary>✅ Răspuns</summary>

**C)** — A e inversat. QA previne, QC (inclusiv testarea) corectează.
</details>

**33.** *(«QA. Ce este code coverage, cum se calculează»)* — Code coverage măsoară:
- A) procentul de cod (instrucțiuni/ramuri/căi) executat de teste
- B) numărul de bug-uri găsite
- C) timpul de execuție al testelor
- D) numărul total de teste scrise

<details><summary>✅ Răspuns</summary>

**A)** — Ex: 80 linii executate din 100 → statement coverage 80%. Tehnică de white box.
</details>

**34.** *(«QA. Manual Testing vs Automation Testing»)* — Un avantaj al testării automate:
- A) e mai bună pentru verificarea aspectului vizual/UI
- B) nu necesită deloc scrierea de cod
- C) nu poate fi planificată
- D) ieftin de repetat, rapid, predictibil și planificabil

<details><summary>✅ Răspuns</summary>

**D)** — A e punctul forte al testării **manuale**. Automată: costisitor de scris scenariile inițial.
</details>

**35.** *(«QA. Testare nefuncțională»)* — Care este un tip de testare NEfuncțională?
- A) unit testing
- B) performance/load, security, usability, internationalization
- C) testarea unei funcții specifice
- D) testarea unei condiții de business

<details><summary>✅ Răspuns</summary>

**B)** — Testarea nefuncțională vizează calitatea (performanță, securitate...), nu funcționalitatea.
</details>

**36.** *(«Unit Teste. De ce? Exemplu»)* — Unit testing (testarea unitară):
- A) testează sistemul complet integrat
- B) e făcută de client înainte de acceptare
- C) testează o unitate mică (funcție/clasă), rulată de programatori, cu rezultate documentate (simulatoare I/O)
- D) verifică doar interfața grafică

<details><summary>✅ Răspuns</summary>

**C)** — A = system testing, B = acceptance testing, D = GUI testing.
</details>

**37.** *(«Use case. Definiție, exemple, elemente»)* — O diagramă Use Case conține:
- A) actori, use case-uri și relații (asociere, generalizare, `<<include>>`/`<<extend>>`)
- B) clase, atribute, metode
- C) stări și tranziții
- D) mesaje ordonate în timp

<details><summary>✅ Răspuns</summary>

**A)** — B = diagrama de clase, C = diagrama de stare, D = diagrama de secvență.
</details>

**38.** *(«Use case»)* — Relația `<<include>>` între două use case-uri înseamnă:
- A) un use case extinde opțional comportamentul altuia
- B) o relație de moștenire între actori
- C) o asociere actor–use case
- D) un use case folosește (obligatoriu) comportamentul definit în alt use case

<details><summary>✅ Răspuns</summary>

**D)** — A descrie `<<extend>>` (opțional) — confuzie frecventă include ↔ extend.
</details>

**39.** *(«Diagrame de clase. Exemplu, detaliere»)* — Care este o relație corectă într-o diagramă de clase?
- A) Sprint, Backlog, Increment
- B) Generalizare (moștenire), Asociere, Agregare, Dependență
- C) GET, POST, PUT, DELETE
- D) Create, Read, Update, Delete

<details><summary>✅ Răspuns</summary>

**B)** — A = Scrum, C = metode HTTP, D = CRUD.
</details>

**40.** *(«Modelare. De ce? Limbaje de modelare»)* — De ce modelăm un sistem software?
- A) pentru a scrie mai mult cod
- B) pentru a evita testarea
- C) pentru a înțelege mai bine, a ne concentra pe un aspect, a oferi un șablon și a documenta deciziile
- D) pentru a mări dimensiunea proiectului

<details><summary>✅ Răspuns</summary>

**C)** — Model = simplificarea realității; un singur model nu e suficient.
</details>

**41.** *(«Principii POO în pachete. Principiul stabilității»)* — Principiul stabilității pachetelor spune că:
- A) un pachet ar trebui să depindă doar de pachete mai stabile decât el
- B) toate pachetele trebuie să fie instabile
- C) pachetele stabile trebuie să fie concrete
- D) dependențele pot forma cicluri

<details><summary>✅ Răspuns</summary>

**A)** — Pachetele stabile ar trebui să fie **abstracte** (C inversat); dependențele = **DAG** fără cicluri (D fals).
</details>

**42.** *(«GoF»)* — Ce este GoF (Gang of Four)?
- A) un limbaj de modelare
- B) o metodologie Agile
- C) un framework de testare
- D) cei 4 autori ai cărții „Design Patterns: Elements of Reusable Object-Oriented Software"

<details><summary>✅ Răspuns</summary>

**D)** — Gamma, Helm, Johnson, Vlissides — cartea care a standardizat cele 23 de pattern-uri clasice.
</details>

---

> 💡 **Distribuția răspunsurilor** e echilibrată. Confuzii cheie IP: elementele pattern-ului, cele 3 tipuri, literele SOLID, **artefacte vs evenimente Scrum**, coeziune vs cuplaj, validare vs verificare, `<<include>>` vs `<<extend>>`, QA (prevenție) vs testare (corecție).
