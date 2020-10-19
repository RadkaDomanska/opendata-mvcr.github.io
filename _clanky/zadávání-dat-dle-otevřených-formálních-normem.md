---
layout: post
detail: true
title: "Zadávání dat dle otevřených formálních normem"
ref: zadávání-dat-dle-otevřených-formálních-normem
lang: cs
author: miroslav_blaško
date: 2020-10
---


## Zadávání dat dle otevřených formálních normem

Zákon 106/1999 Sb. o svobodném přístupu k informacím zavádí pojem [otevřené formální normy (OFN)][ofn-def] jako nástroje pro zajištění schopnosti různých programových vybavení vzájemně si poskytovat služby a efektivně spolupracovat.  Ministerstvo vnitra České republiky jako koordinátor aktivit kolem otevřených dat veřejné správy si dává za cíl definovat tyto sdílené výměnné formáty, tak i proces jakým na tvorbě těchto formátů mohou organizace spolupracovat a dále jednotný způsob, jak data v těchto formátech sdílet. 

Abyste publikovali data v souladu s otevřenou formální normou, potřebujete přizpůsobit vaši existující databázi dle OFN a je to. Ovšem, co když žádnou takovou databázi dosud nemáte? Síla strojově čitelných OFN je v tom, že nám umožňují vygenerovat celý ekosystém pro jejich obsluhu. Tedy i strukturu databáze a zadávací formuláře, kterými databázi naplníme.

Cílem tohoto článku je představit technologie, které nám umožňují zadávací formuláře  generovat a tím poskytnou jednoduchý nástroj pro vytváření dat kompatibilních s otevřenou formální normou.
<!--more-->

## Příklad otevřené formální normy

Pokud jste o OFN jeste neslyšeli, doporučujeme si přečíst nejprve [úvodní článek k OFN][ofn-01-uvod]. Ministerstvo vnitra nabízí OFN, které jsou strojově čitelné, což usnadňuje jejich údržbu. Vydané OFN mají svoji vlastní webovou stránku s definicemi pojmů a konceptuálním schématem, které graficky znázorňuje jak tyto pojmy spolu souvisí. OFN může využívat i struktury definované z jiných OFN jak si ukážeme na konceptuálním schématu OFN [Turistické cíle][turistické-cíle]. 

{% include image.html 
   url="../attachments/články/zadávání-dat-dle-otevřených-formálních-normem/turistický-cíl.svg"
   description="Obrázek 1 – Konceptuální model Otevřené formální normy Turistické cíle"
%}

Schéma popisuje “provozovatele” nebo “vlastníka” Turistického cíle pomocí struktury “Člověk či osoba”. Tato struktura je popsaná v samostatné OFN Lidé a osoby, jak je vidět na dalším obrázku.

{% include image.html 
   url="../attachments/články/zadávání-dat-dle-otevřených-formálních-normem/člověk-či-osoba.svg.svg"
   description="Obrázek 1 – Konceptuální model Otevřené formální normy Lidé a osoby"
%}

OFN “Lidé a osoby” popisuje strukturu “Člověk či osoba” pomocí třídy Člověk a třídy Osoba, která reprezentuje právnickou osobu.

Další části webové stránky k OFN je popis různých formátů dat (např. CSV, XML, JSON, JSON-LD), kterými je možné data podle OFN distribuovat. Tyto formáty však slouží pro technicky zdatné experty, kterým dává návod, jak tyto data tvořit. Pro většinu lidí je tento formát příliš technický a je pro ně potřeba vytvořit aplikaci pro zadávání dat realizovanou například jako chytrý formulář.

V následujícím textu si ukážeme, co se nachází “pod pokličkou OFN” a jak nám to může usnadnit tvorbu takýchto formulářů. Nejdřív se podívejme na to, co vlastně formuláře nabízejí.

## Chytré formuláře pro zadavání dat dle OFN

Chytrý formulář pro plnění OFN dat by měl být efektivní pro zadávání dat netechnickým uživatelem a měl by zabezpečit zadávání jenom korektních dát. To souvisí jak s pochopením  uživatele co vyplňuje tak s možnostmi samotného chytrého formuláře, který také může zamezit uživateli vyplnit některé nevalidní kombinace hodnot.

