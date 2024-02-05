---
title: Campaign Classic – tekniska rekommendationer
description: Upptäck tekniker, konfigurationer och verktyg som du kan använda för att förbättra leveransfrekvensen med Adobe Campaign Classic.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 39ed3773-18bf-4653-93b6-ffc64546406b
source-git-commit: 2e3cebdad1613c852e950c379ddba689a3d8110e
workflow-type: tm+mt
source-wordcount: '1867'
ht-degree: 1%

---

# Campaign Classic – tekniska rekommendationer {#technical-recommendations}

Flera tekniker, konfigurationer och verktyg som du kan använda för att förbättra leveransfrekvensen när du använder Adobe Campaign Classic listas nedan.

## Konfiguration {#configuration}

### Omvänd DNS {#reverse-dns}

Adobe Campaign kontrollerar om en omvänd DNS anges för en IP-adress och att detta korrekt pekar tillbaka till IP-adressen.

En viktig punkt i nätverkskonfigurationen är att se till att rätt omvänd DNS har definierats för var och en av IP-adresserna för utgående meddelanden. Det innebär att det för en viss IP-adress finns en omvänd DNS-post (PTR-post) med matchande DNS-post (A-post) som repeterar den ursprungliga IP-adressen.

Domänvalet för en omvänd DNS har betydelse när vissa Internet-leverantörer hanteras. AOL godkänner i synnerhet endast feedbackslingor med en adress i samma domän som den omvända DNS-adressen (se [Feedback-slinga](#feedback-loop)).

>[!NOTE]
>
>Du kan använda [det här externa verktyget](https://mxtoolbox.com/SuperTool.aspx) för att verifiera konfigurationen av en domän.

### MX-regler {#mx-rules}

MX-regler (Mail eXchanger) är de regler som hanterar kommunikation mellan en sändande server och en mottagande server.

Mer exakt används de för att styra hur snabbt Adobe Campaign MTA (Message Transfer Agent) skickar e-post till varje enskild e-postdomän eller Internet-leverantör (till exempel hotmail.com, comcast.net). Dessa regler baseras vanligtvis på gränser som publiceras av Internet-leverantörer (t.ex. inkluderar inte mer än 20 meddelanden per varje SMTP-anslutning).

>[!NOTE]
>
>Mer information om MX-hantering i Adobe Campaign Classic finns i [det här avsnittet](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/email-deliverability.html#mx-configuration).

### TLS {#tls}

TLS (Transport Layer Security) är ett krypteringsprotokoll som kan användas för att säkra anslutningen mellan två e-postservrar och skydda innehållet i ett e-postmeddelande från att läsas av andra än de avsedda mottagarna.

### Avsändarens domän {#sender-domain}

Om du vill definiera domänen som används för HELO-kommandot redigerar du instansens konfigurationsfil (conf/config-instance.xml) och definierar ett localDomain-attribut enligt följande:

```
<serverConf>
  <shared>
    <dnsConfig localDomain="mydomain.net"/>
  </shared>
</serverConf>
```

Domänen MAIL FROM är den domän som används i tekniska studsmeddelanden. Den här adressen definieras i distributionsguiden eller via alternativet NmsEmail_DefaultErrorAddr.

### SPF-post {#dns-configuration}

En SPF-post kan för närvarande definieras på en DNS-server som en TXT-typpost (kod 16) eller en SPF-typpost (kod 99). En SPF-post har formen av en teckensträng. Exempel:

```
v=spf1 ip4:12.34.56.78/32 ip4:12.34.56.79/32 ~all
```

definierar de två IP-adresserna 12.34.56.78 och 12.34.56.79 som auktoriserade att skicka e-post för domänen. **~alla** betyder att alla andra adresser ska tolkas som SoftFail.

Recommendations för att definiera en SPF-post:

* Lägg till **~alla** (SoftFail) eller **-all** (Misslyckades) till slutet för att avvisa alla servrar utom de som definierats. Utan detta kan servrar förfalska den här domänen (med en neutral utvärdering).
* Lägg inte till **ptr** (openspf.org rekommenderar att detta inte blir kostsamt och otillförlitligt).

>[!NOTE]
>
>Läs mer om SPF i [det här avsnittet](/help/additional-resources/authentication.md#spf).

## Autentisering

>[!NOTE]
>
>Läs mer om de olika formerna av e-postautentisering i [det här avsnittet](/help/additional-resources/authentication.md).

### DKIM {#dkim-acc}

>[!NOTE]
>
>För värdbaserade eller hybridinstallationer, om du har uppgraderat till [Förbättrad MTA](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/sending-emails/sending-an-email/sending-with-enhanced-mta.html#sending-messages)signerar DKIM-autentisering via e-post av Förbättrat MTA för alla meddelanden med alla domäner.

Använda [DKIM](/help/additional-resources/authentication.md#dkim) med Adobe Campaign Classic kräver följande krav:

**Adobe Campaign-alternativdeklaration**: i Adobe Campaign baseras den privata nyckeln för DKIM på en DKIM-väljare och en domän. Det går för närvarande inte att skapa flera privata nycklar för samma domän/underdomän med olika väljare. Det går inte att definiera vilken väljardomän/underdomän som ska användas för autentisering på varken plattformen eller i e-postmeddelandet. Plattformen kommer att välja en av de privata nycklarna, vilket innebär att autentiseringen har en stor chans att misslyckas.

* Om du har konfigurerat DomainKeys för din Adobe Campaign-instans behöver du bara välja **dkim** i [Regler för domänhantering](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#email-management-rules). Om inte, följer du samma konfigurationssteg (privat/offentlig nyckel) som för DomainKeys (som ersatte DKIM).
* Du behöver inte aktivera både DomainKeys och DKIM för samma domän som DKIM är en förbättrad version av DomainKeys.
* Följande domäner validerar för närvarande DKIM: AOL, Gmail.

## Feedback-slinga {#feedback-loop-acc}

En feedback-slinga fungerar genom att på Internet-nivå deklarera en given e-postadress för ett intervall av IP-adresser som används för att skicka meddelanden. Internet-leverantören skickar till den här postlådan, på ungefär samma sätt som för studsmeddelanden, de meddelanden som rapporteras av mottagarna som skräppost. Plattformen bör konfigureras för att blockera framtida leveranser till användare som har klagat. Det är viktigt att du inte längre kontaktar dem även om de inte använde rätt avanmälningslänk. Det baseras på dessa klagomål på att en Internet-leverantör lägger till en IP-adress till blockeringslista. Beroende på Internet-leverantören kommer en klagofrekvens på ungefär 1 % att leda till att en IP-adress blockeras.

En standard håller på att utarbetas för att definiera formatet för meddelanden om feedbackslingor: [Format för rapportering av missbruk av feedback (ARF)](https://tools.ietf.org/html/rfc6650).

Implementering av en feedbackslinga för en instans kräver:

* En postlåda som är dedikerad till instansen, som kan vara studspostlådan
* IP-adresser som är dedikerade till instansen

När du implementerar en enkel feedbackslinga i Adobe Campaign används funktionen för studsmeddelanden. Postlådan för feedbackslingan används som studspostlåda och en regel definieras för att identifiera dessa meddelanden. E-postadresserna till mottagarna som rapporterade meddelandet som skräppost läggs till i karantänlistan.

* Skapa eller ändra en studsregel, **Feedback_loop**, in **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Mail rule sets]** med orsaken **Avvisad** och typen **Hård**.
* Om en postlåda har definierats särskilt för feedbackslingan, definierar du parametrarna för att få åtkomst till den genom att skapa ett nytt externt studentkonto i **[!UICONTROL Administration > Platform > External accounts]**.

Mekanismen fungerar omedelbart för att behandla klagomål. Om du vill vara säker på att den här regeln fungerar som den ska kan du tillfälligt inaktivera kontona så att de inte samlar in dessa meddelanden och sedan kontrollera innehållet i feedbackloopens postlåda manuellt. Kör följande kommandon på servern:

```
nlserver stop inMail@instance,
nlserver inMail -instance:instance -verbose.
```

Om du tvingas använda en enda slingadress för feedback för flera instanser måste du:

* Replikera de meddelanden som tas emot på så många postlådor som det finns instanser av,
* få varje postlåda upphämtad i en enda instans,
* Konfigurera instanserna så att de endast bearbetar meddelanden som berör dem: instansinformationen ingår i Message-ID-huvudet för meddelanden som skickas av Adobe Campaign och finns därför även i svarsslingmeddelandena. Ange bara **checkInstanceName** parameter i instanskonfigurationsfilen (instansen kontrolleras inte som standard och detta kan leda till att en viss adress sätts i karantän på ett felaktigt sätt):

  ```
  <serverConf>
    <inMail checkInstanceName="true"/>
  </serverConf>
  ```

Tjänsten Adobe Campaign Deliverability hanterar din prenumeration på tjänster för feedback-slingor för följande Internetleverantörer: AOL, BlueTie, Comcast, Cox, EarthLink, FastMail, Gmail, Hotmail, HostedEmail, Libero, Mail.ru, MailTrust, OpenSRS, QQ, RoadRunner, Synacor, Terra, UnitedOnline, USA, XS4ALL, Yahoo, Yandex, Zoho.

## List-Unsubscribe {#list-unsubscribe}

### Om List-Unsubscribe {#about-list-unsubscribe}

Lägga till ett SMTP-huvud med namnet **List-Unsubscribe** är obligatoriskt för att säkerställa optimal leveranshantering.Från och med 1 juni 2024 kommer Yahoo och Gmail att kräva att avsändarna följer reglerna för One-Click List-Unsubscribe. Mer information om hur du konfigurerar One-Click List-Unsubscribe finns nedan.


Den här rubriken kan användas som ett alternativ till ikonen&quot;Rapportera som SPAM&quot;. Den visas som en länk för att avbryta prenumerationen i e-postgränssnittet.

Om du använder den här funktionen kan du skydda ditt rykte och få feedback som du kan avbeställa.

Om du vill använda List-Unsubscribe måste du ange en kommandorad som liknar:

```
List-Unsubscribe: <mailto:client@newsletter.example.com?subject=unsubscribe?body=unsubscribe>
```

>[!CAUTION]
>
>Exemplet ovan baseras på mottagartabellen. Om databasimplementeringen görs från en annan tabell måste du skriva om kommandoraden med rätt information.

Följande kommandorad kan användas för att skapa en dynamisk **List-Unsubscribe**:

```
List-Unsubscribe: <mailto:<%=errorAddress%>?subject=unsubscribe%=message.mimeMessageId%>
```

Gmail, Outlook.com och Microsoft Outlook har stöd för den här metoden och en avbeställningsknapp är tillgänglig direkt i gränssnittet. Den här tekniken minskar antalet klagomål.

Du kan implementera **List-Unsubscribe** antingen

* Direkt [lägga till kommandoraden i leveransmallen](#adding-a-command-line-in-a-delivery-template)
* [Skapa en typologiregel](#creating-a-typology-rule)

### Lägga till en kommandorad i en leveransmall {#adding-a-command-line-in-a-delivery-template}

Kommandoraden måste läggas till i det extra avsnittet i e-postmeddelandets SMTP-huvud.

Detta kan göras i varje e-postmeddelande eller i befintliga leveransmallar. Du kan också skapa en ny leveransmall som innehåller den här funktionen.

Lista-Avbeställ: mailto:unsubscribe@domain.com
* Klicka på **avbeställ** öppnar användarens standardklient för e-post. Den här typologiregeln måste läggas till i en typologi som används för att skapa e-post.

Lista-Avbeställ: https://domain.com/unsubscribe.jsp
* Klicka på **avbeställ** link dirigerar om användaren till ditt formulär för att avbryta prenumerationen.

![bild](/help/assets/UTF-8-1.png)


### Skapa en typologiregel {#creating-a-typology-rule}

Regeln måste innehålla skriptet som genererar kommandoraden och den måste inkluderas i e-postrubriken.

>[!NOTE]
>
>Vi rekommenderar att du skapar en typologiregel: funktionen för att avbryta prenumerationen läggs till automatiskt i varje e-postmeddelande.

>[!NOTE]
>
>Lär dig hur du skapar typologiregler i Adobe Campaign Classic i [det här avsnittet](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html#typology-rules).

### Avbeställ prenumeration med ett klick

Från och med den 1 juni 2024 kommer Yahoo och Gmail att kräva att avsändarna följer reglerna för One-Click List-Unsubscribe. För att uppfylla kravet på ett klick för att avbryta prenumerationen måste avsändaren:

1. Lägg till i en&quot;List-Unsubscribe-Post: List-Unsubscribe=One-Click&quot;
2. Inkludera en länk för att avbeställa en URI
3. Stöd för mottagning av HTTP-POSTENS svar från mottagaren, som Adobe Campaign stöder.

Så här konfigurerar du ett klick för att avbryta prenumeration direkt:

* Lägg till följande webbprogram&quot;Avbeställ mottagare utan att klicka&quot; 
   1. Gå till Resurser -> Online -> Webbprogram
   2. Ladda upp&quot;Avbeställ mottagare utan att klicka&quot; [XML](/help/assets/WebAppUnsubNoClick.xml.zip)
* Konfigurera List-Unsubscribe och List-Unsubscribe-Post
   1. Gå till avsnittet SMTP i Leveransegenskaper.
   2. Under Ytterligare SMTP-rubriker anger du följande på kommandoraden (varje rubrik ska vara på en separat rad):

```
List-Unsubscribe-Post: List-Unsubscribe=One-Click
List-Unsubscribe: <https://domain.com/webApp/unsubNoClick?id=<%= recipient.cryptidcamp %>>, <mailto: %=errorAddress%?
subject=unsubscribe%=message.mimeMessageId%>
```

Exemplet ovan aktiverar One-Click List-Unsubscribe för Internet-leverantörer som stöder One-Click samtidigt som mottagare som inte stöder URL list-unsubscribe fortfarande kan begära att prenumerationen avbryts via e-post.


### Skapar en typologiregel som stöder ett klick för att avbryta prenumeration:

**1. Skapa den nya typologiregeln:**

* Klicka på &quot;ny&quot; i navigeringsträdet för att skapa en ny typ


![bild](/help/assets/CreatingTypologyRules1.png)



**2. Fortsätt med att konfigurera typologiregeln:**

* Regeltyp: Kontroll
* Fas: Vid början av målinriktningen
* Kanal: E-post
* Nivå: Ditt val
* Aktiv


![bild](/help/assets/CreatingTypologyRules2.png)


**Koda javascript-koden för typologiregeln:**


>[!NOTE]
>
>Koden som beskrivs nedan ska endast refereras som exempel.
>I det här exemplet beskrivs hur du:
>* Konfigurera en URL List-Unsubscribe och lägger till rubrikerna eller lägger till den befintliga mailto-parametern och ersätter den med: &lt;mailto..>>, https://..
>* Lägg till i sidhuvudet List-Unsubscribe-Post
>I exemplet med post-URL används var headerUnsubUrl = &quot;https://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=&lt;%= mottagare.cryptedId %>&quot; max
>* Du kan lägga till andra parametrar (som &amp;service = ...)
>


```
// Function to add or replace a header in the provided headers 
function addHeader(headers, header, value)  { 
    
  // Create the new header line 
  var headerLine = header + ": " + value; 
    
  // Create a regular expression to find the specified header 
  var regExp = new RegExp(header + ":(.*)$", "i") 
    
  // Split the headers into individual lines 
  var headerLines = headers.split("\n"); 
    
  // Loop through each line 
  for (var i=0; i < headerLines.length; i++) { 
      
    // Check if the specified header exists 
    var match = headerLines[i].match(regExp) 
      
    // If it exists 
    if ( match != null ) { 
        
      // Replace the existing header line 
      headerLines[i] = headerLine; 
        
      // Return the modified headers 
      return headerLines.join("\n"); 
    } 
  } 
    
  // If the header does not exist, add the new header line 
  headerLines.push(headerLine); 
    
  // Return the modified headers 
  return headerLines.join("\n"); 
} 
  
// Function to get the value of a specified header from the provided headers 
function getHeader(headers, header) { 
    
  // Create a regular expression to find the specified header 
  var regExp = new RegExp(header + ":(.*)$", "i") 
    
  // Split the headers into individual lines 
  var headerLines = headers.split("\n"); 
    
  // Loop each line 
  for each (line in headerLines) { 
      
    // Check if the specified header exists 
    var match = line.match(regExp); 
      
    // If it exists 
    if ( match != null ) { 
        
      // Return the header value, removing leading whitespace 
      return match[1].replace(/^\s*/, ""); 
    } 
  } 
    
  // If the header does not exist, return an empty string 
  return ""; 
} 
  
  
// Define the unsubscribe URL 
var headerUnsubUrl = "https://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=<%= recipient.cryptedId %>"; 
  
// Get the value of the List-Unsubscribe header 
var headerUnsub = getHeader(delivery.mailParameters.headers, "List-Unsubscribe"); 
  
// If the List-Unsubscribe header does not exist 
if ( headerUnsub === "" ) { 
  // Add the List-Unsubscribe header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe", "<"+headerUnsubUrl+">"); 
} 
// If the List-Unsubscribe header exists and contains 'mailto' 
else if(headerUnsub.search('mailto')){ 
  // Replace the existing List-Unsubscribe header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe", "<"+headerUnsubUrl+">"); 
} 
  
// Get the value of the List-Unsubscribe-Post header 
var headerUnsubPost = getHeader(delivery.mailParameters.headers, "List-Unsubscribe-Post"); 
  
// If the List-Unsubscribe-Post header does not exist 
if ( headerUnsubPost === "" ) { 
  // Add the List-Unsubscribe-Post header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe-Post", "List-Unsubscribe=One-Click"); 
} 
  
// Return true to indicate success 
return true; 
```


![bild](/help/assets/CreatingTypologyRules3.png)



**3. Lägg till din nya regel i en typologi i ett e-postmeddelande (standardtypologin är OK):**

![bild](/help/assets/CreatingTypologyRules4.png)



**4. Förbered en ny leverans (kontrollera att ytterligare SMTP-huvuden i leveransegenskapen är tomma)**

![bild](/help/assets/CreatingTypologyRules5.png)



**5. Kontrollera under leveransförberedelsen att din nya typologiregel används.**

![bild](/help/assets/CreatingTypologyRules6.png)



**6. Verifiera att List-Unsubscribe finns.**

![bild](/help/assets/CreatingTypologyRules7.png)


## E-postoptimering {#email-optimization}

### SMTP {#smtp}

SMTP (Simple mail transfer protocol) är en Internetstandard för e-postöverföring.

SMTP-felen som inte kontrolleras av en regel visas i **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]** mapp. Dessa felmeddelanden tolkas som standard som ej nåbara felmeddelanden.

De vanligaste felen måste identifieras och en motsvarande regel läggas till i **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]** om du vill ha rätt feedback från SMTP-servrarna. Utan detta kommer plattformen att göra onödiga återförsök (okända användare) eller felaktigt placera vissa mottagare i karantän efter ett visst antal tester.

### Dedikerade IP-adresser {#dedicated-ips}

Adobe tillhandahåller en dedikerad IP-strategi för varje kund med en IP-förstärkning för att bygga upp ett anseende och optimera leveransresultaten.
