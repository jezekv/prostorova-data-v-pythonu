# 2. Verzování kódu a kolaborativní vývoj: Git a GitHub

Tato lekce se věnuje verzování kódu pomocí nástroje **Git**. Zaměříme se na organizaci projektu, týmovou spolupráci a zajištění plné reprodukovatelnosti prostorových analýz.

# Proč vůbec verzovat kód?

Při práci na projektu se kód neustále mění. Něco opravíme, něco přidáme, něco rozbijeme a později potřebujeme zjistit, **co se změnilo, proč se to změnilo a ke které verzi se můžeme vrátit**.

Bez verzovacího systému často vzniká něco jako:

`analyza_final.ipynb` → `analyza_v2.ipynb` → `analyza_oprava.ipynb` → `analyza_opravena_final.ipynb`

To je nepraktické hlavně proto, že názvy souborů nepopisují historii změn a při spolupráci snadno vznikají kolize.

**Git řeší tento problém tím, že udržuje historii vývoje projektu.**

---

## 1.1 Git a GitHub

### Git
**Git je systém pro správu verzí.** Sleduje změny v projektu a ukládá jejich historii. Historie je uložena lokálně, takže Git může fungovat i bez internetu.

### GitHub
**GitHub je online platforma pro Git repozitáře a týmovou spolupráci.** Umožňuje sdílet repozitář a používá se například pro Pull Requests, Code Review a Issues.

> **Git ≠ GitHub.** Git je nástroj. GitHub je jedna z platforem, na kterých můžeme Git repozitář hostovat.

---

## 1.2 Jak Git funguje: jeden mentální model

Nejdůležitější je pochopit tok změn:

```text
upravuji soubory
      ↓
Working Directory
      ↓  git add
Staging Area
      ↓  git commit
Local Repository
      ↓  git push
Remote Repository (např. GitHub)
```

A změny od ostatních se dostávají k nám:

```text
Remote Repository
      ↓  git pull
Local Repository
      ↓
Working Directory
```

- **Working Directory:** Soubory projektu, které právě upravujeme.
- **Staging Area:** Přípravná zóna, do které vybereme změny pro nejbližší commit.
- **Local Repository:** Lokální historie projektu uložená v repozitáři, jehož součástí je složka `.git`.
- **Remote Repository:** Vzdálený repozitář, typicky uložený na GitHubu, se kterým svou lokální historii sdílíme.

---

## 1.3 Commit: Safe Point projektu

**Commit je záznam konkrétního stavu projektu v určitém okamžiku.** Můžeme si ho představit jako bezpečný bod, ke kterému se historie projektu vztahuje.

```text
●──●──●──●──●
   ↑     ↑
 starší  novější
 verze   verze
```

Commit obsahuje zejména:

- popis změny (**commit message**),
- autora a čas,
- unikátní ID,
- návaznost na předchozí historii.

Důležitá je hlavně srozumitelná commit message: z historie má být později poznat, co se stalo.

---

## 1.4 Branches: práce na oddělené větvi

Když chceme vytvořit novou funkci nebo opravit chybu, nechceme vždy zasahovat přímo do hlavní větve `main`.

```text
main       ●──●──●────────●
                 \
feature           ●──●──●
```

**Feature branch** je samostatná větev pro konkrétní úkol. Po dokončení ji můžeme pomocí **merge** začlenit zpět do `main`.

Pokud Git nedokáže automaticky spojit protichůdné změny, vzniká **merge conflict**. V takovém případě musí člověk rozhodnout, která varianta má zůstat.

---

## 1.5 GitHub jako platforma pro spolupráci

GitHub není jen online záloha projektu. Kromě hostování remote repozitáře poskytuje nástroje pro týmovou práci.

### Pull Request

**Pull Request (PR)** je návrh na začlenění změn z jedné větve do druhé. Ostatní mohou změny před sloučením prohlédnout, komentovat a zkontrolovat.

