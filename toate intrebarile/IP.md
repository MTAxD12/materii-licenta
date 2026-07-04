# Ingineria Programării — Toate întrebările de licență (2019–2025), ordonate după frecvență
> Întrebările reale din ultimii 6 ani (fără 2020), grupate pe teme și **ordonate de la cele mai frecvente la cele mai rare**. Apasă pe **„📖 Răspuns"**.
> Legendă: 🔴 foarte frecvent · 🟠 frecvent · 🟡 de 2 ori · ⚪ o dată.

---

## 1. Principiile SOLID (care sunt + detaliere pe fiecare) 🔴 (~15 apariții — în fiecare an, fiecare literă)
> *„Care sunt principiile SOLID? Detaliați S / O / L / I / D."*

<details><summary>📖 Răspuns — cele 5 principii</summary>

**SOLID** = 5 principii de design orientat-obiect (Robert C. Martin):

**S — Single Responsibility Principle:** o clasă trebuie să aibă **un singur motiv de schimbare** (o singură responsabilitate, complet încapsulată). Violare: o clasă care se și desenează, și salvează. → separi responsabilitățile.

**O — Open-Closed Principle:** entitățile trebuie să fie **deschise extensiei, închise modificării**. Adaugi comportament nou fără a modifica codul existent (prin abstractizare + polimorfism). Violare: `switch`/`if` pe tip care se modifică la fiecare caz nou.

**L — Liskov Substitution Principle:** **subtipurile trebuie să fie substituibile** pentru tipurile de bază, fără a strica comportamentul. Violare clasică: `Square extends Rectangle` (redefinește setWidth/setHeight incompatibil).

**I — Interface Segregation Principle:** **clienții nu trebuie forțați să depindă de metode pe care nu le folosesc**. Sparge interfețele „grase" în altele mici. Ex: `Worker(work+eat)` → `Workable` + `Feedable`.

**D — Dependency Inversion Principle:** modulele de nivel **înalt și jos** depind ambele de **abstracții** (nu de concrete). Realizat prin **Dependency Injection**. Violare: `new ClasaConcreta` direct în clasă.

> Alte principii: **DRY** (Don't Repeat Yourself), **YAGNI** (You Aren't Gonna Need It), **KISS** (Keep It Simple, Stupid).
</details>

<details><summary>📖 Detaliere O (Open-Closed) — exemplu unde nu e respectat + soluție</summary>

**ÎNCĂLCARE** (fiecare formă nouă cere modificarea clasei):
```java
class GraphicEditor {
  void draw(Shape s) {
    if (s.type==CIRCLE) drawCircle(s);
    else if (s.type==RECT) drawRect(s);   // la fiecare formă nouă modifici aici!
  }
}
```
**SOLUȚIE** (extensie prin derivare):
```java
abstract class Shape { abstract void draw(); }      // închis la modificare
class Circle extends Shape { void draw() {...} }
class Rectangle extends Shape { void draw() {...} }
class Triangle extends Shape { void draw() {...} }   // formă nouă — nu modifici nimic
class GraphicEditor { void draw(Shape s) { s.draw(); } }
```
</details>

<details><summary>📖 Detaliere L (Liskov) cu exemplu</summary>

Subtipurile trebuie substituibile pentru bază. Încălcare (`Square`/`Rectangle`): un `Square` care redefinește `setWidth/setHeight` să schimbe ambele dimensiuni sparge așteptarea `arie = w*h` a codului care lucrează cu `Rectangle` → `Square` NU e substituibil. Soluție: nu forța o moștenire care nu respectă substituibilitatea.
</details>

<details><summary>📖 Detaliere D (Dependency Inversion) cu exemplu</summary>

Rău: `class Service { MySqlFinder finder; }` — depinde de o clasă concretă. Bine: `class Service { IFinder finder; Service(IFinder f){...} }` — depinde de o **abstracție**, injectată prin constructor. Acum poți schimba SqlFinder/XmlFinder/MockFinder fără a modifica Service.
</details>

---

