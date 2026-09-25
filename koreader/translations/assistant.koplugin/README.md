# assistant.koplugin – opravený slovenský preklad

Opravený slovenský preklad (`sk`) pre KOReader doplnok
[assistant.koplugin](https://github.com/omer-faruq/assistant.koplugin)
(verzia 1.19-dev, commit `660cdfe`).

Pôvodný strojový preklad obsahoval desiatky reťazcov v slovinčine
(„Kopiraj“, „Posodobi“, „Da“/„Ne“, „V redu“…), niekoľko úplne chybných
prekladov (napr. „Testing connection…“ → „Osnovni URL“) a nejednotnú
terminológiu. Opravených bolo 253 z 422 reťazcov.

## Inštalácia

Skopírujte obsah priečinka `l10n/sk/` do nainštalovaného doplnku v čítačke
a prepíšte existujúce súbory:

```
koreader/plugins/assistant.koplugin/l10n/sk/assistant.po
koreader/plugins/assistant.koplugin/l10n/sk/assistant.mo
```

Potom reštartujte KOReader (jazyk rozhrania musí byť slovenčina).

`assistant.mo` je skompilovaný z `assistant.po` (`msgfmt -c -o assistant.mo assistant.po`).

## Licencia

Preklad je odvodené dielo doplnku assistant.koplugin a je šírený pod rovnakou
licenciou GNU GPL v3.
