# Daily run prompt

The Routine sends one standalone message into a fresh session each morning. This file
holds that message.

**How to use it:** copy everything below the horizontal rule — from `Végezd el` to the
last line — and paste it into the Routine's prompt field. Do not copy this heading or
these instructions.

The prompt is deliberately self-contained: it does not reference this repository, so it
works whether or not the Routine's session has the repository checked out. It describes
**one** task producing **one** report per run.

---

Végezd el a Magyar Közlöny napi jogi átvilágítását egy FMCG (gyors forgalmú fogyasztási cikkeket gyártó) vállalat magyarországi leányvállalatának belső jogi csapata számára. A vállalat nevét sehol — sem a riportban, sem az e-mailben — ne szerepeltesd.

**1. A legfrissebb lapszám azonosítása.** Nyisd meg a https://magyarkozlony.hu/ oldalt, és keresd meg a legfrissebb **Magyar Közlöny** lapszámot — nem a Hivatalos Értesítőt és nem mellékletet. Ellenőrizd a lapszámot, a megjelenés dátumát és a hivatalos PDF közvetlen linkjét. Az előző futás óta megjelent minden lapszámot dolgozd fel, a legfrissebbel kezdve.

**Hogy tudod meg, hol tartott az előző futás:** listázd ki a közzétett artifactjaidat, és keresd a „Magyar Közlöny <év>/<szám>" című oldalakat. A legnagyobb sorszámú ilyen artifact nevezi meg az utoljára feldolgozott lapszámot. Ha egyetlen ilyen artifact sincs, csak a legfrissebb lapszámot dolgozd fel.

**2. Ha nincs új lapszám az előző futás óta, ne készíts semmit** — se riportot, se artifactot, se e-mailt. Írj egy sort arról, melyik a legfrissebb lapszám és mikor jelent meg, és állj meg. Riport csak új lapszám esetén készül.

**3. A teljes szöveg beolvasása.** Ne mentsd el a PDF-et és ne készíts jegyzetelt munkapéldányt. Olvasd be a hivatalos PDF teljes szövegét — mellékletekkel, táblázatokkal, átmeneti rendelkezésekkel és hatálybaléptető rendelkezésekkel együtt —, és tartsd nyilván az oldalszámokat, hogy minden megállapítás visszahivatkozható legyen.

A WebFetch eszköz NEM működik a magyarkozlony.hu oldalra (EGRESS_BLOCKED hibával elutasítja, mert megkerüli a környezet engedélyezési listáját). Használj helyette curl-t, szövegkinyerőbe csővezetve:

```
curl -sSL "<hivatalos PDF link>" | pdftotext -layout - issue.txt
```

Ha a pdftotext hiányzik: `apt-get update && apt-get install -y poppler-utils`. A kezdőlapot is curl-lel töltsd le: `curl -sSL https://magyarkozlony.hu/`.

**4. Szűrés.** Vizsgáld át az egész lapszámot az FMCG-relevanciamérce szerint: társasági irányítás, nyilvántartások, jelentéstétel és cégcsoporton belüli megállapodások; foglalkoztatás, juttatások, munkavédelem, idegenrendészet és bérszámfejtés; kereskedelmi szerződések, beszerzés, forgalmazás, fizetési feltételek, versenyjog és fogyasztókkal szembeni gyakorlatok; termékmegfelelőség, vegyi anyagok, termékbiztonság, címkézés, reklám, piacfelügyelet és visszahívások; környezetvédelmi engedélyek, hulladék, csomagolás, kiterjesztett gyártói felelősség, fenntarthatóság, energia és kibocsátás; adatvédelem, kiberbiztonság, digitális szolgáltatások, mesterséges intelligencia, nyilvántartások és hatósági adatszolgáltatás; adó, vám, szankciók, kereskedelmi korlátozások, ingatlan, jogviták, közigazgatási eljárás és végrehajtás; valamint az európai uniós jog magyarországi végrehajtása.

Zárd ki a protokolláris, egyedi kinevezési, kizárólag helyi és kizárólag közszférát érintő tételeket, kivéve, ha hihető üzleti hatást keletkeztetnek.

