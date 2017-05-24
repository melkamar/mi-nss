# MI-NSS
MI-NSS course at FIT CTU. Academic year 2016/2017

## Notes

## Čtvrtek
- Idea je taková, že modulární architektura je škálovatelná, evolvable. Tahle myšlenka se objevila už v 1968. 
- Naproti tomu je další "teorie" - modulární struktura se postupně degraduje, když se celý systém upravuje. "The more you change software, the more the modular structure degrades and the more difficult it will be to maintain it." Jediný co se s tím dá dělat je vynaložit práci na udržení. Dneska se tomu v agile říká *refactoring*.

- A teď, kdo má pravdu? Tohle řeší v NSS. A co s tím dělat? Otázka, jak stavět komplexní a scalable systém je stejná jako otázka, jak udržovat systémy tak, aby nedegradovali.

- Jak aplikovat koncept instability (jako ten most co spadnul v přednášce) na budoucí software? Vstup v případě softwaru je:
    vstup je požadavek na změnu v software
    systém je struktura softwaru
    výstup je počet úprav potřebných v systému - potřebuju něco změnit -> na kolika místech?
    
    "energie mostu" v případě software - velikost software. 
    kombinatorický efekt - změním jednu věc, ale kvůli tomu musim změnit další tři, a kvůli nim zas další a další. -> Když se něco změní, je celková komplexita téhle změny závislá na velikosti systému? Jestli jo, je to kombinatorický efekt.
    
    
Jak řešit instabilitu? Co nejdřív to identifikovat a vyřešit hned. Nebo ještě lépe, zabránit rozhoupání mostu.

Normalized System - modulární struktura bez žádných "ripple effects" (kombinatorických efektů).

Teorie ti ukáže kde v kódu máš kombinatorický efekty - problémy.
Teorie postuluje, že v kódu nesmí být žádný.


### Principy

- Separation of concerns
- Separation of state
    - Když mám nějaký workflow akcí, měl bych si nějak vždy uchovávat stav programu, toho kde jsem.
    - V reálným OOP je něco jako když třídy volají: A -> B -> C -> ...   - zde třída A volá B, ta volá C atd. A musí pořád čekat. Tohle je špatně.
- Data version transparency
    - Data fieldy třídy/tabulky atd mají být nezávislý od jejich interface - přidám novej datafield a ostatní moduly musí furt volat v pohodě.
- Action version transparency
    - Můžu updatnout verzi modulu a ostatní moduly musí pořád bejt v pohodě, tj. zachovám interface.
    
    
    
Chceš systém bez kombinatorických efektů?
    - Jediná možnost architektury - extrémně fine-grained modulární struktura. Hodně malých modulů a jejich agregace do větších a větších.
    - Microservices!
    - Hardware - v CPU je miliardy tranzistorů - to jsou hodně finegrained moduly.
    
Key insight #2:
    - S lidma to nefungovalo, byly faily
    - Podobně jako ve výrobě HW - je třeba použít roboty -> zde generátory kódu.
    - Celý IS by mělo jít postavit z 5 stavebních bloků.
    - Specifikace systému - usecases, stories etc. - tohle se přetransformuje na instance těchto 5 bloků.
    - Dokázalo se, že 5 bloků neobsahuje žádný kombinatorický efekty. Při budování systému se instancují tyhle bloky, pomocí generátoru. Jsou to jenom parametrizovaný verze těch původních templatů. Z definice, vzor nemá kombinatorický efekty, reálný třídy jsou jenom jejich kopie -> takže taky nemají efekty.
    
    
Při použití tohohle přístupu se experimentálně ověřilo, že systém je nejenom flexibilnější, ale i upfront cena je menší a implementace je rychlejší. 

Key insight #3:
    - Teorie není o stavění systému, ale o jeho udržování.
    - Generátory nejsou to, co dělá systém stabilní a flexibilní - to je způsobeno fine-grained architekturou. Generátory jenom pomáhají.
    
Změna funkcionality je většinou ok, dá se vygenerovat a je to. Pak je ale potřeba třeba změnit verzi underlying systému - to je horší.
    - Nová verze Angularu, není backward kompatibilní. Kdyby byla aplikace postavená podle teorie, stačí napsat nový Connector element a bude to fungovat.
    
Měly by se pravidělně regenerovat aplikace - každý 3-6 měsíce. Pravidelně fixovat chybky, nesedět na jedný verzi roky.

Je v 10 let starém systému degradace?
    - 95% kódu je maximálně 6 měsíců stará, není co by degradovalo
    - Zbytek jsou nějaký custom hacky, tam problém být může, ale nemusí
    
-> Proces údržby se mění na proces "regenerace" nebo "rejuvenace", omlazení. Údržba aplikaci zlepšuje, nedegraduje. Když upgraduju na novou verzi, dá se čekat že ta verze bude lepší a lepší než ty starší.

