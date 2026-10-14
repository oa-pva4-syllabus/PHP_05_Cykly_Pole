# PVA4 — PHP 05: Cykly a pole

Obsahem repozitáře je cvičení k druhé části přednášky *Podmínky a cykly*.
Navazuje na `PHP_04_Podminky` — pokračujete ve fiktivní aplikaci
**Maturita**, portálu pro maturitní témata a konzultace.

Na začátku máte aplikaci ve stavu po minulém cvičení: stránky se
rozhodují podle denní doby, termínu i stavu tématu. Pořád v nich ale
spousta kódu **opakuje** totéž: tři ručně psané řádky tabulky, dvě
ručně vložené karty, tři ručně psané odkazy v menu. Témat v tabulce teď
přibylo na osm a k Profilu přibyl seznam konzultací. Dnes to všechno
vypíšete **cykly**.

## Struktura projektu

```
/includes
  config.php        ← ÚKOL 1: položky menu v poli
/templates
  menu.php          ← ÚKOL 1: odkazy menu cyklem
  header.php, footer.php, metric-card.php   ← beze změny
index.php           ← Přehled: ÚKOL 2, 3, 4
aboutme.php         ← Profil: ÚKOL 5, 6, 7
contact.php         ← Kontakt (beze změny)
odpovedi.md         ← odpovědi na otázky z úkolů
```

## Jak postupovat

Zadání úkolů je níže v tomto README. V kódu najdete **značky**
`// --- ÚKOL N: … ---` (v PHP) a `<!-- ÚKOL N: … -->` (v HTML), které
ukazují, kam řešení patří. Komentáře `(z PHP_04)` označují kód z minula —
ten neměníte.

| Úkol | Soubor | Co děláte |
| --- | --- | --- |
| 1 | `config.php`, `menu.php` | Menu z pole — `foreach` s klíčem **[+ odpověď]** |
| 2 | `index.php` | Tabulka témat — `foreach` místo ručně psaných řádků |
| 3 | `index.php` | Známka slovně (`match` v cyklu) a střídání barvy řádků |
| 4 | `index.php` | Statistiky — součet, minimum, maximum v cyklu, karty v cyklu |
| 5 | `aboutme.php` | Konzultace — počítání, `continue` a `break` |
| 6 | `aboutme.php` | Postup konzultací — cyklus `for` |
| 7 | `aboutme.php` | **Bonus:** plán psaní — cyklus `while` **[+ odpověď]** |
| 8 | `odpovedi.md` | Co vypíše? — `break` a `continue` **[odpověď]** |

Úkoly označené **[+ odpověď]** chtějí kromě kódu i krátkou odpověď do
souboru `odpovedi.md` pod nadpis daného úkolu.

Vlastní funkce ani formuláře zatím neznáte a nepotřebujete je.
Vestavěné funkce `count()` a `round()` používat smíte, `min()`, `max()`
a `array_sum()` v tomto cvičení **ne** — počítáte to cyklem.

## Než začnete

V `includes/config.php` přepište `yourName` na své jméno a verzi
aplikace zvyšte na `1.3`.

## Zadání

### ÚKOL 1 · Menu z pole (`config.php`, `templates/menu.php`)

Minule jste do menu přidávali odkaz na Kontakt ručně: zkopírovat řádek
`<a>`, změnit adresu, text a podmínku aktivní stránky. Teď z menu
uděláte pole.

Do pole v `includes/config.php` přidejte klíč `menu`. Klíčem položky je
název stránky bez `.php` (stejný, jaký stránky nastavují do
`$activePage`), hodnotou text odkazu:

```php
'menu' => [
    'index' => 'Přehled',
    'aboutme' => 'Profil',
    'contact' => 'Kontakt',
],
```

V `templates/menu.php` pak tři ručně psané odkazy nahraďte **jedním**
odkazem uvnitř cyklu `foreach ($config['menu'] as $page => $label)`.
Adresa odkazu je `$page` + `.php`, text `$label` a třídu vypíšete stejným
ternárním operátorem jako dosud, jen místo pevného `'index'` porovnáte
s `$page`.

HTML uvnitř cyklu zapíšete tak, že PHP blok po otevírací závorce
ukončíte a zavírací závorku dáte do vlastního bloku:

```php
<?php foreach ($config['menu'] as $page => $label) { ?>
  <a href="…">…</a>
<?php } ?>
```

Text odkazu na Profil je teď jen `Profil` (bez jména) — jméno vidíte
v nadpisu Profilu.