## 2. Reverse Engineering 🔴 (~9 apariții — în fiecare an)
> *„Ce este reverse engineering? Tipuri. Metode. Exemple unde a fost folosit."*

<details><summary>📖 Răspuns</summary>

**Reverse engineering (RE)** = procesul de a **descoperi principiile** tehnologice ale unui dispozitiv/sistem prin **analiza structurii, funcției și operării** sale — pentru a face un produs nou care face același lucru, fără a copia originalul. E opusul **forward engineering** (de la cerințe → implementare).

**Tipuri (generale):**
- **RE1:** dispozitive **mecanice**;
- **RE2:** circuite integrate / **smart cards** (invaziv/distructiv — șlefuire strat cu strat, microscop electronic);
- **RE3:** aplicații **militare** (copierea tehnologiilor altor națiuni);
- **RE4:** **software**.

**La RE software — două situații:** (1) **cod sursă disponibil** (dar slab documentat); (2) **fără cod sursă**. (Black box testing are multe în comun cu RE.)

**Metode/unelte:** decompilatoare (jad, DJ Java Decompiler: `.class`→`.jad`), obținerea de diagrame din cod (ArgoUML), asistenți AI.

**Motive:** interoperabilitate, documentație pierdută, audit de securitate, inteligență competitivă, învățare.

**Exemple celebre:** US B-29 → URSS Tu-4; US AIM-9 Sidewinder → Soviet K-13; US F-22 → Chinese J-20.
</details>

---

## 3. Ce este IP, etapele, care e cea mai importantă etapă 🔴 (~7 apariții)
> *„Ce este ingineria programării. Care sunt etapele realizării unui proiect și care este cea mai importantă etapă."*

<details><summary>📖 Răspuns</summary>

**Ingineria Programării** = abordarea **sistematică** a dezvoltării, funcționării, întreținerii și retragerii programelor **de dimensiuni mari**, realizate de o **echipă** (IEEE, 1983). Prima definiție (NATO, 1968): folosirea de **principii inginerești solide** pentru a obține economic programe sigure și eficiente.

**Etapele dezvoltării:**
```
1. Ingineria cerințelor (Requirements)
2. Proiectarea arhitecturală (Architectural design)
3. Proiectarea detaliată (Detailed design)
4. Scrierea codului (Implementation)
5. Integrarea componentelor (Integration)
6. Validare (Validation) — „construim produsul corect?" (îndeplinește cerințele)
7. Verificare (Verification) — „construim corect produsul?" (corect tehnic)
8. Întreținere (Maintenance)
```

**Cea mai importantă etapă = Ingineria/Analiza cerințelor.**
> Din curs: atenția insuficientă acordată **analizei cerințelor** este **cea mai frecventă cauză** a proiectelor eșuate; ~50% din erori provin din cerințe incorecte/incomplete, iar erorile descoperite târziu costă cel mai mult de reparat.
</details>

---

## 4. Metodologia SCRUM (roluri, artefacte, evenimente) 🔴 (~7 apariții)
> *„Ce e SCRUM. Roluri. Artefacte. Evenimente. Caracteristici."*

<details><summary>📖 Răspuns</summary>

**Scrum** = metodologie **Agile** iterativă. Clientul e **parte din echipă**; livrări frecvente cu verificare/validare imediată; transparență; **discuții zilnice** (Ce ai făcut ieri? Ce faci azi? Ce te blochează?).

**ROLURI:**
- **Product Owner** — gestionează și prioritizează Product Backlog-ul, reprezintă clientul;
- **Scrum Master** — facilitează procesul, înlătură impedimente, apără echipa;
- **Development Team** — auto-organizat, livrează incrementul.

**ARTEFACTE:**
- **Product Backlog** — lista prioritizată a tuturor cerințelor;
- **Sprint Backlog** — sarcinile selectate pentru sprintul curent;
- **Increment** — produsul potențial livrabil la finalul sprintului.

**EVENIMENTE:**
- **Sprint** — iterație fixă (1-4 săptămâni);
- **Sprint Planning** — planificarea sprintului;
- **Daily Scrum** — întâlnirea zilnică;
- **Sprint Review** — demo-ul incrementului către stakeholderi;
- **Sprint Retrospective** — îmbunătățirea procesului.

