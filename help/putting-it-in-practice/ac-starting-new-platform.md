---
title: Starta en ny plattform
description: Läs mer om hur man hanterar slutprodukter när man startar en ny plattform med Adobe Campaign.
feature: I praktiken
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 96ed84da391faaabd3001ddd6a411ddc1f46b033
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 3%

---


# Starta en ny plattform {#starting-new-platform}

Det är viktigt att du behåller domänens och IP-adressens anseende när du skapar en ny plattform som ska användas med Adobe Campaign.

## Ett känsligt steg

Du bör vara mycket försiktig när du börjar skicka e-postmeddelanden på en ny plattform, eftersom plattformen inte har någon brukshistorik och inget rykte när de sändande IP-adresserna aldrig har använts för detta ändamål.

Internetleverantörer misstänker naturligtvis IP-adresser som aldrig har använts för att skicka e-post och som plötsligt börjar skicka stora volymer e-posttrafik. För skräppost används i allmänhet&quot;okända&quot; IP-adresser (adresser som aldrig har varit på blockeringslista) för att skicka så många meddelanden som möjligt före identifieringen.

Du kan inte förvänta dig att uppnå driftshastighet i form av utdata i början av produktionsfasen. Du bör inte heller försöka skicka meddelanden i den här hastigheten eftersom det kan leda till att internetleverantörerna blockerar sändningsadresserna och allvarligt äventyrar resten av startfasen.

## Huvudprinciper

Nedan anges de huvudprinciper som ska följas när en ny plattform startas.

* Konfigurera en dedikerad underdomän som är specifik för e-postkampanjer som skickas från Adobe.

* Om du har den här informationen kan du **importera ogiltiga adresser till karantäntabellen**.
Det händer ofta att en plattform startas när en adresslista används för första gången och som kanske inte är fullständigt kvalificerad. Om du skickar till ogiltiga adresser eller till honeypoadresser kommer detta att bidra till att minska plattformens anseende.

   * Om du har en lista med ogiltiga adresser är det bäst att importera den till karantäntabellen innan du skickar den första gången. Karantäntabellen är tillgänglig via menyerna **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Non deliverables and addresses]** (Campaign Classic) och **[!UICONTROL Administration > Channels > Quarantines > Addresses]** (Campaign Standard).

   * Om du ändå vill ange de ogiltiga adresserna är det bäst att göra detta när plattformens rykte väl har etablerats och bitvis för att &quot;tona ned&quot; användningen av dåliga adresser över tiden.

* **Begränsa** genomströmningshastigheten genom att begränsa antalet matriser. Kontakta Adobe Campaign-administratören om du vill ha mer information om hur du justerar sådana tekniska inställningar.

* **Öka volymen gradvis** för att undvika att markeras som skräppost. Använd inte hela databasen som mål från början, utan lägg till en extra del av listan varje gång du skickar. Detta bör göra att du kan öka volymen i varje steg och samtidigt minska den totala hastigheten för ogiltiga adresser. För att säkerställa en smidig utveckling av startfasen kan du använda vågor.

* **Skicka regelbundet**. I viss utsträckning är det bättre att skicka små tagningar regelbundet i stället för stora kampanjer sporadiskt.
* **Var noga med leveransrapporterna**. Höga felindikatorer kan innebära att en teknisk inställning är felaktigt konfigurerad.

## Ytterligare resurser

Mer information om de principer som anges ovan och hur de tillämpas med Adobe Campaign finns i följande avsnitt:

* [Öka e-postens anseende med IP-uppvärmning](../../help/additional-resources/increase-reputation-with-ip-warming.md)
* [Allt om skräppostsvällning](../../help/additional-resources/all-about-spam-traps.md)

**Adobe Campaign Classic**

* [Optimera leveransen genom karantän](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [Identifiera adresser i karantän för hela plattformen](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#identifying-quarantined-addresses-for-the-entire-platform)
* [Skicka med flera påfyllnader](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves)
* [Leveransövervakning](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/about-delivery-monitoring.html#sending-messages)

**Adobe Campaign Standard**

* [Optimera leveransen genom karantän](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [Identifiera adresser i karantän för hela plattformen](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html)
* [Övervaka en leverans](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/monitoring-a-delivery.html)
