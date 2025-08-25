<a href="https://www.buymeacoffee.com/michalsara" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-red.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>


# Claude-CLI

 ██████╗██╗      █████╗ ██╗   ██╗██████╗ ███████╗       ██████╗██╗     ██╗
██╔════╝██║     ██╔══██╗██║   ██║██╔══██╗██╔════╝      ██╔════╝██║     ██║
██║     ██║     ███████║██║   ██║██║  ██║█████╗  █████╗██║     ██║     ██║
██║     ██║     ██╔══██║██║   ██║██║  ██║██╔══╝  ╚════╝██║     ██║     ██║
╚██████╗███████╗██║  ██║╚██████╔╝██████╔╝███████╗      ╚██████╗███████╗██║
 ╚═════╝╚══════╝╚═╝  ╚═╝ ╚═════╝ ╚═════╝ ╚══════╝       ╚═════╝╚══════╝╚═╝

Jednoduchá CLI aplikace pro komunikaci s Anthropic Claude API.

## Instalace

1. Stáhněte soubor `claude-cli`: `curl -O https://raw.githubusercontent.com/MikeshCZ/claude-cli/main/claude-cli`
2. Učiňte jej spustitelný: `chmod +x claude-cli`
3. (Volitelně) Zkopírujte do PATH: `sudo cp claude-cli /usr/local/bin/`

## Nastavení

1. Získejte API klíč z https://console.anthropic.com/
2. Nastavte API klíč: `claude-cli -k VÁŠ_API_KLÍČ`
3. (Volitelně) Nastavte výchozí model: `claude-cli -d NÁZEV_MODELU`

## Požadavky

- `curl` - pro HTTP požadavky
- `jq` - pro práci s JSON
- `glow` - volitelné pro lepší formátování odpovědí

### Instalace závislostí

**macOS:**
```bash
brew install jq glow
```

**Ubuntu/Debian:**
```bash
sudo apt-get install jq curl glow
```

## Funkce

- ✅ Jednoduché dotazy na Claude API
- ✅ Konfigurace API klíče
- ✅ Výběr modelu pro jednotlivé dotazy
- ✅ Nastavení výchozího modelu
- ✅ Zobrazení aktuálního výchozího modelu
- ✅ Zobrazení dostupných modelů z API
- ✅ Markdown formátování odpovědí
- ✅ Kontextové okno pro pokračování konverzace
- ✅ Permanentní zapnutí/vypnutí kontextového okna (toggle)
- ✅ Historie konverzace ukládaná v ~/.claude-cli-history
- ✅ Restart chatu s vymazáním historie
- ✅ Pipe vstup ze stdin - podporuje kombinaci příkazů s `|`
- ✅ Nápověda

<p align="center">
  <img src="screenshot.png" alt="Screenshot">
</p>

## Použití & příklady

```bash
# Základní dotaz (použije výchozí model)
claude-cli "Jaká je vzdálenost mezi Zemí a Měsícem přepočtena na počet Škoda Fábií?"
claude-cli "Co je to umělá inteligence?"
claude-cli "Napiš krátkou báseň"

# Použití konkrétního modelu pro jeden dotaz
claude-cli -m claude-3-haiku-20240307 "Rychlá otázka"

# Zobrazit aktuální výchozí model
claude-cli --show-model

# Zobrazit dostupné modely z API
claude-cli --list-models

# Nastavení výchozího modelu (uloží se do konfigurace)
claude-cli -d claude-sonnet-4-0

# Zapnutí kontextového okna permanentně
claude-cli --toggle-context

# Restart chatu - vymazání historie
claude-cli --restart-chat

# Zobrazit nápovědu
claude-cli -h

# Nastavení API klíče
claude-cli -k sk-ant-api03-...

# Bez formátování (raw markdown)
claude-cli --no-format "Zobraz mi markdown syntax"

# Pipe vstup ze stdin
git log --oneline | head -5 | claude-cli "Přelož tyto commit messages do češtiny"
cat soubor.txt | claude-cli "Shrň obsah tohoto souboru"
echo "Hello world" | claude-cli "Přelož do češtiny"

# Kombinace pipe vstupu s dotazem
ls -la | claude-cli "Vysvětli mi tyto soubory a adresáře"
```

## Pipe vstup (Stdin)

Claude CLI podporuje příjem dat přes pipe (stdin), což umožňuje snadnou integraci s ostatními příkazy:

### Příklady použití

