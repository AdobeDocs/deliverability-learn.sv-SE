---
title: Uppdatera studskompetens efter avbrott i Italia Online
description: Lär dig hur du uppdaterar studentkvalificering efter avbrott i Italia Online
feature: Deliverability
exl-id: a11e88cf-bf37-42cc-9c09-1d58360459b7
hide: true
hidefromtoc: true
role: Admin
level: Beginner
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 2%

---

# Uppdatera felaktiga hårda studsar efter avbrott i Italia Online {#update-bounce-italia}

## Kontext{#outage-context}

Från och med 22 januari (lokal tid) har Italia Online råkat ut för ett driftstopp som lett till flera förseningar och avvisade e-postmeddelanden. Tjänsten började återupptas med begränsad kapacitet den 26 jan.

De domäner som påverkas är: **libero.it**, **virgilio.it**, **inwind.it**, **iol.it** och **blu.it**.

Detta problem inträffade 2023-1-26, men de flesta av de felaktiga karantänerna inträffade den 26 januari.

Läs mer i det officiella meddelandet [här](https://tecnologia.libero.it/avviato-il-ritorno-online-di-libero-mail-e-virgilio-mail-66832){_blank}.


## Effekt{#outage-impact}

Precis som i de flesta fall när ett avbrott inträffar i en Internet-leverantör (ISP) markerades vissa e-postmeddelanden som skickades via Campaign eller Journey Optimizer felaktigt som studsar. Detta påverkade inte bara Adobe, utan alla som försökte få e-post levererad till Italia Online under driftstoppet.

Symtomen var:

* **Mjuka studsar** med meddelandet `452 requested action aborted: try again later` - dessa försökte automatiskt igen och inga åtgärder behövs.

* **Hårda studsar** med meddelandet `550 <email address> recipient rejected` returnerades av Internet-leverantören den 26 januari, mellan 8.00 och 2.00 lokal tid, för att förhindra att avsändare fortsätter överbelasta sina servrar. Som Italia Online Postmaster har bekräftat är dessa inte riktigt hårda studsar, så vi rekommenderar att alla e-postadresser som uteslöts den 26 januari 2023 på grund av meddelandet tas ur karantän.

## Process som ska uppdateras{#outage-update}

### Adobe Campaign{#ac-update}

Med hjälp av standardlogik för studshantering har Adobe Campaign automatiskt lagt till de här mottagarna i karantänlistan med **[!UICONTROL Status]**-inställningen **[!UICONTROL Quarantine]**. För att korrigera detta måste du uppdatera karantäntabellen i Campaign genom att hitta och ta bort de här mottagarna, eller ändra deras **[!UICONTROL Status]** till **[!UICONTROL Valid]** så att de tas bort i nattrensningsarbetsflödet.

Om du vill hitta de mottagare som påverkades av problemet, eller om det skulle inträffa igen med någon annan Internet-leverantör, kan du läsa instruktionerna nedan:

* För Campaign Classic v7 och Campaign v8, se [den här sidan](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html?lang=sv-SE#unquarantine-bulk){_blank}.
* Mer Campaign Standard finns på [den här sidan](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html?lang=sv-SE#unquarantine-bulk){_blank}.

### Adobe Journey Optimizer{#ajo-update}

Med hjälp av standardlogik för studshantering har Adobe Journey Optimizer automatiskt lagt till de här e-postadresserna till listan över utelämnanden med inställningen **[!UICONTROL Reason]** för **[!UICONTROL Invalid Recipient]**. För att korrigera detta måste du uppdatera listan över inaktiveringar genom att hitta och ta bort dessa e-postadresser.

När adresserna har identifierats kan de tas bort manuellt från listan över undertryckningar med knappen **[!UICONTROL Delete]**. Dessa adresser kan sedan inkluderas i framtida e-postkampanjer.

Läs mer i [det här avsnittet](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/monitor-reputation/manage-suppression-list.html?lang=sv-SE#remove-from-suppression-list){_blank}.