### Issues

**Issues** slouží k evidenci úkolů, chyb a návrhů.

> Issues podporují Markdown, lze vkládat kusy kódu, chybové hlášky i obrázky.

---

## 1.6 Co do Gitu nepatří

Git je určen především pro historii projektu, ne jako obecné úložiště všech souborů.

Typicky nechceme verzovat:

**Velká data** – například `.tiff`, `.gpkg` nebo `.shp`. U binárních datasetů Git nedokáže identifikovat změny a při každém commitu ukládá kompletní soubor. To vede k neúměrnému nárůstu velikosti repozitáře; data je vhodnější držet mimo Git.

**Hesla, API klíče a tokeny** – citlivé údaje nikdy nevkládejte do repozitáře. Pokud je jednou commitnete a pushnete, jejich následné smazání ze souboru je neodstraní z historie.

**Virtuální prostředí a dočasné soubory** – například `.venv`, `__pycache__` nebo `.ipynb_checkpoints`.

### `.gitignore`

Soubor **`.gitignore`** říká Gitu, které soubory a složky nemá sledovat.

Pomáhá tak udržet repozitář čistý a zabránit náhodnému přidání balastu nebo citlivých souborů.

---

# 2. Instalace a konfigurace prostředí

Pro integraci Gitu je nutná instalace do operačního systému.

