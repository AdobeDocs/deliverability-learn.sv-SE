---
title: Vad är leveransstrategin och hur ni definierar leveransmöjligheterna?
description: Förstå hur ni definierar leveransmöjligheterna, varför det är viktigt och de viktigaste leveransvärdena.
feature: levererbarhet
topics: Deliverability
kt: 5255
thumbnail: kt5255.jpg
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: d42a8c3b06308fca0cf3e9db8d634a767fc0cdc6
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 11%

---


# Leveransstrategi och definition

Att utforma framgångsrika e-postmarknadsföringskampanjer kräver en tydlig förståelse för marknadsföringsmålen, oavsett om det gäller prospektering eller CRM-initiativ (customer relationship management). Detta hjälper till att avgöra vem som ska målgruppsanpassas, vad som ska befordras och när utåtriktad marknadsföring är idealisk.

Här är några exempel på strategiska mål för e-postmarknadsföring:

* Förvärva nya kunder
* Konvertera potentiella kunder till förstagångsköpare
* Växande kundrelationer med ytterligare kunderbjudanden
* Behålla lojala kunder
* Förbättra kundnöjdheten och varumärkeslojaliteten
* Återaktivera förlorade eller annullerade kunder

## Leveransdefinition

Det finns två viktiga mätvärden som spelar en roll när det gäller definitionen av leveransbarhet. *Leveransfrekvensen* är procentandelen e-postmeddelanden som inte studsar och som accepteras av Internet-leverantören. Nästa är *placering av inkorgen* - detta används för meddelanden som accepteras av Internet-leverantören och avgör om e-postmeddelandet kommer i inkorgen eller i skräppostmappen.

Det är viktigt att förstå både leveransfrekvensen och inkorgplaceringsfrekvensen i kombination med varandra när du mäter e-postprestanda. En hög leveransfrekvens är inte den enda aspekten av leveransförmågan. Bara för att ett meddelande tas emot via en Internet-leverantörs första kontrollpunkt behöver det inte nödvändigtvis innebära att din prenumerant faktiskt såg och interagerade med din kommunikation.

## Varför leverans är viktigt

Om du inte vet om dina e-postmeddelanden levereras eller om de landar i inkorgen jämfört med skräppostmappen bör du göra det. Här är varför.

Otaliga timmar går till planering och produktion av era e-postkampanjer. Om e-postmeddelandena studsar eller i slutändan hamnar i era prenumeranters skräppostmapp kommer kunderna förmodligen inte att läsa dem, din uppmaning att vidta åtgärder kommer inte att godkännas och du kommer inte att få några intäkter på grund av förlorade konverteringar. Kort och gott: ni har inte råd att ignorera möjligheten att leverera. Det är avgörande för att ni ska lyckas med era e-postmarknadsföringssatsningar och era slutresultat.

Med hjälp av bästa praxis för leverans kan du vara säker på att din e-post får bästa möjliga chans att öppnas, klickas och det ultimata målet - konvertering. Du kan skriva ett lysande ämne och ha vackra bilder och engagerande innehåll. Men om e-postmeddelandet inte levereras har kunden inte möjlighet att konvertera. När det gäller e-postleverans beror alla steg i processen för godkännande av e-post helt och hållet på det första för att programmet ska lyckas.

### Steg 1: E-post levererad

Viktiga faktorer för leveransen:

* **Solid infrastruktur**: IP- och domänkonfiguration, inställning av feedbackslinga (FBL) (inklusive övervakning och bearbetning av klagomål) och regelbunden studsbearbetning. För Adobe-klienter ansvarar Adobe för den här konfigurationen för våra kunders räkning.
* **Stark autentisering**:  [!DNL Sender Policy Framework] (SPF),  [!DNL DomainKeys Identified Mail] (DKIM),  [!DNL Domain-based Message Authentication]Reporting och Conformance (DMARC).
* **Hög listkvalitet**: Explicit deltagande, giltiga metoder för e-postförvärv och engagemangspolicyer.
* **Konsekvent utskicksfrekvens och minimering av volymfluktuationer**.
* **Hög IP och gott anseende**.

