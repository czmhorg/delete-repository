# delete-repository – testovací stub (POC-ONLY)

Stub černé skříňky `<ghOrg>/delete-repository` (axiom *Práva členů organizace*,
`defs/defs.md`) pro testovací organizaci. V produkci repo vlastní a spravuje
organizace a je pro governance černá skříňka – tento stub existuje jen proto,
aby šel v pískovišti otestovat klient `gh-delete` a navazující workflow
`track-delete` end-to-end.

Chování (napodobuje produkci):

1. Trigger: otevření issue s titulkem `Delete repository: <org>/<repo>`
   (přesně to zakládá `gh-delete`); tělo = jediná řádka `<org>/<repo>`.
2. Workflow tělo čte z event souboru (nikdy neinterpoluje do `run:`), validuje
   přísně (jen vlastní organizace, formát názvu) a ověří, že autor issue je
   admin mazaného repa.
3. Pojistky: nikdy nesmaže sebe ani governance repo.
4. Po smazání okomentuje a zavře issue (completed); odmítnutí = komentář
   s důvodem + zavření (not planned).

## Nasazení

Obsah `delete-repository.yml` patří do `.github/workflows/` repa
`<ghOrg>/delete-repository`. Repo se nesyncuje přes `gov-sync.sh` – nasazuje
se ručně (jednorázově, stub se nemění).

Vyžaduje secret **`DELETE_BOT_TOKEN`**: fine-grained PAT omezený na testovací
org s repo permissions **Administration RW** (mazání), **Metadata R**;
mazat umí jen repa, na která PAT dosáhne. Komentáře a zavírání issue jede
přes běžný `GITHUB_TOKEN` workflow.