```bash
# Pouze pipe vstup (bez dotazu)
cat dokument.txt | claude-cli

# Pipe vstup + dotaz
git diff | claude-cli "Zkontroluj tyto změny kódu"

# Zadání projektu, které je pro tyto typy dotazů stejné
cat projekt.txt | claude-cli "Dotaz k tomuto projektu"

# Systémové příkazy s pipe
ps aux | claude-cli "Vysvětli mi tyto procesy. Je tam něco podezřelého?"
df -h | claude-cli "Vyhodnoť stav disku"

# Git příkazy
git log --oneline | head -10 | claude-cli "Shrň poslední změny"
git status | claude-cli "Co mám udělat s těmito změnami?"

# Ostatní  
curl -s https://api.github.com/users/MikeshCZ | claude-cli "Shrň informace o tomto uživateli"
```

### Jak to funguje

- Claude-CLI automaticky detekuje, zda přichází vstup z pipe
- Pipe vstup se kombinuje s dotazem (pokud je poskytnut)
- Pokud není zadán dotaz, použije se pouze pipe vstup
- Pipe vstup se zobrazí před dotazem, oddělený prázdným řádkem

## Kontextové okno (Context Window)

Claude CLI podporuje pokračování konverzace pomocí kontextového okna:

### Jak funguje

- **Defaultně vypnuto**: Bez aktivace každý dotaz je nezávislý
- **Historie se ukládá**: Při zapnutí se konverzace ukládá do `~/.claude-cli-history`
- **Persistentní nastavení**: Jednou zapnuto, zůstává aktivní pro všechny příkazy
- **Efektivní správa tokenů**: Uchovává pouze posledních 20 zpráv pro optimalizaci
- **Bezpečnost**: Historie je uložena s právy 600 (pouze vlastník může číst)

### Zapnutí/vypnutí kontextového okna

#### Permanentní aktivace (doporučený způsob)

```bash
# Zapnutí kontextového okna permanentně
claude-cli --toggle-context

# Po zapnutí fungují všechny dotazy s kontextem automaticky
claude-cli "Ahoj Claude!"
claude-cli "Co jsi říkal v předchozí zprávě?"

# Vypnout kontextové okno
claude-cli --toggle-context
```

#### Dočasné použití

```bash
# Vynutit kontext jen pro tento jeden dotaz
claude-cli --context-window "Tvůj dotaz s kontextem"
claude-cli --context-window "Na co jsem se ptal?"

# Další dotaz už bude bez kontextu (pokud není zapnut toggle)
claude-cli "Normální dotaz"
```

### Správa historie

```bash
# Vymazání historie konverzace
claude-cli --restart-chat

# Zobrazení stavu kontextového okna
cat ~/.claude-cli-history
```

### Tip

- Pro pohodlné používání: `claude-cli --toggle-context` jednou zapne, pak už nemusíš zadávat žádné parametry
- Pro dlouhodobé konverzace doporučujeme občasný restart pomocí `--restart-chat` pro udržení relevantnosti kontextu
- **Pozor, s každým dotazem se posílá i historie a zvyšuje se tím spotřeba input tokenů!**

## Markdown formátování

Aplikace automaticky obarvuje a formátuje odpovědi s následujícími prvky:

### Nadpisy

- `# Nadpis` → ▶ **Nadpis** (fialově tučně)
- `## Nadpis` → ▶ **Nadpis** (modře tučně)  
- `### Nadpis` → ▶ **Nadpis** (tyrkysově tučně)
- `#### Nadpis` → ▶ **Nadpis** (bíle tučně)

### Text formátování

- `**tučný text**` → **tučný text**
- `*kurzíva*` → *kurzíva* 
- `~~přeškrtnuté~~` → ~~přeškrtnuté~~ (šedě)
- `\`inline kód\`` → `kód` (bílý text na šedém pozadí)

### Bloky a struktury

- \`\`\`kód blok\`\`\` → zelený rámek s názvem jazyka
- `> citace` → žlutý pruh s kurzívou
- `- seznam` → tyrkysová odrážka •
- `1. číslovaný` → modrá čísla
- `---` → horizontální čára
- `[odkaz](url)` → podtržený modrý text s URL

### Ovládání

- **Výchozí**: Barevné formátování zapnuto
- `--no-format`: Raw markdown bez formátování
- **Glow**: Pokud je nainstalován, použije se automaticky

## 🧑‍💻 Autor

- [Více o autorovi](https://www.michalsara.cz)

## ☕ Pokud se vám tato repository líbí, můžete **[mi koupit kafe](https://www.buymeacoffee.com/michalsara)**. Díky!