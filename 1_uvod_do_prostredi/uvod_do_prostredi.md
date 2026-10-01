# 1. Teoretický úvod a konfigurace prostředí

Před psaním prvních řádků kódu je dobré nejdříve porozumět ekosystému nástrojů, které budeme používat. Celé vývojové zázemí stavíme na třech vrstvách: **programovacím jazyku**, **vývojovém prostředí** a **správci balíčků a prostředí**.  

## 1.1 Programovací jazyk: Python
**Python** je interpretovaný, vysokoúrovňový programovací jazyk, který se stal de facto standardem pro Spatial Data Science.

### Proč je Python tak populární?
* Syntaxe Pythonu je blízká přirozené angličtině.
* Nemusíte řešit nízkoúrovňové technické detaily počítače (např. ruční správu paměti atd.).
* Pro Python existují tisíce specializovaných knihoven. V našem kurzu využijeme například **GeoPandas** (vektory) a **Xarray** s **Rioxarray** (vícerozměrné rastry).
* Díky obrovské základně uživatelů existuje řešení pro téměř každý problém na platformách jako např. Stack Overflow.

## 1.2 Vývojové prostředí: Jupyter Notebook a VS Code
Vývojové prostředí tvoří kombinace interaktivního formátu zápisu **(Jupyter Notebook)** a editoru kódu **(VS Code)**.

### Jupyter Notebook (`.ipynb`)
 
Není programovací jazyk, ale interaktivní dokument, ve kterém kód spouštíme po logických blocích (buňkách). 

**Proč používat Jupyter Notebook?**
* Skládá se z buněk, které mohou obsahovat buď **spustitelný kód**, nebo **formátovaný text** (Markdown).
* Umožňuje kombinovat kód, výpočty, vizualizace (mapy, grafy) a doprovodný text do jednoho dokumentu.
* Slouží jako pracovní rozhraní. Samotný kód vykonává **kernel:** samostatně běžící proces Pythonu, který drží načtená data v paměti RAM.

### Visual Studio Code (VS Code)

