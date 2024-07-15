---
title: Öka e-postens anseende med IP-uppvärmning
description: Lär dig varför det är viktigt att förbättra ert e-postanseende med IP-uppvärmning och hur ni går vidare för optimala leveransmöjligheter.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: b553a13e-2055-4abc-b784-fd52792380d0
source-git-commit: eba8162150b5662ca18687b873114858f8eb00cc
workflow-type: tm+mt
source-wordcount: '1582'
ht-degree: 2%

---

# Öka e-postens anseende med IP-uppvärmning

<!--Increase your email reputation with IP warming

## IP Warming overview

In the Adobe Deliverability Consulting and Deliverability Operations teams, we have a vested interest in helping new Campaign customers be as successful as possible as they embark on the route of an IP warming process. If you’ve never been a part of such a project, you may have a lot of questions about it. Let’s get down to the details!-->

## Komma igång

Adobe kräver att kunderna delar sin konfiguration för att hjälpa Adobe Deliverability-teamet att förstå ert unika program. De frågor vi ställer är utformade för att hjälpa Adobe Deliverability-teamet att få en uppfattning om ditt sändningsrykte och din e-postvolym. Utan en konkret förståelse för er affärsmodell, era marknadsföringsmål för e-post och anseende kommer vi inte att kunna anpassa vår strategi och det finns risk för leveransproblem.

Till att börja med får du egna dedikerade IP-adresser (Internet Protocol). När du skickar e-post är en IP-adress den väg som används för att leverera e-postmeddelanden till dina kunder. IP-adresser och domäner används för att identifiera avsändare i ett nätverk till mottagande Internet-leverantörer. Adobe tilldelar rätt antal dedikerade IP-adresser för att skicka e-post, baserat på din sändningsvolym, e-postprogram, datasegmenteringsrutiner och ditt kontrakt.

