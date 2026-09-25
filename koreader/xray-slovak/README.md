# X-Ray pre KOReader – slovenský preklad

Slovenský preklad pluginu [X-Ray pre KOReader](https://github.com/ultimatejimmy/xray.koplugin)
(verzia z commitu `fbabe4e`).

## Obsah

| Súbor | Popis |
|---|---|
| `languages/sk.po` | Preklad všetkých 477 textov používateľského rozhrania |
| `prompts/sk.lua` | Slovenské AI prompty – AI vracia popisy postáv, miest, časovej osi atď. po slovensky |
| `xray-slovak.patch` | Úplná zmena pre upstream repozitár: oba súbory vyššie, registrácia jazyka `sk` a podpora skloňovania |

## Rýchla inštalácia do čítačky

1. Skopírujte `languages/sk.po` do `koreader/plugins/xray.koplugin/languages/`.
2. Skopírujte `prompts/sk.lua` do `koreader/plugins/xray.koplugin/prompts/`.
3. Reštartujte KOReader a v ponuke **X-Ray → Nastavenia → Jazyk** zvoľte **SK**.

Samotné kopírovanie súborov stačí: plugin nájde jazyky automaticky podľa súborov `.po`.
V zozname sa jazyk zobrazí ako „SK“. Ak chcete názov „Slovenčina“ a automatické
rozpoznanie slovenčiny zo systému alebo z knihy, použite patch.
Podpora skloňovania (nižšie) je tiež iba v patchi.

## Použitie patchu (úplná integrácia)

```sh
git clone https://github.com/ultimatejimmy/xray.koplugin
cd xray.koplugin
git apply /cesta/k/xray-slovak.patch
```

Patch pridáva:
- `sk = "Slovenčina"` do zoznamu názvov jazykov (`xray_ui.lua`),
- `sk` medzi jazyky na automatické rozpoznanie (`xray_ui.lua`, `xray_aihelper.lua`),
- desatinnú čiarku pre slovenčinu v prevode jednotiek (`xray_units.lua`),
- slovenčinu do zoznamu jazykov v README,
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

Zapína sa iba vtedy, keď je **jazyk X-Ray slovenčina** alebo keď má **kniha v metadátach
slovenčinu (sk) alebo češtinu (cs)**. Pre ostatné jazyky sa správanie nemení.

Obmedzenia:
- Mená kratšie ako 4 písmená (Eva, Ján) sa neskloňujú – pri nich sa ďalej hľadá presný tvar,
  prípadne sa použije AI.
- Nepravidelné tvary a zdrobneniny (Zuzana → Zuzkou) sa nerozpoznajú.
- Výnimočne môže vzniknúť falošná zhoda (napr. „Marek“ a slovo „marketing“).

Kontrola `tools/check_translations.py` v upstream repozitári pre `sk.po` prešla.
