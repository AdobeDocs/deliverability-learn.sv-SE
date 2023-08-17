---
title: Felsökning av slutprodukter
description: Lär dig att identifiera och åtgärda leveransproblem.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4cc85124-e7e4-4cd5-99a9-23d2d8cf08fe
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 0%

---

# Felsökning av slutprodukter {#troubleshooting}

Nedan följer några tips som kan hjälpa dig att identifiera och åtgärda leveransproblem.

## Identifiera ett leveransproblem {#identify-deliverability-issue}

För att identifiera ett eventuellt problem, de element som listas på [den sidan](/help/ongoing-monitoring.md) kan dra uppmärksamheten till dig.

<!--
Mailing or campaign metrics: unsubscribe, abuse complaint and/or bounce rates are higher than usual.
Subscriber activity: opens, clicks and/or transactions are lower than usual.
Seed accounts show filtered or non-delivered mailings.
-->

## Hypotetiskt möjliga orsaker {#potential-causes}

Ställ följande frågor för att identifiera möjliga orsaker till leveransproblemet:

* Har det nyligen skett en förändring i listsegmenteringen?
* Har jag skaffat några nya datakällor?
* Skickade jag oavsiktligt en karantänfil?
* Kan problemet bero på mitt meddelandeinnehåll?
* Skickas jag ofta tillräckligt för att behålla värma IP-adresser?
* Segmenterar jag mina utskick efter aktivitet/engagemang, eller skickar jag fullständiga filer?
* Vad är det &quot;säkra&quot; segmentet i min fil i termer av senaste användning?
* Har jag strategier för omaktivering och återbekräftelse för segment som inte är definierade som säkra?

## Åtgärda problemet {#address-issue}

### Klagomål

[Klagomål](/help/metrics/complaints.md) definieras av prenumeranter som rapporterar e-post som skräppost genom att klicka på motsvarande knapp i sin inkorg.

Om ditt leveransproblem har orsakats av klagomål:
* Du måste försöka avgöra varför mottagarna klagar.
* Du kan också överväga att flytta länken för att avbryta prenumerationen till det övre av e-postmeddelandet. Detta uppmuntrar prenumeranter att avbryta prenumerationen i stället för att klaga på skräppostknappen.

Avsändare kan samla in mycket information från sina [feedback-slinga](/help/transition-process/infrastructure.md#feedback-loops) klagomål:
* Det är viktigt att lagra data och leta efter mönster i saker som anmälningskälla, hur länge adressen har prenumererats eller till och med vissa beteendedemografiska profiler.
* Klagomål kan ofta identifiera en riskfylld datakälla eller ett risksegment i filen. Risky definieras som den som mest sannolikt klagar, vilket kan skada anseendet, och i sin tur antalet inkorgar.

Klagomålen kommer också från prenumeranter som inte längre vill ha e-post:
* Detta kan ofta bero på för många meddelanden, att de som prenumererar inte uppfattade meddelandet, att de inte förväntade sig meddelandet eller inte minns att de valde.
* Det är också viktigt att utföra en granskning för att säkerställa att alla insamlingspunkter är tydliga och att det inte finns några förkryssade kryssrutor i inköpsplatserna.
* Du bör också skicka ett välkomstmeddelande när prenumeranter väljer att gå med för att ange tonen och förklara hur ofta de kan förvänta sig att få e-postmeddelanden från dig.

### Datasäkerhet

**Hårda studsar** inträffar när du skickar till en **adress som inte kan levereras** på en Internet-leverantör. En adress kan vara olevererbar av många orsaker, till exempel:
* Felstavad adress. Detta kan åtgärdas med en datavalideringstjänst i realtid eller genom att kräva en bekräftad anmälan innan marknadsföringsmeddelanden skickas till den adressen.
* Ogiltig lista eller datakälla. Om det kommer från en ny källa kontrollerar du hur adresserna samlades in och att det fanns behörighet.
* Posta till en adress som vid ett tillfälle var aktiv, men som har stängts eller avslutats efter en inaktivitetsperiod.

### Engagemang

Förutom klagomål och informationskvalitet har internetleverantörer mer koncentrerat sig på **positivt engagemang** för att fatta leveransbeslut. De vill se om era prenumeranter öppnar era e-postmeddelanden eller tar bort dem utan att läsa dem. Eftersom de inte delar dessa data med avsändare måste vi använda den information vi har och översätta öppningar/klick/transaktioner som engagemang.

Som en del av det pågående anseendeunderhållet är det viktigt att förstå hur engagerade prenumeranter finns på din lista och utveckla en **riskhierarki för senaste nytt** för prenumeranterna på varje fil. Senaste tid definieras som senaste öppnings-/klicknings-/transaktionsdatum eller registreringsdatum. Tidsramen kan skilja sig åt lodrätt. Så här gör du:

1. Fastställ aktiva (&quot;säkra&quot;) segment för varje vertikal. Detta är vanligtvis prenumeranter som har varit aktiva under de senaste 3-6 månaderna.
1. Minska frekvensen för inaktivitet.
1. Skapa en [återengagemang](/help/additional-resources/re-engagement.md) för måttliga riskinaktioner. Det är normalt 6-9 månader utan engagemang.
1. Utveckla en bekräftelsekampanj för riskinaktivitet. Detta är vanligtvis prenumeranter som inte har interagerat med ett e-postmeddelande på 9-12 månader.
1. Slutligen anger du en avlämningsregel och tar bort prenumeranter som inte har öppnat e-postmeddelanden på&quot;x&quot; månader. Vi rekommenderar vanligtvis 12+ månader, men det kan skilja sig åt beroende på försäljnings- och köpcykel.
