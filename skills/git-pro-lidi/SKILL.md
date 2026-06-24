---
name: git-pro-lidi
disable-model-invocation: true
description: 'Ručně spouštěná pomůcka pro netechnické česky mluvící uživatele v existujících Git repozitářích. Vyvolej výslovně pro fráze: ulož mi práci, uloz mi praci, bezpečné uložení, bezpecne ulozeni, checkpoint, commit, zazálohuj to, zazalohuj to, záloha, zaloha, pošli to do cloudu, posli to do cloudu, sdílej to, sdilej to, stáhni poslední změny, stahni posledni zmeny, pull, push, merge, spoj naši práci, spoj nasi praci, sluč to, sluc to, co se změnilo, co se zmenilo, vrať to zpátky, vrat to zpatky, konflikt, Git pro lidi. Nepoužívej pro worktrees, rebase-first postupy ani kompletní nastavování GitHub účtu.'
---

# Git pro lidi

## Přehled

Schovej Git mechaniku za běžný jazyk a zároveň uživateli česky vysvětli následky každého kroku. Upřednostni bezpečné místní uložení, výslovné potvrzení pro sdílení nebo destruktivní akce a jednoduchou spolupráci přes merge v jedné existující složce projektu.

## Slovník pro uživatele

Nejdřív používej tyto výrazy. Git termíny doplň do závorky jen tehdy, když to pomůže:

- "společná verze" pro `main` nebo `master`
- "tvoje pracovní verze" pro osobní průběžnou větev uživatele
- "jejich pracovní verze" pro větev kolegy
- "bezpečné uložení" pro místní commit
- "poslat do cloudu", "zálohovat" nebo "sdílet" pro push
- "stáhnout poslední změny" pro fetch/pull a merge
- "spojit práci" pro merge

Vždy vysvětli, co se změní, kde se to změní a kdo to může vidět. Nenuť uživatele učit se Git slovník předtím, než může bezpečně jednat.

## Pevná pravidla

- Nikdy nepoužívej Git worktrees.
- Nepoužívej rebase jako výchozí postup. Preferuj obyčejný merge.
- Nevytvářej větve proaktivně. Použij existující větve, pokud už existují. Novou větev vytvoř jen tehdy, když uživatel výslovně požádá o samostatnou pracovní verzi nebo zálohu.
- Při spolupráci preferuj jednu průběžnou osobní pracovní větev na uživatele, ne jednu větev na každý úkol.
- Neinstaluj Git aliasy a neměň globální Git konfiguraci, pokud uživatel výslovně nepožádá o pomoc s nastavením.
- Ber tuto v1 jako pomoc pro existující repozitáře. Pokud Git, remote nebo cloudová záloha chybí, diagnostikuj a vysvětli problém místo toho, abys z úkolu udělal kompletní setup účtů.
- Nikdy nepushuj, nepublikuj, nemaž, nezahazuj, neresetuj, nepřepisuj, nepoužívej force push a neřeš konflikt bez srozumitelného potvrzení.

## Začni diagnostikou

Tyto read-only kontroly proveď automaticky, než doporučíš nebo uděláš další krok:

1. Ověř, zda je dostupný Git: `git --version`.
2. Ověř, zda je aktuální složka v repozitáři: `git rev-parse --is-inside-work-tree`.
3. Zjisti stav: `git status --short --branch`.
4. Zjisti aktuální větev: `git branch --show-current`.
5. Zjisti remotes: `git remote -v`.
6. Zjisti poslední aktivitu a možné spolupracovníky: `git log --format="%h%x09%an%x09%ar%x09%s" -20`.
7. Pokud jde o spolupráci, zkontroluj větve: `git branch --all --verbose --no-abbrev`.

Výsledky popisuj jako viditelnou evidenci, ne jako jistotu. Říkej "Vidím nedávné commity od dvou jmen" místo "repozitář používají dva lidé."

## Bezpečnostní reference

Před workflow, které commitne, pushne, pullne, mergne, vyřeší konflikt nebo obnoví práci, přečti `references/rizikove-kontroly.md`.

## Směrování frází

Převeď české uživatelské věty na nejmenší bezpečný postup:

- "zkontroluj projekt" -> pouze diagnostika.
- "ulož mi práci", "uloz mi praci", "bezpečné uložení", "bezpecne ulozeni", "checkpoint", "commit" -> místní bezpečné uložení.
- "zálohuj to", "zazalohuj to", "záloha", "zaloha", "sdílej to", "sdilej to", "pošli to do cloudu", "posli to do cloudu", "push" -> push až po potvrzení.
- "stáhni poslední změny", "stahni posledni zmeny", "stáhni změny od kolegy", "stahni zmeny od kolegy", "pull", "sync" -> fetch/pull přes merge, nejdřív bezpečné uložení při lokálních změnách.
- "spoj naši práci", "spoj nasi praci", "sluč to dohromady", "sluc to dohromady", "merge" -> merge existujících větví až po vysvětlení zdroje a cíle.
- "co se změnilo?", "co se zmenilo?", "diff", "historie" -> shrnutí lokálních změn nebo historie.
- "vrať to zpátky", "vrat to zpatky", "revert", "restore" -> nejdřív nedestruktivní obnova; destruktivní kroky potvrdit.

## Ulož mi práci

Větu "ulož mi práci" ber jako svolení vytvořit jeden místní checkpoint commit, pokud rizikové kontroly projdou.

1. Proveď diagnostiku a přečti `references/rizikove-kontroly.md`.
2. Česky shrň změněné soubory.
3. Pokud existuje riziko, zastav a požádej o potvrzení. Pojmenuj konkrétní riziko.
4. Pokud riziko nevidíš, připrav všechny aktuální změny přes `git add -A`.
5. Vytvoř jeden commit se srozumitelnou zprávou, například `checkpoint: ulozeni aktualni prace`.
6. Ukaž krátký hash commitu a vysvětli, že uložení je zatím jen na tomto počítači.

Preferuj jedno bezpečné uložení před čistou vývojářskou historií. Nedělej partial staging, pokud uživatel výslovně neřekne, že chce uložit jen část práce.

## Zálohovat nebo sdílet

Před každým pushem vždy požádej o potvrzení.

1. Ověř remote a cílovou větev.
2. Vysvětli, že push pošle uložené změny mimo tento počítač a mohou je vidět lidé s přístupem k repozitáři.
3. Pokud má aktuální větev několik lokálních commitů, doporuč nejdřív zálohu do vlastní pracovní větve uživatele.
4. Pokud práce působí hotově, můžeš navrhnout pozdější sloučení do společné verze.
5. Push proveď jen po výslovném potvrzení.

Pokud remote neexistuje, vysvětli, že v1 umí diagnostikovat chybějící cloudovou zálohu, ale nemá bez výslovné žádosti zakládat účty ani repozitáře.

## Stáhnout poslední změny

Použij merge-based aktualizaci.

1. Pokud existují lokální změny, vysvětli, že uživatel má neuloženou práci, a doporuč bezpečné uložení před stažením změn od někoho dalšího.
2. Nepoužívej stash jako výchozí postup.
3. Když je práce čistá nebo uložená, stáhni informace přes `git fetch --all --prune`.
4. Pull proveď přes merge, například `git pull --no-rebase`, pokud má větev upstream.
5. Pokud upstream chybí, vysvětli dostupné větve a doporuč nejbezpečnější další krok.
6. Pokud vznikne konflikt, zastav a použij konflikt workflow.

## Spojit práci

Používej existující větve. Nevytvářej nové větve, pokud o to uživatel nepožádá.

1. Vysvětli zdroj a cíl lidsky: "Přinesu jejich pracovní verzi do tvojí pracovní verze" nebo "Spojím tvoji pracovní verzi do společné verze."
2. Vyžaduj čistý pracovní stav nebo bezpečné uložení před mergem.
3. Preferuj nejdřív přinést společnou verzi do pracovní verze uživatele, zkontrolovat výsledek a teprve potom mergnout pracovní verzi do společné verze, až uživatel řekne, že je práce hotová.
4. Používej obyčejný `git merge`, ne rebase.
5. Při konfliktech zastav.

## Konflikty

Když vznikne merge konflikt:

1. Zastav před úpravou conflict markerů.
2. Vypiš dotčené soubory.
3. Vysvětli, co změnila strana uživatele a co změnila druhá strana.
4. Rozliš, jestli jde o obsah, strukturu, nastavení, generovaný výstup nebo binární/assets soubor.
5. Doporuč řešení normální češtinou.
6. Před aplikací řešení požádej o potvrzení.

## Co se změnilo

Pro lokální změny shrň `git status --short`, `git diff --stat` a vybrané čitelné diffy. Pro historii shrň poslední `git log` záznamy. Hodnoty, které vypadají jako tajné údaje, začerni nebo neukazuj; nikdy uživateli neopakuj tokeny, klíče ani hesla.

## Vrátit nebo obnovit

Preferuj vratné možnosti:

- Nejdřív použij `git status`, `git log` a `git diff`.
- Pro už nasdílené commity preferuj `git revert`.
- Před rizikovou obnovou preferuj vytvořit nové bezpečné uložení.
- Potvrď `git restore`, `git reset`, mazání větví, force push nebo jakékoliv přepsání.