Cílem chytrého formuláře je:
Poskytnout vhodné omezení typů položek formuláře - např. “jméno” a “příjmení” Člověka musí začínat velkým písmenem a pozostávat jenom ze znaků bez čísel, “věk” Člověka je číslo v rozmezí 0-130.  
Poskytnout vhodné alternativy vyplnění stejných dat - tak například u třídy Člověk je  možné vyplnit separátně “jméno” a “příjmení”, nebo můžeme vyplnit obojí v jedné položce “celé jméno” v případě, kdy nelze jméno strukturovat. Zároveň však nesmí být možné některé alternativy kombinovat jako například vyplnění “pohlaví” právnické osoby, nebo “IČO” u Člověka v případě, že figuruje v roli fyzické osoby.
Poskytnout jasný popis dané položky formuláře - napríklad u třídy Člověk a položky “celé jméno” by formulář měl poskytnout nápovědu jestli se jedná o jméno včetně titulů nebo bez nich.
Vhodně položky formuláře uspořádat - špatné uspořádání položek brzdí efektivnímu vyplňování formuláře a v některých případech může vést až ke špatné interpretaci položky formuláře. Například položka “jméno” má předcházet položce “příjmení”, “datum narození” typicky předchází “datumu úmrtí”. “Název” by měl být jednou z prvních položek formuláře Turistického cíle a  “IČO” jednou z prvních položek formuláře pro právnickou osobu. Dále, vlastnosti tříd jako “jméno” a “pohlaví” Člověka  je vhodné mít před popisem vztahů k jiným objektům jako například “partner” Člověka, nebo “příslušnost k organizaci”.
Zabezpečit konzistenci dat i napříč položkami - například v Turistickém cíli je vhodné zamezit, aby konkrétní Vstupné Turistického cíle platilo v jiný den jako jsou “otevírací hodiny” tohoto objektu. Například, formulář by měl zamezit zadání Vstupného jen pro pondelky, v případě že má Turistický cíl v pondělky zavřeno. 

Vytvoření chytrého formuláře pro Turistické cíle jak je popsán výše je netriviální úkol i pro zdatného programátora, který vyžaduje hodně času pro implementaci a následné testování funkčnosti formuláře. Tento čas bude muset vynaložit znovu a znovu každá firma poskytující OFN formulář pro Turistické cíle například pro obce. A co se stane když se daná OFN rozšíří nebo dokonce změní? Jak dlouho bude trvat až se nová aktualizace formuláře dostane opět k obcím a kolik nato firmy znovu vynaloží úsilí?

Architektura nad kterou je OFN postavena umožňuje tyto nastolené otázky ignorovat. Je postavena na myšlence, že chytré formuláře je možné generovat automaticky, když máte dostatečně chytré schéma dat. Každá OFN je reprezentovaná pomocí technologií Semántického webu, co umožňuje k základnímu popisu OFN připojit potřebnou sémantiku (tj. popis významu dat) vhodnou pro generování chytrých formulářů.

Jedným z těchto sémantických popisů je Sémantický slovník pojmů veřejné správy (SGoV), který doplňuje sémantiku OFN zejména z pohledu legislativy a agend veřejné správy. Např. OFN adresa umístění Turistického cíle je přímo mapovaná na [jeden z legislativních podslovníků SGoV][slovník-lsgov-359-2011]. Ten definuje pojmy ze Zákona č, 111/2009 Sb., o základních registrech, kde je pojem adresa definován. Také pojmy jako je fyzická osoba a právnická osoba, které nejsou přímo definované v legislativě nýbrž v právní teorii, jsou definovány ve [Slovníku veřejného sektoru][slovník-veřejného-sektoru], který je dalším z podslovníků SGoV.  Slovníky SGoV jsou založené na principech, které cílí na kvalitu strojového popisu i srozumitelnost pro širokou veřejnost. OFN přejímá tyto principy. Díky jejich propojení je možné ve formuláři jasně vysvětlit uživateli význam použitých pojmů (jako např. vysvětlení pojmu “celé jméno”) a také odkázat ho na příslušnou legislativu (jako je to např. u “adresy” Umístění Turistického cíle).