Prime Radiant - metaprodukt - inspekce ostatních aplikací, kontrola kvality largescale systémů
- Databáze prime radiantu obsahuje informace o všech ostatních skeletonech
- Nejabstraktnější generátor - řeknu mu, že chci vygenerovat tohle a tohle 
- Usecase - ok, chci vidět modulární strukturu aplikace #4 - ok, jsou tam dvě verze. Pro první verzi jsou dvě různý instance - využívající dva různý source stacky. Tj. zabraňuju vendor lock-in - je to univerzálnější, odpoutávám se od konkrétní underlying technologie.
- Můžu se tam podívat na obláčky - můžu inspectovat a řešit proč se něco děje.



### Metodologie

Klasicky vezmu požadavky, ale potom co je mám (stories, usecases atd), je musím přeložit do NS -> jako input generátorům kódu.

První iterace - hned udělám skeleton, ten je production-ready, a hned ho vezmu za stakeholderem. Ten je mnohem schopnější říct jestli jsou v zadání chyby, když vidí prototyp. Klasický agilní sprinty.

Ken Schwaber(?) - koautor Scrumu. Agilní metodologie zrychluje iterace, změny, ale zároveň i degradaci systému. "Před 15 lety, když jsem udělal scrum, jsem myslel, že jsem vyřešil problém flexibility. Teď ale vidím, že je to horší než lepší."


ESB je cool, lepší než propojení všeho se vším. Ale v praxi - firmy se dostanou do stavu, kdy maj tři různý ESB a mají víc problémů než co ušetří. ESB má totiž v sobě taky kombinatorický efekt - změnou ESB ovlivním všechno. Řešení je neměnit ESB nikdy, ale pak zas nevím o vendor lock-in, dependencích atd.

Potřeubju jednu ESB na každý funkční concern. Barevný elementy - červený databáze, žlutá auth, zelená něco dalšího, např. Krychličky nad bílejma cihlama jsou proxy / adaptéry.

Rozdíl mezi těma prvníma dvěma teoriema - souvisí s kombinatorickýma efektama. Když se jich při konstrukci systému zbavím, pak je to všechno růžový, plugnplay a krásný. Když je tam necham, tak je modularita horší než lepší.

Modularita v praxi je dneska založená na intuici, rules of thumbs atd. Nejsou žádně pevně daný pravidla jak kód dělit do modulů - každý programátor/tým to udělá trochu jinak.

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

## Sobota
Custom code - je odděleně, jednoduše může být re-injected do nových verzí elementů atd. A i tak je ho jenom 5-10% v celý codebase.

### Prime radiant
Pracuje se všema čtyřma dimenzema evolvability:

- Mirrors
    - Modely, konceptuální elementy, OMO
- Skeletons
    - Interní struktura elementů. V code generation termínech jsou to templaty pro třídy.
- Frameworks
- Custom code

Na základě výběru daných mirrorů,

Vezme invoice, k tomu pak pro specifikovanou množinu frameworků vyinstanciuje skeletony a skrz to konkrétní třídy. Takhle pospojuje všechny základní třídy, a tím udělá instanci aplikace. Potom, co máme všechny ty třídy připravený a vygenerovaný, potom naházíme custom kód jak do elementů, tak mimo ně.

#### DEMO!
Prime Radiant je klasická web app.

Aplikace se skládají z komponent, komponenty obsahují modely - mirrory.
Jedna řádka - jedna aplikace - ve fieldech jde specifikovat všechny dimenze, tj. modely, jakou strukturu mají mít, jaký frameworky používám atd.

Skeleton template - ty fancy třídy kolem té hlavní - *Data pro persistence, *Bean pro transakce, *Remote, *Local atd.

Jako vždy, je to o tom udržovat sumu, ale mít možnosti produktu (všechny kombinace elementů/frameworků).

Formálně taky dokázali, že ve všech těch dimenzích můžou dělat změny, přidávat elementy atd., aniž by došlo k rippling efektu. Všechny změny jsou contained.

If for a bounded set of elements if you want to change an implementation detail, the change is bounded.

However, if you have a code generator, you can change a concern (skeleton), and have the change bounded, yet all applications will be updated at once. New concerns are also easily added.

Oh, persistency? Mám zelenej případ frameworku, takže to mam všude možně naduplikovaný - ale mám generátor kódu, můžu změnit definici a přegenerovat VŠECHNO.

#### Performance
Podle nich, neni čas to dokazovat:

Separation of concerns pomáhá v tom, že nejsou takový dlouhý pipeliny, který by musely alokovat zdroje, čekat atd.

OLTP - On-Line Transaction Processing systems
- Commandmenty pro OLTP jsou dost podobný těm evolvability constraintům, je to dobrý.

#### Remarks
Je důležitý mít taxonomii i v relačním modelu (slide 34). Mám základní entity, ale pak mám i nějaký sekundární - to jsou např. značka auta, typ auta, blabla. Není to to hlavní co potřebuju v modelu pro půjčovnu aut. Pak mám třeba tabulku pro historii výpůjček, to je zas jiná "dimenze". Pak finance, spojení s bankou atd. (tohle všechno na úrovni modelu/DB)

