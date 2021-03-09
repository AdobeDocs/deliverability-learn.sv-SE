---
title: Studsar
description: Lär dig mer om olika typer av studsar
feature: Mätvärden
topics: Deliverability
kt: 7047
thumbnail: kt7047.jpg
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: d42a8c3b06308fca0cf3e9db8d634a767fc0cdc6
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 2%

---


# Studsar

Satser är resultatet av ett leveransförsök och fel där Internet-leverantören tillhandahåller meddelanden om misslyckanden. Hantering av studsar är en viktig del av listhygienen. När ett visst e-postmeddelande har studsat flera gånger i rad flaggas det för undertryckning i den här processen. Antalet och typen av studsar som krävs för att utlösa inaktiveringen varierar från system till system. Den här processen förhindrar att system fortsätter att skicka ogiltiga e-postadresser. Satser är en av de viktigaste data som internetleverantörer använder för att fastställa IP-anseendet. Det är mycket viktigt att hålla ett öga på denna mätmetod. &quot;Levererat&quot; jämfört med &quot;studsat&quot; är förmodligen det vanligaste sättet att mäta leveransen av marknadsföringsmeddelanden: Ju högre procenttal som levereras, desto bättre.

Vi ska gräva i två olika typer av studsar.

## Hårda studsar

Hårda studsar är permanenta fel som genereras efter att en Internet-leverantör har fastställt att ett postförsök till en prenumerantadress inte kan levereras. Inom Adobe Campaign läggs hårda studsar som kategoriseras som olevererbara till i karantänen, vilket betyder att de inte kommer att försökas igen. Det finns vissa fall där ett hårt studsande skulle ignoreras om orsaken till felet är okänd.
Här är några vanliga exempel på hårda studsar:

* Adressen finns inte
* Kontot är inaktiverat
* Felaktig syntax
* Felaktig domän

## Mjuka studsar

Mjuka studsar är tillfälliga fel som internetleverantörer genererar när de har svårt att leverera e-post. Mjuka fel kommer att upprepas flera gånger (med olika variationer beroende på hur anpassade leveransinställningar eller leveransinställningar som är klara att användas) för att försöka leverera korrekt. Adresser som kontinuerligt mjuka studsar kommer inte att läggas till i karantän förrän det maximala antalet försök har gjorts (som återigen varierar beroende på inställningarna). Några vanliga orsaker till mjuka studsar är:

* Postlådan är full
* Tar emot e-postserver nedåt
* Problem med avsändarens rykte

![studstyper](../assets/bounce-types.png)

>[!NOTE]
>
>Satser är en viktig indikator på ett renomméproblem eftersom de kan markera en dålig datakälla (hård studs) eller ett anseendeproblem med en Internet-leverantör (mjuk studs).
>
>Mjuka studsar förekommer ofta som en del av e-postutskick och bör kunna lösas under återförsöksbearbetningen innan de karakteriseras som ett problem med äkta leverans. Om din låga avhoppsfrekvens är större än 30 procent för en enskild Internet-leverantör och inte kan lösas inom 24 timmar är det en bra idé att kontakta din Adobe Campaign-konsult.

## Produktspecifika resurser

**Adobe Campaign Standard**

* [Lista med rapporter - studssammanfattning (produktdokumentation)](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/bounce-summary.html?lang=en#reporting)
* [Förstå leveransfel (produktdokumentation)](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-delivery-failures.html?lang=en#about-delivery-failures)

**Adobe Campaign Classic**

* [Förstå leveransfel (produktdokumentation)](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html?lang=en#sending-messages)