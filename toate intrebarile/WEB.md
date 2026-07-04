# Tehnologii Web — Toate întrebările de licență (2019–2025), ordonate după frecvență
> Întrebările reale din ultimii 6 ani (fără 2020), grupate pe teme și **ordonate de la cele mai frecvente la cele mai rare**. Apasă pe **„📖 Răspuns"**.
> Legendă: 🔴 foarte frecvent · 🟠 frecvent · 🟡 de 2 ori · ⚪ o dată.

---

## 1. Interdependența dintre SOA și MVC 🔴 (~7 apariții, „+1+1+1" într-un an)
> *„Diferența dintre SOA și MVC, discuție, exemplificare și o interdependență între ele."*

<details><summary>📖 Răspuns</summary>

**MVC (Model-View-Controller)** = arhitectură **internă** a unei aplicații:
- **Controller** — preia cererile HTTP (din acțiunile utilizatorului), apelează un model, alege un view;
- **Model** — datele + regulile (constrângerile);
- **View** — modul de prezentare a datelor.

**SOA (Service-Oriented Architecture)** = arhitectură de **sistem**: funcționalitatea e împărțită în **servicii independente**, slab cuplate, invocabile prin rețea, care partajează un contract formal.

**Diferența:** MVC organizează **o aplicație**; SOA descompune **sistemul** în servicii.

**Interdependența:**
- Abordarea clasică: o aplicație Web expune funcționalități clienților printr-o arhitectură **MV\***, pe baza datelor stocate.
- Migrarea spre SOA: fiecare funcționalitate devine un **serviciu independent** (care poate folosi alte servicii interne + stocări proprii).
- În SOA, o aplicație MVC poate **FI** un serviciu, iar **Controller-ul (sau Modelul) poate INVOCA servicii** externe (proprii/terțe), sincron sau asincron.
> Deci: MVC = micro-nivel (o aplicație), SOA = macro-nivel (sistemul); se completează.
</details>

---

## 2. Arborele DOM: bine-formatat sau valid? 🔴 (~6 apariții)
> *„Poate fi creat arborele DOM doar dacă documentul XML/HTML e bine-formatat sau exclusiv când e valid? Argumentați. (Hint: niciuna nu e strict necesară pentru validare)"*

<details><summary>📖 Răspuns</summary>

**Răspuns: e suficient ca documentul să fie BINE-FORMATAT (well-formed); validarea NU este necesară.**

