# Claude CLI

Jednoduchá CLI aplikace pro komunikaci s Anthropic Claude API.

## Instalace

1. Stáhněte soubor `claude-cli`
2. Učiňte jej spustitelný: `chmod +x claude-cli`
3. (Volitelně) Přesuňte do PATH: `sudo mv claude-cli /usr/local/bin/`

## Nastavení

1. Získejte API klíč z https://console.anthropic.com/
2. Nastavte API klíč: `claude-cli -k VÁŠ_API_KLÍČ`

## Použití

```bash
# Základní dotaz
claude-cli "Jaká je vzdálenost mezi Zemí a Měsícem přepočtena na počet Škoda Fábií?"

# Použití konkrétního modelu
claude-cli -m claude-3-haiku-20240307 "Rychlá otázka"

# Zobrazit nápovědu
claude-cli --help
```

## Požadavky

- `curl` - pro HTTP požadavky
- `jq` - pro práci s JSON

### Instalace závislostí

**macOS:**
```bash
brew install jq
```

**Ubuntu/Debian:**
```bash
sudo apt-get install jq curl
```

## Funkce

- ✅ Jednoduché dotazy na Claude API
- ✅ Konfigurace API klíče
- ✅ Výběr modelu
- ✅ Zpracování chyb
- ✅ Markdown formátování odpovědí
- ✅ Nápověda

## Příklady

```bash
# Nastavení API klíče
claude-cli -k sk-ant-api03-...

# Jednoduché dotazy
claude-cli "Co je to umělá inteligence?"
claude-cli "Napiš krátkou báseň"

# Použití jiného modelu
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