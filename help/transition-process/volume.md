---
title: Volym - Tips för mjuk övergång
description: Mängden post som du skickar är avgörande för att vi ska få ett gott rykte. Se vad du kan göra för en smidig övergång.
topics: Deliverability
jira: KT-7055
thumbnail: kt7055.jpg
doc-type: article
activity: understand
role: Admin,User
level: Beginner
team: ACS
exl-id: 1bc56061-0c64-4033-b49c-66618916bca6
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 1%

---

# Volym

Mängden post som du skickar är avgörande för att vi ska få ett gott rykte. Ställ dig i en Internet-leverantörs skor - om du börjar se en massa trafik från någon du inte vet skulle det vara alarmerande. Det är riskabelt att skicka stora mängder post direkt och det kommer troligen att orsaka problem med anseendet som ofta är svåra att lösa. Det kan vara frustrerande, tidskrävande och kostsamt att gräva sig ur dåligt rykte och bränna och blockera problem som uppstår när man skickar för mycket för tidigt.

Volymtröskeln varierar mellan olika Internet-leverantörer och kan också variera beroende på era genomsnittliga engagemangsmått. Vissa avsändare kräver en mycket låg och långsam volymavgång, medan andra kan tillåta en brantare volymavgång. Vi rekommenderar att du arbetar med en expert, som en Adobe-konsult, för att ta fram en skräddarsydd volymplan.

Här är en lista med tips och tips för hur du smidigt kan gå över:

* **Behörighet** är grunden för alla lyckade e-postprogram.
* **Låg och långsam** - börja med låga utskicksvolymer och öka sedan i takt med att du bekräftar ditt avsändarrykte.
* Med en **tandem-poststrategi** kan du öka volymen på Campaign samtidigt som du sluter din ESP utan att störa e-postkalendern.
* **Engagemang är viktigt** - börja med de prenumeranter som öppnar och klickar på dina e-postmeddelanden regelbundet.
* **Följ planen** - våra rekommendationer har hjälpt hundratals Campaign-kunder att förbättra sina e-postprogram.
* **Övervaka ditt e-postkonto för svar**. Det är en dålig upplevelse för kunden att använda noreply@xyz.com eller inte svara.
* Inaktiva adresser kan ha negativ påverkan på leveransen. **Återaktivera och återge behörighet på din nuvarande plattform**, inte på dina nya IP-adresser.
* **Domäner** - använd en sändande domän som är en underdomän till företagets faktiska domän
   * Om din företagsdomän till exempel är xyz.com, ger email.xyz.com större trovärdighet åt Internet-leverantörerna än xyzemail.com
* **Genomskinlighet** - registreringsinformation för din e-postdomän ska vara tillgänglig offentligt och ska inte vara privat.

I många fall följer inte transaktionsmejl den traditionella uppvärmningsmetoden. Det är uppenbart svårt att styra volymen i transaktionsmejl på grund av dess natur, eftersom det i allmänhet krävs en användarinteraktion för att utlösa e-postberöringen. I vissa fall kan transaktionsmejl enkelt överföras utan en formell plan. I andra fall kan det vara bättre att övergå varje meddelandetyp över tiden för att sakta utöka volymen. Du kan till exempel använda följande övergångar:

1. Inköpsbekräftelser - engagemang i allmänhet
2. Övergivna kundvagnar - medelhög - hög engagemang i allmänhet
3. Välkomstmeddelanden - hög engagemangsnivå men kan innehålla felaktiga adresser beroende på vilka listsamlingsmetoder du använder
4. Återvinning av e-post - generellt lägre engagemang

## Produktspecifika resurser

**Campaign**

* Läs mer om hur du hanterar leveransen när du startar en ny plattform med Adobe Campaign i [det här avsnittet](/help/additional-resources/ac-starting-new-platform.md).
* Lär dig hur du skickar med flera vågor med Adobe Campaign Classic i [det här avsnittet](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html?lang=sv-SE#sending-using-multiple-waves).
* Lär dig hur du delegerar en underdomän till Adobe Campaign Classic eller Standard fullständigt i [det här avsnittet](/help/additional-resources/ac-domain-name-setup.md).
* [Kontrollpanelen: Fullständig delegering av underdomäner (självstudiekurs)](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html?lang=sv-SE) - *Lär dig hur du delegerar en underdomän till Adobe Campaign Classic fullständigt.*
* [Kontrollpanelen: Fullständig delegering av underdomäner (självstudiekurs)](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html?lang=sv-SE) - *Lär dig hur du delegerar en underdomän till Adobe Campaign Standard fullständigt.*

## Ytterligare resurser

* Läs mer om hur du kan förbättra ditt e-postanseende med IP-uppvärmning i [det här avsnittet](/help/additional-resources/increase-reputation-with-ip-warming.md).
