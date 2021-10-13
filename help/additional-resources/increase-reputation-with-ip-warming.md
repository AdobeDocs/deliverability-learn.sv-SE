---
title: Öka e-postens anseende med IP-uppvärmning
description: Learn why it is important to improve your email reputation with IP warming, and how to proceed for optimal deliverability.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: b553a13e-2055-4abc-b784-fd52792380d0
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '1600'
ht-degree: 2%

---

# Öka e-postens anseende med IP-uppvärmning

<!--Increase your email reputation with IP warming

## IP Warming overview

In the Adobe Deliverability Consulting and Deliverability Operations teams, we have a vested interest in helping new Campaign customers be as successful as possible as they embark on the route of an IP warming process. If you’ve never been a part of such a project, you may have a lot of questions about it. Let’s get down to the details!-->

## Komma igång

Adobe kräver att kunderna delar sin konfiguration för att hjälpa Adobe Deliverability-teamet att förstå ert unika program. De frågor vi ställer är utformade för att hjälpa Adobe Deliverability-teamet att få en uppfattning om ditt rykte och din e-postvolym. Utan en konkret förståelse för er affärsmodell, era marknadsföringsmål för e-post och anseende kommer vi inte att kunna anpassa vår strategi och det finns risk för leveransproblem.

Till att börja med får du egna dedikerade IP-adresser (Internet Protocol). När du skickar e-post är en IP-adress den väg som används för att leverera e-postmeddelanden till dina kunder. IP-adresser och domäner används för att identifiera avsändare i ett nätverk till mottagande Internet-leverantörer. Adobe assigns the appropriate number of dedicated IP addresses for sending emails, based on your sending volume, email programs, data segmentation practices, and your contract.

