---
title: "Ekstern dokumentation"
description: "This is a description."
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

Dokumentationen er bygget med en statisk-side generator ved navn Hugo. Erfaring med Hugo er ikke nødvendig til at redigere og skrive dokumentation, dog hvis man ønsker at indføre ændriger i sidens layout kræver det at man kender lidt til Hugo, se eventuelt Hugos egen [dokumentationsside](https://gohugo.io/documentation/).

## Installation
For at installere miljøet skal man *clone* GitHub-arkivet og derefter installere alle afhængigheder via `npm`, når detter er gjort kan man starte webserveren og begynde med at skrive dokumentation.

```
$ git clone <GITHUB LINK INDSÆTTES HER> krl-dokumentation
$ npm install
$ npm run start
```

Ved at køre `npm run start` kører man reelt set følgende kommando `hugo server --disableFastRender --buildDrafts`, de to flag, `--disableFastRender` og `--buildDrafts` sørger henholdsvis for at gen-rendere hele siden, frem for kun det som er blevet ændret eller tilføjet, og for at "bygge" markdown filer hvor parameteren `draft` er sat til `true` - den anvendes altså for at vise dokumenter som endnu ikke vurderes til at være færdige.

### Installation af Hugo (extended version)
For at gøre dokumentationsskrivningen nemmere kan det være en fordel at installere Hugo på maskinen således at man kan gøre brug af kommandoer som automatisk generer filer hvor *YAML Front Matter* er udfyldt. Dette er ikke strengt nødvendigt, idet at dette miljø kommer med Hugo indbygget, dog vil det ikke være muligt at køre Hugo kommandoer direkte.

Det er nemmest at installere Hugo via. pakke-manageren `chocolatey`, denne gør det nemt at installere, opdatere og fjerne pakker på en måde som er nem og overskuelig.

1. Gå ind på [chocolatey's installationsside](https://chocolatey.org/install) og følg vejledningen til installation
2. Åbn et PowerShell terminal med Administratorrettigheder
   - Dette er nødvendigt for at tilføje Hugo til PATH-variablet
3. Kør kommandoen `choco install hugo-extended -confirm` og følg beskederne i terminalen for at installere den udvidet version af Hugo.

Der findes også andre måder at installere Hugo på som kan ses [her](https://gohugo.io/getting-started/installing/). Den ovenstående fremgangsmåde blev valgt af `PIMI` efter at have anvendt Hugo til andre projekter.

## Skrive-process

Skrive processen starter ved at enten åbne en eksisterende fil eller ved oprettelse af en ny fil. I de følgende afsnit vil der blive beskrevet hvordan begge cases skal håndteres.

### Eksisterende dokumentationssider
For eksisterende sider er det nok at åbne den tilhørende fil, eksempelsvist `docs/opsætning/ekstern_dokumentation.md`, i en teksteditor efter eget valg hvorefter man er klar til at redigere i filen. Hvis man har startet webserveren som beskrevet i [det forrige afsnit](#installation) og åbnet hjemmesiden i en browser vil ens ændriger blive renderet når man gemmer dokumentet.

### Nye dokumentationssider
For at oprette en ny side, eksempelsvist en ny side omhandlede opsætning af et andet værkøj ved navn "KRL-NY" skal man køre kommandoen `hugo new docs/opsætning/KRL-NY.md`. Hugo vil så genere filen *relativt* til den nuværende sti, dvs. for at den ovenstående kommando fungerer som forventet skal man være i roden af mappen/miljøet. Den genererede fil vil have følgende indhold:

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
- `menu` under denne parameter skal man specificere den eksakte menu hvor siden skal stå under. I dette tilfælde vil siden står under `docs`, dog er `parent` ikke specificeret hvilket vil medføre at siden bliver behandlet som en ny rod, se eksempelsvis **OPSÆTNING** i menuen til venstre. Hvis vi specificerer en `parent` vil siden blive vist i menuen som et 'barn' af `parent`.
- `weight` er det parameter som sørger for placeringen af siden i menuen, et lavere tal svarer til en højere placering i menuen, med andre ord vil sider med en lav vægt komme før sider med høj vægt.
- `toc` afgører om der skal vises en indholdsfortegnelse for siden. Bemærk at indholdsfortegnelsen bliver kun vist hvis der er nok plads på siden dvs. at vindeuet hvor siden vises er bredt nok.

Der findes flere variabler end dem som er nævnt/brugt indtil videre. Den fulde oversigt over variabler er til at finde [her](https://gohugo.io/content-management/front-matter/).

### Markdown
Dokumentationen skrives i et markup sprog kaldt Markdown. Markdown er et relativt lille markup sprog som også tillader rå HTML i sit indhold. Dette gør det ideelt til mindre sider hvor formateringen af teksten ikke kræver det store. Der findes mange forskellige "smag" af Markdown, men for denne side (altså for hele dokumentationssiden) anvendes der Goldmark. Du kan læse mere om selve Goldmark [her](https://github.com/yuin/goldmark), mens du [her](https://guides.github.com/features/mastering-markdown/#syntax) kan finde en cheat-sheet for generel markdown syntax.