**Relaterade ämnen:**
* [Smidig övergång vid byte av e-postplattform](../../help/transition-process/switching-email-platforms.md)
* [IP-strategi](../../help/transition-process/infrastructure.md#ip-strategy)
* [Internetleverantörspecifika hänsyn under IP-uppvärmning](../../help/transition-process/isp-specific-considerations-during-ip-warming.md)

## IP-värmare: Varför är det klart? {#why-ip-warming}

Internet-leverantörer (ISP) eller MBP (Mailbox Providers) vidtar försiktighetsåtgärder när de upptäcker en okänd IP-adress och sändande domän. Internetleverantörer/MBP håller IP-adressen och den sändande domänen under hög granskning för att avgöra om e-postmeddelanden som skickas från den här IP-adressen och domänen är skräppost eller inte. Det här är standardproceduren som associeras med alla nya sändande IP-adresser, oavsett avsändartyp.

Internetleverantörer granskar noggrant sändningsvolymen, sändningsfrekvensen, klagomål och avhoppsfrekvensen som genererats av dessa utskick. De kontrolleras noga eftersom de är tecken på avsändarens anseende - vare sig det är bra eller dåligt.

Denna process med att undersöka dessa datapunkter tar naturligtvis tid och kan inte göras på en dag eller två. Anseendet byggs över tid. Det här är som att låta en främling vara hemma. Har du reservationer för att någon du aldrig träffat ska komma in i ditt hem?

Troligen är svaret ja. Du skulle vilja analysera den här personen och deras motiv. Betyder de skada? Är de ett hot? Internet-leverantörer gör samma sak för att skydda sitt nätverk från skadlig eller oönskad trafik. Positiva anseendevärden hjälper er att komma långt i en framgångsrik process för IP-uppvärmning. Därför betonar vi vikten av att börja med att skicka små e-postvolymer och börja skicka till era engagerade kunder först. Mer information finns i [Målvillkor när ny trafik skickas](/help/transition-process/targeting-criteria.md).

Att skicka stora mängder e-post från en helt ny IP eller IP-adress direkt från porten är en dålig praxis och kommer troligen att orsaka leveransproblem. Observera att även om du börjar skicka små volymer och gradvis ökar dem enligt rekommendationerna, är det fortfarande nödvändigt att följa vedertagna standarder för e-postmarknadsföring.

![](../../help/assets/ip-warming-volume-trend.png)

## Tillstånd till e-post (explicit deltagande)

Detta är den viktigaste komponenten när det gäller att hantera och utöka en prenumerantlista via e-post. I takt med att lagstiftningen mot skräppost växer och blir mer omfattande internationellt bör det vara en marknadsförares primära fokus att se till att de har fått uttryckligt (eller uttryckligt) medgivande från varje abonnent på deras lista. Det innebär att varje prenumerant aktivt har gått med på att ta emot e-postmeddelanden från ert varumärke. Detta skiljer sig från implicit samtycke där en person läggs till i en e-postlista efter att ha vidtagit en åtgärd som inte uttryckligen registrerade sig för ett e-postprogram.

Läs mer om [Adobe&#39;s Acceptable Use Policy](https://www.adobe.com/legal/terms/aup.html).

## Anseendemått: Vad är Internet-leverantörer som letar efter?

Internet-leverantörer använder avancerad teknik för att fatta väl underbyggda beslut om huruvida de ska leverera e-post de får från externa nätverk eller inte. De har ibland komplicerade och egenutvecklade algoritmer i sin verktygslåda som kan hjälpa dem i den här processen.

Några av de granskade datapunkterna är:

* Skräppostfällor
* Träffar i Blockeringslista
* E-poststudsar
* Prenumerationsengagemang

Internetleverantörer behöver särskilda tekniska konfigurationer som är anpassade till deras policyer och bästa praxis. Adobe konfigurerar dina IP-adresser och delegerade underdomäner för att identifiera dig som en ansvarsfull och betrodd avsändare. Detta kallas [e-postautentisering](/help/transition-process/infrastructure.md#authentication). Autentisering hjälper mottagare att validera om en avsändare har behörighet att skicka från den IP-adressen eller domänen.

Autentisering gör det möjligt för Internet-leverantörer att validera att det företag som skickar från en domän eller IP har rätt att göra det. Det handlar om att bevisa din identitet och se till att du inte låtsas vara någon annan och att någon annan inte låtsas vara du.

På Adobe kommer vi att konfigurera SPF och DKIM som standard och vi kommer att konfigurera DMARC på begäran. Internetleverantörer refererar SPF och DKIM som de primära autentiseringsuppgifterna. Många Internet-leverantörer införlivar också DMARC (domänbaserad meddelandeautentisering, rapportering och överensstämmelse) i sina filtreringsbeslut. Oautentiserade e-postmeddelanden är inte nödvändigtvis blockerade, men de går igenom ytterligare filtrering.

## IP-uppvärmning: Vad kan man förvänta sig

### Begränsad eller blockerad e-post

Spammar skickar hela tiden från nya IP-adresser - de bränns genom en pool med IP-adresser tills de stängs och processen upprepas i en annan pool med IP-adresser. Därför behandlar internetleverantörer trafik som skickas från nya IP-adresser noggrant. De blockerar IP-adresser från att skicka stora mängder e-post eftersom de misstänker att detta är en skadlig aktivitet som utförs av spamrater.

Därför är det inte ovanligt att du får meddelanden om uppskov eller strypning när du börjar posta från dina nya IP-adresser. Efter några försök skickas meddelandet normalt.

Det kan ta några dagar att uppnå ett normalt trafikflöde genom Internet-leverantörer som skjuter upp nya avsändare. Men sluta inte skicka e-post - fortsätt att fokusera på att bara skicka till de mest engagerade e-postprenumeranterna.

I sällsynta fall blockerar Internet-leverantören den nya avsändaren. Adobe övervakar ditt konto, och om ett sådant block misstänks, kommer att kontakta Internet-leverantören för att försöka åtgärda situationen på bästa möjliga sätt.

Kom ihåg att konsekvens är avgörande här. Oregelbundna sändningsmönster och oregelbundna e-postmönster kommer att medföra vissa leveransproblem.

### Klagomål

[Klagomål](/help/metrics/complaints.md) uppstår när en prenumerant märker ett e-postmeddelande som skräppost via sitt e-postprogram. Detta skickar ett meddelande till Internet-leverantören om klagomålet. Om det finns tillräckligt många klagomål som kommer in i Internet-leverantören kommer Internet-leverantören att agera för att skydda sina kunder - eventuellt blockera många e-postmeddelanden från att komma åt prenumeranterna eller dirigera en del e-postmeddelanden till huvudmappen i motsats till prenumerantens inkorgar. Om ditt leveransproblem orsakas av klagomål är det viktigt att fastställa varför mottagarna klagar.

Prenumeranter klagar av olika anledningar. Ibland vill en prenumerant inte få mer e-post från dig, kanske för att de känner att de får för många meddelanden om samma ämne, inte förväntade sig meddelandet eller inte kommer ihåg att registrera sig för att ta emot e-postmeddelanden.

### Datasäkerhet

Hårda studsar inträffar när du skickar till en adress som inte kan levereras hos en Internet-leverantör. En adress kan vara olevererbar av många anledningar, t.ex. ett misstag vid inskrivning av adressen eller utskick till en adress som tidigare var aktiv men som har stängts eller avslutats efter en inaktivitetsperiod.

Om du stöter på ett stort antal hårda studsar är det viktigt att förstå varför. Granska hur adresserna samlades in och bekräfta att behörighet gavs. Ibland stänger man e-postkontot och meddelar inte de som har den adressen i marknadsföringslistan.

### Engagemang

Internetleverantörer letar efter enhetlig volym och god datakvalitet. Ni kommer att öka trafiken långsamt och stadigt under de kommande fyra till åtta veckorna. Ibland tar det mer eller mindre tid att utföra en avbildning baserat på volym och mål, men vanligtvis är det minst en 8-veckorsprocess.

E-posttrafiken bör ske långsamt och stadigt, och öka varje vecka tills hela listan har skickats. Dessutom följer varje segment schemat tills det är klart. Börja med de senaste prenumeranterna först och avsluta med de minst engagerade prenumeranterna sist. Observera också att vissa internetleverantörer kan behöva en mer anpassad strategi på grund av hur de hanterar ny trafik.

Läs mer om [engagemang](/help/engagement.md).

## Stanna kvar på kursen

Du kan vara benägen att skynda på processen med IP-uppvärmning genom att skicka fler volymer än vad som rekommenderas. Du slipper lägga tid på att identifiera dina mest engagerade prenumeranter och inte skicka e-post till dessa prenumeranter först i ett försök att skapa ett positivt rykte. Snälla motstå detta begär! Det kommer inte att hjälpa dig på lång sikt.

Det är mycket viktigt att du börjar skicka e-post med ditt mycket engagerade innehåll (med e-post!) prenumeranter endast för de inledande faserna av IP-uppvärmningen. Dessa kunder är era mest värdefulla och deras benägenhet att öppna era e-postmeddelanden hjälper er att börja visa internetleverantörer som ni är en marknadsförare som skickar intresseväckande och eftersökta e-postmeddelanden. Här visas även internetleverantörer som ni följer reglerna och följer bästa praxis.

## Slutsats

Kom ihåg: IP-uppvärmning är en maraton - inte en sprint!  Processen kan verka betungande och tidskrävande, men det skulle vara mer värt att försöka reparera ett rykte som skadats genom att inte följa beprövade och sanna e-postrutiner.

Ju bättre sändningsrutiner är, och ju högre anseendepoängen är hos Internet-leverantörer, desto troligare blir det att era e-postmeddelanden levereras. IP-uppvärmning och förstärkning, tillsammans med bästa praxis för utformning av e-postmeddelanden, hjälper dig att optimera leveransen till inkorgen.

Vårt globala leveransteam är er partner i den här processen och kommer att hjälpa er att lyckas under den här IP-uppvärmningsfasen.
