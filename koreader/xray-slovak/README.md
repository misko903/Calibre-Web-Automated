# X-Ray pre KOReader – slovenský preklad

Slovenský preklad pluginu [X-Ray pre KOReader](https://github.com/ultimatejimmy/xray.koplugin)
(verzia z commitu `fbabe4e`).

## Obsah

| Súbor | Popis |
|---|---|
| `languages/sk.po` | Preklad všetkých 477 textov používateľského rozhrania |
| `prompts/sk.lua` | Slovenské AI prompty – AI vracia popisy postáv, miest, časovej osi atď. po slovensky |
| `xray-slovak.patch` | Úplná zmena pre upstream repozitár (oba súbory vyššie + registrácia jazyka `sk` v kóde a README) |

## Rýchla inštalácia do čítačky

1. Skopírujte `languages/sk.po` do `koreader/plugins/xray.koplugin/languages/`.
2. Skopírujte `prompts/sk.lua` do `koreader/plugins/xray.koplugin/prompts/`.
3. Reštartujte KOReader a v ponuke **X-Ray → Nastavenia → Jazyk** zvoľte **SK**.

Samotné kopírovanie súborov stačí: plugin nájde jazyky automaticky podľa súborov `.po`.
V zozname sa jazyk zobrazí ako „SK“. Ak chcete názov „Slovenčina“ a automatické
rozpoznanie slovenčiny zo systému alebo z knihy, použite patch.

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
- slovenčinu do zoznamu jazykov v README.

Kontrola `tools/check_translations.py` v upstream repozitári pre `sk.po` prešla.