## 2.1 Instalace Git
[Oficiální stránka ke stažení (git-scm.com)](https://git-scm.com/downloads)

Stáhněte instalátor a ponechte výchozí nastavení. Následně v terminálu spusťte `git --version`.

## 2.2 Globální konfigurace (Identita)
Aby mohl Git přiřadit změny konkrétnímu autorovi, je nutné nastavit jméno a e-mail. Tyto údaje se propisují do historie commitů.

Otevřete příkazový řádek (`Win` &rarr; `cmd` &rarr; `Enter`) a zadejte:

```bash
git config --global user.name "Vaše jméno"
git config --global user.email "Váš email"
```

## 2.3 GitHub CLI: Instalace a autorizace

Git neví, že existuje nějaký GitHub, dokud mu ručně nepředáte přesnou adresu. Použijeme k tomu oficiální nástroj **GitHub CLI** (příkaz `gh`). 

### Instalace nástroje přes příkazovou řádku
Nejrychlejší způsob, jak GitHub CLI nainstalovat bez hledání instalátorů na webu, je použít správce balíčků přímo v terminálu:

```bash
winget install --id GitHub.cli
```

### Autorizace
V terminálu zadejte příkaz:

```bash
gh auth login
```

Průvodce v terminálu projděte následovně (šipkami vybíráte, Enterem potvrzujete):
1. What account do you want to log into?  
   - Vyberte `GitHub.com`

2. What is your preferred protocol for Git operations?  
   - Vyberte `HTTPS`

3. Authenticate Git with your GitHub credentials?  
   - Zvolte `Yes` (Tento krok zajistí, že se přihlášení "propíše" i do Gitu).

4. How would you like to authenticate GitHub CLI?  
   - Vyberte `Login with a web browser`

5. Propojení s prohlížečem:  
   - Terminál vám ukáže osmimístný kód (např. BD21-4A55).  
   - Stiskněte Enter. Automaticky se otevře prohlížeč.  
   - Vložte zobrazený kód a potvrďte tlačítkem "Authorize github".  

### Ověření stavu

```bash
gh auth status
```
Měli byste vidět potvrzení: `Logged in to github.com account <vaše-jméno>`.

---
# 3. Git v příkazové řádce: Praktické cvičení

V této části si vyzkoušíte základní workflow přímo v terminálu. Jednotlivé kroky naleznete v samostatném souboru `cviceni_git.md`.

## Přehled základních příkazů

| Akce | Příkaz v terminálu | Popis a význam |
| :--- | :--- | :--- |
| **Initialize** | `git init` | Inicializuje nový lokální repozitář v aktuálním adresáři. |
| **Status** | `git status` | Zobrazí stav souborů (změněné, připravené k zápisu, nesledované). |
| **Diff** | `git diff` | Zobrazí konkrétní změny v řádcích u souborů. |
| **Stage (+)** | `git add <soubor>` | Přesune konkrétní změny do oblasti připravených změn (**Staging Area**). |
| **Commit** | `git commit -m "zpráva"` | Vytvoří trvalý záznam v historii. |
| **Log** | `git log` | Zobrazí historii provedených commitů a jejich ID. |
| **Branch** | `git branch <název>` | Vytvoří novou vývojovou větev pro izolovanou práci. |
| **Checkout** | `git checkout <název>` | Přepne pracovní adresář do zvolené větve nebo ke konkrétnímu commitu. |
| **Merge** | `git merge <zdroj>` | Integruje změny ze zdrojové větve do té, ve které se právě nacházíte. |
| **Clone** | `git clone <url>` | Vytvoří lokální kopii existujícího vzdáleného repozitáře (typicky z GitHubu). |
| **Push** | `git push` | Propíše vaše lokální commity do vzdáleného repozitáře v cloudu. |
| **Pull** | `git pull` | Stáhne nejnovější změny z cloudu a automaticky je sloučí s vaší lokální verzí. |

---

# 4. Workflow ve VS Code

Visual Studio Code má integrovanou podporu pro Git (záložka **Source Control**). Níže je popsán standardní postup.

## 4.1 Inicializace repozitáře
1.  Otevřete složku projektu ve VS Code.
2.  Přejděte na panel **Source Control** (ikona větvení vlevo, zkratka `Ctrl+Shift+G`).
3.  Zvolte **Initialize Repository**.

## 4.2 Staging a Commit (Uložení změn)
Git rozlišuje mezi "pracovním adresářem" a "historií". Uložení je dvoufázový proces:

1.  **Stage Changes (Příprava):** Vyberte soubory, které chcete zahrnout do commitu, kliknutím na ikonu `+` u názvu souboru.
2.  **Commit (Potvrzení):** Do pole *Message* zadejte popis změny. Potvrďte tlačítkem **Commit** (ikona ✔️).

## 4.3 Práce s větvemi (Branching)
1.  V dolní liště VS Code klikněte na název aktuální větve (obvykle `main` nebo `master`).
2.  Zvolte **+ Create new branch...**.
3.  Zadejte název.
4.  VS Code vás automaticky přepne do nové větve.

## 4.4 Slučování větví (Merge)
Když je práce ve vývojové větvi hotová, je třeba ji včlenit do hlavní linie projektu:

1. **Přepnutí:** Klikněte na název větve v dolní liště a přepněte se zpět do cílové větve (např. `main`).
2. **Výběr příkazu:** Otevřete příkazovou paletu (`Ctrl+Shift+P`), napište `Git: Merge Branch...` a stiskněte Enter.
3. **Zdroj:** Vyberte větev, kterou chcete sloučit (např. vaši rozpracovanou funkci).
4. **Dokončení:** Git se pokusí o automatické sloučení. Pokud narazí na konflikty, VS Code je barevně zvýrazní přímo v editoru, kde můžete zvolit možnost *Accept Current Change* (ponechat verzi z `main`) nebo *Accept Incoming Change* (přijmout verzi z vaší větve).

## 4.5 Publikace na GitHub (Push)
Pro zálohování kódu do cloudu:

1.  V panelu **Source Control** klikněte na **Publish Branch**.
2.  Při prvním použití budete vyzváni k autorizaci přes prohlížeč.
3.  Zvolte možnost **Publish to GitHub public repository** (nebo private).

---

# 5. Google Colab a GitHub

Zatímco ve VS Code pracujeme s lokálním klonem repozitáře, Google Colab běží na dočasném virtuálním stroji. Jakékoli soubory stažené nebo vytvořené mimo připojený Google Drive jsou po ukončení relace smazány. Z tohoto důvodu vyžaduje verzování odlišný přístup.

## 5.1 GUI přístup
Google Colab disponuje vestavěnou funkcí pro přímé ukládání notebooků na GitHub bez nutnosti používat příkazovou řádku.

### Uložení práce na GitHub (Push)
V prostředí Colab vyžaduje standardní příkaz `git push` konfiguraci autentizace při každém spuštění (kvůli dočasnosti instance). Místo toho můžeme využívat nativní API integraci, která je jednodušší:

1.  V horním menu zvolte **Soubor (File)** > **Uložit kopii na GitHub (Save a copy in GitHub)**.
2.  Při prvním použití budete vyzváni k autorizaci propojení Google Colab s vaším GitHub účtem.
3.  V dialogovém okně nastavte:
    * **Repository:** Vyberte cílový repozitář.
    * **Branch:** Zvolte větev.
    * **File path:** Nastavte cestu k souboru.
    * **Commit message:** Popište změny.
4.  Klikněte na **OK**. Notebook se uloží jako nový commit a automaticky se otevře v náhledu na GitHubu.

### Otevření notebooku z GitHubu
Pro načtení existující práce:
1.  V menu zvolte **Soubor (File)** > **Otevřít notebook (Open notebook)**.
2.  Přepněte na záložku **GitHub**.
3.  Vyhledejte repozitář nebo vložte URL adresu.
4.  Kliknutím na ikonu "Open in Colab" (často odznak v `README.md`) nebo výběrem ze seznamu se spustí nová relace.

## 5.2 Správa dat v Colabu
Hlavní rozdíl oproti lokálnímu vývoji je v dočasnosti prostředí. V Colabu se data po ukončení relace mažou.

* **Kód (Notebook):** Ukládáme na **GitHub** (viz bod 5.1).
* **Velká data (Rastr/Shapefile):** Ukládáme na **Google Drive**.

Pro přístup k datům na Disku je nutné je připojit (Mount):

```python
from google.colab import drive
drive.mount('/content/drive')
```

## 5.3 Práce s větvemi (Branches) v Google Colab

Google Colab nemá plnohodnotné grafické rozhraní pro správu Git repozitáře (jako panel *Source Control* ve VS Code). Práce s větvemi proto probíhá specifickým způsobem.

### Doporučený postup
Jelikož Colab neumožňuje snadné řešení konfliktů (Merge Conflicts), doporučuji tento postup:

1.  Vytvořte si větev na webu GitHubu (tlačítko *Branch* -> *New branch*).
2.  Otevřete tuto větev v Colabu (postup A).
3.  Pracujte a ukládejte přímo do této větve (postup B).
4.  Pokud potřebujete sloučit změny do `main`, vytvořte **Pull Request** na webu GitHubu.

### A. Otevření konkrétní větve
Pokud chcete pracovat na jiné větvi než `main` (např. chcete opravit kód ve větvi `development`):

1.  Jděte na **Soubor (File)** > **Otevřít notebook (Open notebook)**.
2.  Vyberte záložku **GitHub**.
3.  Zadejte URL repozitáře nebo vyhledejte název.
4.  Vedle názvu repozitáře je rozbalovací nabídka **Branch**, kde standardně svítí `main`. **Zde přepněte na požadovanou větev.**
5.  Zobrazí se seznam notebooků dostupných v této konkrétní větvi.

### B. Uložení do nové nebo existující větve
Když máte hotovou práci a chcete ji uložit, Colab vám umožní vybrat cílovou větev v dialogovém okně.

1.  Zvolte **Soubor (File)** > **Uložit kopii na GitHub (Save a copy in GitHub)**.
2.  V okně, které se otevře, se zaměřte na pole **Branch** a vyberte **požadovanou větev**.
3.  Zadejte *Commit message* a potvrďte.