**5. Minősítés.** Minden tételt sorolj be: **Magas** (valószínű intézkedés vagy lényeges kitettség), **Közepes** (értékelést vagy megerősítést igényel), **Alacsony** (távoli vagy kontextuális relevancia). A megerősített jogszabályszöveget szigorúan válaszd el a saját értékelésedtől. Felelősöket ne rendelj hozzá.

**6. A riport felépítése.** Egyetlen riport készül, ebben a sorrendben:
- Fejléc: lapszám, megjelenés dátuma, a hivatalos PDF linkje, oldalszámtartomány, tételszám.
- Egyértelmű megállapítás arról, hogy szükséges-e azonnali jogi teendő.
- Számláló: hány Magas / Közepes / Alacsony / Kizárt tétel van.
- **Megfigyelési lista**: a releváns tételek prioritás szerint csökkenő sorrendben. Tételenként: prioritás, oldalszám, cím, a hivatalos magyar megnevezés, a vonatkozó jogszabályszöveg szó szerinti idézete a forrás megjelölésével, majd pontosan három mező — **Joghatás**, **Relevancia**, **Hatálybalépés**. Más mező nem szerepelhet: se felelős, se határidő, se javasolt intézkedés, se függőségi megjegyzés, se csoportszintű egyeztetés.
- **Szűrési napló**: táblázat a lapszám MINDEN tételéről — azonosító, tárgy, oldal, eredmény (prioritás vagy a kizárás indoka).
- **Módszertan és korlátok**.

Módosító jogszabálynál a joghatást magyarázd el, ne a módosító szöveget ismételd. A hivatalos magyar megnevezéseket és azonosítókat szó szerint őrizd meg.

**7. Nyelv és forma.** A riport végig **magyarul** készül — a címsorok, összefoglalók, értékelések és a prioritáscímkék (Magas / Közepes / Alacsony) is. Betűtípus: **Segoe UI**. Jelöld meg: „Belső munkapéldány — az észrevételek nem részei a hivatalos közzétételnek." Tedd közzé HTML artifactként **„Magyar Közlöny <év>/<szám>" címmel** — ebből tudja a következő futás, hol tartottál —, és add meg a teljes riportot a válaszban is.

**Vizuális dizájn — arany tematika, világos és sötét módra egyaránt.** Az e-mailt teljes HTML dokumentumként építsd fel (`<html><head>…</head><body>…</body></html>`), ne csak egy törzstöredékként, hogy a `<head>`-ben elhelyezhesd az alábbi meta-tageket. Ugyanazt a palettát alkalmazd az artifactban és az e-mailben is. Minden témázott elemen adj meg egyszerre inline stílust (ez a világos alapértelmezés, ezt minden kliens — az Outlook is — látja) és egy class nevet (ezt csak a sötét módot támogató kliensek olvassák):

- A `<head>`-ben: `<meta name="color-scheme" content="light dark">` és `<meta name="supported-color-schemes" content="light dark">`, valamint egyetlen `<style>` blokk, kizárólag ezzel a tartalommal:
  ```
  @media (prefers-color-scheme: dark) {
    .bg-page   { background-color: #15110B !important; }
    .bg-card   { background-color: #241C12 !important; border-color: #B8963E !important; }
    .text-body { color: #EDE3CC !important; }
    .text-head { color: #E9C46A !important; }
    .header-band  { background-color: #241C12 !important; }
    .header-title { color: #E9C46A !important; }
    .tile-bg   { background-color: #241C12 !important; }
    .tile-label{ color: #EDE3CC !important; }
  }
  ```
  Ebben a blokkban CSS-változót ne használj — csak ezeket a class-szabályokat, a media query és az `!important` gondoskodik róla, hogy csak a sötét módot támogató kliensben írják felül az inline világos stílust, máshol (így az Outlookban is, amely a `<style>` blokkot egyébként eltávolítja) hatástalanok maradnak.