⚠️ Confuzie frecventă: **artefacte** (backlog-uri, increment) vs **evenimente** (sprint, daily, review, retro).
</details>

---

## 5. Pachete și principiile OOP în crearea pachetelor (stabilitate) 🔴 (~7 apariții)
> *„Despre pachete și principiile OOP în crearea pachetelor. Principiul stabilității pachetelor."*

<details><summary>📖 Răspuns</summary>

**Pachet** = container logic pentru elemente înrudite (de obicei clase); definește un spațiu de nume; poate conține subpachete (structură arborescentă). Necesar când aplicația crește — clasa e prea „mică" ca unitate de organizare.

**Principii de COEZIUNE a pachetelor** (ce clase pun împreună):
- **Release Reuse Equivalency** — unitatea de reutilizare = unitatea de release (pachetul);
- **Common Closure** — clasele care se schimbă împreună stau în același pachet;
- **Common Reuse** — clasele reutilizate împreună stau în același pachet.

**Principii de CUPLAJ între pachete:**
- **Acyclic Dependencies** — dependențele trebuie să formeze un **DAG** (fără cicluri);
- **Stable Dependencies** — un pachet ar trebui să depindă doar de pachete **mai stabile** decât el;
- **Stable Abstractions** — pachetele **stabile** ar trebui să fie **abstracte**; cele instabile — concrete.

**Metrici:** Ca (afferent), Ce (efferent), **Instabilitate I = Ce/(Ca+Ce)** (I=0 max stabil, I=1 max instabil), Abstractness A = AC/TC.
</details>

---

## 6. Design Patterns (definiție, elemente, tipuri + fiecare pattern) 🔴 (colectiv, foarte frecvent)
> *„Design Patterns. Definiție. Elemente. Tipuri. Exemple. + detaliere pe un pattern."*

<details><summary>📖 Definiție, elemente, tipuri</summary>

**Design pattern** = o **soluție generală, reutilizabilă**, la o problemă care apare **frecvent** în design (NU cod concret/bibliotecă). Oferă un **vocabular comun**. Autorii **GoF (Gang of Four)** = Gamma, Helm, Johnson, Vlissides.

**4 elemente:** **Nume**, **Problemă**, **Soluție**, **Consecințe** (trade-off-uri).

**3 tipuri:**
- **Creaționale** (crearea obiectelor): Abstract Factory, Builder, Factory Method, **Prototype**, Singleton;
- **Structurale** (compunerea claselor/obiectelor): **Adapter**, Bridge, Composite, **Decorator**, Facade, **Flyweight**, **Proxy**;
- **Comportamentale** (algoritmi + responsabilități): **Chain of Responsibility**, Command, Interpreter, Iterator, **Mediator**, Memento, **Observer**, State, Strategy, Template Method, **Visitor**.
</details>

<details><summary>📖 Mediator (comportamental) — cel mai des cerut</summary>

**Intent:** definește un obiect care **încapsulează cum interacționează un set de obiecte** → reduce cuplajul (obiectele nu se mai cunosc direct între ele, vorbesc doar cu mediatorul).
**Exemple:** turnul de control (avioanele nu comunică între ele); bursa; aplicația de chat.
**Consecințe:** reduce cuplajul, reutilizare ușoară; **dezavantaj:** mediatorul poate deveni un „God Object".
</details>

<details><summary>📖 Adapter (structural)</summary>

**Intent:** **convertește interfața** unei clase în alta așteptată de client → permite claselor cu interfețe **incompatibile** să lucreze împreună. AKA Wrapper. *Ex: priză 15A ↔ ștecher 5A (un ConnectorAdapter traduce interfața).* Păstrează S și O din SOLID.
</details>

<details><summary>📖 Decorator (structural)</summary>

**Intent:** **atașează responsabilități suplimentare unui obiect dinamic** — alternativă flexibilă la subclasare. Înconjori obiectul în alt obiect (decoratorul) care adaugă comportament și delegă. *Ex: brad de Crăciun (BallDecorator, StarDecorator).* Dezavantaj: **ordinea** aplicării decoratorilor contează.
</details>

