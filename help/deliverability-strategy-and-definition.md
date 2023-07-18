---
title: Vad är levererbarhetstrategin och hur definierar jag levererbarheten?
description: Förstå hur lervererbarhet definieras, varför det är viktigt och de viktigaste levererbarhetsvärdena.
topics: Deliverability
jira: KT-5255
thumbnail: kt5255.jpg
doc-type: article
activity: understand
role: Admin, Leader, User
level: Beginner
team: ACS
exl-id: 5285eda9-5099-48d5-b150-ce2c376ee549
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 100%

---

# Levererbarhetsstrategi och definition

För att utforma framgångsrika e-postmarknadsföringskampanjer krävs en klar förståelse av marknadsföringsmålen, oavsett om det gäller prospektering eller CRM-hantering (customer relationship management). Detta hjälper dig att avgöra vilka som ingår i målgruppen, vad som ska marknadsföras och när tillfället är idealiskt.

Här är några exempel på strategiska mål för e-postmarknadsföring:

* Skaffa nya kunder
* Konvertera prospekt till förstagångsköpare
* Öka kundrelationerna med ytterligare erbjudanden
* Behålla lojala kunder
* Förbättra kundnöjdheten och varumärkeslojaliteten
* Återaktivera förlorade eller missade kunder

## Definition av levererbarhet

Det finns två viktiga mätvärden för definitionen av levererbarhet. Det första är *leveransfrekvensen* som är procentandelen av e-postmeddelanden som inte studsar och som accepteras av internetleverantören. Det andra är *inkorgsplaceringen* som gäller för meddelanden som accepteras av internetleverantören och visar om e-postmeddelandet hamnar i inkorgen eller i skräppostmappen.

Det är viktigt att förstå både leveransfrekvensen och inkorgsplaceringsfrekvensen i förhållande till varandra när du mäter e-postprestandan. En hög leveransfrekvens är inte den enda aspekten av levererbarheten. Att ett meddelande tas emot via en internetleverantörs första kontrollpunkt innebär inte nödvändigtvis att din prenumerant faktiskt såg och interagerade med din kommunikation.

## Varför levererbarhet är viktigt

Om du inte vet om dina e-postmeddelanden levereras eller om de landar i inkorgen eller skräppostmappen borde du veta det. Här är anledningen.

Otaliga timmar går åt till att planera och producera era e-postkampanjer. Om e-postmeddelandena studsar eller i slutändan hamnar i prenumeranternas skräppostmapp kommer kunderna förmodligen inte att läsa dem, din handlingsuppmaning kommer inte att följas och du får inga intäkter på grund av uteblivna konverteringar. Kort och gott: du har inte råd att ignorera levererbarheten. Den är avgörande för hur framgångsrika dina e-postmarknadsföringssatsningar blir och för ditt slutresultat.

Genom att följa bästa praxis för levererbarhet kan du säkerställa att din e-post får bästa möjliga chans att öppnas och klickas, och att du når det ultimata målet: konvertering. Du kan skriva en lysande ämnesrubrik och ha vackra bilder och engagerande innehåll. Men om e-postmeddelandet inte levereras har du inte möjlighet att konvertera kunden. När det gäller e-postlevererbarhet är varje steg i processen för mottagande av e-post beroende av det föregående steget för att programmet ska lyckas.

### Steg 1: E-post levererad

Viktiga faktorer för leveransen:

* **Solid infrastruktur**: IP- och domänkonfiguration, inställning av återkopplingsslinga (FBL) (inklusive övervakning och bearbetning av klagomål) och regelbunden studsbearbetning. För Adobe-klienter ansvarar Adobe för den här konfiguration för våra klienters räkning.
* **Stark autentisering**: [!DNL Sender Policy Framework] (SPF), [!DNL DomainKeys Identified Mail] (DKIM), [!DNL Domain-based Message Authentication], Reporting och Conformance (DMARC).
* **Hög listkvalitet**: Uttrycklig önskan att delta, giltiga metoder för e-postförvärv och engagemangspolicyer.
* **Konsekvent utskicksfrekvens och minimering av volymfluktuationer**.
* **Högt IP- och domänanseende**.

