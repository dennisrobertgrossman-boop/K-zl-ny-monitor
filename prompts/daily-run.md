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

Végezd el a Magyar Közlöny napi jogi átvilágítását a Henkel Magyarország Kft. belső jogi csapata számára.

**1. A legfrissebb lapszám azonosítása.** Nyisd meg a https://magyarkozlony.hu/ oldalt, és keresd meg a legfrissebb **Magyar Közlöny** lapszámot — nem a Hivatalos Értesítőt és nem mellékletet. Ellenőrizd a lapszámot, a megjelenés dátumát és a hivatalos PDF közvetlen linkjét. Az előző futás óta megjelent minden lapszámot dolgozd fel, a legfrissebbel kezdve.

**Hogy tudod meg, hol tartott az előző futás:** listázd ki a közzétett artifactjaidat, és keresd a „Magyar Közlöny <év>/<szám>" című oldalakat. A legnagyobb sorszámú ilyen artifact nevezi meg az utoljára feldolgozott lapszámot. Ha egyetlen ilyen artifact sincs, csak a legfrissebb lapszámot dolgozd fel.

**2. Ha nincs új lapszám az előző futás óta, ne készíts semmit** — se riportot, se artifactot, se e-mailt. Írj egy sort arról, melyik a legfrissebb lapszám és mikor jelent meg, és állj meg. Riport csak új lapszám esetén készül.

**3. A teljes szöveg beolvasása.** Ne mentsd el a PDF-et és ne készíts jegyzetelt munkapéldányt. Olvasd be a hivatalos PDF teljes szövegét — mellékletekkel, táblázatokkal, átmeneti rendelkezésekkel és hatálybaléptető rendelkezésekkel együtt —, és tartsd nyilván az oldalszámokat, hogy minden megállapítás visszahivatkozható legyen.

A WebFetch eszköz NEM működik a magyarkozlony.hu oldalra (EGRESS_BLOCKED hibával elutasítja, mert megkerüli a környezet engedélyezési listáját). Használj helyette curl-t, szövegkinyerőbe csővezetve:

```
curl -sSL "<hivatalos PDF link>" | pdftotext -layout - issue.txt
```

Ha a pdftotext hiányzik: `apt-get update && apt-get install -y poppler-utils`. A kezdőlapot is curl-lel töltsd le: `curl -sSL https://magyarkozlony.hu/`.

**4. Szűrés.** Vizsgáld át az egész lapszámot a Henkel-relevanciamérce szerint: társasági irányítás, nyilvántartások, jelentéstétel és cégcsoporton belüli megállapodások; foglalkoztatás, juttatások, munkavédelem, idegenrendészet és bérszámfejtés; kereskedelmi szerződések, beszerzés, forgalmazás, fizetési feltételek, versenyjog és fogyasztókkal szembeni gyakorlatok; termékmegfelelőség, vegyi anyagok, termékbiztonság, címkézés, reklám, piacfelügyelet és visszahívások; környezetvédelmi engedélyek, hulladék, csomagolás, kiterjesztett gyártói felelősség, fenntarthatóság, energia és kibocsátás; adatvédelem, kiberbiztonság, digitális szolgáltatások, mesterséges intelligencia, nyilvántartások és hatósági adatszolgáltatás; adó, vám, szankciók, kereskedelmi korlátozások, ingatlan, jogviták, közigazgatási eljárás és végrehajtás; valamint az európai uniós jog magyarországi végrehajtása.

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

**8. E-mail — minden elkészült riportot el kell küldeni a denes.grossman@henkel.com címre.** Használd a Gmail (vagy más e-mail) eszközt.
- Tárgy: `Magyar Közlöny <év>. évi <szám>. szám — napi jogi átvilágítás (<n> magas / <n> közepes / <n> alacsony)`
- Törzs: a teljes riport önhordó HTML-ként (`htmlBody`), minden elemen beágyazott (inline) stílussal, Segoe UI betűtípussal. Külső stíluslap, CSS-változó és `<style>` blokk nem használható — az e-mail kliensek eltávolítják. Flex és grid helyett `<table>` elrendezést használj. Adj meg egyszerű szöveges `body` változatot is.
- Az artifact linkjét soha ne küldd el a riport helyett: az privát, külső címzettnél nem nyílik meg.
- Ha ebben a futásban nincs elérhető e-mail eszköz, azt a válaszod legelején írd ki — nevezd meg, hogy a riportot nem sikerült elküldeni a denes.grossman@henkel.com címre. Soha ne hagyd ki csendben.

**9. Zárás.** A válasz végén szerepeljen: melyik lapszámot vizsgáltad át, hány Magas / Közepes / Alacsony tétel van, elment-e az e-mail, és milyen korlátok merültek fel. Soha ne találj ki kötelezettséget, joghatást, oldalszámot vagy idézetet. Ha a hivatalos oldal vagy a PDF nem érhető el, írd ki szó szerint a blokkolt hosztnevet, ne kerüld meg, és kérd be a hivatalos linket — nem hivatalos másolatot soha ne használj helyette.

A kimenet belső jogi szűrés, nem jogi tanácsadás. Minden szakkifejezést szigorúan a jogi definíciójának megfelelően használj, és minden rövidítést oldj fel az első előfordulásakor. A tárhelyre ne véglegesíts semmit és ne nyiss egyesítési kérelmet — ez a futás kizárólag olvasás és értékelés.