### Steg 2: E-postinkorgsplacering

Internetleverantörer har unika, komplexa och ständigt föränderliga algoritmer för att avgöra om din e-post placeras i inkorgen, skräppostmappen eller skräppostmappen.

Här är några viktiga faktorer för placering av inkorgen:

* Levererat e-postmeddelande
* Högt engagemang
* Låga klagomål (mindre än 0,1 procent totalt)
* Enhetlig volym
* Låg skräppostsvällning
* Låg hård studsfrekvens
* Brist på blockeringslista

### Steg 3: E-postengagemang - öppnar

Här är några viktiga faktorer för öppen ränta:

* E-post som levereras och landas i inkorgen
* Varumärkesigenkänning
* Engagerande ämnesrad och sidhuvuden
* Personanpassning
* Frekvens
* Innehållets relevans eller värde

### Steg 4: E-postinteraktion - klick

Här är några viktiga faktorer för klickfrekvens:

* E-post levererad, landad i inkorgen och öppnad
* Stark CTA
   * Detta är den främsta åtgärden ni vill uppnå från er målgrupp. Normalt är det ett klick på en URL. Se till att det är tydligt och enkelt för användaren att hitta.
* Innehållets relevans eller värde

### Steg 5: Konvertering

Här är några viktiga konverteringsfaktorer:

* Alla de ovanstående
* Övergång från e-post via en fungerande URL-adress till en landningssida eller försäljningssida
* Landing page experience
* Varumärkesigenkänning, -uppfattning och -lojalitet

### Potentiell påverkan på intäkterna

Konvertering är nyckeln, men vad är alternativet? Er leveransstrategi kan stärka eller skapa problem med e-postmarknadsföringsprogrammet nirvana. Följande diagram visar den potentiella intäktsförlust som en svag leveransstrategi kan ha på ert marknadsföringsprogram. Som du kan se för ett företag med en konverteringsgrad på 2 procent och ett genomsnittsköp på 100 dollar är varje minskning på 10 procent av inkorgen nästan 20 000 dollar. Tänk på att dessa nummer är unika för alla avsändare.

| Skickat | Procent | Levererat | Procent | Inkorg | Nummer finns inte i inkorgen | Konverteringsgrad | Antal förlorade | Genomsnittlig | Förlorad |
|------|-----------|-----------|----------|-------|---------------------|-----------------|-----------------|----------|-----------|
|  | levererad |  | inkorg |  |  |  | Konverteringar | köp | omsättning |
| 100 kB | 99% | 99 kB | 100 % | 99 kB | - | 2% | 0 | 100 dollar | $ - |
| 100 kB | 99 % | 99 kB | 90 % | 89,1 kB | 9 900 | 2 % | 198 | 100 dollar | 19 800 dollar |
| 100 kB | 99 % | 99 kB | 80 % | 79,2 kB | 19 800 | 2 % | 396 | 100 dollar | 39 600 dollar |
| 100 kB | 99 % | 99 kB | 70 % | 69,3 kB | 29 700 | 2 % | 594 | 100 dollar | 59 400 dollar |
| 100 kB | 99 % | 99 kB | 60 % | 59,4 kB | 39 600 | 2 % | 792 | 100 dollar | 79 200 dollar |
| 100 kB | 99 % | 99 kB | 50 % | 49,5 kB | 49 500 | 2 % | 990 | 100 dollar | 99 000 dollar |
| 100 kB | 99 % | 99 kB | 40 % | 39,6 kB | 59 400 | 2 % | 1188 | 100 dollar | 118 800 dollar |
| 100 kB | 99 % | 99 kB | 30 % | 29,7 kB | 69 300 | 2 % | 1386 | 100 dollar | 138 600 dollar |
| 100 kB | 99 % | 99 kB | 20 % | 19,8 kB | 79 200 | 2 % | 1584 | 100 dollar | 158 400 dollar |
