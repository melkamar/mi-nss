# mi-nss
MI-NSS course at FIT CTU. Academic year 2016/2017

## Pátek

Obecně se očekává, že management a rychlost vývoje společností se bude zvyšovat, bude to komplikovanější a horší. Proto potřebujem něco stabilního do Agile environmentu.

Je potřeba mít nějaký design rules pro návrh, nějaký metodologie jak na to, abych měl kvalitu, flexibilitu, rychlost atd.

- Metodologie by měla bejt:
    - Systematická, měla by mít postupně kroky, který rozumně popíšou co kdy dělat. V tuhle chvíli se ale jako společnost pohybujem spíš pryč od tohohle cíle.
    - Waterfall sice určuje jaký stepy se mají dělat, ale už nepopisuje, jak má správný a kvalitní artefakt (každého z kroků) vypadat.
    - Comprehensive popis metodologie -> obsahuje jak popis procesů, tak technik (jak něčeho dosáhnout).
    - Agilní metodika:
        - Sprinty - proces
        - Refactoring - technique
    - Většina nových věcí v IT landscape jsou Tooly, který jsou použitý pro *podporu* technik a procesů - samotný techniky a procesy nejsou tak nový.
    - Existuje hrozně nových modelovacích jazyků.
    - Obecně se říká "všechno v IT se pohybuje rychle", ale metodologie jsou celkem zamrzlý a nic moc se neděje.
    - Lidi/studenti neznají metodologie, je jich přes 1000. A jak si tu svou vůbec vybrat? Je to džungle, nedá se říct která metodologie se hodí kam.
    - Většina metodologií pochází z akademické sféry. Některý jsou free, Unified Process, některý jsou tučně placený. -- Většina těch akademických metodologií teoreticky předpovídá nějaký zlepšení, ale málokdy se udělal nějaký praktický test. A když už jo, pak výsledky nebyly moc přesvědčivý.
    - "There is a widespread *belief* that methodologies help" - věří se tomu, ale chybí důkazy.

*Proč se profesoři začali věnovat tomu co dělají?* -> Myslí si, že metodologie obecně nepomáhají.

"Software crisis" - už v 1969. SW byl drahej, programátoři byli divný, nebyl konsenzus. Všechno bylo ad-hoc. Všude bylo `goto`. Hrůza.

Už dávno se chtělo zbavit těch goto bloků - Dijkstra na tom dělal. Ukázal, že jakýkoliv software kde jsou goto se dá převést na program kde jsou iterace, sekvence a "selections"*.

Entitní relace v RDBMS, diagramy atd - 1970.
Information hiding, encapsulation - 1980.


Zajímá nás *structured design* - coupling v software. Tehdá se ještě neřešil coupling jako OOP, ale v oldschool jazycích.

V 1970 byl claim - pokud máme moduly, tak nám sníží práci, komplexitu, budem je moct používat jako black boxes.

Zdá se, že když je low coupling, tak je to supr, změnou neovlivním tolik modulů. ALE.

Cohesion - jednotnost funkcionality uvnitř modulu. Čím úzčeji zaměřený, tím lepší.

Stamp coupling - pošlu všechna data co mám, druhej modul to nějak použije. Lepší je ale poslat jenom ty atributy, který přímo vedou k výpočtu.

#### Druhá generace metodologií - object oriented
- Design UML je výsledek totálního failu. Tři lidi co to navrhli měli OO metodologii, publishnuli každej knihu. Cíl těch tří bylo, aby napsali jednu společnou metodologii pro design OO systémů, ale tohle jim prostě nešlo. Na konci se na to vykašlali. Jediný, na čem se dokázali shodnout, byla notace toho jak to zapisovat -> UML.

- Jedna metodologie prostě nestačí na všechno, měla by se adaptovat ta, která se hodí pro daný sektor, projekt, tým. To ale znamená, že firmy musí u každýho projektu přemejšlet, jak to dělat.

- Martin Fowler - refactoring, analysis patterns

- Příklad na coupling:
    - Je coupling mezi Register a Sale?
        - V prvním příkladu ano, je
        - Ve druhém taky, ano
    - Je coupling mezi Sale a Payment?
        - V prvním příkladu - je tam coupling! Metoda addPayment u Sale bere jajko parametr Payment, takže jsou propojený.
    - Je coupling mezi Register a Payment?
        - V prvním - ano
        - Ve druhým - není.

Druhej je lepší. 3:2. (ale prej to neni tak úplně pravda, budem o tom povídat furt).

