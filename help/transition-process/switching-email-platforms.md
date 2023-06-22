---
title: Smidig övergång vid byte av e-postplattform.
description: När du flyttar ESP:er (e-postleverantörer) är det inte möjligt att även övergå befintliga, etablerade IP-adresser. Det är viktigt att ni följer bästa praxis för att utveckla ett positivt rykte när ni börjar om på nytt.
topics: Deliverability
jira: KT-5259
thumbnail: kt5259.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 5444d576-5bc1-4fa6-9956-c63dc3c60440
source-git-commit: 9444f8601f2f349398ee5deb9d5f4d4f7abb44f5
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 11%

---

# Smidig övergång vid byte av e-postplattform

När du flyttar ESP (e-postleverantörer) går det inte att även övergå befintliga, etablerade IP-adresser. Det är viktigt att ni följer bästa praxis för att utveckla ett positivt rykte när ni börjar om på nytt. Eftersom de nya IP-adresserna som du kommer att använda ännu inte har något anseende kan internetleverantörerna inte lita fullständigt på den post som kommer från dem och måste vara försiktiga med vad de tillåter att de levereras till sina kunder.

Att skapa ett gott rykte är en process. Men när det väl är fastställt kommer små negativa indikatorer att ha mindre betydelse för er och era postutskick.

![Övergångsprocess](../assets/transition-process.png)

Hur lång tid det tar att värma upp dina IP-adresser och domäner kan variera, men upp till åtta veckors test är vanligt för vanliga avsändare att etablera ett anseende hos de flesta Tier 1-leverantörer (Gmail, Microsoft, Verizon/Yahoo/AOL, etc.).

I nästa avsnitt kommer vi att undersöka några viktiga områden som vi ska fokusera på att ta med oss på rätt sätt:

1. [Infrastruktur](/help/transition-process/infrastructure.md)
2. [Målkriterier](/help/transition-process/targeting-criteria.md)
3. [Internetleverantörspecifika hänsyn under IP-uppvärmning](/help/transition-process/isp-specific-considerations-during-ip-warming.md)
4. [Volym](/help/transition-process/volume.md)
