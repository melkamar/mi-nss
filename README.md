# MI-NSS
MI-NSS course at FIT CTU. Academic year 2016/2017

## Notes
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