<details><summary>📖 Proxy (structural)</summary>

**Intent:** oferă un **substitut/placeholder** pentru alt obiect, pentru a **controla accesul** la el (lazy loading, restricționare, caching). *Ex: ATM ca proxy spre bancă; image proxy pentru imagini mari.* Păstrează O din SOLID.
</details>

<details><summary>📖 Flyweight (structural)</summary>

**Intent:** folosește **partajarea** pentru a suporta eficient un **număr mare de obiecte fine-grained** → economie de memorie (partajezi starea intrinsecă comună; starea extrinsecă e ținută de client). *Ex: FontData partajat între mii de caractere identice.*
</details>

<details><summary>📖 Observer (comportamental)</summary>

**Intent:** definește o dependență **one-to-many**: când un obiect (subject) își schimbă starea, toți **dependenții (observers) sunt notificați și actualizați automat** (publish-subscribe). *Ex: input de la tastatură ca eveniment; Java Swing.* Dezavantaj: ordinea notificării e aleatoare.
</details>

<details><summary>📖 Chain of Responsibility (comportamental)</summary>

**Intent:** **pasează cererea de-a lungul unui lanț** de handlere până când unul o tratează → nu specifici explicit receptorul. *Ex: sistem de help context-sensitive; tratarea excepțiilor.* Dezavantaj: unele cereri pot rămâne netratate.
</details>

<details><summary>📖 Visitor (comportamental)</summary>

**Intent:** reprezintă o **operație pe elementele unei structuri**; permite adăugarea unei **operații noi fără a modifica clasele** elementelor (double dispatch). Dezavantaj: la o clasă nouă în structură, toți vizitatorii se actualizează.
</details>

<details><summary>📖 Prototype (creațional)</summary>

**Intent:** creează obiecte noi prin **clonarea** unui obiect prototip existent (evită reinițializări, util când clasele se specifică la runtime). *Ex: editor muzical; diviziune celulară; Cloneable.*
</details>

---

## 7. Principiile GRASP (coeziune, cuplaj) 🟠 (~5 apariții)
> *„Ce reprezintă GRASP. Detalii despre coeziune / cuplaj."*

<details><summary>📖 Răspuns</summary>

**GRASP** = General Responsibility Assignment Software Patterns (Craig Larman) — principii pentru **alocarea responsabilităților** claselor/obiectelor. Ex: Information Expert, Creator, Controller, Low Coupling, High Cohesion, Polymorphism.

**Coeziunea (cohesion):** măsură a cât de **strâns legate/focalizate** sunt responsabilitățile unei clase. **Coeziune mare = bine** (clasa face puține lucruri, strâns legate → ușor de înțeles/refolosit/menținut). Coeziune slabă = clasa face multe lucruri nerelaționate → greu de înțeles/menținut.

**Cuplajul (coupling):** măsură a gradului de **dependență** a unei clase de alte clase. **Cuplaj mic = bine** (o clasă cu cuplaj redus nu depinde de multe altele → ușor de înțeles în izolare, de refolosit, schimbările nu se propagă). Cuplaj mare = schimbări în lanț, clase greu de refolosit.

Obiectivul: **coeziune mare + cuplaj mic** → design modular.
</details>

---

## 8. Quality Assurance: Manual Testing vs Automation Testing 🟠 (~5 apariții)
> *„QA. Testare manuală vs testare automată."*

<details><summary>📖 Răspuns</summary>

**QA (Quality Assurance)** = procese planificate care oferă încredere în adecvarea produsului („fit for purpose", „right first time"). QA = **prevenție** (procese); testarea = parte din **Quality Control** (corecție).

| | **Manuală** | **Automată** |
|--|-------------|--------------|
| Definiție | testerul joacă rolul de end-user, parcurge manual cazurile | program care controlează execuția testelor, compară rezultate |
| Avantaje | bună pt **UI/aspect vizual** (text, layout, ordinea elementelor) | **ieftin de repetat**, rapid, predictibil/planificabil |
| Dezavantaje | lentă, greu de repetat, costisitoare | scrierea scenariilor e dificilă, necesită cunoștințe tehnice |