**Relaterade ämnen:**
* [Smidig övergång vid byte av e-postplattform](../../help/transition-process/switching-email-platforms.md)
* [IP-strategi](../../help/transition-process/infrastructure.md#ip-strategy)
* [Internetleverantörspecifika hänsyn under IP-uppvärmning](../../help/transition-process/isp-specific-considerations-during-ip-warming.md)

## IP-uppvärmning: Varför är det gjort? {#why-ip-warming}

Internet-leverantörer (ISP) eller MBP (Mailbox Providers) vidtar försiktighetsåtgärder när de upptäcker en okänd IP-adress och sändande domän. Det här är standardproceduren som associeras med alla nya sändande IP-adresser, oavsett avsändartyp. ISPs/MBPs put the IP and sending domain under high scrutiny to determine if the emails being sent from this IP and domain are spam or not.  Det här är standardproceduren som associeras med alla nya sändande IP-adresser, oavsett avsändartyp.

Internetleverantörer granskar noggrant sändningsvolymen, sändningsfrekvensen, klagomål och avhoppsfrekvensen som genererats av dessa utskick. De kontrolleras noga eftersom de är tecken på avsändarens anseende - vare sig det är bra eller dåligt.

Denna process med att undersöka dessa datapunkter tar naturligtvis tid och kan inte göras på en dag eller två. Anseendet byggs över tid. This process is like letting a stranger in your home. Would you have reservations about having someone you have never met enter your home?

Troligen är svaret ja. Du skulle vilja analysera den här personen och deras motiv. Betyder de skada? Är de ett hot? Internet-leverantörer gör samma sak för att skydda sitt nätverk från skadlig eller oönskad trafik. Positiva anseendevärden hjälper er att komma långt i en framgångsrik process för IP-uppvärmning. Därför betonar vi vikten av att börja med att skicka små e-postvolymer och börja skicka till era engagerade kunder först. Mer information om detta finns i [Målvillkor när du skickar ny trafik](/help/transition-process/targeting-criteria.md).

Att skicka stora mängder e-post från en helt ny IP eller IP-adress direkt från porten är en dålig praxis och kommer troligen att orsaka leveransproblem. Observera att även om du börjar skicka små volymer och gradvis ökar dem enligt rekommendationerna, är det fortfarande nödvändigt att följa vedertagna standarder för e-postmarknadsföring.

![](../../help/assets/ip-warming-volume-trend.png)

## Tillstånd till e-post (explicit deltagande)

Detta är den viktigaste komponenten när det gäller att hantera och utöka en prenumerantlista via e-post. I takt med att lagstiftningen mot skräppost växer och blir mer omfattande internationellt bör det vara en marknadsförares primära fokus att se till att de har fått uttryckligt (eller uttryckligt) medgivande från varje abonnent på deras lista. Det innebär att varje prenumerant aktivt har gått med på att ta emot e-postmeddelanden från ert varumärke. Detta skiljer sig från implicit samtycke där en person läggs till i en e-postlista efter att ha vidtagit en åtgärd som inte uttryckligen registrerade sig för ett e-postprogram.

Läs mer om [Adobe’s Acceptable Use Policy](https://www.adobe.com/legal/terms/aup.html).

## Anseendemått: Vad letar internetleverantörer efter?

Internet-leverantörer använder avancerad teknik för att fatta väl underbyggda beslut om huruvida de ska leverera e-post de får från externa nätverk eller inte. De har ibland komplicerade och egenutvecklade algoritmer i sin verktygslåda som kan hjälpa dem i den här processen.

Några av de granskade datapunkterna är:

* Skräppostfällor
* Blockeringslista träffar
* E-poststudsar
* Prenumerationsengagemang

Internetleverantörer behöver särskilda tekniska konfigurationer som är anpassade till deras policyer och bästa praxis. Adobe konfigurerar dina IP-adresser och delegerade underdomäner för att identifiera dig som en ansvarsfull och betrodd avsändare. Detta kallas [e-postautentisering](/help/transition-process/infrastructure.md#authentication). Autentisering hjälper mottagare att validera om en avsändare har behörighet att skicka från den IP-adressen eller domänen.

Autentisering gör det möjligt för Internet-leverantörer att validera att det företag som skickar från en domän eller IP har rätt att göra det. Det handlar om att bevisa din identitet och se till att du inte låtsas vara någon annan och att någon annan inte låtsas vara du.

På Adobe kommer vi att konfigurera SPF och DKIM som standard och vi kommer att konfigurera DMARC på begäran. Internetleverantörer refererar SPF och DKIM som de primära autentiseringsuppgifterna. Många Internet-leverantörer införlivar också DMARC (domänbaserad meddelandeautentisering, rapportering och överensstämmelse) i sina filtreringsbeslut. Oautentiserade e-postmeddelanden är inte nödvändigtvis blockerade, men de går igenom ytterligare filtrering.

## IP Warming: What to expect

### Begränsad eller blockerad e-post

Spammar skickar hela tiden från nya IP-adresser - de bränns genom en pool med IP-adresser tills de stängs och processen upprepas i en annan pool med IP-adresser. Därför behandlar internetleverantörer trafik som skickas från nya IP-adresser noggrant. De blockerar IP-adresser från att skicka stora mängder e-post eftersom de misstänker att detta är en skadlig aktivitet som utförs av spamrater.

Därför är det inte ovanligt att du får meddelanden om uppskov eller strypning när du börjar posta från dina nya IP-adresser. Efter några försök skickas meddelandet normalt.

Det kan ta några dagar att uppnå ett normalt trafikflöde genom Internet-leverantörer som skjuter upp nya avsändare. Men sluta inte skicka e-post - fortsätt att fokusera på att bara skicka till de mest engagerade e-postprenumeranterna.

I sällsynta fall blockerar Internet-leverantören den nya avsändaren. Adobe övervakar ditt konto, och om ett sådant block misstänks, kommer att kontakta Internet-leverantören för att försöka åtgärda situationen på bästa möjliga sätt.

Kom ihåg att konsekvens är avgörande här. Oregelbundna sändningsmönster och oregelbundna e-postmönster kommer att medföra vissa leveransproblem.

### Klagomål

[](/help/metrics/complaints.md) Slutför när en prenumerant märker ett e-postmeddelande som skräppost via sitt e-postprogram. Detta skickar ett meddelande till Internet-leverantören om klagomålet. Om det finns tillräckligt många klagomål som kommer in i Internet-leverantören kommer Internet-leverantören att agera för att skydda sina kunder - eventuellt blockera många e-postmeddelanden från att komma åt prenumeranterna eller dirigera en del e-postmeddelanden till huvudmappen i motsats till prenumerantens inkorgar. Om ditt leveransproblem orsakas av klagomål är det viktigt att fastställa varför mottagarna klagar.

Prenumeranter klagar av olika anledningar. Sometimes a subscriber doesn’t want to receive any more email from you, perhaps because they feel they’re getting too many messages on the same topic, they weren’t expecting the message, or don’t remember signing up to receive your emails.

### Data validity

Hårda studsar inträffar när du skickar till en adress som inte kan levereras hos en Internet-leverantör. An address can be undeliverable for many reasons, such as a mistake in typing the address or mailing to an address that was previously active but has been closed or terminated after a period of inactivity.

If you encounter a substantial number of hard bounces, it’s important to understand why. Granska hur adresserna samlades in och bekräfta att behörighet gavs. Sometimes people close their email account and don’t notify those who have that address on their marketing list.

### Engagemang

Internetleverantörer letar efter enhetlig volym och god datakvalitet. Ni kommer att öka trafiken långsamt och stadigt under de kommande fyra till åtta veckorna. Sometimes ramp-ups require more or less time based on your volume and goals, but typically, it is at least an 8-week process.

Email traffic should deploy in a slow and steady progression, increasing each week until the entire list has been sent. Dessutom följer varje segment schemat tills det är klart. Start with the most recent subscribers first, and finish with the least engaged subscribers last. Observera också att vissa internetleverantörer kan behöva en mer anpassad strategi på grund av hur de hanterar ny trafik.

Learn more on [engagement](/help/engagement.md).

## Stay the course

You may be tempted to rush the process of IP warming by sending more volume than is recommended, neglecting to spend time identifying your most-engaged subscribers and failing to mail these subscribers first in an effort to build a positive reputation. Please resist this urge! Det kommer inte att hjälpa dig på lång sikt.

Det är mycket viktigt att du börjar skicka e-post med ditt mycket engagerade innehåll (med e-post!) prenumeranter endast för de inledande faserna av IP-uppvärmningen. Dessa kunder är era mest värdefulla och deras benägenhet att öppna era e-postmeddelanden hjälper er att börja visa internetleverantörer som ni är en marknadsförare som skickar intresseväckande och eftersökta e-postmeddelanden. It also shows ISPs that you are playing by the rules and following best practices.

## Slutsats

Remember: IP Warming is a marathon - not a sprint!  Processen kan verka betungande och tidskrävande, men det skulle vara mer värt att försöka reparera ett rykte som skadats genom att inte följa beprövade och sanna e-postrutiner.

Ju bättre sändningsrutiner är, och ju högre anseendepoängen är hos Internet-leverantörer, desto troligare blir det att era e-postmeddelanden levereras. IP-uppvärmning och förstärkning, tillsammans med bästa praxis för utformning av e-postmeddelanden, hjälper dig att optimera leveransen till inkorgen.

Vårt globala leveransteam är er partner i den här processen och kommer att hjälpa er att lyckas under den här IP-uppvärmningsfasen.
