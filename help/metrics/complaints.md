---
title: Klagomål
description: 'Lär dig mer om klagomål som registreras när en användare anger att ett e-postmeddelande är oönskat eller oväntat. '
feature: Mätvärden
topics: Deliverability
kt: 7048
thumbnail: kt7048.jpg
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 550821608eb7049f739a156536dd31b6b2faa2fa
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 3%

---


# Klagomål

Klagomål registreras när en användare anger att ett e-postmeddelande är oönskat eller oväntat. Den här prenumerationsåtgärden loggas vanligtvis antingen via prenumerantens e-postklient när de klickar på skräppostknappen eller via ett skräppostrapporteringssystem från en annan leverantör.

## ISP-klagomål

De flesta Internet-leverantörer på nivå 1 och vissa på nivå 2 erbjuder en skräppostrapporteringsmetod till sina användare eftersom avanmälnings- och avanmälningsprocesser tidigare har använts på ett skadligt sätt för att validera en e-postadress. Adobe Campaign får dessa klagomål via ISP FBL. Detta fastställs under konfigurationsprocessen för alla Internet-leverantörer som tillhandahåller FBL och gör det möjligt för Adobe Campaign att automatiskt lägga till e-postadresser som klagas på karantänregistret för att undertryckas. Inspikar i ISP-klagomål kan vara en indikator på dålig listkvalitet, mindre än optimala listinsamlingsmetoder eller svaga engagemangspolicyer. De märks också ofta när innehållet inte är relevant.

## Klagomål från tredje part

Det finns flera skräppostgrupper som tillåter skräppostrapportering på en bred nivå. Kompletterande mätvärden som används av dessa tredje parter används för att tagga e-postinnehåll för att identifiera skräppost. Processen kallas även fingeravtryck. Användare av dessa metoder för klagomål från tredje part sparar vanligtvis mer information om e-post, så de kan ha större effekt än andra klagomål om de inte besvaras.

>[!NOTE]
>
>Internetleverantörer samlar in klagomål och använder dem för att fastställa en avsändares allmänna anseende. Alla klagomål ska undertryckas och inte längre kontaktas så snabbt som möjligt och i enlighet med lokala lagar och förordningar.

## Produktspecifika resurser

**Adobe Campaign Classic**

* [Spårningsindikatorer](https://experienceleague.adobe.com/docs/campaign-classic/using/reporting/reports-on-deliveries/delivery-reports.html#tracking-indicators)

**Adobe Campaign Standard**

* [Rapport om klagomål](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/complaints.html#reporting)