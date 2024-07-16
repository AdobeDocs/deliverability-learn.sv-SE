---
title: Studsar
description: Lär dig mer om olika typer av studsar.
topics: Deliverability
jira: KT-7047
thumbnail: kt7047.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 6338eb67-3efd-476e-8b26-97bbb6a1d35f
source-git-commit: 9444f8601f2f349398ee5deb9d5f4d4f7abb44f5
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 100%

---

# Studsar

Studsar är resultatet av att en leverans har misslyckats, då internetleverantören meddelar misslyckanden. Studshantering är en viktig del av listhygienen. När ett visst e-postmeddelande har studsat flera gånger i rad flaggas det för undantag i den här processen. Antalet och typen av studsar som krävs för att utlösa undantaget varierar från system till system. Den här processen förhindrar att system fortsätter att skicka till ogiltiga e-postadresser. Studsar är en av de viktigaste uppgifterna som internetleverantörer använder för att fastställa IP-anseendet. Det är mycket viktigt att hålla ett öga på denna mätmetod. &quot;Levererat&quot; jämfört med &quot;studsat&quot; är förmodligen det vanligaste sättet att mäta leveransen av marknadsföringsmeddelanden: ju högre procenttal som levereras, desto bättre.

Vi ska titta närmare på två olika typer av studsar.

## Hårda studsar

Hårda studsar är permanenta fel som genereras när en internetleverantör fastställer att e-post inte kan levereras till en prenumerantadress. Inom Adobe Campaign läggs hårda studsar som kategoriseras som olevererbara i karantänen, vilket innebär att inga fler försök att skicka dem görs. I vissa fall ignoreras en hård studs om orsaken till felet är okänd.
Här är några vanliga exempel på hårda studsar:

* Adressen finns inte
* Kontot är inaktiverat
* Felaktig syntax
* Felaktig domän

## Mjuka studsar

Mjuka studsar är tillfälliga fel som internetleverantörer genererar när de har svårt att leverera e-post. Vid mjuka fel upprepas försöket flera gånger (beroende på om leveransinställningarna är anpassade eller klara att användas) för att leveransen ska lyckas. Adresser som kontinuerligt genererar mjuka studsar kommer inte att läggas i karantän förrän det maximala antalet försök har gjorts (beroende på inställningarna). Några vanliga orsaker till mjuka studsar är:

* Postlådan är full
* Mottagande e-postserver är nere
* Problem med avsändarens anseende

![studstyper](../assets/bounce-types.png)

>[!NOTE]
>
>Studsar är en viktig indikator på ett anseendeproblem eftersom de kan markera en dålig datakälla (hård studs) eller ett anseendeproblem med en internetleverantör (mjuk studs).
>
>Mjuka studsar förekommer ofta vid e-postutskick och bör försöka lösas genom återförsöksprocessen innan de betecknas som äkta levererbarhetsproblem. Om frekvensen för mjuka studsar är större än 30 % för en enskild internetleverantör och inte kan lösas inom 24 timmar är det en bra idé att kontakta din Adobe Campaign-konsult för levererbarhet.

## Produktspecifika resurser

**Adobe Campaign Classic**

* [Typ av leveransfel och orsaker](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html?lang=sv#delivery-failure-types-and-reasons)
* [Hantering av e-post som studsar](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html?lang=sv#bounce-mail-management)
* [Rapport om ej levererbar e-post eller e-post som studsar](https://experienceleague.adobe.com/docs/campaign-classic/using/reporting/reports-on-deliveries/global-reports.html?lang=sv#non-deliverables-and-bounces)

**Adobe Campaign Standard**

* [Typ av leveransfel och orsaker](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-delivery-failures.html?lang=sv#delivery-failure-types-and-reasons)
* [Kvalifikation av studsmeddelanden](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-delivery-failures.html?lang=sv#bounce-mail-qualification)
* [Studssammanfattningsrapport](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/bounce-summary.html?lang=sv#reporting)