Design patterny - řeklo se, že než furt dokola hledat nějakou metodologii, je lepší mít katalog best practices.
- Ale, tohle není vědecky podložený. Obecně se má za to, že to je pravda, že je to dobrý řešení. Ale je to jenom konsenzus, není to dokázaný. Byly i nějaký pokusy, někdy se ukázalo, že patterny byly pomalejší než ad hoc řešení.
- V SW je hodně uncertainty. Staví se obří systémy, ale underlying science nemá žádný obecně platný pravidla, pravdy atd.
- The amount of laws we have compared to other engineering disciplines is low.

Project manažeři obecně chtěj software rychle, levně. Těžko můžu strávit další dva měsíce na úpravách sw, když funguje už teď, jenom neni tak flexibilní.

#### Chapter 12

Hledáme teorii o věcech, které se vyvíjejí v čase.

Příklady instability
- Most - špatně
- Childbirth - stahy způsobí release hormonu, kterej způsobí další stahy -> dobře
- Bomby -> "dobře"

Vezmeme způsoby studia systémů a instability za jejich běhu, a aplikujeme to na evoluci systému.

Saturn V - 120 tun payload, dneska je to jenom 45. Ne všechno se vyvíjí k lepšímu a většímu.

Saturn V není stabilní ve smyslu vývoje designu. Chci, aby vynesl víc payloadu -> přidám motor. Ale to potřebuje víc paliva, silnější pumpu, kvalitnější fuel tank atd. -- Design raket není stabilní. Modularita je ok, ale evoluce ne.

V reálu nám to tak nevadí, ale SW chceme evolvovat.

Chceme dostat stabilní transformaci konceptu Person na Class Person. Stabilizujeme od základních bloků.
Přidám atribut, je ok že se mi promítne ve třídě. Ale nesmí se projevit ve funkcích, které se ho nijak netýkají, např. volají konstruktor, ale ten pak změním a musel bych měnit všechna volání.

*Concern* - change driver, things, that can change independently
Důkaz, že separation of concerns je správně -> pokud to nedělám, budu muset copypastovat t_1,1 - např. komunikátor se sítí. Pak ale když budu chtít udělat změnu, budu to muset udělat na všech rozkopírovaných místech. To jsou ty forbidden terms, kombinatorický efekt.

Integration bus - concerns u peer to peer je to, že nody musí reagovat na změny protokolu svého a svého cíle. U ESB předpokládám, že se nebude měnit. Je snažší zůstat u špatného způsobu, než to předělat na ESB, protože v ESB budu muset dělat víc komunikace (v danou chvíli, ne dlouhodobě).

Separation of states - např. držím si v nějakym souboru záznamy. Eventuelně musím tohle distribuovat po síti. Najednou se mi může stát, že padne server a nemůžu číst. Tuhle výjimku musím řešit, ale všichni volající na to pak musí být schopni zareagovat, to je blbě.

Sink pipeline - podobně jako assembly lines, jo, všechno supr. Ale je tu rozdíl. Assembly - někdo přidá dveře, někdo je zašroubuje, další nabarví, atd. Ale sink pipeline - někdo začne přidávat dveře, ale než skončí, zeptá se někoho dalšího, aby to tam zašrouboval, a než ten skončí, začne to někdo barvit.
- Assembly - není coupling! po !každym kroku je to done
- Sink pipeline - je tu coupling. Když se něco sesere, nejsem schopen říct ve který funkci! Možná že to bylo už od tý nejpůvodnější, která dala špatný data.

Analogie assembly line bude nějaký (stateful) controller! Proces, co orchestruje volání funkcí.

Bacha na Maven, já budu mít třeba jednu, dvě dependence, ale tranzitivně budu stahovat celej vesmír.

Data structure vyžaduje kombinaci registrů a instrukcí.

-> Měl bych mít dva typy tříd - Data Class a Action Class.

Coupling není sám o sobě špatnej, když se kvůli němu nedělaj žádný ripple efekty.

Design Ambiguity Revisited
- Autor říká, že není coupling mezi Registerem a Paymentem.
- ALE, nemůžu si bejt jistej, že nic co se stane v Paymentu neovlivní Register. Můžou se třeba začít vracet corrupt data.
- Podle přednášejících v příkladu chybí čtvrtá krabička, controller se stavem.

#### Po předvečeři
Oddělení modulů - mám x modulů, ale několikanásobně víc možností, jak je pospojovat. ALE, ve chvíli kdy to začnu měnit, a mám to udělaný špatně, může se mi stát že budu mít exponenciálně modulů který musim upravovat.

Coupling nejsou jenom šipky v designu, musím uvažovat i formát dat co vracim, výjimky co jsou házený, jdk, atd.

Modularizace je fajn, vypadá sexy, ale pokud to neudělam správně, bez harmful couplingu, je to v háji, projeví se mi to časem -> Exponential ripple costs.