VS Code je v současnosti asi nejoblíbenější editor kódu.  
[Odkaz ke stažení VS Code](https://code.visualstudio.com/)  

**Proč používat VS Code?**
*   Máte v něm editor, terminál, prohlížeč proměnných i interaktivní okna pro mapy.
*   Pomocí pluginů si VS Code přizpůsobíte (např. podpora pro GitHub nebo specifické GIS formáty).
*   Editor je zcela zdarma a postavený na open-source principech.

## 1.3 Správce prostředí a balíčků: Anaconda (Conda)
Namísto instalace Pythonu přímo do operačního systému je pro prostorová data výhodnější použít distribuci **Anaconda**, která spravuje izolovaná prostředí a minimalizuje konflikty mezi knihovnami.  
[Odkaz ke stažení Anacondy](https://www.anaconda.com/download)  
[Návod na instalaci Anacondy](https://www.anaconda.com/docs/getting-started/anaconda/install)

* **Anaconda vs. Conda:** Anaconda je distribuce obsahující Python a stovky předinstalovaných knihoven, zatímco Conda je samotný nástroj (příkaz v systému), který instaluje balíčky a spravuje virtuální prostředí.
* Conda jako správce prostředí umožňuje vytvořit oddělený prostor pro každý projekt s vlastní verzí Pythonu i balíčků. Změna knihovny v jednom projektu neovlivní ostatní projekty v počítači.
* Knihovny jako GeoPandas mají složité závislosti na C++ knihovnách (GDAL, GEOS, PROJ). Conda tyto závislosti řeší automaticky při instalaci, zatímco standardní nástroj `pip` zde často selhává.

> **Tip:** Pokud preferujete rychlou a úspornou instalaci bez stovek předem přibalených balíčků, skvělou alternativou k plné Anacondě je **Miniconda**. Ta nainstaluje čistý Python se správcem Conda a potřebné knihovny si přidáte na míru.

---

# 2. Inicializace a konfigurace Jupyter Notebooku ve VS Code

Tento návod popisuje postup od prvního spuštění editoru po zprovoznění interaktivního prostředí Jupyter. Předpokladem je správně nainstalovaný editor VS Code a distribuce Anaconda.

## 2.1 Instalace nezbytných rozšíření
VS Code vyžaduje pro interpretaci kódu Python a práci s notebooky instalaci dvou doplňků.

1.  Spusťte **Visual Studio Code**.
2.  Přejděte do sekce **Extensions** (ikona v levém panelu nebo zkratka `Ctrl+Shift+X`).
3.  Vyhledejte a nainstalujte rozšíření **Python** (vydavatel Microsoft).
4.  Vyhledejte a nainstalujte rozšíření **Jupyter** (vydavatel Microsoft).

## 2.2 Vytvoření prvního notebooku
Soubory Jupyter Notebook využívají příponu `.ipynb`.

1.  V horním menu zvolte **File** > **New File...**
2.  Z nabídky typů souborů vyberte **Jupyter Notebook**.
3.  Soubor uložte přes **File** > **Save As...** s příponou `.ipynb` (např. `cviceni_01.ipynb`).

## 2.3 Výběr Kernelu (propojení s Conda prostředím)
Aby notebook věděl, v jaké instalaci Pythonu má příkazy provádět, musíme mu přiřadit **kernel** (výpočetní proces).

1.  V pravém horním rohu editoru klikněte na tlačítko **Select Kernel**.
2.  V rozbalovacím menu zvolte **Python Environments...**.
3.  Vyberte instanci označenou jako `base (conda)` nebo verzi Pythonu odpovídající vaší instalaci Anacondy.
4.  Úspěšné propojení je signalizováno zobrazením verze Pythonu v pravém horním rohu (např. *Python 3.10.x*).

> **Tip:** V praxi se pro jednotlivé projekty běžně vytvářejí samostatná izolovaná prostředí. Pro účely našeho kurzu však pro jednoduchost využijeme výchozí prostředí `base`, kde je většina nástrojů již předpřipravena.

## 2.4 Ověření funkčnosti prostředí
Pro kontrolu, zda notebook skutečně komunikuje se zvoleným Conda prostředím, otestujeme první buňku.

1.  Ujistěte se, že je typ buňky nastaven na **Code**.
2.  Vložte následující kód:
    ```python
    import sys
    print(sys.executable)
    ```
3.  Spusťte buňku pomocí ikony **Play** vlevo nebo klávesovou zkratkou **Shift + Enter**.
4.  Výstup bez chybových hlášení s cestou do složky `anaconda3` potvrzuje připravenost prostředí.

## 2.5 Základní prvky rozhraní
* **Code Cell:** Buňka pro zápis a spouštění algoritmu.
* **Markdown Cell:** Buňka pro strukturovaný text a dokumentaci.
* **Variables:** Tlačítko v horní liště pro zobrazení aktivních proměnných v paměti RAM.
* **Restart:** Restartování kernelu (zastaví proces na pozadí a vymaže všechny proměnné z paměti RAM).

---

# 3. Instalace balíčků a správa vývojového prostředí

Python je navržen jako modulární jazyk. Ve své základní distribuci neobsahuje nástroje pro práci s prostorovými daty, proto je nutné využít externí knihovny. Tato kapitola se věnuje metodice instalace knihovny **GeoPandas**.

## 3.1 Problém více instalací Pythonu
V operačním systému se běžně vyskytuje více různých instalací Pythonu současně (např. systémový Python, verze v rámci ArcGIS Pro či QGIS, distribuce Anaconda nebo různá virtuální prostředí).

### Rizika instalace přes Shell (`!`)
Častým přístupem k instalaci bývá využití příkazu **Shell Escape `!`**, např. `!pip install`. Tento příkaz dočasně opustí prostředí Pythonu a zavolá systémovou příkazovou řádku. 
* **V čem je problém:** Systémový terminál může používat úplně jinou instalaci Pythonu, než kterou využívá váš aktuálně otevřený notebook. 
* **Následek:** Instalace v terminálu sice proběhne úspěšně, ale v notebooku se následně objeví chyba `ModuleNotFoundError`, protože knihovna byla nahrána "do jiného Pythonu".

## 3.2 Metoda Conda
Pro práci s komplexními balíčky je **Conda** doporučeným standardem, protože umí kromě samostatného Pythonu nainstalovat i potřebné nízkoúrovňové systémové knihovny.

**Proč instalovat GeoPandas přes Condu?**
Knihovna GeoPandas je závislá na nízkoúrovňových knihovnách psaných v C++ (zejména GDAL, GEOS a PROJ). Zatímco standardní `pip` vyžaduje jejich kompilaci v systému (což bývá zdrojem chyb), Conda je instaluje v již zkompilované a otestované podobě.

### Instalace přes Anaconda Prompt
Instalaci je sice možné provést více způsoby, ale nejspolehlivější a nejbezpečnější cestou je použití Anaconda Promptu (případně Miniconda Promptu). Ten má vždy správně nastavené systémové cesty k nástroji Conda bez rizika narušení ostatních programů v systému.

1. V nabídce Start vyhledejte a spusťte **Anaconda Prompt** (nebo Miniconda Prompt).
2. Zadejte příkaz a potvrďte klávesou Enter:
   
```bash
conda install -c conda-forge geopandas -y
```
3. Počkejte na dokončení instalace a okno zavřete.

> **Tip:** V cloudových službách, které nepoužívají Condu (např. Google Colab), instalujeme přes `pip` přímo v notebooku. Tyto systémy mají systémové závislosti většinou předpřipravené.

## 3.3 Základní koncept knihovny GeoPandas

GeoPandas je open-source knihovna, která rozšiřuje datové struktury Pandas o práci s prostorovými daty. Umožňuje tak kombinovat klasickou tabulkovou analýzu s prostorovými operacemi.

### Koncepční datový model
GeoPandas definuje dvě základní třídy:

| Třída | Analogie | Ekvivalent v GIS |
| :--- | :--- | :--- |
| **GeoSeries** | Sloupec obsahující geometrie (vektor bodů, linií nebo polygonů) | Geometrický sloupec (prostorová složka bez atributů) |
| **GeoDataFrame** | Tabulka atributů obsahující alespoň jeden sloupec typu **GeoSeries** | Vektorová vrstva (geometrie propojená s atributovou tabulkou) |

### Technologické zázemí
Knihovna GeoPandas funguje jako sjednocující rozhraní pro specializované nízkoúrovňové knihovny:

* **Shapely:** Výpočetní geometrie (využívá knihovnu **GEOS**).
* **PyProj:** Matematické transformace mezi souřadnicovými systémy (využívá knihovnu **PROJ**).
* **Fiona/PyOGRIO:** Čtení a zápis vektorových dat (využívá knihovnu **GDAL**).

## 3.4 První načtení a vizualizace prostorových dat

### Příprava prostředí:
```python
# Import
import geopandas as gpd
```

### Načtení dat:
```python
# Načtení dat přímo z oficiálního zdroje Natural Earth
url = "https://naturalearth.s3.amazonaws.com/110m_cultural/ne_110m_admin_0_countries.zip"
world = gpd.read_file(url)

# Zobrazení souřadnicového systému
print(world.crs)

# Zobrazení atributové tabulky (prvních 5 záznamů)
world.head()
```

### Vizualizace:
```python
# Vykreslení mapy
world.plot()

# world[world.NAME == "Czechia"].plot()
```

### Výpočet rozlohy:
```python
# Původní data Natural Earth jsou ve WGS84 (stupně).
# Ve stupních nelze správně počítat plochu → potřebujeme metry.

# 1. Vyfiltrování České republiky z načtené vrstvy světa
czechia = world[world.NAME == "Czechia"].copy()

# 2. Převedení do souřadnicového systému S-JTSK
czechia_projected = czechia.to_crs(epsg=5514)

# 3. Výpočet plochy
# .area vrací výsledek v jednotkách systému (zde m2)
# Pro kilometry čtvereční musíme dělit 1 000 000
plocha_m2 = czechia_projected.area.item()
plocha_km2 = plocha_m2 / 1_000_000

# 4. Výpis výsledku
print(f"Aktuální souřadnicový systém: {czechia_projected.crs.name}")
print(f"Vypočítaná rozloha ČR: {plocha_km2:.2f} km²")
```

---

# 4. Dokumentace v Jupyter Notebooku: Markdown

Textové buňky v notebooku slouží k dokumentaci metodiky i interpretaci výsledků. Formátují se pomocí syntaxe **Markdown**.

## 4.1 Základní syntaxe Markdown
Markdown je navržen pro přehledné formátování textu pomocí několika základních znaků.

### Struktura a text

* **Nadpisy:** Definují se počtem znaků `#` na začátku řádku.  
  `# Nadpis 1`  
  `## Nadpis 2`  
  `### Nadpis 3`

* **Tučné písmo:** Text uzavřete do dvojitých hvězdiček `**text**`.  
  **Takhle vypadá tučný text.**

* **Kurzíva:** Text uzavřete do jednoduchých hvězdiček `*text*`.  
  *Takhle vypadá kurzíva.*

* **Seznamy:** Pro odrážky použijte `-` nebo `*`, pro číslovaný seznam `1.`.  
  * Položka A
  * Položka B

* **Citace:** Použijte znak `>` na začátku řádku.  
  > Toto je bloková citace, která je odsazená od zbytku textu.

* **Horizontální čára:** Použijte `---` na samostatném řádku.      
---

* **Nový řádek vs. odstavec:** Pro nový odstavec nechte mezi řádky **jeden prázdný řádek**. Pro pouhé zalomení řádku bez mezery vložte na konec předchozího řádku **dvě mezery**.

### Odkazy a obrázky
* **Odkaz:** `[Název odkazu](URL_adresa)`  
* **Obrázek:** `![Popis obrázku](URL_adresa_k_obrazku)`  
  [Takhle vypadá odkaz](https://www.czu.cz/cs)

### Tabulky

\| Jméno | Příjmení | Bydliště |<br>
\| :--- | :---: | ---: |<br>
\| Jan | Novák | Jilemnice |<br>
\| Jana | Nováková | Stará Paka |

| Jméno | Příjmení | Bydliště |
| :--- | :---: | ---: |
| Jan | Novák | Jilemnice |
| Jana | Nováková | Stará Paka |

### Práce s kódem

* **Inline kód:** Použijte zpětné apostrofy `` `Inline` ``.  
  Například název funkce `gpd.read_file()`.

* **Blok kódu (víceřádkový):** Pro celé bloky kódu použijte tři zpětné apostrofy nad a pod kódem. Za úvodní tři apostrofy napište název jazyka.
    
  \```python<br>
     print("Hello World")<br>
    \```  
   
  ```python
    print("Hello World")
    ```

---

# 5. Online a cloudová řešení

Pokud není k dispozici dostatečně výkonný hardware nebo se nedaří lokální instalace, lze využít cloudová řešení. Ta umožňují vytvářet a spouštět notebooky přímo ve webovém prohlížeči bez jakékoliv instalace do počítače.

## 5.1 Výhody a nevýhody cloudových řešení

Práce v cloudu přináší specifické benefity, ale i limity, které je nutné zohlednit.

| Výhody | Nevýhody |
| :--- | :--- |
| **Nulová instalace:** Prostředí je předkonfigurované a připravené k okamžitému použití. | **Závislost na internetu:** Bez stabilního připojení nelze psát ani spouštět kód. |
| **Výpočetní výkon:** Přístup k výkonným CPU a GPU, a to i zdarma. | **Omezení operační paměti:** U bezplatných verzí může dojít k ukončení procesu při zpracování velkých datasetů. |
| **Dostupnost dat:** Přímý přístup k satelitním archivům bez nutnosti stahovat stovky GB na lokální disk. | **Dočasnost:** Bezplatná sezení jsou časově omezená; neuložená data mohou být po odpojení smazána. |
| **Kolaborace:** Snadné sdílení notebooků podobně jako u dokumentů Google Docs. |  **Soukromí a bezpečnost:** Data jsou nahrávána na servery třetích stran (riziko u citlivých údajů). |

## 5.2 Přehled vybraných platforem

Níže jsou uvedeny některé portály, které nabízejí Jupyter rozhraní.

### [Google Colab](https://colab.research.google.com/)
Nejrozšířenější online platforma pro obecný vývoj v Pythonu. 
* **Hlavní výhoda:** Integrace s Google Drive a bezplatný přístup k výkonným GPU.
* **Využití:** Rychlé prototypování, zpracování větších objemů dat a strojové učení.

### [Jupyter.org (Try Jupyter)](https://jupyter.org/try)
Oficiální demo rozhraní projektu Jupyter.
* **Hlavní výhoda:** Okamžitý přístup k prostředí bez nutnosti registrace.
* **Využití:** Krátkodobé testování syntaxe, výuka základů Pythonu. Data se po zavření prohlížeče neukládají.

### [Copernicus Data Space Ecosystem](https://dataspace.copernicus.eu/analyse/jupyterlab)
Evropská platforma pro přímou práci s archivy satelitů Sentinel.

### [OpenScienceLab (ASF)](https://opensarlab-docs.asf.alaska.edu/)
Specializovaná laboratoř Alaska Satellite Facility pro zpracování radarových dat.

### [ICOS Jupyter Hub](https://www.icos-cp.eu/data-services/tools/jupyter-notebook)
Portál zaměřený na environmentální data o skleníkových plynech.
