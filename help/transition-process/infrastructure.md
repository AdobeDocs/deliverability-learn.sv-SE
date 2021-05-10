---
title: Infrastruktur
description: 'Lär dig vad som krävs för att skapa en e-postinfrastruktur på rätt sätt. '
feature: Övergångsprocess
topics: Deliverability
kt: 7052
thumbnail: kt7052.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 4025d95c-cc77-4e0c-9904-aaf60019b18c
translation-type: tm+mt
source-git-commit: 65eb1fd03e6a6617ef24661c371f850d1f8e6054
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 0%

---

# Infrastruktur

Framgångsrik leverans är beroende av en stark grund. E-postinfrastruktur är en viktig del. En korrekt konstruerad e-postinfrastruktur innehåller flera komponenter - nämligen domän(er) och IP-adress(er). Dessa komponenter är som maskinerna bakom de e-postmeddelanden du skickar och de är ofta navet för att skicka anseende. Produktionskonsulter ser till att dessa element är korrekt konfigurerade under implementeringen, men på grund av anseendeelementet är det viktigt för dig att ha denna grundläggande förståelse.

## Domänkonfiguration och strategi {#domain-setup-and-strategy}

Tiderna har ändrats och vissa Internet-leverantörer (som Gmail och Yahoo) har nu lagt till domänens anseende som en extra poäng när det gäller att knyta e-postens anseende till en avsändare. Ditt domänrykte baseras på din avsändande domän i stället för på din IP-adress. Detta innebär att ert varumärke har företräde när det gäller beslut om filtrering av Internet-leverantörer.

En del av introduktionsprocessen för nya avsändare på Adobe-plattformar är bland annat att konfigurera dina sändande domäner och se till att din infrastruktur är korrekt etablerad. Du bör samarbeta med en expert om vilka domäner du tänker använda på lång sikt. Här följer några tips som utgör en bra domänstrategi:

* Var så tydlig och reflekterande som möjligt av varumärket med den domän du väljer, så att användarna inte felaktigt identifierar posten som skräppost. Några exempel är nyhetsbrev.foo.com, kvitton.foo.com och så vidare.
* Du bör inte använda din förälder eller företagsdomän eftersom det kan påverka leveransen av post från organisationen till Internet-leverantörer.
* Använd en underdomän till din överordnade domän för att legitimera den sändande domänen.
* Separera dina underdomäner för meddelandekategorierna Transactional och Marketing. Detta gör att e-posttrafiken kan flöda på ett mer tillförlitligt sätt eftersom internetleverantörer letar efter den här sändningsmetoden, som är en känd e-postmetod och som rekommenderas varmt.

## IP-strategi {#ip-strategy}

Det är viktigt att utforma en välstrukturerad IP-strategi för att skapa ett gott rykte. Antalet IP-adresser och inställningar varierar beroende på er affärsmodell och era marknadsföringsmål. Samarbeta med en expert för att ta fram en tydlig strategi för att komma igång. Tänk på följande saker som är viktiga att tänka på:

* **Alltför många** IPscan utlöser anseendeproblem eftersom det är en vanlig taktik hos skräppost till  **snöskon**, vilket är en taktik som används av skräppost där trafiken sprids över många IP-adresser för att maximera leveransen av skräppost. Även om du inte är skräppost kan du se ut som en om du använder för många IP-adresser, särskilt om dessa IP-adresser inte har haft någon tidigare trafik.
* **Alltför få** IPscan orsakar dataflödesproblem och kan ge upphov till anseendeproblem. Dataflödet varierar beroende på Internet-leverantör. Hur mycket och hur snabbt en Internet-leverantör är villig att acceptera baseras vanligtvis på sin infrastruktur och sina anseendetrösklar.
* Det är viktigt att separera trafiken för meddelandetyper. Det är viktigt att i minsta möjliga mån avgränsa marknadsföring och transaktionell post i separata IP-pooler.
* Beroende på er e-poststrategi kan det även vara tillrådligt att separera olika produkter eller marknadsföringsströmmar från olika IP-pooler om ert rykte är drastiskt annorlunda. Vissa marknadsförare segmenterar också efter region. Att separera IP-adressen för trafik med lägre anseende löser inte problemet med anseende, men det förhindrar problem med e-postleveranser med gott anseende. Ni vill trots allt inte offra er goda publik för en mer riskfylld.

## Feedback-slingor {#feedback-loops}

Bakom kulisserna behandlar Adobe-plattformarna data om studsar, klagomål, avanmälan med mera. Inställningarna av dessa feedbackslingor är en viktig del av leveransen. Klaganden kan skada ett anseende, så du bör skicka e-postadresser som registrerar klagomål från målgruppen. Observera att Gmail inte skickar tillbaka dessa data. Avsändningsrubriker och interaktionsfiltrering är särskilt viktiga för Gmail-prenumeranter, som nu har större delen av prenumerationsdatabaserna.

## Autentisering {#authentication}

Autentisering är den process som Internet-leverantörer använder för att validera en avsändares identitet. De två vanligaste autentiseringsprotokollen är [!DNL Sender Policy Framework] (SPF) och [!DNL DomainKeys Identified Mail] (DKIM). Dessa är inte synliga för slutanvändaren, men hjälper Internet-leverantörer att filtrera e-post från verifierade avsändare. [!DNL Domain-based Message Authentication Reporting and Conformance] (DMARC) blir allt populärare, även om dess policyer ännu inte införlivats av alla internetleverantörer i deras anseende.

### SPF

[!DNL Sender Policy Framework] (SPF) är en autentiseringsmetod som gör att ägaren till en domän kan ange vilka e-postservrar som de använder för att skicka e-post från den domänen.

### DKIM

[!DNL Domain Keys Identified Mail] (DKIM) är en autentiseringsmetod som används för att identifiera förfalskade avsändaradresser (kallas ofta förfalskning). Om DKIM är aktiverat kan mottagaren bekräfta om avsändaren har behörighet att skicka e-post från den domänen.

### DMARC

[!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC) är en autentiseringsmetod som gör att domänägare kan skydda sin domän från obehörig användning. DMARC använder SPF eller DKIM eller båda för att tillåta en domänägare att kontrollera vad som händer med e-post som inte kan autentiseras: levererat, i karantän eller avvisat.

## Produktspecifika resurser

**Campaign**

* Lär dig hur du delegerar en underdomän till Adobe Campaign Classic eller Standard i [det här avsnittet](/help/additional-resources/ac-domain-name-setup.md).
* [Kontrollpanelen: Fullständig delegering av underdomäner (självstudiekurs)](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) -  *Lär dig hur du delegerar en underdomän till Adobe Campaign Classic fullständigt.*
* [Kontrollpanelen: Fullständig delegering av underdomäner (självstudiekurs)](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) -  *Lär dig hur du delegerar en underdomän till Adobe Campaign Standard fullständigt.*
* Läs mer om hur du implementerar en feedbackslinga för en Campaign Classic-instans i [det här avsnittet](/help/additional-resources/acc-technical-recommendations.md#feedback-loop-acc).

## Ytterligare resurser

* Läs mer om autentiseringsmetoderna SPF, DKIM och DMARC i [det här avsnittet](/help/additional-resources/authentication.md).
* Lär dig mer om hur du kan förbättra ditt e-postanseende med IP-uppvärmning i [det här avsnittet](/help/additional-resources/increase-reputation-with-ip-warming.md).