You destroy the evolvability by making the system more flexible. If not done properly.

Cross-cutting concern - concern kterej je požadovanej všude v systému, prořízne napříč funkcionální strukturou. Např. logging, encryption, atd.
- Persistence, access control, remote access - tohle potřebuju napříč všema datama.
    - Např mam hotovej systém, všechno ok. Pak někdo přijde a že mam přidat data access control na všechno. Jsem v háji.
- Cross cutting jsou nebezpečný, protože je mam z definice všude. A když změnim něco z toho concernu, projeví se mi to na spoustě míst.
- Musim decouplovat nejen modely, ale i tohle concerny.
- To že mam nějaký standardní řešení neznamená že to bude ok a bez problémů, i tak bych to měl encapsulovat.
- Není dobrý, že duplikuju nastavení loggerů, například.
- Další řešení je, že budu mít miniframework pro něco a budu mít "relay moduly", který budou volat framework. Core změny udělam na úrovni frameworku (červený plochy), a neprojeví se mi to dole, v "businessu".
- Dneska mam ale std řešení, takže to můžu používat. ALE musim si dávat pozor v encapsulation a oddělit ten persistence concern or data. (Na slajdu se zelenou střechou ty červený obdélníky).
- Když pak ale budu chtít vyměnit framework, budu muset vyměnit všechny konektory do framworku - zelený svislý čáry. Lepší je udělat nějakej mezi-adaptér.
- Žlutá je dobrá, ale není vždy realistická ani všemocná. Musel bych v ní v podstatě naduplikovat interface frameworku. Je to tempting, ale zelená je (minimálně dneska) reálnější. Co když wrapnu jeden framework, ale ten druhý má totálně jiný API? Můžu stejně dostat ripples.
- Taky možnost diverzifikace - použít různý najednou. Přednášející používaj většinou zelenou, a protože maj fancy generátory kódu, neni problém přegenerovávat zelený krychličky.
- Ale nehledě na to, jakou architekturu použijem, ještě důležitější je encapsulation krychliček.
Frameworky se používají na cross-cutting concerny, protože ty jsou sdílený mezi všema lidma. Tak to někdo vytvořil.
- Data class nemá vědět jak se ukládat, bude tam nějaká ClassPersistor class, která bude anotovaná a která se o to bude starat. Data class je prostě tupá.

Letadlo - chlazení je embedded, na každym chobotu je jedno chlazení. Kdyby bylo centrální co rozvádí chlad, byl by to zelenej framework.

Mám persistence pro Invoice, potřebuju InvoiceSaver. Order -> OrderSaver. Stejně tak s access rights, všechny concerny musím encapsulovat. Takže co musim udělat, abych se zbavil ripples - nestačí jen oddělit třídy a použít framework - musim ke každý třídě udělat adaptér, encapsulator - integrace na nejnižší úrovni. Každá krabička včetně jejich encapsulatorů je **element**.

#### Revisiting transformations
Transformace z doménový třídy na sw třídu přímo není dostatečná. Nejdřív se to musí encapsulovat.

Element - jako bysme měli cihly pro stavění, který by už v sobě obsahovaly strukturální stabilitu, ochranu, izolaci, dráty pro elektriku, trubky pro vodu atd.

### Poslední fáze
Když mám fancy elementy, tak je změna do konceptuální třídy contained - budu třeba dělat víc než jen jednu, ale všechny budou obsažený uvnitř elementu (třídy + encapsulatorů). Nebude škálovat s velikostí systému.

Pět definovaných elementů je schopno naimplementovat teoreticky jakýkoliv informační systém - je to abstrakce instrukcí procesorů.

Vždycky bude nějaká dependence - na databázi, na frameworku, na něčem. To akceptujem, musíme jí teď jenom separate and encapsulate (tm).

V Javě, persistence - dependence na JPA - ta je encapsulovaná.
Remote call, transakce - to samý (RMI, EJB).

##### Evolvability
Dimenze:
- Mirrors (models) - přidávám další a další konceptuální modely, atributy atd.
- Skeletons (structures) - pokaždý když dostanu nový concern, např. chci všechno od teď logovat do NSA, nebude to předělávání celý aplikace.
- Utilities (tech frameworks) - přijde nový framework, nová non-backward-compatible verze, něco -> chcito použít žavés
- Craftings (custom code) - potřebuju možnost si customizovat chování, implementace tasků, obrazovek atd.

Tyhle dimenze se evolvujou nezávisle na sobě, změna v jedné mi nesmí ripplovat ostatními. Aby byla aplikace evolvable, tohle musí fungovat.























todo: Aspect oriented programming