### Steg 2: Inkorgsplacering för e-post

Internetleverantörer har unika, komplexa och ständigt föränderliga algoritmer för att avgöra om din e-post placeras i inkorgen eller skräppostmappen.

Här är några viktiga faktorer för placering i inkorgen:

* Levererat e-postmeddelande
* Högt engagemang
* Få klagomål (mindre än 0,1 % totalt)
* Enhetlig volym
* Låg skräppostfångst
* Låg hård studsfrekvens
* Frånvaro av blockeringslisteproblem

### Steg 3: E-postengagemang – öppnar

Här är några viktiga faktorer för öppningsfrekvens:

* E-post som levereras och landar i inkorgen
* Varumärkesigenkänning
* Engagerande ämnesrad och rubriker
* Personalisering
* Frekvens
* Innehållets relevans eller värde

### Steg 4: E-postinteraktion – klick

Här är några viktiga faktorer för klickfrekvens:

* E-post levererad, landade i inkorgen och öppnades
* Stark CTA
   * Detta är den främsta åtgärden ni vill att er målgrupp ska utföra. Normalt är det ett klick på en URL. Se till att det är tydligt och enkelt för användaren att hitta.
* Innehållets relevans eller värde

### Steg 5: Konvertering

Här är några viktiga konverteringsfaktorer:

* Alla de ovanstående
* Övergång från e-post via en fungerande URL-adress till en landningssida eller försäljningssida
* Upplevelsen på landningssidan
* Igenkänning av, uppfattning om och lojalitet mot varumärket

### Potentiell påverkan på intäkterna

Konvertering är nyckeln, men vad är alternativet? Er levererbarhetsstrategi kan stärka ert e-postmarknadsföringsprogram eller ställa till problem med det. Följande diagram visar den potentiella intäktsförlust som en svag leveransstrategi kan ha på ert marknadsföringsprogram. Som du kan se motsvarar varje minskning på 10 % av inkorgsplaceringen nästan 20 000 dollar för ett företag med en konverteringsgrad på 2 % och ett genomsnittsköp på 100 dollar. Tänk på att dessa siffror är unika för alla avsändare.

| Skickat | Procent | Levererat | Procent | Inkorg | Antal inte i inkorgen | Konverteringsgrad | Antal förlorade | Genomsnittlig | Förlorad |
|------|-----------|-----------|----------|-------|---------------------|-----------------|-----------------|----------|-----------|
|      | levererad |           | inkorg |       |                     |                 | Konverteringar | köp | intäkt |
| 100 000 | 99 % | 99 000 | 100 % | 99 000 | – | 2 % | 0 | 100 USD | USD – |
| 100 000 | 99 % | 99 000 | 90% | 89 100 | 9 900 | 2 % | 198 | 100 USD | 19 800 USD |
| 100 000 | 99 % | 99 000 | 80% | 79 200 | 19 800 | 2 % | 396 | 100 USD | 39 600 USD |
| 100 000 | 99 % | 99 000 | 70 % | 69 300 | 29 700 | 2 % | 594 | 100 USD | 59 400 USD |
| 100 000 | 99 % | 99 000 | 60 % | 59 400 | 39 600 | 2 % | 792 | 100 USD | 79 200 USD |
| 100 000 | 99 % | 99 000 | 50 % | 49 500 | 49 500 | 2 % | 990 | 100 USD | 99 000 USD |
| 100 000 | 99 % | 99 000 | 40 % | 39 600 | 59 400 | 2 % | 1 188 | 100 USD | 118 800 USD |
| 100 000 | 99 % | 99 000 | 30 % | 29 700 | 69 300 | 2 % | 1386 | 100 USD | 138 600 USD |
| 100 000 | 99 % | 99 000 | 20% | 19 800 | 79 200 | 2 % | 1 584 | 100 USD | 158 400 USD |
