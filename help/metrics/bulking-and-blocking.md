---
title: Skicka och blockera e-postmeddelanden
description: Lär dig varför Internet-leverantörer placerar e-postmeddelanden i massmappar eller blockerar dem.
feature: Mätvärden
topics: Deliverability
kt: 7051
thumbnail: kt7051.jpg
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 283f1cb2bb40818e11daa1a3753e8428b47e08ee
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---


# Massor och blockering

## Bulkar

Bulking är leveransen av e-post till skräppostmappen hos en Internet-leverantör. Den kan identifieras när en öppningshastighet som är lägre än normalt (och ibland klickfrekvens) kombineras med en hög levererad hastighet. Orsaken till varför e-postmeddelanden skickas varierar mellan olika Internet-leverantörer. I allmänhet krävs dock en flagga som påverkar utskickandet av rykte (till exempel listhygien) för att meddelanden ska placeras i mappen bulk. Det är en signal på att ryktet minskar, vilket är ett problem som måste korrigeras omedelbart innan det påverkar fler kampanjer. Samarbeta med er Adobe-konsult för att åtgärda eventuella problem som beror på er situation.

## Blockera

Ett block uppstår när skräppostindikatorerna når en egen Internet-leverantör och Internet-leverantören börjar blockera e-post (märkbart genom utskicksförsök) från en avsändare. Det finns olika typer av block. I allmänhet förekommer block som är specifika för en IP-adress, men de kan också förekomma på den sändande domänen eller entitetsnivån. För att lösa ett block krävs särskild expertis, så kontakta din Adobe-konsult för att få hjälp.

## Blockeringslistning

En blockeringslistning inträffar när en blockeringslista-hanterare från tredje part registrerar skräppostliknande beteende som är kopplat till en avsändare. Orsaken till ett blockeringslista publiceras ibland av den blocklist parten. En lista är vanligtvis baserad på IP-adress, men i allvarligare fall kan det vara via IP-intervall eller till och med en sändande domän. För att lösa problem med blockeringslistning bör ni få support från er Adobe-konsult för att kunna lösa och förhindra fler listor. Vissa listor är mycket allvarliga och kan orsaka långvariga och svåra problem med anseendet. Resultatet av en blockeringslistning varierar beroende på blockeringslista, men kan påverka leveransen av all e-post.

## Ytterligare resurser

* Läs mer på [Svarta listor i realtid](/help/additional-resources/blocklist-databases.md) som underhåller databaser med IP-adresser och domäner som kan komma att användas av spamrater.
