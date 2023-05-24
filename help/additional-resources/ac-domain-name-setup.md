---
title: Konfiguration av domännamn
description: Lär dig hur du delegerar en underdomän till Adobe Campaign.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4d52d197-d20e-450c-bfcf-e4541c474be4
source-git-commit: 82f7254a9027f79d2af59aece81f032105c192d5
workflow-type: tm+mt
source-wordcount: '2061'
ht-degree: 2%

---

# Konfiguration av domännamn

I det här dokumentet beskrivs affärsmässiga och tekniska krav för konfiguration och delegering av domännamn. Du måste välja en e-postsändande underdomän och, om du vill, en externt riktad underdomän som värd för webbkomponenter (landningssidor, avanmälningssida) för den Adobe-plattform du använder.

>[!NOTE]
>
>Du kan också konfigurera nya underdomäner med hjälp av Kontrollpanelen (tillgänglig som beta). Läs mer i [det här avsnittet](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/setting-up-new-subdomain.html#must-read).

## Underdomäner

Med Adobe kan digital marknadsföring bli den kontextuella motor som driver ert varumärkes marknadsföringsprogram för kundengagemang.  E-post är fortfarande grunden för digitala marknadsföringsprogram. Det har dock blivit svårare än någonsin att nå inkorgen.

Genom att skapa en underdomän för e-postkampanjer kan varumärken isolera olika typer av trafik (till exempel marknadsföring kontra företag) i specifika IP-pooler och med specifika domäner, vilket snabbar upp [IP-uppvärmningsprocess](../../help/additional-resources/increase-reputation-with-ip-warming.md) och förbättra den övergripande leveransen. Om du delar en domän och den blockeras eller läggs till i blockeringslista kan det påverka företagets e-postleverans. Men kända problem eller blockeringar på en domän som är specifik för din e-postmarknadsföring kommer att påverka just det e-postflödet.  Om du använder huvuddomänen som avsändare eller Från-adress för flera e-postströmmar kan det också bryta e-postautentiseringen, vilket gör att dina meddelanden blockeras eller placeras i skräppostmappen.

### Delegering

Delegering av domännamn är en metod som tillåter ägaren av ett domännamn (tekniskt: en DNS-zon) för att delegera en indelning av den (tekniskt: en DNS-zon under den, som kan kallas en underzon) till en annan entitet. Om en kund hanterar zonen&quot;example.com&quot; kan han delegera underzonen&quot;marketing.example.com&quot; till Adobe Campaign.

Detta innebär att Adobe Campaign DNS-servrar endast har fullständig behörighet för den zonen och inte för den översta domänen. Adobe Campaign DNS-servrar kommer att ge auktoritativa svar på frågor om domännamn i den zonen, till exempel&quot;t.marketing.example.com&quot;, men inte&quot;www.example.com&quot;.

Genom att delegera en underdomän för användning med Adobe Campaign kan klienterna förlita sig på att Adobe upprätthåller den DNS-infrastruktur som krävs för att uppfylla branschens krav på levererbarhet för avsändardomäner för e-postmarknadsföring, samtidigt som de behåller och kontrollerar DNS för sina interna e-postdomäner.  Deldomänsdelegering tillåter:

Klienter som vill behålla sin image genom att använda ett DNS-alias med sina domännamn Adobe för att självständigt implementera alla tekniska standarder för att optimera leveransen under e-postutskick

## Alternativ för DNS-konfiguration

För att kunna tillhandahålla en molnbaserad hanterad tjänst rekommenderar Adobe starkt att klienter använder delegering via underdomäner när de distribuerar Adobe Campaign.  Adobe erbjuder emellertid klienterna ett alternativ - CNAME-konfiguration - för att konfigurera DNS.

| Alternativ | Beskrivning | Adobe ansvar | Klientansvar |
|--- |------- |--- |--- |
| Delegering till underdomäner till Adobe Campaign | Klienten delegerar en underdomän (email.example.com) till Adobe. I det här scenariot kan Adobe leverera Campaign som en hanterad tjänst genom att kontrollera och underhålla alla aspekter av DNS som krävs för att leverera, återge och spåra e-postkampanjer. | Fullständig hantering av underdomänen och alla DNS-poster som krävs för Adobe Campaign. | Korrekt delegering av underdomänen till Adobe |
| Använda CNAME:er | Klienten skapar en underdomän och använder CNAME:er för att peka mot Adobe-specifika poster.  Med den här konfigurationen delar både Adobe och kunden ansvaret för att underhålla DNS:er. | Hantering av DNS-poster som krävs för Adobe Campaign. | Skapa och kontrollera underdomänen och skapa/hantera de CNAME-poster som krävs för Adobe Campaign. |

## Nödvändiga DNS-poster

| Posttyp | Syfte | Exempel på post/innehåll |
|--- |--- |--- |
| MX | Ange e-postservrar för inkommande meddelanden | <i>email.example.com</i></br><i>10 inbound.email.example.com</i> |
| SPF (TXT) | Princip för avsändare | <i>email.example.com</i></br>&quot;v=spf1 redirect=__spf.campaign.adobe.com&quot; |
| DKIM (TXT) | Identifierad e-post för DomainKeys | <i>klient._domainkey.email.example.com</i></br>&quot;v=DKIM1; k=rsa;&quot;&quot;DKIMPUBLICKEY HERE&quot; |
| Värdposter (A) | Spegla sidor, bildvärdskap och spårningslänkar, alla sändande domäner | m.email.example.com IN A 123.111.100.99</br>t.email.example.com IN A 123.111.100.98</br>email.example.com IN A 123.111.100.97 |
| Omvänd DNS (PTR) | Mappar klientens IP-adresser till ett klientprofilerat värdnamn | 18.101.100.192.in-addr.arpa domännamnpekare r18.email.example.com |
| CNAME | Anger ett alias för ett annat domännamn | t1.email.example.com är ett alias för t1.email.example.campaign.adobe.com |


Domänbaserad Message Authentication, Reporting och Conformance (DMARC) rekommenderas för att autentisera e-postavsändare och säkerställa att e-postmeddelanden som skickas från din domän är betrodda.

Exempel på DMARC TXT-post:

```
_dmarc.email.example.com

“v=DMARC1; p=none; rua=mailto:mailauth-reports@myemail.com” 
```

Du kan implementera DMARC manuellt eller kontakta Adobe för att få hjälp med att konfigurera DMARC för ditt varumärke.

## Installationskrav

### Delegering till underdomän

Detta kräver att klienten skapar en underdomän i sina DNS-servrar och definierar namnservrarna för den här underdomänen som ska underhållas av Adobe.  En klient vars huvuddomännamn är &quot;example.com&quot; och som vill delegera hanteringen av &quot;marketing.example.com&quot; till Adobe för sina e-postleveranser måste till exempel materialisera delegeringen för att lägga till följande typposter i sin DNS:

```
marketing.example.com. NS a.ns.campaign.adobe.com.
marketing.example.com. NS b.ns.campaign.adobe.com.
marketing.example.com. NS c.ns.campaign.adobe.com.
marketing.example.com. NS d.ns.campaign.adobe.com.
```

Delegering av ett domännamn innebär att den här domänen kommer att dedikeras till att leverera e-post via Adobe Campaign-plattformen och därför inte kan användas på andra sätt (till exempel för att skicka e-post från en annan e-postinfrastruktur).

Under konfigurationsprocessen ser Adobe till att domänen är ansluten till infrastrukturen för inkommande e-post i Adobe för att hantera och bearbeta återbundna e-postmeddelanden som kommer tillbaka till dessa domäner (DNS-postkonfiguration av MX-typ).

### Använda CNAME:er

Om klienten väljer att använda CNAME i stället för att delegera en underdomän till Adobe, kommer Adobe under konfigurationsfasen att tillhandahålla de poster som ska placeras i klientens DNS-servrar och konfigurera motsvarande värden i Adobe Campaign DNS-servrar.

## Allmänna krav för distribution

När ni implementerar en ny marknadsföringslösning för företag finns det krav på externt inriktade komponenter.  Dessa kan vara värdtjänster för landningssidor och webbformulär, konfigurera länkar och webbsidor som ska spåras, visa spegelsidor och konfigurera en avanmälningssida.

Dessa krav hanteras via komponenter som finns både på Adobe och hos kunden, men de innehåller URL:er som kan ses av mottagarna av e-postmeddelandena.  För att undvika att ha URL:er som anger den underliggande tekniska lösningen eller värdtjänstleverantören, kan underdomäner konfigureras så att de blir genomskinliga för e-postmeddelandets mottagare.  Om du till exempel tittar på en URL som http://www.customer.com/ blir domänen&quot;www.customer.com&quot;.  Underdomänen för detta skulle vara &quot;www&quot;.

### Krav för deldomän

Bestäm vilka underdomäner som ska användas för profilerade URL:er (spegelsidor och URL:er för spårning) från Adobe Campaign-programmet.  Bestäm också vad&quot;Från adress&quot;,&quot;Från namn&quot; och&quot;Till adress&quot; ska vara för varje underdomän för e-postleveranser.

Fyll i tabellen nedan. Första raden är bara ett exempel.

| Underdomän | Från adress | Från namn | Svarsadress |
|--- |--- |--- |--- |
| emails.customer.com | news@emails.customer.com | Kund | customercare@customer.com |
| </br> | </br> | </br> | </br> |

>[!NOTE]
>
>* Syftet med fältet Svara till adress är när du vill att mottagaren ska svara på en annan adress än Från adress.  Adobe rekommenderar att svarsadressen är giltig och länkad till en övervakad postlåda, men inget obligatoriskt fält krävs.  Kunden måste vara värd för den här postlådan.  Det kan vara en supportpostlåda, till exempel customercare@customer.com, där e-postmeddelanden läses och besvaras.
>* Om kunden inte väljer&quot;Svara till adress&quot; är standardadressen alltid `<tenant>-<type>-<env>@<subdomain>`.
>* När Svara-till-adressen har konfigurerats på det här sättet skickas svar till en oövervakad postlåda.
>* När du skickar e-postmeddelanden från Adobe Campaign övervakas inte postlådan&quot;Från adress&quot; och marknadsföringsanvändare kan inte komma åt den här postlådan. Adobe Campaign erbjuder inte heller möjlighet att svara automatiskt eller vidarebefordra e-postmeddelanden som tas emot i den här postlådan.
>* Adressen för Campaign From/Sender och Feladressen får inte vara &quot;missbruk&quot; eller &quot;postmaster&quot;.


## Delegera underdomäner

De underdomäner som valts för Adobe Campaign-plattformen måste delegeras genom att fyra NS-poster (Name Server) skapas.  Detta gör att underdomänen kan delegeras korrekt till Adobe.  Nedan visas ett exempel på en underdomänsdelegering och respektive DNS-instruktioner.  Ersätt&quot;emails.customer.com&quot; med den underdomän som du vill delegera.  Observera att underdomänen måste vara unik och inte redan kan användas av en annan part (till exempel en befintlig ESP eller MSP).

| Delegerad underdomän | DNS-instruktioner |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com. </br> `<subdomain>` NS b.ns.campaign.adobe.com. </br> `<subdomain>` NS c.ns.campaign.adobe.com. </br> `<subdomain>` NS d.ns.campaign.adobe.com. |

## Spårning, spegelsidor, resurser

När de e-postsändande underdomänerna har delegerats till Adobe Campaign skapar Adobe TechOps-teamet två eller flera lägre domäner för att hantera spårning och spegla sidor oberoende av varandra.

| Typ | Domän |
|--- |--- |
| Spegla sidor | m.`<subdomain>` |
| Spåra | t.`<subdomain>` |
| Resurser | res.`<subdomain>` |

## Driftsättning i molnet (valfritt)

Detta gäller endast om Adobe Campaign Classic ligger helt och hållet i molnet av Adobe.  Detta är en valfri konfiguration.

Alla enkäter, webbformulär och landningssidor som ska utvecklas hanteras via Adobe Campaign som är fullt värd i molnet.  Om det behövs kan ytterligare en underdomän delegeras till Adobe (till exempel web.customer.com) som ska användas för alla webbkomponenter i verktyget.  Observera att underdomänen måste vara unik och inte kan användas av en annan part (till exempel en befintlig ESP eller MSP).

| Delegerad underdomän | DNS-instruktioner |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com.</br>`<subdomain>` NS b.ns.campaign.adobe.com.</br>`<subdomain>` NS c.ns.campaign.adobe.com.</br>`<subdomain>` NS d.ns.campaign.adobe.com. |

>[!NOTE]
>
>Som standard kommer alla webbkomponenter i verktyget att använda den ursprungliga underdomänen som har delegerats för e-post.

## Distribution av molnmeddelanden (valfritt)

Om Adobe Campaign Classic Marketing Instance ligger hos kunden måste kunden göra ytterligare tekniska konfigurationer.

Alla enkäter, webbformulär och landningssidor som ska utvecklas hanteras via Adobe Campaign marknadsföringsinstans, där mottagaruppgifterna finns.

Ytterligare DNS-konfiguration för CNAME krävs för att distribuera externt tillvända webbkomponenter som finns hos Adobe Campaign marknadsinstans.  På så sätt blir webbkomponenter (till exempel web.customer.com) tillgängliga för allmänheten på Internet och märkta med kundens domän.

Brandväggen måste också konfigureras så att den ger åtkomst till den Adobe Campaign-marknadsinstans som är värd för dessa webbkomponenter (på port 80 eller 443).

**Best Practice Recommendations:**

Den underdomän som ska vara värd för webbkomponenter är synlig för kunderna, så se till att göra den korrekt varumärkesprofilerad och enkel att komma ihåg eftersom den kan behöva skrivas in manuellt, till exempel: https://web.customer.com.
Om några formulär behöver lagras på säkra sidor (HTTPS) krävs ytterligare teknisk konfiguration, vilket beskrivs nedan.

| Delegerad underdomän | DNS-instruktioner |
|--- |--- |
| `<subdomain>` | `<subdomain>` CNAME `<internal customer server>` |

## Återgivna tjänster

Efter dessa delegeringar säkerställer den infrastruktur som Adobe har infört att följande tjänster utförs för varje delegerad eller CNAME-utjämnad sändande domän:

* Skapa inkorg för postmaster@ och missbruk@
* Konfigurera feedbackloopar för den delegerade domänen
* På begäran kommer Adobe också att konfigurera en DMARC-post enligt specifikationen. Din Deliverability Consultant kan hjälpa er att utforma en långsiktig DMARC-policy och plan för era avsändande domäner.
Parametrar som har upprättats av Adobe är endast giltiga från den tidpunkt då delegeringen slutfördes och sedan verifierades av Adobe, och fungerar fortfarande.  Alla Adobe Campaign Cloud-erbjudanden omfattar domännamnsdelegering som standard.

## Fakturerings- och implementeringsvillkor

* I enlighet med det ursprungliga kontraktet och den typ av paket som valts kan även andra delegationer utöver de som ingår som standard utöver denna ursprungliga delegering ingå.
* Utöver dessa inkluderade delegationer kommer ytterligare delegationer att faktureras.
* Faktureringsmetoden för dessa ytterligare delegeringar är en extra månadskostnad, som anges i det ursprungliga kontraktet.

Dessa delegeringar accepteras förutsatt att KLIENTEN väljer de associerade domännamnen som är dedikerade till leveranser via Adobe Campaign-verktyget och att delegeringskraven som anges i det relevanta dokumentet är korrekt implementerade.

## Avbrutna tjänster

KLIENTEN kommer när som helst att kunna göra en skriftlig begäran om att inte längre dra nytta av delegeringstjänsterna och själva ta på sig de nödvändiga DNS-konfigurationerna.

Om detta skulle inträffa kommer Adobe att förse KLIENTEN med en uppskattning av antalet tjänstdagar som krävs för att återgå till icke-domändelegeringsläge.

Adobe kommer att befrias från allt ansvar för att uppnå den ovannämnda leveransräntan om kunden inte uppfyller de åtaganden som anges ovan.

Om du säger upp tjänsten Marketing Cloud upphör automatiskt domändelegeringarna och DNS-underhållet för dessa domäner av Adobe.

## Övervaka underdomäner med Kontrollpanelen

När underdomäner har konfigurerats för din instans kan du övervaka dem med Kontrollpanelen.

Detta gör att du kan visa alla underdomäner som du har delegerat till Adobe Campaign samt begära förnyelse av deras SSL-certifikat.

Mer information om detta hittar du i den [dedikerade dokumentationen](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-subdomains.html#subdomains-and-certificates).

>[!NOTE]
>
>[Kontrollpanelen](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=sv) är endast tillgängligt för kunder som använder Adobes hanterade tjänster.
