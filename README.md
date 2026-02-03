# LeoExpress Packagist (Satis)
![LeoExpress Packagist](img/orchestrator.png)

Tento repozitář slouží jako soukromý Composer repozitář pro balíčky **LeoExpress**. Využívá nástroj **Satis** ke generování statického seznamu balíčků, který je hostován na GitHub Pages.

## 🛠 Jak to funguje

1.  **Konfigurace**: Soubor `satis.json` obsahuje seznam všech PHP balíčků a konfiguraci výstupu.

2.  **Automatizace**: GitHub Action (`deploy.yml`) se spouští automaticky každou hodinu, při pushi do větve `master` nebo ručně přes `workflow_dispatch`.

3.  **Sestavení**: V rámci workflow se spouští Docker kontejner `composer/satis`, který využívá token `MY_SATIS_TOKEN` pro autentizaci vůči GitHub API.

4.  **Deployment**: Výsledná metadata jsou publikována do větve `gh-pages`. Webové rozhraní je dostupné na adrese [https://leoexpress.github.io/packagist](https://leoexpress.github.io/packagist) definované v konfiguraci.


## 📁 Struktura souborů

*   `satis.json`: Hlavní konfigurační soubor definující repozitáře a šablonu.

*   `.github/workflows/deploy.yml`: Definice CI/CD procesu pro build a nasazení.

*   `views/index.html.twig`: Vlastní Twig šablona pro vzhled výsledného webu.


## 🚀 Jak přidat nový balíček

1.  Otevři soubor `satis.json`.

2.  Přidej nový záznam do sekce `repositories`:

```json
    { "type": "vcs", "url": "https://github.com/LeoExpress/phpPkg-novy-nazev" }
```
> [!NOTE]
> Doporučení: Název repozitáře začínal `phpPkg-`


3. Změny commitni a pushni do větve `master`. Build se spustí automaticky.


## 🔐 Požadavky na nastavení

### 🔑 Secrets

Pro správný chod buildu je nutné mít v nastavení repozitáře (Settings > Secrets) nastaven:

*   **`MY_SATIS_TOKEN`**: Personal Access Token s právy pro přístup k repozitářům.

### 🛡️ Oprávnění

Pro správné stažení balíčků je nutné mít v repozitáři uživatele, který má uložený platný token v `~/.config/compsoer/auth.json`,
minimálně s právem čtení. 

## 📦 Použití v projektu

Do svého projektu (soubor `composer.json`) přidej tento repozitář:

**composer.json**
```json
    {
        "repositories": [
            {
                "type": "composer",
                "url": "https://leoexpress.github.io/packagist"
            }
        ],
        "require": {
            "leo/utils": "*"
        }
    }
```
## 🧹 Údržba

Workflow automaticky promazává staré běhy akcí a ponechává pouze poslední 4 záznamy pro udržení čisté historie.