**Argumentare:**
- Pentru a construi arborele **DOM**, documentul trebuie doar **bine-formatat** (well-formed): marcaje corect **închise și imbricate**, un singur element rădăcină, sintaxă XML corectă. Un parser fără validare (Expat, libxml) construiește arborele DOM atâta timp cât documentul e well-formed.
- **Validarea** (conformitatea cu o **DTD** sau **XML Schema**) e o etapă **separată și opțională** — verifică structura/semantica față de o gramatică, NU e necesară pentru a construi arborele.
- **La HTML în browser:** browserul îți creează arborele DOM **indiferent** de cum e formatat documentul sau dacă există HTML invalid — pur și simplu **ignoră/repară** erorile (parsere „lenient").

**Concluzie:** well-formed = suficient pentru DOM; valid = NU e necesar. (Reciproc: dacă documentul NU e nici măcar bine-formatat, arborele XML strict nu se poate construi corect — dar browserul HTML tot va încerca să-l repare.)

**Legătura DOM ↔ transfer asincron (Ajax):** browserul construiește DOM-ul din HTML; **DOM-ul poate fi modificat ulterior** cu JavaScript (inclusiv cu date primite **asincron prin Ajax/Fetch**), **fără reîncărcarea** paginii.
</details>

---

## 3. Server web, proxy web, gateway web, server de aplicații web 🟠 (~5 apariții)
> *„Principalele asemănări și deosebiri între server web, proxy web, gateway web, server de aplicații web. Ce este un server web, ce implică?"*

<details><summary>📖 Răspuns</summary>

**Server web** = program (daemon) care **îndeplinește cereri HTTP** și servește resurse (reprezentări) clienților. E **stateless** (fiecare cerere e independentă). Ex: Apache, NGINX, IIS. Implementare: pre-forked/threaded sau asincron (non-blocking, single-threaded — NGINX, Node.js).

**Server de aplicații web** = **generează dinamic** reprezentările cerute și rulează **logica aplicației**; impune adesea o arhitectură (MVC). Ex: Zend/PHP, ASP.NET, Node.js, Spring.

**Proxy web** = intermediar **lângă client** (forward) sau lângă servere (reverse); are rol și de server, și de client, acționând în numele lor.

**Gateway web** = intermediar care **ascunde serverul țintă** (clientul nu știe de el); poate face **load balancing**, **caching**, traducere de protocol (HTTPS→HTTP).

**Asemănări:** toate procesează/dirijează trafic HTTP între client și resurse; pot face caching; sunt componente ale arhitecturii web.
**Deosebiri:** server web = servește/rutează; app server = generează dinamic + logică; proxy = lângă client; gateway = ascunde serverul țintă.
</details>

---

## 4. Servicii Web vs Microservicii web 🟠 (~3 apariții)
> *„Servicii Web vs Microservicii web. Asemănări și deosebiri."*

<details><summary>📖 Răspuns</summary>

**Serviciu Web** = software folosit **de la distanță** de alte aplicații, oferind o funcționalitate prin **API** (URI + HTTP + JSON/XML); implementarea e **black box**. În SOA: tinde spre **share-as-much** (contract formal, eventual middleware).

**Microserviciu** = implementează **o singură funcționalitate**, disponibilă ca **un proces separat**; construit să fie **înlocuit, nu reutilizat**. Tinde spre **share-as-little**.

| Aspect | Servicii Web (SOA) | Microservicii |
|--------|--------------------|--------------| 
| Partajare | share-as-much | share-as-little |
| Cuplare | contract, eventual middleware | fără middleware (decoupling) |
| Granularitate | pot fi mari | foarte mici, 1 proces |
| Reutilizare | reutilizabile | construite să fie înlocuite |
| Scalare | replicare servicii | replicare individuală |
| Comunicare | sincron/asincron, orchestrare | asincronă (point-to-point / publish-subscribe) |

**Asemănări:** ambele = software ca servicii, slab cuplate, interoperabile, invocate prin rețea (HTTP), deployment distribuit.
</details>

---

## 5. Principiul DRY în dezvoltarea web 🟠 (~3 apariții)
> *„Context unde apare principiul DRY în aplicații web. (Cum îl aplici în CSS cu @media?)"*

<details><summary>📖 Răspuns</summary>

**DRY (Don't Repeat Yourself):** „fiecare cunoaștere trebuie să aibă o reprezentare **unică**, neambiguă în sistem" — eviți duplicarea.

**Contexte în aplicații web:**
- **Template engines / componente** — un layout/partial (header, footer) reutilizat, în loc de a copia HTML pe fiecare pagină;
- **CSS** — clase reutilizabile, variabile CSS/preprocesoare (Sass);
- **MVC** — logica de business o singură dată în Model, nu duplicată;
- **funcții/module/biblioteci** — cod reutilizabil;
- **API-uri** — o singură sursă de adevăr (un endpoint) consumată de mai mulți clienți.

**Cum aplici DRY în CSS cu `@media`** (întrebare directă): **generalizezi** stilul comun (o singură dată, mobile-first), apoi **suprascrii** doar diferențele în media queries:
```css
.card { padding: 1rem; width: 100%; }          /* stil comun, o singură dată */
@media (min-width: 768px) { .card { width: 50%; } }   /* doar diferența */
```
Beneficiu: schimbi într-un singur loc, mai puține bug-uri din inconsistență.
</details>

---

## 6. Folosirea mai multor limbaje în aplicații web mari 🟠 (~3 apariții)
> *„Folosirea mai multor limbaje de programare în aplicații web mari. De ce? Avantaje/dezavantaje."*

<details><summary>📖 Răspuns</summary>

Aplicațiile mari folosesc **mai multe limbaje**, fiecare pentru ce e mai bun. *Ex: Flickr — PHP (logică), Perl (validare), Java (stocare); Netflix — Python, Java, Node.js, React.*

**De ce (avantaje):**
- **„Right tool for the job"** — fiecare limbaj pentru ce excelează (Python pt ML/date, JS pt front-end, Java/Go pt backend scalabil);
- **microservicii** — fiecare serviciu poate fi în alt limbaj, independent;
- reutilizarea bibliotecilor/ecosistemelor specifice;
- performanță (limbaj rapid pt componente critice);
- folosirea expertizei echipelor.

**Dezavantaje:**
- **complexitate crescută** (build, deployment, tooling pt fiecare limbaj);
- mentenanță mai grea, integrare între componente;
- echipa trebuie să cunoască mai multe limbaje;
- debugging cross-limbaj dificil, risc de duplicare (anti-DRY);
- overhead de comunicare între servicii (serializare, API-uri).
</details>

---

## 7. Cookie-uri și sesiuni din punct de vedere al securității 🟠 (~3 apariții)
> *„Ce înseamnă pentru securitatea aplicației web folosirea de cookie-uri și sesiuni. Exemple."*

<details><summary>📖 Răspuns</summary>

**Cookie** = date (`name=value`, max 4KB) stocate la client, trimise înapoi serverului la fiecare cerere (`Set-Cookie` în răspuns, `Cookie` în cerere).
**Sesiune** = fiecare vizitator primește un **SID (Session ID)**, stocat de obicei într-un cookie; datele de sesiune stau pe **server** (în cookie e doar SID-ul).

**Riscuri de securitate:**
- cookie-uri **3rd party** → urmărire/confidențialitate (privacy);
- cookie accesibil din JS → poate fi **furat prin XSS**;
- cookie pe canal necriptat → interceptat;
- SID furat → **deturnarea sesiunii** (session hijacking).

**Măsuri (atributele cookie-ului):**
- **`secure`** → cookie-ul circulă **doar pe HTTPS** (canal criptat);
- **`httpOnly`** → cookie-ul **NU poate fi citit de JS** → protecție contra XSS;
- **`sameSite`** (Strict/Lax/None) → controlează cererile cross-site → protecție contra **CSRF**;
- stocarea datelor sensibile pe **server** (nu în cookie).

*Exemplu: la login, `Set-Cookie: sid=...; secure; HttpOnly` — cookie-ul de sesiune e doar pe HTTPS și inaccesibil din JS.*
</details>

---

## 8. Autentificare și autorizare în API-uri web 🟡 (2 apariții)
> *„Autentificare și autorizare. Ce înseamnă, diferența, cum se folosesc în API-uri (exemplu, JWT)."*

<details><summary>📖 Răspuns</summary>

**Autentificare** = a dovedi **CINE ești** (verificarea identității — login: nume+parolă, eventual 2FA).
**Autorizare** = a stabili **CE AI VOIE** să faci/accesezi (permisiuni). Întâi te autentifici, apoi ești autorizat.

**În API-uri (pașii):**
1. înregistrezi aplicația → primești o **cheie de acces** (API key);
2. aplicația se **autentifică** (cu cheia) pentru a fi **autorizată**;
3. autorizarea cu **consimțământul utilizatorului** (ex. **OAuth**);
4. sesiunea se menține prin **token-uri** (auth tokens).

**Cu JWT (JSON Web Token):** după autentificare, serverul emite un **JWT semnat** (obiect JSON cu identitatea/permisiunile). La fiecare cerere, clientul trimite token-ul; serverul îl **verifică** (semnătură HMAC / chei publice-private) și autorizează accesul — fără a păstra stare pe server (stateless).
*Ex: OAuth 2.0 + OpenID Connect folosesc JWT ca ID token.*
</details>

---

## 9. (Dez)avantaje MVC în diverse aplicații web 🟡 (2 apariții)
> *„(Dez)avantaje MVC în diverse tipuri de aplicații web (statice, e-commerce etc.)."*

<details><summary>📖 Răspuns</summary>

**MVC** separă aplicația în Model (date+reguli), View (prezentare), Controller (preia cereri, coordonează).

**Avantaje:**
- **separarea responsabilităților** (date / logică / prezentare) — separation of concerns;
- mentenanță și testare mai ușoare;
- **mai multe view-uri** pentru același model;
- reutilizarea modelelor/view-urilor; dezvoltare în echipă.

**Dezavantaje:**
- **complexitate suplimentară** pentru aplicații mici/statice (overhead nejustificat);
- curbă de învățare; structură impusă de framework;
- risc de controllere „grase"; indirectare (mult cod de legătură).

**Pe tipuri de aplicații:**
- **aplicație statică simplă** → MVC e **overkill** (nu ai logică/date dinamice);
- **e-commerce / aplicație complexă** → MVC **strălucește** (multe modele, view-uri, logică de business).
</details>

---

## 10. GET vs POST 🟡 (2 apariții)
> *„Post vs Get. Folosirea GET în defavoarea POST — avantaje/dezavantaje pentru securitate și navigare."*

<details><summary>📖 Răspuns</summary>

| Aspect | **GET** | **POST** |
|--------|---------|----------|
| Scop | obține o reprezentare | trimite date / creează resursă |
| Datele | în **URL** (query string) — vizibile | în **body** — nu apar în URL |
| Safe/Idempotent | **da/da** | nu/nu |
| Bookmark/cache | **da** | nu |
| Date mari/sensibile | **NU** | da |

**Securitate:** la **GET** datele se văd în **URL** → ajung în istoric, bookmark-uri, loguri de server, header Referer → **NU pui parole/date sensibile în GET**. La **POST** datele sunt în body (dar tot trebuie HTTPS pentru criptare).

**Navigare:** **GET** e potrivit pentru navigare — URL-uri **bookmark-abile, partajabile, cache-abile**, butonul „back" merge fără efecte. Cu **POST** re-trimiterea cere confirmare („vrei să retrimiți formularul?") — nu navighezi cu POST. Resursele se **obțin** cu GET.
</details>

---

## 11. Cascada în CSS 🟡 (~2 apariții)
> *„Cascade în CSS + exemple (!important)."*

<details><summary>📖 Răspuns</summary>

**CSS = Cascading Style Sheets.** „Cascading" = mecanismul prin care se decide **care regulă câștigă** când mai multe reguli vizează același element.

**Algoritmul cascadei (ordinea de departajare):**
1. **origine + importanță** — user-agent(browser) < utilizator < **autor** < `!important` autor < `!important` utilizator;
2. **specificitatea** selectorului — inline > `#id` > `.class`/`[attr]`/`:pseudo` > element > `*`;
3. **ordinea în sursă** — la specificitate egală, câștigă **ultima declarată**.

Apoi: dacă nu există declarație → **moștenire** de la părinte → valoarea inițială.

**`!important`** ridică o declarație peste cele normale (inversează ordinea importanței):
```css
p { color: blue; }
p { color: red !important; }   /* câștigă red, deși ambele au aceeași specificitate */
```
A se folosi cu grijă — e greu de suprascris ulterior.
> *Cele 3 moduri de aplicare CSS: inline (`style=`), intern (`<style>`), extern (`<link>`).*
</details>

---

## 12. Design web responsive 🟡 (2 apariții)
> *„Definește «design web responsive». Cum și când se pune în aplicare."*

<details><summary>📖 Răspuns</summary>

**Responsive Web Design (RWD)** = abordare prin care **aceeași** pagină/aplicație se **adaptează automat** la dispozitiv și context (dimensiune ecran, orientare) — arată și funcționează bine pe mobil, tabletă, desktop, smart TV. (Opusul: versiuni separate `m.site.com`.)

**Cum se pune în aplicare (3 ingrediente):**
1. **layout fluid** — unități relative (`%, em, rem, vw, fr`), Flexbox/Grid;
2. **imagini/media flexibile** — `img { max-width: 100% }`, `srcset`/`<picture>`;
3. **media queries** — reguli CSS condiționate de dispozitiv:
```css
@media (min-width: 768px) { .container { width: 750px; } }
```
+ **`<meta name="viewport" content="width=device-width, initial-scale=1">`** (obligatoriu pe mobil). Abordare **„mobile-first"** (pornești de la ecran mic, adaugi reguli pentru ecrane mari).

**Când:** la orice aplicație cu utilizatori pe **dispozitive eterogene** (practic mereu azi); esențial pentru **PWA** și accesibilitate.
</details>

---

## 13. Arhitectura pe straturi (N-tier / layered) 🟡 (2 apariții)
> *„(Dez)avantajele arhitecturii pe straturi în dezvoltarea aplicațiilor Web (N-tier, layered systems)."*

<details><summary>📖 Răspuns</summary>

Principiu: **separation of concerns** — separi prezentarea (interfață), procesarea (business logic) și datele (persistență). Aplicație pe **3 niveluri**: Client (prezentare) → Application Server (logică) → Storage (date).

Principiu de design: **layers of isolation** — fiecare strat oferă servicii doar celor vecine; un strat nu „vede" un strat ne-vecin.

**Avantaje:**
- **izolarea modificărilor** (schimbi un strat fără a rupe altul);
- reutilizare și mentenanță mai ușoară;
- testare independentă pe straturi;
- securitate (straturi de izolare: firewall, gateway);
- poate încapsula sisteme legacy (black box).

**Dezavantaje:**
- **overhead de performanță** (cererea trece prin mai multe straturi);
- complexitate suplimentară;
- risc de rigiditate/„spaghetti" dacă straturile sunt prea cuplate.
</details>

---

## 14. Diferența dintre JSON și HTML 🟡 (2 apariții)
> *„Diferența (și asemănări) dintre HTML și JSON."*

<details><summary>📖 Răspuns</summary>

Ambele sunt **formate text** folosite pe web, dar cu scopuri **diferite**:

| Aspect | **HTML** | **JSON** |
|--------|----------|----------|
| Scop | **prezentarea** conținutului către om (în browser) | **schimb/modelare de date** procesabile de software |
| Tip | limbaj de **marcare** (markup) | format de **serializare** a datelor |
| MIME | `text/html` | `application/json` |
| Sintaxă | elemente/taguri `<p>`, atribute | perechi `cheie: valoare`, obiecte `{}`, tablouri `[]` |
| Consumator | utilizatori umani (afișare) | aplicații/servicii (procesare) |

**Asemănări:** ambele text, ambele reprezintă structuri ierarhice, ambele folosite pe web.
> În REST/Ajax, **JSON** e formatul preferat pentru **transferul de date**; **HTML** e reprezentarea pentru **afișare**. Aceeași resursă poate avea ambele reprezentări.
> (Notă: comparația mai naturală ar fi **XML vs JSON** — ambele formate de date; XML e mai verbose, JSON mai compact.)
</details>

---

## 15. Expresii regulate pentru validarea formularelor 🟡 (2 apariții)
> *„(Dez)avantaje pentru folosirea expresiilor regulate pentru validarea datelor din câmpurile unui form."*

<details><summary>📖 Răspuns</summary>

**Avantaje:**
- **concise și puternice** — validezi formate complexe (email, telefon, CNP) într-o linie;
- **reutilizabile, standardizate** (suport în orice limbaj);
- validare rapidă pe **client** (feedback instant) ȘI pe **server**;
- atributul HTML5 `pattern="..."` direct pe `<input>`.

**Dezavantaje:**
- **greu de citit/întreținut** („write-only code"), ușor de greșit;
- pot fi prea permisive/stricte (regex pt email „corect" e foarte complex);
- risc de **ReDoS** (Regular expression Denial of Service) la regex prost scrise;
- validarea pe **client NU e suficientă** — trebuie **repetată pe server** (clientul poate fi ocolit);
- nu validează logica de business (ex. „data nașterii în trecut").
</details>

---

## 16. Rolul codurilor HTTP în randarea paginii 🟡 (2 apariții)
> *„Rolul codurilor HTTP + ce rol au în randarea HTML-ului (Status Code)."*

<details><summary>📖 Răspuns</summary>

Codurile de stare ghidează ce face **browserul** cu răspunsul:
- **1xx Informational** (100 Continue, 101 Switching) — rar relevante la randare;
- **2xx Success** (200 OK) — browserul **primește reprezentarea și o RANDEAZĂ**;
- **3xx Redirection** (301/302/303) — browserul face **REDIRECT automat** la altă adresă (304 Not Modified → folosește versiunea din **cache**);
- **4xx Client Error** (404, 403) — browserul **randează o pagină de eroare** (ex. „404 Not Found" generată automat când fișierul nu există);
- **5xx Server Error** (500, 503) — pagină de eroare server.

*Exemplu: accesezi un URL inexistent → server-ul întoarce **404** → browserul randează automat „Not Found"; la **301/302** navighează automat la noua adresă.*
</details>

---

## 17. Template engines (sisteme de redare pe bază de machete) 🟡 (2 apariții)
> *„Cum funcționează și în ce context este utilizat un sistem de redare a conținutului pe baza machetelor (template engine)?"*

<details><summary>📖 Răspuns</summary>

**Template engine** = combină o **machetă (template)** cu **date persistente** (ex. din BD), folosind un **procesor** care **substituie** variabilele → generează documente HTML (sau alt format).

**Cum funcționează:**
```
1. Definești un TEMPLATE cu „locuri" pentru variabile:
   <h1>[@username]</h1>  <img src="[@photoURL]" />
2. Programul de pe server completează variabilele cu valori reale (din BD):
   $profile->set('username', 'Tux');
3. Procesorul SUBSTITUIE [@variabilă] cu valoarea → HTML-ul final trimis clientului.
```

**Context de folosire:** separă **prezentarea** (HTML) de **logică** și **date** (= View-ul din MVC, separation of concerns); reutilizare; designerii lucrează pe șabloane, programatorii pe logică.

**Exemple:** pe server — Smarty, Blade (PHP), Twig, Mustache, Pug (Node), Razor (.NET); pe client — Handlebars, Mustache.js.
</details>

---

## 18. Conținut static vs dinamic 🟡 (2 apariții)
> *„Avantajele și dezavantajele generării de conținut static în comparație cu generarea dinamică."*

<details><summary>📖 Răspuns</summary>

**Static** = fișiere fixe (HTML/CSS/imagini) servite direct. **Dinamic** = generat la cerere de server (din BD/logică, prin CGI/server de aplicații/framework).

| | **Static** | **Dinamic** |
|--|-----------|-------------|
| Performanță | **rapid** (fără procesare), ușor de pus în CDN/cache | mai lent (procesare per request) |
| Scalabilitate | foarte bună | necesită resurse server |
| Securitate | suprafață de atac mică | mai expus (cod server, BD) |
| Personalizare | **nu** (același conținut pt toți) | **da** (per utilizator/context) |
| Mentenanță | simplă; greu la conținut mult | flexibil; surse heterogene |
| Exemplu | prezentare, blog static (SSG/JAMstack) | magazin online, dashboard, feed |

> JAMstack/SSG pre-randează markup-ul → conținut static în CDN (performanță).
</details>

---

## 19. Legătura hipermedia – URI – REST (– teoria automatelor) 🟡 (2 apariții)
> *„Conexiunile dintre hipermedia, URI, REST și teoria automatelor."*

<details><summary>📖 Răspuns</summary>

O aplicație Web **REST** se comportă ca un **automat finit (nedeterminist)**:
- **stările** = reprezentările resurselor;
- **tranzițiile** = transferuri de date prin metodele protocolului (HTTP);
- declanșatorul tranziției = accesarea altei resurse printr-un **URI**.

```
resource1 ──GET──► repr.2 ──POST──► repr.3 ──GET──► repr.4
(fiecare stare = o reprezentare; fiecare săgeată = o metodă HTTP)
```

- **URI** — fiecare resursă (stare) e adresată printr-un URI;
- **Hipermedia** — graful de **link-uri** dintre resurse; din el, dintr-o stare se pot face tranziții către alte stări (**HATEOAS** = „Hypermedia As The Engine Of Application State"): link-urile din reprezentări **conduc** schimbarea stării;
- **REST** — stilul care leagă totul: resurse identificate prin URI, reprezentări interconectate prin hipermedia, tranziții prin verbele HTTP.
</details>

---

## 20. Media Types (MIME), header-e HTTP și Ajax 🟡 (2 apariții)
> *„Flow-ul/legătura dintre MIME types, header-ele HTTP și Ajax. + 3 atribute din header request."*

<details><summary>📖 Răspuns</summary>

**MIME / Media Types** = tipul conținutului unei resurse (`Content-Type: type/subtype`), ex. `text/html`, `application/json`, `image/png`. Dat de **server**, nu de extensie.

**Ajax** (XMLHttpRequest / Fetch) = transfer **asincron** de date între browser și server, **fără reîncărcarea** paginii.

**Flow-ul MIME → header-e → Ajax:**
```
1. Programul JS creează o cerere (fetch/XHR).
2. Cererea poartă HEADERE HTTP — ex. Accept: application/json (clientul cere un MIME).
3. Serverul răspunde cu Content-Type care indică MIME-ul datelor (JSON/XML/HTML).
4. JS verifică status + Content-Type, procesează datele și modifică pagina cu DOM — fără reload.
```
Deci Ajax folosește **headerele** (`Accept`/`Content-Type`) pentru a negocia **MIME-ul** datelor schimbate.

**3 atribute din header de CERERE (request):** `Host`, `User-Agent`, `Accept` (sau `Referer`, `Accept-Language`, `Cookie`, `Authorization`).
</details>

---

## 21. Subiecte apărute o dată (mai rare) ⚪🟡

<details><summary>📖 GraphQL vs REST (2023)</summary>

**GraphQL** = limbaj de interogare pentru API-uri, strong-typed, un **singur endpoint**; clientul cere **exact** datele dorite → rezolvă **over/under-fetching**. **REST** = stil arhitectural, **multiple endpoint-uri**, serverul decide ce date întoarce.
| | GraphQL | REST |
|--|---------|------|
| Endpoint-uri | unul | multiple |
| Cine decide datele | clientul | serverul |
| Tipizare | strictă | slabă |
Dezavantaj GraphQL: caching mai greu, complexitate.
</details>

<details><summary>📖 SOAP vs REST (2022)</summary>

**SOAP** = **protocol** (reguli stricte, doar **XML**, plic SOAP, contract WSDL). **REST** = **stil arhitectural** (orice format, uzual **JSON**, folosește verbele HTTP, stateless). SOAP e „greu"/verbose, potrivit enterprise (WS-Security); REST e ușor, potrivit web/mobile. Asemănare: ambele = comunicare machine-to-machine prin rețea.
</details>

<details><summary>📖 Rolul design patterns în dezvoltarea web + 2 exemple (2021)</summary>

Șabloanele de proiectare = soluții reutilizabile → cod mentenabil, extensibil, vocabular comun. **Exemple concrete în web:** **MVC** (arhitectura aplicației: Controller preia cererea, Model = date, View = prezentare); **Singleton** (o singură conexiune la BD / obiect de configurare); **Front Controller** (punct unic de intrare — routing); **Observer** (evenimente/pub-sub); **Decorator** (middleware — auth, logging); **Proxy** (reverse proxy/caching).
</details>

<details><summary>📖 Tipuri de stocare persistentă într-o aplicație web (2025)</summary>

**La client:** cookie-uri (max 4KB), **Web Storage** (localStorage/sessionStorage), **IndexedDB** (obiecte, API asincron).
**La server:** **fișiere**; **baze relaționale (SQL)** — MySQL, PostgreSQL, SQLite; **NoSQL** — MongoDB, Redis, Cassandra, DynamoDB; modele XML/RDF.
</details>

<details><summary>📖 De ce sunt mai multe parsere, rolul lor, interdependențe (2025)</summary>

Există mai multe pentru că au **roluri și compromisuri diferite**: (1) **fără validare** (verifică doar well-formed — Expat); (2) **cu validare** (verifică conformitatea cu DTD/XML Schema — Xerces); după modelul de acces: **DOM** (arbore în memorie, acces aleator, consumă memorie) vs **SAX/pull** (pe evenimente, rapid, memorie mică, fără acces aleator). **Interdependențe:** validarea **presupune** întâi well-formedness; un parser DOM/SAX poate fi cu SAU fără validare; bibliotecile mari oferă mai multe moduri în același pachet (un DOM poate folosi intern un parser de tip eveniment).
</details>

---

> ⭐ **Strategie rapidă:** primele 7 teme (SOA vs MVC, DOM well-formed/valid, server/proxy/gateway/app server, servicii vs microservicii, DRY, mai multe limbaje, securitate cookie/sesiuni) acoperă majoritatea examenelor Web.
