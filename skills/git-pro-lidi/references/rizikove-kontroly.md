# Rizikové kontroly

Přečti tento soubor před commitem, pushem, pullem, mergem, řešením konfliktů nebo obnovou práce.

## Zastav před commitem

Zastav a vysvětli riziko před bezpečným uložením, pokud se objeví něco z toho:

- Nedávno aktivní soubory jsou teď smazané. Porovnej smazané soubory s nedávnou aktivitou přes `git log --name-only --since="30 days ago" --format=`.
- Změnilo se mnoho souborů najednou, hlavně napříč nesouvisejícími složkami.
- Byly přidané nebo změněné velké soubory. Soubory nad 25 MB ber jako varování a soubory nad 100 MB jako vysoké riziko, pokud repozitář už zjevně podobné assety nesleduje záměrně.
- Změnily se soubory, které vypadají jako tajné údaje: `.env`, `.env.*`, credentials, privátní klíče, service account soubory, tokeny, API klíče nebo soubory se slovy `secret`, `token`, `key`, `credential` nebo `password`.
- Objevily se generované nebo dependency složky: `node_modules`, `dist`, `build`, `.next`, `.nuxt`, `.turbo`, `coverage`, `.cache`, exporty nebo dočasné složky.
- Změnily se binární/design/media soubory a obsah nejde přečíst: obrázky, videa, archivy, PDF, PSD/AI/Figma exporty, databáze nebo kancelářské dokumenty.
- Přejmenování vypadá jako hromadné smazání plus hromadné vytvoření.
- Repozitář je uprostřed merge, rebase, cherry-pick nebo bisect.
- `.gitignore` chybí nebo působí podezřele vzhledem k typu projektu.

## Pravidlo pro tajné údaje

Varuj netechnické uživatele, že tajné údaje nikdy nesmí skončit v commitu ani v pushi. Vysvětli česky:

"Tohle vypadá, že může obsahovat soukromý token nebo heslo. Kdyby se to nasdílelo, někdo jiný by mohl získat přístup k tvému účtu, CMS, serveru nebo placené integraci. Tajné údaje ukládej do lokálního ignorovaného souboru, například `.env.local`, a do sdíleného repozitáře dávej jen názvy proměnných jako `API_KEY=sem-doplnit-klic`."

Nepiš detekované tajné hodnoty zpátky uživateli. Pojmenuj soubor a typ rizika, ne samotné tajemství.

## Zastav před pushem

Před pushem se vždy zeptej. Uveď:

- cílový remote a větev
- zda je aktuální větev společná verze nebo pracovní verze
- zda existují lokální commity, které ještě nejsou zálohované v cloudu
- kdo může práci vidět, pouze podle dostupné evidence
- zda push obsahuje velké soubory nebo soubory vypadající jako tajné údaje

Pokud je na společné větvi mnoho lokálních commitů, doporuč je nejdřív zálohovat do vlastní pracovní větve uživatele. Takovou větev vytvoř jen po výslovném potvrzení.

## Pull s neuloženou prací

Když uživatel chce stáhnout poslední změny a existují lokální změny:

1. Vysvětli, že uživatel má neuloženou místní práci.
2. Doporuč bezpečné uložení před stažením práce někoho dalšího.
3. Nepoužívej stash jako výchozí postup.
4. Pokračuj až po potvrzení bezpečného uložení nebo po zvolení jiné bezpečné cesty.

## Šablona pro vysvětlení konfliktu

Když vznikne konflikt, vysvětli:

- "Tyto soubory potřebují rozhodnutí: ..."
- "Tvoje strana změnila: ..."
- "Druhá strana změnila: ..."
- "Konflikt se týká obsahu/nastavení/struktury/generovaných souborů/assetů."
- "Doporučuji ... protože ..."
- "Tyto soubory nezměním, dokud to nepotvrdíš."

U lidsky psaného obsahu, obchodních dokumentů, poznámek, designů a konfigurace ber uživatele jako toho, kdo rozhoduje. U generovaných souborů doporuč regeneraci nebo výběr verze jen tehdy, když to podporuje evidence z projektu.
