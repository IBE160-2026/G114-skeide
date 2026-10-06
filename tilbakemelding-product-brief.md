# Tilbakemelding på product brief

| | |
|---|---|
| **Gruppe** | G114 – G114-skeide |
| **Product brief** | `Product Brief.md` (commit f720cc4) |
| **Tilbakemelding fra** | Faglærer i IBE160 (utarbeidet med KI-støtte) |
| **Dato** | 2026-10-06 |

## Samlet vurdering

- **Bør revideres før dere går videre.** Rett punktene markert «Endre» før dere lager PRD og arkitektur.

**Det som er bra:**

1. Problemet er relevant og godt forklart: studenter og førstegangssøkere blir filtrert bort av ATS-systemer fordi de mangler riktige nøkkelord, og generiske KI-søknader blir avvist ved første øyekast. Idéen om Work Experience Buddy som et «recruitment mirror» som viser hvordan søknaden blir lest, er en god vinkling.
2. Briefen følger BMAD-strukturen med alle delene, og avgrensningen utad er tydelig: intervjusimulering, automatisk innsending, nettleserutvidelser, mobilapp og betalte abonnementer er ute.

**De viktigste endringene:**

1. Suksesskriteriene kan ikke måles i emnet. Brukerengasjement, konvertering, flere tilbakemeldinger fra arbeidsgivere innen 30 dager og gjenbruk er forretningsmål. Erstatt dem med funksjonelle kriterier, for eksempel «brukeren kan laste opp en CV og lime inn en stillingsannonse, og ser en liste over krav i annonsen som er oppfylt og ikke oppfylt i CV-en».
2. Gjør løsningen konkret. Briefen beskriver hva WEB skal oppnå, men lite om hvordan brukeren faktisk bruker den. Beskriv kjerneflyten i 3–5 steg, og hva «transparent» betyr i praksis: hva viser appen om hvordan ATS leser søknaden?
3. Avgrens v1. Sikker innlogging, CV i flere formater, annonse via direkte lenke, analyse, tilpasset søknadsbrev og eksport er mye for én person. Særlig henting av annonser via lenke er krevende og ustabilt, fordi hver jobbside er bygd ulikt. Start med innliming av tekst.

## Vanskelighetsgrad og gjennomførbarhet

### Vurdert vanskelighetsgrad

- **Middels**

**Sammenlignbart med:** 2) AI CV- og søknadsassistent (middels). Briefen dekker nesten de samme funksjonene: søknadsbrev, CV-forbedringer, gap-analyse og ATS-tilbakemelding.

**Begrunnelse:**

| Faktor | Nivå (lav / middels / høy) | Kommentar |
|---|---|---|
| Domenelogikk – hvor mange og hvor kompliserte regler og beregninger må stemme? | Middels | ATS-tilpasning og analyse av krav som er oppfylt eller ikke. Kan delvis gjøres regelbasert med nøkkelordsmatching, noe som gir kontrollerbar logikk. |
| Datamodell – antall entiteter og relasjoner mellom dem | Middels | Bruker, CV, stillingsannonse, analyse, søknadsbrev og eventuelt eksporterte versjoner. |
| Brukere, roller og innlogging | Middels | «Securing user authentication» er med i v1. Én rolle, men innlogging må gjøres trygt. |
| KI-funksjonalitet i appen, f.eks. kall til språkmodell, prompts i koden og håndtering av usikre svar | Høy | Søknadsbrev, CV-forslag og analyse med språkmodell. Krever at modellen ikke finner på erfaring. |
| Integrasjoner og eksterne tjenester, f.eks. API-er, betaling og e-post | Middels–høy | LLM-API med nøkkel og kostnad, og henting av annonser fra lenker (skraping). |
| Sanntid, samtidighet eller flere brukere som påvirker hverandre | Lav | Ingen. |
| Filhåndtering, f.eks. opplasting, PDF-lesing og eksport | Høy | CV i flere formater inn og eksport ut. Lesing av CV-er med kolonner og tabeller er ofte upålitelig. |
| Sikkerhet og personvern | Middels–høy | CV-er er personopplysninger og sendes til en ekstern KI-tjeneste. Briefen sier «secure», men ikke hvordan. |

**Hva vanskelighetsgraden betyr for dere:**

- _Middels:_ Et godt balansert valg. Pass på at kjerneflyten blir ferdig og stabil før dere legger til mer. For WEB kan kjerneflyten være: lim inn eller last opp CV (PDF eller tekst) → lim inn annonse → se analyse av oppfylte og manglende krav → få forslag til søknadsbrev som du kan redigere. Få den stabil før eksport og flere filformater.

