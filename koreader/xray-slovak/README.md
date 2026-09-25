# X-Ray pre KOReader – slovenský a český preklad

Slovenský a český preklad pluginu [X-Ray pre KOReader](https://github.com/ultimatejimmy/xray.koplugin)
(verzia z commitu `fbabe4e`).

## Obsah

| Súbor | Popis |
|---|---|
| `languages/sk.po`, `languages/cs.po` | Preklad všetkých 477 textov používateľského rozhrania (slovenčina, čeština) |
| `prompts/sk.lua`, `prompts/cs.lua` | AI prompty – AI vracia popisy postáv, miest, časovej osi atď. po slovensky / po česky |
| `xray-slovak.patch` | Úplná zmena pre upstream repozitár: súbory vyššie, registrácia jazykov `sk` a `cs` a podpora skloňovania |

## Rýchla inštalácia do čítačky

1. Skopírujte `languages/sk.po` (a/alebo `cs.po`) do `koreader/plugins/xray.koplugin/languages/`.
2. Skopírujte `prompts/sk.lua` (a/alebo `cs.lua`) do `koreader/plugins/xray.koplugin/prompts/`.
3. Reštartujte KOReader a v ponuke **X-Ray → Nastavenia → Jazyk** zvoľte **SK** alebo **CS**.

Samotné kopírovanie súborov stačí: plugin nájde jazyky automaticky podľa súborov `.po`.
V zozname sa jazyk zobrazí ako „SK“/„CS“. Ak chcete názvy „Slovenčina“/„Čeština“ a automatické
rozpoznanie jazyka zo systému alebo z knihy, použite patch.
Podpora skloňovania (nižšie) je tiež iba v patchi.

## Použitie patchu (úplná integrácia)

```sh
git clone https://github.com/ultimatejimmy/xray.koplugin
cd xray.koplugin
git apply /cesta/k/xray-slovak.patch
```

Patch pridáva:
- `sk = "Slovenčina"` a `cs = "Čeština"` do zoznamu názvov jazykov (`xray_ui.lua`),
- `sk` a `cs` medzi jazyky na automatické rozpoznanie (`xray_ui.lua`, `xray_aihelper.lua`),
- desatinnú čiarku pre slovenčinu a češtinu v prevode jednotiek (`xray_units.lua`),
- slovenčinu a češtinu do zoznamu jazykov v README,
- podporu skloňovania (pozri nižšie),
- testy pre skloňovanie (`spec/`).

## Skloňovanie mien a miest

Bez patchu plugin porovnáva iba presný text, takže „Petra“ nenájde postavu „Peter“.
Patch pridáva:

- **Ťuknutie na slovo:** ak sa nenájde presná zhoda, plugin skúsi skloňované tvary:
  „Petrovi“ → Peter, „Janka“ → Janko, „Bratislave“ → Bratislava, „Pavlom“ → Pavol.
  Až potom ponúkne načítanie z AI.
- **Nájsť zmienky:** okrem presného mena sa hľadajú aj skloňované tvary
  („Peter“ nájde aj „Petra“, „Petrovi“; „Bratislava“ aj „Bratislave“).
- **Veľké Ľ, Ĺ, Ŕ** sa správne prevádzajú na malé písmená.

Zapína sa iba vtedy, keď je **jazyk X-Ray slovenčina alebo čeština** alebo keď má **kniha v metadátach
slovenčinu (sk) alebo češtinu (cs)**. Pre ostatné jazyky sa správanie nemení.

Obmedzenia:
- Mená kratšie ako 4 písmená (Eva, Ján) sa neskloňujú – pri nich sa ďalej hľadá presný tvar,
  prípadne sa použije AI.
- Nepravidelné tvary, zdrobneniny a zmeny spoluhlásky (Zuzana → Zuzkou, Praha → Praze) sa nerozpoznajú.
  Pri ťuknutí na slovo ich však rozpozná načítanie z AI, ktoré vráti základný tvar (1. pád).
- Výnimočne môže vzniknúť falošná zhoda (napr. „Marek“ a slovo „marketing“).

Kontrola `tools/check_translations.py` v upstream repozitári pre `sk.po` aj `cs.po` prešla.
