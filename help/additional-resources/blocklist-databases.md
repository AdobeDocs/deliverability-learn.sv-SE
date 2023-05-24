---
title: Real-time Blackhole Lists
description: Lär dig mer om organisationer som upprätthåller listor över IP-adresser och domäner som kan komma att användas av skräppost.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4155b89f-a636-404c-8951-563c1b4d0289
source-git-commit: e7427d6109f3201affa58decde36294d1631bf5b
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 6%

---

# Real-time Blackhole Lists

Flera organisationer har databaser med IP-adresser och domäner som används av skräppost. Att konsultera dessa webbplatser kan vara användbart för att förstå varför vissa meddelanden avvisades som skräppost. Det är i allmänhet möjligt att begära att en adress som felaktigt lagts till i dessa listor tas bort.

Dessa databaser kallas för RBL (BlackHill Lists i realtid) och söks igenom via en DNS-mekanism. Det finns tre typer av RBL:

* Efter IP-adress: listar IP-adresser som skickar skräppost eller som sannolikt kommer att vidarebefordra skräppost.
* Efter avsändardomän: listar avsändardomäner (den fullständiga domänen för den studsande e-postadressen) som skickar skräppost eller felaktigt konfigurerad.
* Efter webbdomän: listar de domäner (högnivådomäner som är registrerade hos registratorerna) som finns i URL:erna för länkarna och bilderna i skräppostinnehållet. I Adobe är den domän som ska beaktas vanligtvis den adress som används för spårning.

Nedan följer en lista över de mest använda RBL:erna. En mer omfattande lista finns på [https://www.dnsstuff.com/](https://tools.dnsstuff.com/).

* **Spamhaus**

   Se [https://www.spamhaus.org/](https://www.spamhaus.org/)

   Databasen är viktigare. Att klassificeras i denna förteckning är i allmänhet en allvarlig situation. Om detta händer måste ni agera omedelbart och varna de kommersiella tjänsterna, leveransmöjligheterna och [Adobe kundtjänst](https://helpx.adobe.com/se/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html).

* **SpamCop**

   Se [https://www.spamcop.net/](https://www.spamcop.net/)

   Det är en av de mest kända databaserna. Om en av dina IP-adresser placeras i den här listan betyder det vanligtvis att SpamCop-användarna har deklarerat dina meddelanden som skräppost eller att du har skickat meddelanden till en SpamCop-honeypot.

* **URIBL**

   Se [https://www.uribl.com/](https://www.uribl.com/)

   Den här listan identifierar de domäner som regelbundet visas i meddelanden som deklarerats som skräppost. Om din domän visas i den här listan kan det påverka leveransmöjligheterna avsevärt. Du bör informera om leveranstjänster och [Adobe kundtjänst](https://helpx.adobe.com/se/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) omedelbart.

* **SURBL**

   Se [https://surbl.org/](https://surbl.org/)

   SURBL identifierar de webbplatser som regelbundet visas i skräppost. Om din domän visas i den här listan kan det påverka leveransmöjligheterna avsevärt. Du bör informera om leveranstjänster och [Adobe kundtjänst](https://helpx.adobe.com/se/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) omedelbart.

* **iX Manitu**

   Detta är en lista över IP-adresser och används ofta i Tyskland. Se [https://www.heise.de/ix/nixspam/](https://www.heise.de/ix/nixspam/)

<!--* SORBS

  [https://www.nl.sorbs.net](https://www.nl.sorbs.net) compiles a list of IP addresses that are reputed to be dynamic IP address (i.e. attributed temporarily to ISP subscribers) or "open relay" addresses. Certain domains check whether the IP address of a sender is not listed on this site before accepting email. Checking the IP addresses on this site can prove useful.-->
