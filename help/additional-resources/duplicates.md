---
title: Dubbletter
description: Lär dig identifiera och begränsa dubbletter för att förbättra slutresultatet.
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
exl-id: f89dbb38-a8d4-4294-b017-6fed72591593
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 4%

---

# Dubbletter {#duplicates}

Att ha dubbla e-postadresser kan få flera följder:

* Samma meddelande skickas mer än en gång. Även om Adobe utför en dedupliceringsprocedur som standard innan det skickas, finns det inget som hindrar att samma meddelande skickas av olika åtgärder med samma innehåll när ett mål delas.
* Begäran om att avbryta prenumerationen har inte uppfyllts. Om en mottagare säger upp prenumerationen efter att ha fått ett meddelande, är deras duplicerade profil fortfarande berättigad till framtida meddelanden.

Förutom detta steg i anmälningsförfarandet kommer denna situation sannolikt att leda till att användare anser att meddelandena är skräppost och att utlösa ett förfarande för blockeringslista hos Internet-leverantören.

Du måste vara särskilt försiktig när du utför åtgärder i databasen:

* Import måste konfigureras noggrant, särskilt när du väljer avstämningsnyckel.
* Ändrade e-postadresser kan också vara en källa till dubbletter. Två adresser med olika domäner kan dirigeras till samma postlåda, till exempel om ett företag har ändrat namn och har underhållit den tidigare domänen under en viss tid: joe.doe@amce-co.com och joe.doe@acme-rebranded.com.
* Automatisk import, oavsett om det gäller listor eller från andra databaser, är element som ska beaktas vid hantering av profiler. Vad händer när du tar bort eller flyttar en profil i en annan partition? Den kan återskapas i den inledande partitionen med en automatisk import, till exempel när en inköpsorder placeras.
* Det går att implementera lagring av profiler i olika mappar med hjälp av vyer i stället för partitioner. På så sätt är du säker på att profilerna finns i samma fysiska partition samtidigt som du fortfarande aktiverar rätt behörigheter för att visas och hanteras.

Det finns också fall där dubbletter mellan olika partitioner är normala. Om du till exempel skickar för tredje part eller olika företagsenheter är det logiskt att samma person är mottagare av olika anledningar. Det är dock sällan normalt att hitta dubbletter inom samma partition.

## Produktspecifika resurser

Borttagning av dubbletter skyddar det avsändande anseendet och garanterar en god karantänhantering. Läs mer i följande avsnitt i produktdokumentationen:

**Adobe Campaign Classic**

* [Dedupliceringsaktivitet](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/targeting-activities/deduplication.html)
* [Använda sammanfogningsfunktionen för aktiviteten Deduplicering](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/deduplication-merge.html)

**Adobe Campaign Standard**

* [Deduplicera data från en importerad fil](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/workflow-use-case/data-management/deduplicating-data-imported-file.html)