Manuală: plan de test → cazuri detaliate → testeri urmează pașii → raport (folosit de manageri pt decizia de release). Automată: automatizează un proces manual existent (GUI testing, code-driven testing).
</details>

---

## 9. Metodologia Agile 🟠 (~5 apariții)
> *„Metodologia Agile. Ce este? Caracteristici."*

<details><summary>📖 Răspuns</summary>

**Agile** = abordare **iterativă**, centrată pe colaborare și adaptare. Caracteristici:
- **satisfacerea rapidă a clientului** prin livrare continuă de software util (săptămânal);
- progresul se măsoară prin partea **funcțională** a proiectului;
- chiar și **modificările târzii** în cerințe sunt binevenite;
- **cooperare strânsă** client ↔ programatori;
- **discuții face-to-face** = cea mai bună formă de comunicare;
- adaptare continuă;
- evidențierea și rezolvarea problemelor (nu ascunderea lor).

Scrum, XP, Lean, Kanban sunt metodologii Agile.
</details>

---

## 10. Importanța / scopul Ingineriei Programării (statistici, exemple) 🟠 (~5 apariții)
> *„Importanța IP-ului, statistici, exemple (cursul 1). Care e scopul IP?"*

<details><summary>📖 Răspuns</summary>

**Scopul IP:** obținerea de programe **sigure, eficiente**, dezvoltate **sistematic** de echipe, în timp/buget rezonabile.

**De ce e importantă:** din ce în ce mai multe sisteme sunt controlate de software (trafic aerian, bănci, telefonie, medicină); economiile statelor depind de software; proiectele mari sunt printre cele mai complicate produse.

**Statistici:** majoritatea proiectelor mari **eșuează parțial/total** (studii: succes ~17-33%; multe implementări ERP nereușite, peste buget cu ~25%). Costul **întreținerii** > costul realizării; costul software > costul hardware.

**Erori celebre:** **Ariane 5** (explozie, 500M$), **Mars Climate Orbiter** (confuzie English/metric), IBM OS360 (~1000 greșeli/relansare), Therac-25 (decese).
</details>

---

## 11. Modele de dezvoltare software (cascadă, XP, spirală) 🟠 (~4 apariții)
> *„Modele de dezvoltare. Detalii despre cascadă, extreme programming sau spirală."*

<details><summary>📖 Răspuns</summary>

**Cascadă (Waterfall)** — etape secvențiale (Cerințe → Design → Implementare → Testare → Acceptare).
- **+** simplu, ușor de administrat, fiecare pas dă un produs bine definit;
- **−** erorile se **propagă** între pași; clientul vede produsul **abia la final**; rigid.

**Spirală** — iterativ, cu 4 activități/ciclu: pregătire → **gestiunea riscului** → dezvoltare → planificare.
- **+** păstrează avantajele cascadei + ia în calcul **riscul** (concurent, plecarea unui arhitect, schimbarea cerințelor).

**Extreme Programming (XP)** — model lightweight, Agile.
- colaborare > ierarhii/documentații; reprezentant al clientului mereu disponibil; **cod simplu, test întâi (TDD)**; **programare în perechi**; integrare continuă; 40h/săptămână;
- **+** productivitate; **−** documentație insuficientă, doar dezvoltatori senior.

> Alte modele: Ad-hoc, Prototipizare, RUP (Inception/Elaboration/Construction/Transition), V-Model, TDD.
</details>

---

## 12. Metodologia Kanban 🟠 (~4 apariții)
> *„Metodologia Kanban. Definiție, caracteristici."*

<details><summary>📖 Răspuns</summary>

**Kanban** (Taiichi Ohno, Toyota, 1953; pt software: Ladas/Anderson) = sistem **vizual** de management al fluxului de lucru, cu accent pe livrare **just-in-time**, fără a supraîncărca echipa.

