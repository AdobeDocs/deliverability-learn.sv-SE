---
title: Microsoft (Hotmail, Outlook, Windows Live etc.)
description: Microsoft är vanligtvis den näst största eller tredje största leverantören beroende på hur listan är uppbyggd, och de hanterar trafik något annorlunda jämfört med andra Internet-leverantörer.
topics: Deliverability
jira: KT-5319
doc-type: article
activity: understand
role: Admin, Leader, User
level: Beginner
team: TM
exl-id: d706cb90-828a-4ab3-8f93-c9bd71553d63
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 1%

---

# [!DNL Microsoft] ([!DNL Hotmail], [!DNL Outlook], [!DNL Windows Live] osv.)

[!DNL Microsoft] är vanligtvis den näst största eller tredje största leverantören beroende på hur din lista är uppbyggd, och de hanterar trafik något annorlunda än andra Internet-leverantörer.

Här är några högdagrar:

## Vilka data är viktiga

[!DNL Microsoft] fokuserar på avsändarens rykte, klagomål, användarinteraktion och deras egen grupp med betrodda användare (kallas även Sender Reputation Data eller SRD) som de avfrågar efter feedback.

## Vilka data finns tillgängliga?

Med det egna avsändarrapporteringsverktyget [!DNL Smart Network Data Services] (SNDS) för [!DNL Microsoft] kan du se mått för hur mycket e-post du skickar och hur mycket e-post som accepteras, liksom klagomål och skräppostfällor. Tänk på att delade data är ett exempel och inte återspeglar exakta tal, men det bästa är att det visar hur [!DNL Microsoft] ser dig som avsändare. [!DNL Microsoft] lämnar ingen offentlig information om sin betrodda användargrupp, men dessa data är tillgängliga via programmet [!DNL Return Path Certification] mot en extra avgift.

## Avsändarens rykte

[!DNL Microsoft] har traditionellt fokuserats på att skicka IP i sina namnbedömningar och filtreringsbeslut. De arbetar aktivt med att utöka sina funktioner för att skicka domäner. Båda drivs till stor del av de traditionella anseendepåverkarna, som klagomål och skräppostfällor. Leveransmöjligheterna kan också i hög grad påverkas av programmet för certifiering av returtåglägen, som har specifika kvantitativa och kvalitativa programkrav.

## Insikter

[!DNL Microsoft] kombinerar alla sina mottagande domäner för att etablera och spåra avsändarens rykte. Detta inkluderar [!DNL Hotmail], [!DNL Outlook], MSN, [!DNL Windows Live] och så vidare, samt alla e-postmeddelanden som skickas via Office 365. [!DNL Microsoft] kan vara särskilt känslig för volymfluktuationer, så du bör använda specifika strategier för att öka och minska från stora utskick i stället för att tillåta volymbaserade plötsliga ändringar.

[!DNL Microsoft] är också särskilt strikt under IP-uppvärmningens första dagar, vilket vanligtvis innebär att de flesta e-postmeddelanden filtreras först. De flesta internetleverantörer anser att avsändarna är oskyldiga tills de har bevisats skyldiga. [!DNL Microsoft] är motsatsen och anser dig skyldig tills du bevisar dig oskyldig.
