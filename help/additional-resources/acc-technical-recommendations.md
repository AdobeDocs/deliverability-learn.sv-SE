---
title: Campaign Classic – tekniska rekommendationer
description: Upptäck tekniker, konfigurationer och verktyg som du kan använda för att förbättra leveransfrekvensen med Adobe Campaign Classic.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 39ed3773-18bf-4653-93b6-ffc64546406b
source-git-commit: 3ceca47634f946488115ccbef5cb9ffb5aba8b07
workflow-type: tm+mt
source-wordcount: '2086'
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

Lägga till ett SMTP-huvud med namnet **List-Unsubscribe** är obligatoriskt för att säkerställa optimal leveranshantering.

Den här rubriken kan användas som ett alternativ till ikonen&quot;Rapportera som SPAM&quot;. Den visas som en&quot;Unsubscribe&quot;-länk i Internet-leverantörens e-postgränssnitt.

Om du använder den här funktionen minskar du antalet klagomål och hjälper dig att skydda ditt rykte. Feedback kommer att utföras som en avbeställning.

Gmail, Outlook.com, Yahoo! och Microsoft Outlook stöder den här metoden. Länken&quot;Avbeställ&quot; finns direkt i gränssnittet. Exempel:

![bild](../assets/List-Unsubscribe-example-Gmail.png)

>[!NOTE]
>
>Länken &quot;Avbeställ&quot; kanske inte alltid visas. Det kan bero på varje enskild Internet-leverantörs specifika kriterier och policy. Kontrollera därför att dina meddelanden skickas av en avsändare:
>
>* Med gott rykte
>* Under Internet-leverantörernas tröskel för skräppostklagomål
>* Fullt autentiserad

Det finns två versioner av rubrikfunktionen för List-Unsubscribe:

* **mailto List-Unsubscribe** - Klicka på **Avbeställ** som skickar ett förifyllt e-postmeddelande till den adress för att avsluta prenumerationen som anges i e-posthuvudet. [Läs mer](#mailto-list-unsubscribe)

<!--OR: With this method, clicking the **Unsubscribe** link opens the user's default email client with a pre-filled email to the unsubscribe address specified in the email header. This allows the user to unsubscribe simply by sending the email without any further manual steps.-->

* **&quot;One-Click&quot; List-Unsubscribe** - Klicka på **Avbeställ** avbeställer du prenumerationen direkt. [Läs mer](#one-click-list-unsubscribe)

>[!IMPORTANT]
>
>Från och med 1 juni 2024, Yahoo! och Gmail kommer att kräva att avsändarna följer **One-Click List-Unsubscribe**. [Läs mer om den här ändringen](../guidance-around-changes-to-google-and-yahoo.md)
>
>Lär dig hur du konfigurerar One-Click List-Unsubscribe i [det här avsnittet](#one-click-list-unsubscribe).

### mailto List-Unsubscribe {#mailto-list-unsubscribe}

Klicka på **Avbeställ** som skickar ett förifyllt e-postmeddelande till den adress för att avsluta prenumerationen som anges i e-posthuvudet.

Om du vill använda&quot;mailto&quot; List-Unsubscribe måste du ange en kommandorad där du anger en e-postadress, till exempel: `List-Unsubscribe: <mailto:client@newsletter.example.com?subject=unsubscribe?body=unsubscribe>`

>[!CAUTION]
>
>Exemplet ovan baseras på mottagartabellen. Om databasimplementeringen görs från en annan tabell måste du skriva om kommandoraden med rätt information.

Du kan också skapa en dynamisk&quot;mailto&quot; List-Unsubscribe med en kommandorad som: `List-Unsubscribe: <mailto:<%=errorAddress%>?subject=unsubscribe%=message.mimeMessageId%>`

Att implementera **mailto List-Unsubscribe** i Campaign kan du antingen:

* Lägg till kommandoraden direkt i leverans- eller leveransmallen - [Lär dig mer](#adding-a-command-line-in-a-delivery-template)

* Skapa en typologiregel - [Lär dig mer](#creating-a-typology-rule)

#### Lägga till en kommandorad i en leverans eller mall {#adding-a-command-line-in-a-delivery-template}

Kommandoraden måste läggas till i **[!UICONTROL Additional SMTP headers]** i e-postmeddelandets SMTP-huvud.

Detta kan göras i varje e-postmeddelande eller i befintliga leveransmallar. Du kan också skapa en ny leveransmall som innehåller den här funktionen.

Ange till exempel följande skript i **[!UICONTROL Additional SMTP headers]** fält: `List-Unsubscribe: mailto:unsubscribe@domain.com`. Klicka på **avbeställ** skickar ett e-postmeddelande till unsubscribe@domain.com.

Du kan också använda en dynamisk adress. Om du till exempel vill skicka ett e-postmeddelande till den feladress som definierats för plattformen kan du använda följande skript: `List-Unsubscribe: <mailto:<%=errorAddress%>?subject=unsubscribe%=message.mimeMessageId%>`

![bild](../assets/List-Unsubscribe-template-SMTP.png)

<!--
List-Unsubscribe: mailto:unsubscribe@domain.com 
* Clicking the **unsubscribe** link opens the user's default email client. This typology rule must be added in a typology used for creating email.

List-Unsubscribe: https://domain.com/unsubscribe.jsp 

* Clicking the **unsubscribe** link redirects the user to your unsubscribe form.

  ![image](../assets/UTF-8-1.png)
-->

#### Skapa en typologiregel {#creating-a-typology-rule}

Regeln måste innehålla skriptet som genererar kommandoraden och den måste inkluderas i e-postrubriken.

Lär dig hur du skapar typologiregler i Adobe Campaign v7/v8 i [det här avsnittet](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html#typology-rules).

>[!NOTE]
>
>Vi rekommenderar att du skapar en typologiregel: funktionen för att avbryta prenumerationen läggs till automatiskt i varje e-postmeddelande med den här typologiregeln.

### One-Click List-Unsubscribe {#one-click-list-unsubscribe}

Klicka på **Avbeställ** som säger upp prenumerationen direkt, vilket kräver endast en åtgärd.

Från och med 1 juni 2024, Yahoo! och Gmail kommer att kräva att avsändarna följer One-Click List-Unsubscribe. [Läs mer om den här ändringen](../guidance-around-changes-to-google-and-yahoo.md)

För att uppfylla detta krav måste avsändarna

* Lägg till följande kommandorad: `List-Unsubscribe-Post: List-Unsubscribe=One-Click`.
* Inkludera en länk för att avbryta prenumerationen för URI.
* Stöd för mottagning av HTTP-POSTENS svar från mottagaren, som Adobe Campaign stöder. Du kan också använda en extern tjänst.

Om du vill ha stöd för enklickssvaret för POST av en prenumeration direkt i Adobe Campaign v7/v8 måste du lägga till det i webbprogrammet&quot;Avbeställ mottagare utan klick&quot;. För att göra detta:

1. Gå till **[!UICONTROL Resources]** > **[!UICONTROL Online]** > **[!UICONTROL Web applications]**.

1. Ladda upp&quot;Avbeställ mottagare utan att klicka&quot; [XML](/help/assets/WebAppUnsubNoClick.xml.zip) -fil.

Konfigurera **One-Click List-Unsubscribe** i Campaign kan du antingen:

* Lägg till kommandoraden i leverans- eller leveransmallen - [Lär dig mer](#one-click-delivery-template)
* Skapa en typologiregel - [Lär dig mer](#one-click-typology-rule)

#### Configuring One-Click List-Unsubscribe in the delivery or template {#one-click-delivery-template}

Följ stegen nedan för att konfigurera en klickning för att avbryta prenumerationen i leverans- eller leveransmallen.

1. Gå till **[!UICONTROL SMTP]** i leveransegenskaperna.

1. Under **[!UICONTROL Additional SMTP Headers]** anger du kommandoraden som i exemplet nedan. Varje rubrik ska vara på en separat rad.

Exempel:

```
List-Unsubscribe-Post: List-Unsubscribe=One-Click
List-Unsubscribe: <https://domain.com/webApp/unsubNoClick?id=<%= recipient.cryptedId %> >, < mailto:<%@ include option='NmsEmail_DefaultErrorAddr' %>?subject=unsubscribe<%=escape(message.mimeMessageId) %> >
```

![bild](../assets/List-Unsubscribe-1-click-template-SMTP.png)

Exemplet ovan aktiverar One-Click List-Unsubscribe för Internet-leverantörer som stöder One-Click, samtidigt som mottagare som inte stöder mailto fortfarande kan begära att prenumerationen avbryts via e-post.

#### Skapa en typologiregel som stöder One-Click List-Unsubscribe {#one-click-typology-rule}

Följ stegen nedan om du vill konfigurera en enklickslista för att avbryta prenumerationen med en typologiregel.

1. Gå till **[!UICONTROL Typolgy rules]** och klicka **[!UICONTROL New]**.

   ![bild](../assets/CreatingTypologyRules1.png)


1. Konfigurera den nya typologiregeln som:

   * **[!UICONTROL Rule type]**: **[!UICONTROL Control]**
   * **[!UICONTROL Phase]**: **[!UICONTROL At the start of targeting]**
   * **[!UICONTROL Channel]**: **[!UICONTROL Email]**
   * **[!UICONTROL Level]**: ditt val
   * **[!UICONTROL Active]**


   ![bild](../assets/CreatingTypologyRules2.png)

1. Koda javascript-koden för typologiregeln som i exemplet nedan.

   >[!NOTE]
   >
   >Koden som beskrivs nedan ska endast refereras som exempel.

   I det här exemplet beskrivs hur du:
   * Konfigurera en&quot;mailto&quot; List-Unsubscribe. Den lägger till rubrikerna eller lägger till de befintliga&quot;mailto:&quot;-parametrarna och ersätter dem med: &lt;mailto..>>, https://...
   * Lägg till i sidhuvudet En klickning - Avsluta prenumeration. Den använder `var headerUnsubUrl = "https://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=<%= recipient.cryptedId %>"÷`

   >[!NOTE]
   >
   >Du kan lägga till andra parametrar (t.ex. &amp;service =..).

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


   ![bild](../assets/CreatingTypologyRules3.png)

1. Lägg till din nya regel i en typologi som gäller för e-post.

   >[!NOTE]
   >
   >Du kan lägga till den i standardtypologin.

   ![bild](../assets/CreatingTypologyRules4.png)

1. Förbered en ny leverans.

   >[!CAUTION]
   >
   >Verifiera att **[!UICONTROL Additional SMTP headers]** fältet i leveransegenskaperna är tomt.

   ![bild](../assets/CreatingTypologyRules5.png)

1. Kontrollera under leveransförberedelsen att din nya typologiregel används.

   ![bild](../assets/CreatingTypologyRules6.png)

1. Verifiera att länken Avbeställ finns.

   ![bild](../assets/CreatingTypologyRules7.png)

## E-postoptimering {#email-optimization}

### SMTP {#smtp}

SMTP (Simple mail transfer protocol) är en Internetstandard för e-postöverföring.

SMTP-felen som inte kontrolleras av en regel visas i **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]** mapp. Dessa felmeddelanden tolkas som standard som ej nåbara felmeddelanden.

De vanligaste felen måste identifieras och en motsvarande regel läggas till i **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]** om du vill ha rätt feedback från SMTP-servrarna. Utan detta kommer plattformen att göra onödiga återförsök (okända användare) eller felaktigt placera vissa mottagare i karantän efter ett visst antal tester.

### Dedikerade IP-adresser {#dedicated-ips}

Adobe tillhandahåller en dedikerad IP-strategi för varje kund med en IP-förstärkning för att bygga upp ett anseende och optimera leveransresultaten.