**Caracteristici/principii:**
- **vizualizarea** fluxului de lucru (board cu coloane) — aspect esențial pentru a înțelege munca și workflow-ul;
- **start with existing process** — nu prescrie roluri/pași specifici;
- schimbare **incrementală, evolutivă** (continuous improvement);
- respectă rolurile/responsabilitățile actuale;
- leadership la toate nivelurile;
- limitarea muncii în curs (WIP), livrare JIT, „pull" din cerere.

Spre deosebire de Scrum: **fără iterații fixe (sprinturi) și fără roluri prescrise**.
</details>

---

## 13. Quality Assurance: Testare nefuncțională 🟠 (~4 apariții)
> *„QA. Testare nefuncțională."*

<details><summary>📖 Răspuns</summary>

**Testarea nefuncțională** verifică aspecte de **calitate** (nu funcționalitatea propriu-zisă) — că software-ul se comportă bine chiar la input invalid/neașteptat.

**Tipuri:**
- **Performance / Load testing** — poate gestiona multe date/utilizatori? (load = nr. concurent așteptat; **stress** = „spargi" aplicația; **endurance** = încărcare susținută/memory leaks; **spike** = vârf brusc);
- **Usability testing** — interfața e ușor de folosit/înțeles?
- **Security testing** — protejează datele confidențiale (Confidentiality, Integrity, Authentication, Authorization, Availability, Non-repudiation);
- **Internationalization (i18n) & Localization (l10n)** — adaptare la limbi/regiuni.
</details>

---

## 14. Unit Testing (de ce? exemplu) 🟠 (~3 apariții)
> *„Ce este Unit testing și un exemplu. De ce?"*

<details><summary>📖 Răspuns</summary>

**Unit testing** = testarea unei **unități mici** (funcție, clasă, funcționalitate). Rulat de **programatori**, predefinit, cu rezultate **documentate**, folosind **simulatoare** de Input/Output.

**De ce:** găsești erorile devreme (ieftin), permite refactorizare cu încredere, documentează comportamentul, face dezvoltarea predictibilă.

**Exemplu (JUnit-style):**
```java
@Test
void testAdunare() {
    Calculator c = new Calculator();
    assertEquals(5, c.aduna(2, 3));   // verifică 2+3 == 5
}
```
Face parte din **white box testing**; se măsoară cu **code coverage** (% cod executat de teste).
</details>

---

## 15. Use case (definiție, elemente, exemple) 🟠 (~3 apariții)
> *„Diagrame Use-Case. Elemente componente. Definiție. Exemple."*

<details><summary>📖 Răspuns</summary>

**Use case** = descrierea unei mulțimi de **secvențe de acțiuni** pe care un sistem le execută când interacționează cu **actori**, ducând la un **rezultat observabil**. Precizează **CE** face sistemul, nu CUM.

**Diagrama Use Case** (comportamentală) captează cerințele și delimitează **granițele** sistemului. **Elemente:**
- **Use case-uri** = funcționalități (oval cu nume = frază verbală);
- **Actori** = entități externe (utilizator uman, sistem software/hardware);
- **Relații:**
  - **asociere** (actor–use case): comunicare;
  - **generalizare** (actor–actor, uc–uc): ierarhie/caz particular;
  - **dependență** (uc–uc): `<<include>>` (folosește obligatoriu comportamentul altuia) / `<<extend>>` (extinde opțional).

*Ex: „Autentificare" `<<include>>` „Verificare parolă".*
</details>

---

## 16. Diagrame de clase 🟡 (2 apariții)
> *„Ce este o diagramă de clase, tipuri de relații. Exemplu (de desenat pe tablă)."*

<details><summary>📖 Răspuns</summary>

**Diagrama de clase** (structurală) modelează **vocabularul** sistemului. Conține clase/interfețe, obiecte, relații. O clasă are 3 compartimente: **Nume / Atribute / Metode**.

**Tipuri de relații:**
- **Generalizare** (moștenire, „is-a") — săgeată cu triunghi gol ▷;
- **Asociere** — linie simplă (cu nume + **multiplicitate**: 1, 0..*, 1..*);
- **Agregare** (parte-întreg slab) — romb gol ◇;
- **Dependență** — săgeată punctată ┄>.

*Exemplu de desenat:*
```
┌──────────┐ 1    0..* ┌───────────┐ 0..*   0..* ┌─────────┐
│ Profesor │───────────│ Disciplină│──────────────│ Student │
└──────────┘   predă    └───────────┘   urmează    └─────────┘
```
Profesor 1—0..* Disciplină (asociere „predă"); Disciplină 0..*—0..* Student (asociere „urmează").
</details>

---

## 17. Modelare (de ce? limbaje de modelare) 🟡 (2 apariții)
> *„Modelare, de ce e folositoare. Exemple de limbaje de modelare."*

<details><summary>📖 Răspuns</summary>

**Model** = simplificarea realității; planul detaliat (blueprint) al unui sistem.

**De ce modelăm:** pentru a **înțelege** mai bine ce avem de făcut; a ne **concentra** pe un aspect la un moment dat; a **vizualiza** structura/comportamentul; a oferi un **șablon** de construcție; a **documenta** deciziile. (Un singur model nu e suficient.)

**Limbaje de modelare:**
- **grafice:** UML, flowchart, rețele Petri, ORM, EXPRESS, arbori comportamentali, modelarea proceselor de business;
- **specifice:** AML (algebric), DSL, VRML.

**UML** (Unified Modeling Language) = succesorul Booch + OMT + OOSE; diagrame structurale (clase, componente, deployment), comportamentale (use case, activitate, stare), de interacțiune (secvență, comunicare).
</details>

---

## 18. Subiecte apărute o dată (mai rare) ⚪

<details><summary>📖 Ce este code coverage și cum se calculează</summary>

**Code coverage** = tehnică de **white box testing** care măsoară **cât din cod** e executat de teste. Formula: `coverage = (elemente executate / total elemente) × 100`. Tipuri: **statement** (instrucțiuni), **branch** (ramuri if/else), **path** (căi), **function**. Ex: 80 linii executate din 100 → statement coverage 80%. Unelte: JaCoCo, EclEmma.
</details>

<details><summary>📖 Când e necesară / când NU aplicarea IP (2022-2024)</summary>

IP (metodologii/principii) e necesară la proiecte **mari, în echipă** (bancă, trafic aerian, ERP). NU e neapărat necesară la un proiect **mic, personal** (ex. un scraper de uz propriu) — nu te apuci de SCRUM etc., ai **pierde timp aiurea**. „Principii" = tot ce ține de IP: DRY, SOLID, SCRUM, cascadă etc.
</details>

<details><summary>📖 Ingineria cerințelor: definiție, cine, de ce, pași (2021)</summary>

**Ingineria cerințelor** = procesul de înțelegere a nevoilor și așteptărilor clientului. **Cine:** Project/Program Manager sau Business Analyst. **De ce:** atenția insuficientă la cerințe = cea mai frecventă cauză de eșec. **Pași:** stabilirea limitelor aplicației → găsirea clientului → identificarea cerințelor → analiza → specificarea (document obligatoriu) → gestionarea. **Tipuri de cerințe:** utilizator, funcționale, de performanță, constrângeri.
</details>

<details><summary>📖 Testare software: definiție, dilema testării (2019)</summary>

**Testarea** = procesul de a evalua un sistem (manual/automat) pentru a verifica conformitatea cu cerințele sau a identifica diferențe. Nu e o fază — e integrată în toate etapele. **Dilema testării:** nu știi niciodată **când se finalizează** testarea (când te oprești?) — te oprești când nr. de erori găsite scade sub un prag / nu mai apar defecte critice / s-a terminat timpul.
</details>

---

> ⭐ **Strategie rapidă:** IP e materia cu cele mai multe subiecte, dar și cea mai „învățabilă". Prioritate maximă: **SOLID** (toate 5), **Reverse Engineering**, **ce e IP + etape + cea mai importantă etapă**, **Scrum** (roluri/artefacte/evenimente), **Pachete/stabilitate**, **Design Patterns** (măcar Mediator, Adapter, Prototype). Astea 6 acoperă aproape sigur cel puțin o întrebare.
