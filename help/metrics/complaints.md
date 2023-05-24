---
title: Klagomål
description: Lär dig mer om klagomål som registreras när en användare anger att ett e-postmeddelande är oönskat eller oväntat.
topics: Deliverability
kt: 7048
thumbnail: kt7048.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 0343820d-f5af-4b8a-bcab-dbb47ae7aecb
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 100%

---

# Klagomål

Klagomål registreras när en användare anger att ett e-postmeddelande är oönskat eller oväntat. Den här prenumerationsåtgärden loggas vanligtvis antingen via prenumerantens e-postklient när prenumeranten klickar på skräppostknappen, eller via ett skräppostrapporteringssystem från en annan leverantör.

## Klagomål från internetleverantörer

De flesta internetleverantörer på nivå 1, och vissa på nivå 2, erbjuder en funktion för skräppostrapportering till sina användare eftersom avanmälningsprocesser tidigare har använts på ett skadligt sätt för att validera e-postadresser. Adobe Campaign tar emot sådana klagomål via internetleverantörens återkopplingsslinga. Detta fastställs under konfigurationsprocessen för alla internetleverantörer som tillhandahåller återkopplingsslinga, så att Adobe Campaign automatiskt kan lägga till e-postadresser som har klagat i karantänlistan. Ökningar av internetleverantörsklagomål kan indikera dålig listkvalitet, listinsamlingsmetoder som inte är optimala eller svaga engagemangspolicyer. Ofta observeras de även när innehållet inte är relevant.

## Klagomål från tredje part

Det finns flera skräppostgrupper som tillåter en bredare skräppostrapportering. Klagomålsmätvärden som används av dessa tredje parter används för att tagga e-postinnehåll för att identifiera skräppost. Processen kallas även signaturinsamling. De som använder dessa metoder för klagomål från tredje part har vanligtvis större kunskap om e-post, så klagomålen kan få större effekt än andra klagomål om de inte besvaras.

>[!NOTE]
>
>Internetleverantörer samlar in klagomål och använder dem för att avgöra en avsändares allmänna anseende. För varje klagomål ska e-postadressen snarast tillföras undantagslistan och inte längre kontaktas, i enlighet med lokala lagar och förordningar.

## Produktspecifika resurser

**Adobe Campaign Classic**

* [Spårningsindikatorer](https://experienceleague.adobe.com/docs/campaign-classic/using/reporting/reports-on-deliveries/delivery-reports.html?lang=sv#tracking-indicators)

**Adobe Campaign Standard**

* [Rapport om klagomål](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/complaints.html?lang=sv#reporting)