Mělo by taky být víc encapsulace na úrovni business procesů, chybí struktura, všechno je to ad hoc. Oddělení tasků, sub-flows...

Lifecycles by měly být taky separated a encapsulated. Implementovány stavovými automaty, připojenými k entitám (slide 54). Lifecycle relationship je mezi entitama JENOM pokud je tam relationship mezi nima na konceptuální úrovni.


## Conclusion
Současný styl architektury je sice modulární, vypadá dobře, ale jsou tam ty ripple efekty.

Co je na tom špatně? NIC! :D Takhle se SW staví už dekády a transformoval společnost a celou planetu. Funguje to. Nicméně, ve chvíli kdy budu chtít současný software změnit, budu mít problém.

Teď máme statickou modularitu. Ale chceme evolvable modularitu, potom bude možný bejt agilní, rychlej, dobrej.

Kdyby všechny ty metodologie opravdu fungovaly, nejspíš by se za takovouhle dobu uchytily. To se ale nestalo, takže nic moc.

Když chcem postavit velkou strukturu, systém, a nemám žádný daný vědecký podklady, bude to všechno subjektivní, věc názoru. Jedna z možných, ale ne jediná, je tahle jejich Systems Theoretic Stability.

- Doug Mc Ilroy - building blocks, modularita.
- Manny Lehman - čim víc vyvíjím software, tím víc se to sere.

Fine grained moduly jsou fragile, a to z důvodu, že jejich výhody existují jenom pokud striktně kontroluju coupling a combinatorial effects.

Conclusion - v určitých, velmi omezených podmínkách, má Ilroy pravdu. Ale jinak Lehman.

Issue 3 - viz ESB, všichni souhlasí že je to dobrý, ale i tak se to nepoužívá tolik. Muselo by se to aplikovat systematicky, všude, pořád. Design patterns taky  nejsou straightforward, je tam místo pro subjektivitu, to je špatně. Proto jsou potřeba generátory kódu. Neříkají, že jejich generátory jsou dobrý nebo ani nejlepší. Co říkají je, že modularita je tak křehká, že potřebuješ víc než jenom knihy a programátory, abys udržel coupling a combinatorial efekty.

Issue 4 - UP říká, že máme traceability napříč analýzou, designem, implementací. Ale tohle je moc optimistický. Mám cross-cutting concerny, které jdou napříč spektrem funkcí, změna v nich ovlivní spoustu míst. Řešení? Prime radiants, tam jasně vidím, že když změním nastavení, skeleton, model, tak mi ukáže/vygeneruje co se kde změnilo.

### Looking forward
Chtějí stavět na tom co maj, jak teorii tak tooling - Radiant vypadá blbě.
Ale, současný Prime Radiant ukazuje, hodně detailně, strukturu celýho systému.
Na tom se dá stavět novej typ softwaru, třeba pro kontrolu toho, jestli se nepoužívá kód znovu a znovu, zbytečně. Pak se dá doporučovat refactoring, případně automatizovat. Tohle je PhD. materiál a dá se na tom pracovat hned.

Usecases jdou i Beyond IT. Modulární telefony? Měly vydržet navěky, jenom by se vyměňovaly prvky. Ale z pohledu NS teorie to má faily, Google to nakonec taky vzdal.

Tesla auta - hlavní challenge jsou baterie, Musk bude muset udělat novou fancy baterku. Ale to neudělal, šel jinudy. Místo toho vzal existující baterky a pospojoval je. Samotná neni moc silná, ale ta technologie je scalable - prostě jich zapojíš víc. Tohle s monolitickejma baterkama nejde. Tam je potřeba design nový baterky, jako u Saturn V. Teď to taky dělá s baterkama do domu.

Combinatorial effects v dokumentech:
- Máme stovku slajdů o NS, tam jsou někde puzzlíky - obrázky. Co když budu chtít místo puzzlíků použít jinej obrázek? Budu to muset změnit na hrozně slajdech. To je ripple.

Máš několik verzí dokumentu a u všech verzí variace (např. jazyky). To je hrozně moc kombinací. Případně firma má 170 dokumentů o safety, ty se musí ročně udržovat, je to fail, lidi dělaj chyby, za to jsou pokuty.

Jak aplikovat NS? Zase, co jsou funkce, a co jsou cross-cutting concerns? Modul by měl bejt jeden kurz, ten má nějaký atributy. Pak nějakej generátor může generovat dokumenty.

Sice je Prime radiant cool, umí to věci, ale furt nejsme nikde poblíž biologickým systémům.

### Internship
něco jako 4 týdny, všechno zaplacený atd. 15.8. - 15.9.
Bylo by to samostatnější, jakože "pracuj na týhle featuře tohohle".
















todo: Aspect oriented programming