### Gjennomførbarhet med BMAD og Claude Code

Dere skal planlegge med BMAD (product brief → PRD → arkitektur → epics og stories) og implementere med Claude Code. Vurderingen under tar hensyn til at det må være tid til hele denne flyten, og til testing, retting og README til slutt.

| Spørsmål | Vurdering (OK / risiko / stor risiko) | Kommentar |
|---|---|---|
| **Tid og omfang** – kan v1 realistisk bli ferdig og stabil i løpet av semesteret, med tid til flere iterasjoner? | Risiko | Seks funksjonsområder i v1 for én person er mye. Repoet har foreløpig bare briefen, så det haster å komme i gang med PRD. |
| **BMAD-flyten** – er briefen konkret nok til at PRD, arkitektur og stories kan lages uten store hull, og blir det overkommelig mange stories? | Risiko | Briefen er generell, og PRD-en vil mangle konkret flyt, brukssituasjoner og testbare krav. |
| **Egnet for Claude Code** – bruker løsningen en vanlig, godt dokumentert teknologistakk som Claude Code håndterer godt, eller krever den nisjeteknologi, spesialmaskinvare eller mye manuell konfigurasjon? | OK | Webapp med innlogging, filopplasting og LLM-API er godt dokumentert. Skraping av jobbsider er det minst forutsigbare. |
| **Kontroll på KI-ens arbeid** – kan gruppen selv avgjøre om koden gjør det riktige? Krever domenet kunnskap gruppen ikke har, f.eks. avanserte beregninger eller fagregler, så er det vanskelig å kvalitetssikre. | Risiko | Det er vanskelig å vite hvordan ekte ATS-systemer vurderer søknader. Vær ærlig om at appens «ATS-tilbakemelding» er en forenklet modell, og beskriv reglene den bruker. |
| **Testbarhet** – finnes det tydelige regler og forventede resultater som tester kan skrives mot? | Stor risiko | Ingen av suksesskriteriene kan bli testtilfeller. Faste testpar av fiktiv CV og annonse med kjente gap kan gi testbare forventninger. |
| **Kjørbar for sensor** – kan appen kjøres lokalt etter README, uten gruppens nøkler, betalte kontoer eller egen infrastruktur? | Stor risiko | Avhengig av LLM-nøkkel. Ingen plan for testmodus eller mock-svar er beskrevet. |
| **Avhengigheter og kostnader** – krever løsningen betalte API-er, f.eks. språkmodeller, og finnes det en plan for kostnad, testmodus eller mock-data? | Risiko | Modell, kostnad og testmodus er ikke nevnt. |

**Konklusjon om gjennomførbarhet:**

- **Gjennomførbart med justert omfang.** Se forslagene under.

**Forslag til justering av omfang eller vanskelighetsgrad:**

1. La v1 bestå av: CV som PDF eller tekst, annonse som innlimt tekst, analyse av oppfylte og manglende krav, og redigerbart søknadsbrev. Annonse via lenke, flere filformater og eksport flyttes til et senere trinn.
2. Gjør «transparens» til en konkret, testbar funksjon: vis hvilke nøkkelord fra annonsen som finnes og mangler i CV-en, med en enkel, forklart poengsum. Det kan beregnes i kode og testes, uavhengig av språkmodellen.

## Hvorfor product brief er viktig for mappen

Product brief er utgangspunktet for PRD, arkitektur, stories og til slutt koden. Del 1 av mappen vurderes blant annet på om sensor kan følge en sporbar vei fra plan til ferdig app. Den vurderes også på om appen gjør det dere har beskrevet, om den er testet, om den er godt designet, og om den kan kjøres etter README. Et uklart, for stort eller for lite brief gjør alt dette vanskeligere senere. Det er mye enklere å rette nå enn sent i semesteret.

## 1. Gjennomgang av briefens deler

