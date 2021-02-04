---
title: "Dokumentation"
description: "En opsætningsguide til KRLs software dokumentation."
lead: "Denne side beskriver hvordan man opsætter et miljø til at skrive ekstern dokumentation i forbindelse med udvikling af KRLs IT-services."
date: 2021-01-18T13:47:28+01:00
lastmod: 2021-01-18T13:47:28+01:00
draft: false
images: ['../../../images/screenshot.png']
menu:
  docs:
    parent: "opsaetning"
weight: 999
toc: true
---

Dokumentationen er bygget med en statisk-side generator ved navn Hugo. Erfaring med Hugo er ikke nødvendig til at redigere og skrive dokumentation, dog hvis man ønsker at indføre ændringer i sidens layout kræver det at man kender lidt til Hugo, se eventuelt Hugos egen [dokumentationsside](https://gohugo.io/documentation/).

## Installation
For at installere miljøet skal man *clone* GitHub-arkivet og derefter installere alle afhængigheder via `npm`, når dette er gjort kan man starte webserveren og begynde med at skrive dokumentation.

```
$ git clone https://github.com/PIMI-krl/KRL-DOCS
$ npm install
$ npm run start
```

Ved at køre `npm run start` kører man reelt set følgende kommando `hugo server --disableFastRender --buildDrafts`, de to flag, `--disableFastRender` og `--buildDrafts` sørger henholdsvis for at tegne hele siden om, frem for kun det som er blevet ændret eller tilføjet, og for at "bygge" markdown filer hvor parameteren `draft` er sat til `true` - den anvendes altså for at vise dokumenter som endnu ikke vurderes til at være færdige.

Dokumentationssiden er baseret på [Doks](https://getdoks.org/) med mindre ændringer, referer til Doks egen [dokumentationsside](https://getdoks.org/docs/prologue/introduction/) for mere information om hvordan Doks, og denne side, er bygget.

### At arbejde med Git
Idet dokumentationssiden anvender Git og GitHub, vil kendskab til disse være nødvendig. Hvis man ikke er fortrolig med Git, eller har brug for en opfriskning, kan man med fordel besøge [git - the simple guide](https://rogerdudler.github.io/git-guide/) som gennemgår de mest basale begreber og handlinger/kommandoer. 

Eventuelt kan man anvende [GitHub desktop](https://desktop.github.com/) som har en meget overskuelig grænseflade. Denne applikation gør det også nemt at *committe* enkelte linjer fra en given fil, frem for hele filen, hvis der er behov for dette.

#### Branches

Pr d. 27/1-2021 er der to branches på Git'ten, *master* og *dev*. Ændringer vil automatisk blive kompileret og bygget når de bliver pushet til *master*, i og med at vi har et begrænset antal compilations-minutter hos Netlify (300 pr. måned) pusher vi ikke direkte til *master* men til *dev* i stedet. Efter at ændringerne er blevet pushet til *dev*, kan man i slutningen af sin arbejdsdag tjekke hvor mange compilations-minutter der er tilbage og derefter merge *dev* med *master* således at ændringer bliver tilgængelige for alle.

Hvis det er et krav at ændringerne kommer ud med det samme, kan man selvfølgelig blot *merge* så snart der er behov for dette.

## Skrive-process

Skriveprocessen afhænger af om man skal redigere i noget dokumentation som allerede eksisterer, eller tilføje noget nyt - eksempelsvis når en ny komponent er blevet udviklet. Det følgende afsnit vil gå igennem begge cases.

### Eksisterende dokumentationssider
For eksisterende sider er det nok at åbne den tilhørende fil, eksempelsvist `docs/opsætning/ekstern_dokumentation.md`, i en teksteditor efter eget valg hvorefter man er klar til at redigere i filen. Hvis man har startet webserveren som beskrevet i [det forrige afsnit](#installation) og åbnet hjemmesiden i en browser vil ens ændringer blive renderet når man gemmer dokumentet.

### Nye dokumentationssider
For at oprette en ny side, eksempelsvist en ny side omhandlede opsætning af et andet værktøj ved navn "KRL-NY" skal man køre kommandoen `npm run create docs/opsætning/KRL-NY.md`. Denne kommando virker kun korrekt hvis den *køres fra roden af projektet*.
Den genererede fil vil have følgende indhold:

```yaml
---
title: "KRL NY"
description: ""
lead: ""
date: 2021-01-18T15:04:19+01:00
lastmod: 2021-01-18T15:04:19+01:00
draft: true
images: []
menu:
  docs:
    parent: ""
weight: 999
toc: true
---
```

#### YAML Front Matter

Indholdet mellem `---` er det man kalder YAML Front Matter hvilket svarer til filens metadata. De forskellige dele er beskrevet forneden.

- `title` er titlen på siden, dvs. at det er muligt at navngive selve filen uden brug af danske bogstaver men stadig have at siden indeholder disse.
- `description` er en kort beskrivelse af siden som kommer med i `<head>` elementet som en meta egenskab. Denne beskrivelse vil dermed ikke fremgå i selve dokumentationen, men kan blive brugt af søgemaskiner samt skærmlæsere. Som udgangspunkt er det ikke strengt nødvendigt at udfylde dette parameter, men det skader heller ikke.
- `lead` er indledningen til siden og bliver renderet direkte under sidens titel. Denne er igen ikke strengt nødvendig dog er den til stor hjælp for andre læsere, herunder ens fremtidige jeg, idet den giver hurtigt indblik i hvad siden indeholder.
- `date` og `lastmod` er lidt selvforklarende, den første er den dato hvor siden er blevet oprettet hvor den sidste er datoen for hvornår siden er sidst blevet opdateret.
- `draft` anvendes af Hugo til at finde ud af om siden skal bygges eller ej - med forudsætning for at Hugos server *ikke* er blevet startet med `--renderDrafts`.
- `images` er en liste af paths til billeder relateret til billedet. Disse anvendes af interne 'templates'.
- `menu` under denne parameter skal man specificere den eksakte menu hvor siden skal stå under. I dette tilfælde vil siden står under `docs`, dog er `parent` ikke specificeret hvilket vil medføre at siden bliver behandlet som en ny rod, se eksempelsvis **OPSÆTNING** i menuen til venstre på dokumentationssiden. Hvis vi specificerer en `parent` vil siden blive vist i menuen som et 'barn' af `parent`. For at definere en ny forælder skal dette gøres i filen `config/_default/menus.toml`.
- `weight` er det parameter som sørger for placeringen af siden i menuen, et lavere tal svarer til en højere placering i menuen, med andre ord vil sider med en lav vægt komme før sider med høj vægt.
- `toc` afgør om der skal vises en indholdsfortegnelse for siden. Bemærk at indholdsfortegnelsen bliver kun vist hvis der er nok plads på siden dvs. at vinduet hvor siden vises er bredt nok.

Der findes flere variabler end dem som er nævnt/brugt indtil videre. Den fulde oversigt over variabler er til at finde [her](https://gohugo.io/content-management/front-matter/).

### Markdown
Dokumentationen skrives i et markup sprog kaldt Markdown. Markdown er et relativt lille markup sprog som også tillader rå HTML i sit indhold. Dette gør det ideelt til mindre sider hvor formateringen af teksten ikke kræver det store. Der findes mange forskellige "smag" af Markdown, men for denne side (altså for hele dokumentationssiden) anvendes der Goldmark. Du kan læse mere om selve Goldmark [på GoldMark's GitHub side](https://github.com/yuin/goldmark). GitHUb har en relativt kortfattet [introduktion til markdown](https://guides.github.com/features/mastering-markdown/#syntax) syntax.