I zbylé požadavky chytrého formuláře lze realizovat jako další rozšíření sémantiky OFN Turistického cíle. Ukážeme si alespoň pár příkladů. Pojmy “název” Turistického cíle a “IČO” právnické osoby jsou kategorizované ve Slovníku SGoV jako tzv. “vlastnost objektu”. Lze vytvořit ještě specifickejší pojem “identifikační vlastnost objektu” tedy “vlastnost objektu”, která objekt identifikuje. Pak můžeme pro generátor formulářů definovat pravidlo, že “identifikační vlastnosti” se mají zobrazovat jako první položky formuláře k objektu. Tedy “název” by měla být první položka objektu Turistický cíl a “IČO” první položka objektu Osoba. Také můžeme pomocí sémantických technologií vyjádřit, že vlastnost “jméno” Člověka a vlastnost “příjmení” Člověka “je částí” vlastnosti “celé jméno”. Generátor formulářů pak může sémantiky tohto typu přetavit do pravidla, díky kterému uživateli zobrazí alternativně k vyplnění vlastnosti, které jsou “celkem” (tedy “celé jméno”), nebo vlastnosti, které reprezentují všechny jeho “části” (tedy “jméno”, “příjmení”, ”tituly před jménem”, “tituly za jménem”). Podobně lze vyjádřit i informaci, že časová specifikace pro Vstupné Turistického cíle musí “být uvnitř časové specifikace” “otevírací doby” Turistického cíle.

## Ukázka nástroje pro správu formulářů

Jak bylo vysvětleno v předcházejícím textu, pojmy definované v OFN Turistické cíle je možné rozšířit o významy, které pak umožní generování chytrých formulářů. Generátor formulářů je pak systém ve kterém jsou zakódovány pravidla jak tyto významy z OFN zpracovat a vygenerovat pro uživatele chytrý formulář. Jak si ale pozorný čitatel jistě všiml, u příkladu Turistického cíle jsme použili jenom generické rozšíření významů, jelikož pojmy “identifikační vlastnost objektu”, “je částí”, “je uvnitř časové specifikace” lze samozřejmě použít i pro jiné OFN. A proto stačí napsat jenom jeden generátor formulářů a bude fungovat pro všechny OFN. Navíc když se OFN změní tak generátor vytvoří nový chytrý formulář bez jakéhokoliv zásahu.  No a migrace dat ze staré verze formulářů do nové verze lze automatizovat podobne, co rozvedem v dalším pokračování tohoto příběhu. Pro demonstraci popsaných myšlenek v tomto čĺánku bylo vytvořené [demo aplikace pro správu formulářů dle OFN][ofn-form-manager-demo]. Rozšířili jsme významy pěti OFN, které tato aplikace může aktuálně vyplňovat -- [Turistické cíle][turistické-cíle], [Sportoviště][sportoviště], [Události][události], [Aktuality][aktuality] a [Pracovní místa][pracovní-místa]. Část formuláře, kterou aplikace generuje pro Turistické cíle je vidět na následujícím obrázku.

{% include image.html 
   url="../attachments/články/zadávání-dat-dle-otevřených-formálních-normem/aplikace-pro-zadávání-dat.png"
   description="Obrázek 1 – Snímek obrazovky aplikace pro zadávání dat dle OFN"
%}

[ofn-def]: https://www.zakonyprolidi.cz/cs/1999-106#p3-9 "Definice Otevřené formální normy"
[ofn-01-uvod]: https://pod-test.mvcr.gov.cz/články/otevřené-formální-normy-01-úvod "Otevřená data a otevřené formální normy"
[turistické-cíle]: https://ofn.gov.cz/turistické-cíle/2020-07-01/ "OFN Turistické cíle"
[události]: https://ofn.gov.cz/události/2020-07-01/ "OFN Události"
[aktuality]: https://ofn.gov.cz/aktuality/2020-07-01/ "OFN Aktuality"
[sportoviště]: https://ofn.gov.cz/sportoviště/2020-07-01/ "OFN Sportoviště"
[pracovní-místa]: https://ofn.gov.cz/pracovní-místa/draft/ "OFN Pracovni místa"
[slovník-lsgov-359-2011]: https://slovník.gov.cz/legislativní/sbírka/359/2011/slovník "Slovník zákona č. 111/2009 Sb., o základních registrech"
[slovník-veřejného-sektoru]: https://slovník.gov.cz/veřejný-sektor "Slovník veřejného sektoru (V-SGoV)"
[ofn-form-manager-demo]: https://kbss.felk.cvut.cz/ofn-form-manager-demo "Demo aplikace pro zadavání dat dle OFN (přihlášení pomocí uživatele 'testuser' a hesla 'testuser')"