| Del av brief | Status | Kommentar |
|---|---|---|
| Executive Summary – er det klart hva appen er, og hvilket problem den løser? | OK | Det er klart at WEB skal hjelpe studenter med å tilpasse CV og søknad til annonser og ATS. |
| The Problem – er problemet konkret, med reelle situasjoner og brukere? | Juster | Godt forklart, men uten en konkret situasjon. Beskriv for eksempel en student som søker deltidsjobb og får avslag uten å vite hvorfor. |
| The Solution – beskriver løsningen brukeropplevelsen, ikke bare teknologi? | Endre | Beskriver mål og fordeler, ikke hva brukeren gjør. Legg til kjerneflyten steg for steg og hva brukeren ser i analysen. |
| What Makes This Different – er vurderingen ærlig og realistisk? | Juster | «Recruitment mirror» er en god idé, men nevn konkrete eksisterende verktøy (for eksempel Jobscan og Teal) og hva WEB gjør annerledes enn dem. |
| Who This Serves – er primærbrukerne tydelige, og vet vi hva de trenger? | OK | Studenter som søker deltids- eller førstegangsjobb er en tydelig primærbruker. |
| Success Criteria – kan kriteriene faktisk sjekkes eller testes? | Endre | Alle kriteriene er forretnings- eller bruksmål som ikke kan måles i emnet. Legg til funksjonelle, testbare kriterier. |
| Scope – er det klart hva som er med i første versjon, og hva som ikke er det? | Juster | Tydelig hva som er ute, men «In»-delen er stor for én person. Prioriter og flytt annonse via lenke og eksport til senere trinn. |
| Vision – henger visjonen sammen med resten uten å blåse opp omfanget? | OK | Karriereveiledning og læringsstier er en naturlig retning og holdes utenfor v1. |

## 2. Utgangspunkt for del 1 av mappen

Punktene følger kriteriene i sensorveiledningen for del 1. Vektene i parentes viser hvor mye hvert kriterium teller i del 1.

| Kriterium i del 1 | Hva briefen bør legge til rette for | Status | Kommentar |
|---|---|---|---|
| **1. Prosess og KI-styring** (30 %) | Brief som er presis nok til at PRD og stories kan bygges direkte på den, slik at krav kan spores fra brief til kode. | Endre | Briefen er for generell til at krav kan spores fra brief til kode. Lag den gjerne på nytt med `bmad-product-brief`, og legg den i en BMAD-mappe (for eksempel `_bmad-output/planning-artifacts/`). |
| **2. Funksjonalitet og omfang** (20 %) | Realistisk omfang for gruppen og semesteret: en tydelig kjerneflyt som kan bli ferdig og stabil, og nok innhold til å vise reell funksjonalitet. | Juster | Nok funksjonalitet, men for mye på én gang for én person. Avgrens som foreslått over. |
| **3. Kvalitetssikring og testing** (15 %) | Suksesskriterier og funksjoner som er konkrete nok til å bli testtilfeller. | Endre | Ingen testbare kriterier. Nøkkelordsanalyse i kode og faste testpar gir et godt testgrunnlag. |
| **4. Design og brukeropplevelse** (10 %) | Tydelige brukere og brukssituasjoner som designet kan bygges rundt, gjerne med de viktigste skjermbildene eller flytene skissert. | Juster | Brukeren er tydelig, men flyten er ikke beskrevet. Skisser analysevisningen og redigeringen av søknadsbrevet. |
| **5. Kodekvalitet og arkitektur** (10 %) | Teknologivalg som er begrunnet og ikke mer komplekse enn appen trenger. | OK | Ingen teknologivalg i briefen, og det er riktig. Hold arkitekturen enkel. |
| **6. README og kjørbarhet** (10 %) | Løsning som andre kan kjøre lokalt uten betalte kontoer, og uten tilgang til gruppens egne tjenester og nøkler. | Endre | Avhengig av LLM-nøkkel. Planlegg `.env.example`, testmodus og fiktive eksempel-CV-er og -annonser. |
| **7. Ryddighet i repoet** (5 %) | En plan for hvor hemmeligheter, testdata og dokumentasjon skal ligge. | Juster | Briefen ligger i roten med mellomrom i filnavnet. Flytt planleggingsdokumenter til en egen mappe. Ekte CV-er skal ikke ligge i det offentlige repoet. |

## 3. Neste steg for gruppen

1. Skriv om suksesskriteriene til funksjonelle, testbare kriterier, og fjern forretningsmålene eller flytt dem til visjonen.
2. Beskriv kjerneflyten steg for steg og hva «transparent ATS-tilbakemelding» konkret viser, og avgrens v1 til innlimt annonse og CV som PDF eller tekst.
3. Velg språkmodell og beskriv testmodus. Gå deretter videre til PRD.

Oppdater product brief i repoet når dere har gjort endringene, slik at historikken viser hvordan planen utviklet seg. Det er en del av prosessen sensor ser etter.
