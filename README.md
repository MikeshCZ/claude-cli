# Claude CLI

Jednoduchá CLI aplikace pro komunikaci s Anthropic Claude API.

## Instalace

1. Stáhněte soubor `claude-cli`
2. Učiňte jej spustitelný: `chmod +x claude-cli`
3. (Volitelně) Přesuňte do PATH: `sudo mv claude-cli /usr/local/bin/`

## Nastavení

1. Získejte API klíč z https://console.anthropic.com/
2. Nastavte API klíč: `claude-cli -k VÁŠ_API_KLÍČ`
3. (Volitelně) Nastavte výchozí model: `claude-cli -d NÁZEV_MODELU`

## Použití

```bash
# Základní dotaz (použije výchozí model)
claude-cli "Jaká je vzdálenost mezi Zemí a Měsícem přepočtena na počet Škoda Fábií?"

# Použití konkrétního modelu pro jeden dotaz
claude-cli -m claude-3-haiku-20240307 "Rychlá otázka"

# Nastavení výchozího modelu
claude-cli -d claude-sonnet-4-20250514

# Zobrazit aktuální výchozí model
claude-cli --show-model

# Zobrazit nápovědu
claude-cli -h
```

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
- ✅ Markdown formátování odpovědí
- ✅ Nápověda

## Příklady

```bash
# Nastavení API klíče
claude-cli -k sk-ant-api03-...

# Nastavení výchozího modelu (uloží se do konfigurace)
claude-cli -d claude-sonnet-4-20250514

# Zobrazení aktuálního výchozího modelu
claude-cli --show-model

# Jednoduché dotazy (použije výchozí model)
claude-cli "Co je to umělá inteligence?"
claude-cli "Napiš krátkou báseň"

# Použití konkrétního modelu pro jeden dotaz
claude-cli -m claude-3-haiku-20240307 "Rychlá otázka"

# Bez formátování (raw markdown)
claude-cli --no-format "Zobraz mi markdown syntax"
```

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