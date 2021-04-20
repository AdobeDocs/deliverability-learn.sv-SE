---
title: Massutskick och blockering av e-postmeddelanden
description: Lär dig varför internetleverantörer placerar e-postmeddelanden i mappar för massutskick eller blockerar dem.
feature: Metrics
topics: Deliverability
kt: 7051
thumbnail: kt7051.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 4b280f90-73b9-4b88-adb8-57b6a46ddad7
translation-type: ht
source-git-commit: e433002423bd1ab2f4a89425198c16160dae0719
workflow-type: ht
source-wordcount: '322'
ht-degree: 100%

---

# Massutskick och blockering

## Massutskick

Massutskick leder till leverans av e-post till skräppostmappen hos en internetleverantör. Detta kan identifieras när en öppningshastighet (och ibland klickfrekvens) som är lägre än normalt kombineras med en hög levererad hastighet. Orsaken till att e-postmeddelanden klassificeras som massutskick varierar mellan olika internetleverantörer. I allmänhet hissas dock en varningsflagga för avsändarens anseende (till exempel listhygien) om meddelanden placeras i mappen för massutskick. Det är ett tecken på att anseendet försämras, vilket är ett problem som måste åtgärdas omedelbart innan det påverkar fler kampanjer. Samarbeta med er Adobe-levererbarhetskonsult för att åtgärda eventuella problem beroende på er situation.

## Blockering

En blockering uppstår när skräppostindikatorerna når internetleverantörens egen tröskel och internetleverantören börjar blockera e-post (märkbart genom utskicksförsök som studsar) från en avsändare. Det finns olika typer av blockeringar. I allmänhet förekommer blockeringar specifikt för en IP-adress, men de kan också förekomma på den sändande domänens eller entitetens nivå. För att lösa en blockering krävs särskild expertis, så kontakta din Adobe-levererbarhetskonsult för att få hjälp.

## Blockeringslistning

En blockeringslistning inträffar när en blockeringslistehanterare från en tredje part registrerar skräppostliknande beteende som är kopplat till en avsändare. Orsaken till en blockeringslista publiceras ibland av den blockeringslistande parten. En listning är vanligtvis baserad på IP-adress, men i allvarligare fall kan den baseras på IP-intervall eller till och med en sändande domän. För att lösa problem med blockeringslistning bör ni söka support från er Adobe-levererbarhetskonsult för att förhindra fler listningar. Vissa listningar är mycket allvarliga och kan orsaka långvariga och svåra anseendeproblem. Konsekvensen av en blockeringslistning varierar beroende på blockeringslistan, men kan påverka leveransen av all e-post.

## Ytterligare resurser

* Läs mer på [Real-time Blackhole Lists](/help/additional-resources/blocklist-databases.md) som underhåller databaser med IP-adresser och domäner som sannolikt kommer att användas av avsändare av skräppost.