**Očekávaný výstup (zdrojový kód):** na každé stránce tři odkazy, aktivní
má třídu `text-brand-600`, např. na `index.php`
`<a href="index.php" class="text-brand-600">Přehled</a>`.

**Do `odpovedi.md`:** Kolik souborů a kolik řádků byste teď upravili,
abyste přidali do menu odkaz na novou stránku? Kolik to bylo v PHP_06?

### ÚKOL 2 · Tabulka historických témat (`index.php`)

Pole `$topics` má teď osm témat, tabulka ale vypisuje jen tři ručně
psané řádky. Smažte je a nahraďte **jedním** řádkem `<tr>` uvnitř cyklu
`foreach ($topics as $topic)`. Místo `$topics[0]['title']` vypisujete
`$topic['title']` atd.

**Očekávaný výstup:** tabulka má osm řádků, poslední je
`E-shop s výrobky studentské firmy` — `David Novotný` — `2025` — `4`.
Přidáte-li do pole deváté téma, objeví se v tabulce bez další úpravy.

### ÚKOL 3 · Známka slovně a střídání řádků (`index.php`)

1. Ve sloupci Známka vypište za číslem slovní hodnocení v závorce, např.
   `1 (výborně)`. Slovo získáte výrazem `match` **uvnitř cyklu**:
   `1` výborně, `2` chvalitebně, `3` dobře, `4` dostatečně,
   `5` nedostatečně, jinak `neznámá známka`.
2. Aby se dlouhá tabulka dobře četla, má každý druhý řádek šedé pozadí.
   Změňte hlavičku cyklu na `foreach ($topics as $index => $topic)` —
   `$index` je pořadí prvku od nuly. Řádek s **lichým** indexem
   (`$index % 2 === 1`) dostane `<tr class="bg-gray-50">`, ostatní
   `<tr class="bg-white">`.

Kód pro `match` a třídu řádku patří do PHP bloku s otevírací závorkou
cyklu, před HTML řádku.

**Očekávaný výstup (zdrojový kód):** řádky se střídají
`bg-white`, `bg-gray-50`, `bg-white`, …; v tabulce je `2 (chvalitebně)`
a `4 (dostatečně)`.

### ÚKOL 4 · Statistiky témat (`index.php`)

Pod značkou `ÚKOL 4: statistiky témat` jsou připravené proměnné.
Jedním cyklem `foreach` přes `$topics` spočítejte:

- `$gradeSum` — součet všech známek,
- `$bestGrade` — nejlepší (nejmenší) známku,
- `$worstGrade` — nejhorší (největší) známku.

Pozor na výchozí hodnotu: kdyby `$bestGrade` začínal na `0`, žádná
známka by nebyla menší. Před cyklem proto do `$bestGrade` i `$worstGrade`
uložte známku **prvního** tématu (`$topics[0]['grade']`).

Za cyklem spočítejte průměr: `round($gradeSum / count($topics), 2)`.

Pak u značky `ÚKOL 4: karty vložte v cyklu` nahraďte dvě ručně vložené
karty polem `$cards` a cyklem. Každá karta je pole s klíči `label`
a `value`:

| `label` | `value` |
| --- | --- |
| `Dostupná témata` | `$availableTopics` |
| `Moje konzultace` | `$myConsultations` |
| `Průměrná známka` | `$averageGrade` |
| `Nejlepší / nejhorší známka` | `$bestGrade . ' / ' . $worstGrade` |

V cyklu přes `$cards` nastavte `$cardLabel` a `$cardValue` a vložte
`templates/metric-card.php` — stejně jako minule, jen jednou.

**Očekávaný výstup:** čtyři karty, `Průměrná známka` **2.25**,
`Nejlepší / nejhorší známka` **1 / 4**.

### ÚKOL 5 · Konzultace (`aboutme.php`)

Na Profilu přibylo pole `$consultations`. Každá konzultace má datum,
téma a stav: `done` (absolvovaná), `planned` (naplánovaná) nebo
`cancelled` (zrušená).

1. **Počet absolvovaných.** Pod značkou `ÚKOL 5` cyklem spočítejte do
   `$myConsultations`, kolik konzultací má stav `done`. Karta *Odevzdání
   práce* pak ukáže `Konzultace: 2 z 3`.