- Világos értékek (az inline alapértelmezés, ezt látja mindig az Outlook is): fejléc sáv tömör `#1F1710` háttér, `#F0D999` félkövér címszöveg; törzs háttér `#FFFFFF` vagy `#FFFDF8`, törzsszöveg `#2B2118`; kártya- és táblázatszegély tömör, 1–2 px, `#D4B96A`; alcímek és linkek `#A67C00`, félkövéren; számlálócsempe fehér vagy krémszínű mezőn `#D4B96A` felső szegéllyel, címke `#2B2118`.
- Sötét értékek (csak a fenti override blokkon keresztül érvényesülnek, ugyanazon a szerkezeten): törzs háttér `#15110B`, törzsszöveg `#EDE3CC`; kártya háttér `#241C12`, szegély `#B8963E`; fejléc sáv háttér `#241C12`, címszöveg `#E9C46A`; alcímek és linkek `#E9C46A`; számlálócsempe háttér `#241C12`, címke `#EDE3CC`.
- A prioritási jelvények mindkét témában azonosak — saját tömör háttérszínt hordoznak, ezért a környező téma nem befolyásolja a kontrasztjukat: **Magas** `#8B1E1E`, **Közepes** `#A67C00`, **Alacsony** `#6B5A2E`, **Kizárva** (csak a szűrési naplóban) `#5B5346`, mindegyik fehér, félkövér szöveggel.
- Halvány árnyalatot sötét szöveggel sehol ne használj — ez hatott kimosottnak az Outlook világos témájában. Kerekített sarkot, színátmenetet és árnyékot ne használj olyan helyen, ahol a jelentés ettől függne — az Outlook asztali kliense ezeket kiszámíthatatlanul jeleníti meg. A biztonságos alapértelmezés mindkét témában az egyszerű téglalap és a tömör kitöltés.

**8. E-mail — minden elkészült riportot el kell küldeni a denes.grossman@henkel.com címre.** Használd a Gmail (vagy más e-mail) eszközt.
- Tárgy: `Magyar Közlöny <év>. évi <szám>. szám — napi jogi átvilágítás (<n> magas / <n> közepes / <n> alacsony)`
- Törzs: a teljes riport önhordó HTML-ként (`htmlBody`), minden elemen beágyazott (inline) stílussal, Segoe UI betűtípussal. Külső stíluslap és CSS-változó nem használható. A fenti sötétmód-override blokkon kívül más `<style>` blokkot ne használj — az e-mail kliensek eltávolítják. Flex és grid helyett `<table>` elrendezést használj. Adj meg egyszerű szöveges `body` változatot is.
- **A HTML-t közvetlenül, teljes szövegével írd be a `htmlBody` paraméterbe.** Soha ne hivatkozz rá fájlútvonallal, és soha ne használj shell-behelyettesítést (például `$(cat valami.html)`): a levélküldő eszköz paramétere nem shell, a behelyettesítés nem fut le, és a címzett a nyers `$(cat ...)` szöveget kapja meg. Ha a riportot előbb fájlba írtad, olvasd vissza, és a tartalmát illeszd be a paraméterbe.
- **Egy riporthoz egy levél megy.** Küldés előtt győződj meg róla, hogy a törzs a kész HTML. Ha mégis hibás levél ment ki, a javítottat küldd válaszként ugyanabba a levélszálba (`replyThreadId`), ne új szálként.
- Az artifact linkjét soha ne küldd el a riport helyett: az privát, külső címzettnél nem nyílik meg.
- Ha ebben a futásban nincs elérhető e-mail eszköz, azt a válaszod legelején írd ki — nevezd meg, hogy a riportot nem sikerült elküldeni a denes.grossman@henkel.com címre. Soha ne hagyd ki csendben.

**9. Zárás.** A válasz végén szerepeljen: melyik lapszámot vizsgáltad át, hány Magas / Közepes / Alacsony tétel van, elment-e az e-mail, és milyen korlátok merültek fel. Soha ne találj ki kötelezettséget, joghatást, oldalszámot vagy idézetet. Ha a hivatalos oldal vagy a PDF nem érhető el, írd ki szó szerint a blokkolt hosztnevet, ne kerüld meg, és kérd be a hivatalos linket — nem hivatalos másolatot soha ne használj helyette.

A kimenet belső jogi szűrés, nem jogi tanácsadás. Minden szakkifejezést szigorúan a jogi definíciójának megfelelően használj, és minden rövidítést oldj fel az első előfordulásakor. A tárhelyre ne véglegesíts semmit és ne nyiss egyesítési kérelmet — ez a futás kizárólag olvasás és értékelés.