2. **Nejbližší konzultace.** Druhým cyklem najděte **první** konzultaci se
   stavem `planned` a uložte do `$nextConsultation` text
   `datum – téma`. Jakmile ji najdete, cyklus ukončete příkazem `break` —
   další naplánované už nepotřebujete.
3. **Seznam.** V kartě *Konzultace* u značky `<!-- ÚKOL 5 -->` vypište
   cyklem každou konzultaci jako položku seznamu:

   ```html
   <li>24. 9. 2026 – Výběr tématu (absolvována)</li>
   ```

   Stav vypište česky `absolvována` / `naplánována`. Zrušené konzultace
   ve výpisu **nebudou** — přeskočte je příkazem `continue`.

**Očekávaný výstup:** `Konzultace: 2 z 3`, seznam se čtyřmi položkami
(bez `Analýza zadání`), `Nejbližší konzultace: 5. 11. 2026 – Návrh databáze`.

### ÚKOL 6 · Postup konzultací (`aboutme.php`)

Pod řádek `Konzultace: 2 z 3` přidejte řadu očíslovaných koleček —
jedno za každou **povinnou** konzultaci. Kolečka absolvovaných
konzultací jsou modrá, zbylá šedá. Počet opakování znáte předem
(`$requiredConsultations`), proto použijte cyklus `for`
s čítačem `$i` od 1:

```html
<div class="mt-3 flex gap-2">
  <!-- jedno kolečko pro každé $i -->
  <span class="flex h-8 w-8 items-center justify-center rounded-full text-sm font-medium bg-brand-600 text-white">1</span>
</div>
```

Kolečko s číslem `$i <= $myConsultations` má třídy
`bg-brand-600 text-white`, ostatní `bg-gray-100 text-gray-400`.

**Očekávaný výstup:** kolečka `1` a `2` modrá, `3` šedé.

### ÚKOL 7 · Bonus: plán psaní (`aboutme.php`) [+ odpověď]

Práce má mít `$pagesTotal` stran a vy zvládnete `$pagesPerWeek` stran
týdně. Za kolik týdnů ji napíšete? Počet týdnů dopředu neznáte, proto
použijte cyklus `while`: dokud je napsaných stran méně než
`$pagesTotal`, přičtěte týdenní počet stran a zvyšte `$weeksNeeded`
o jedna. (Šlo by to i dělením — tady jde o cyklus.)

Do karty *Odevzdání práce* u značky `<!-- ÚKOL 7 -->` vypište:

```
Při 4 stranách týdně napíšete 30 stran za 8 týdnů.
```

(Všechna čísla vypisujte z proměnných.)

**Do `odpovedi.md`:** Co by se vypsalo, kdyby `$pagesTotal` bylo `0`?
A co kdybyste místo `while` použili `do...while`? Proč?

### ÚKOL 8 · Co vypíše? [odpověď]

Do `odpovedi.md` napište, co vypíše úryvek s `break` a `continue`, a
proč. Nejdřív tipněte, pak vyzkoušejte.

## Jak spustit stránku

**Přes FlyEnv:** nastavte tuto složku jako document root webu a otevřete
`http://localhost/index.php` (port si ověřte v nastavení FlyEnv).

**Bez nastavování** — vestavěný server PHP, ve složce s cvičením spusťte:

```sh
php -S localhost:8000
```

…a otevřete `http://localhost:8000/index.php`. Parametry z minula
(`?hour=`, `?days=`, `aboutme.php?status=approved`) fungují dál.

## Konvence, které se od vás čekají

Stejné jako v předchozích cvičeních, navíc z přednášky o cyklech:

- **Pole procházíte `foreach`**, `for` jen tam, kde potřebujete čítač
  (známý počet opakování).
- **Proměnná cyklu v jednotném čísle**, pole v množném:
  `foreach ($topics as $topic)`, `foreach ($cards as $card)`.
- **Složené závorky** píšete u cyklu i podmínky vždy.
- Uvnitř `while` se musí měnit to, na čem podmínka závisí — jinak se
  stránka zacyklí.

## Jak poznáte, že to máte správně

- Žádná stránka neobsahuje ručně opakované řádky tabulky, karty ani odkazy
  menu — všechno vypisuje cyklus.
- Přidáte-li téma do `$topics`, objeví se v tabulce a změní statistiky.
- Na žádné stránce se neobjeví `Warning` ani `Fatal error`.

## Odevzdání

Práci odevzdáváte do svého repozitáře přes **Classroom 50**:

```sh
git add .
git commit -m "Cviceni 05 - vypracovano"
git push
```
