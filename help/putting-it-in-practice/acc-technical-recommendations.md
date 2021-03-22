---
title: Campaign Classic - Tekniska rekommendationer
description: Upptäck tekniker, konfigurationer och verktyg som du kan använda för att förbättra leveransfrekvensen med Adobe Campaign Classic.
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
source-wordcount: '1579'
ht-degree: 1%

---


# Campaign Classic - Tekniska rekommendationer {#technical-recommendations}

Flera tekniker, konfigurationer och verktyg som du kan använda för att förbättra leveransfrekvensen när du använder Adobe Campaign Classic listas nedan.

## Konfiguration {#configuration}

### Omvänd DNS {#reverse-dns}

Adobe Campaign kontrollerar om en omvänd DNS anges för en IP-adress och att detta korrekt pekar tillbaka till IP-adressen.

En viktig punkt i nätverkskonfigurationen är att se till att rätt omvänd DNS har definierats för var och en av IP-adresserna för utgående meddelanden. Det innebär att det för en viss IP-adress finns en omvänd DNS-post (PTR-post) med matchande DNS-post (A-post) som repeterar den ursprungliga IP-adressen.

Domänvalet för en omvänd DNS har betydelse när vissa Internet-leverantörer hanteras. I AOL accepteras endast feedbackslingor med en adress i samma domän som den omvända DNS-adressen (se [Feedback-slinga](#feedback-loop)).

>[!NOTE]
>
>Du kan använda [det här externa verktyget](https://mxtoolbox.com/SuperTool.aspx) för att verifiera konfigurationen av en domän.

### MX-regler {#mx-rules}

MX-regler (Mail eXchanger) är de regler som hanterar kommunikation mellan en sändande server och en mottagande server.

Mer exakt används de för att styra hur snabbt Adobe Campaign MTA (Message Transfer Agent) skickar e-postmeddelanden till varje enskild e-postdomän eller ISP (till exempel hotmail.com, comcast.net). Dessa regler baseras vanligtvis på gränser som publiceras av Internet-leverantörer (t.ex. inkluderar inte mer än 20 meddelanden per varje SMTP-anslutning).

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

definierar de två IP-adresserna 12.34.56.78 och 12.34.56.79 som auktoriserade att skicka e-post för domänen. **~** betyder att alla andra adresser ska tolkas som SoftFail.

Recommendations för att definiera en SPF-post:

* Lägg till **~alla** (SoftFail) eller **-all** (Fail) i slutet om du vill avvisa alla servrar utom de som definierats. Utan detta kan servrar förfalska den här domänen (med en neutral utvärdering).
* Lägg inte till **ptr** (openspf.org rekommenderar att detta inte ska vara kostsamt och otillförlitligt).

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
>Om du har uppgraderat till [Förbättrat MTA](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/sending-emails/sending-an-email/sending-with-enhanced-mta.html#sending-messages) för värdbaserade eller hybridbaserade installationer, signeras DKIM-e-postautentisering av Förbättrat MTA för alla meddelanden med alla domäner.

För att använda [DKIM](/help/additional-resources/authentication.md#dkim) med Adobe Campaign Classic krävs följande krav:

**Adobe Campaign-alternativdeklaration**: i Adobe Campaign baseras den privata nyckeln för DKIM på en DKIM-väljare och en domän. Det går för närvarande inte att skapa flera privata nycklar för samma domän/underdomän med olika väljare. Det går inte att definiera vilken väljardomän/underdomän som ska användas för autentisering på varken plattformen eller i e-postmeddelandet. Plattformen kommer att välja en av de privata nycklarna, vilket innebär att autentiseringen har en stor chans att misslyckas.

* Om du har konfigurerat DomainKeys för din Adobe Campaign-instans behöver du bara välja **dkim** i [Domänhanteringsreglerna](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#email-management-rules). Om inte, följer du samma konfigurationssteg (privat/offentlig nyckel) som för DomainKeys (som ersatte DKIM).
* Du behöver inte aktivera både DomainKeys och DKIM för samma domän som DKIM är en förbättrad version av DomainKeys.
* Följande domäner validerar för närvarande DKIM: AOL, Gmail.

## Feedback-slinga {#feedback-loop-acc}

En feedback-slinga fungerar genom att på Internet-nivå deklarera en given e-postadress för ett intervall av IP-adresser som används för att skicka meddelanden. Internet-leverantören skickar till den här postlådan, på ungefär samma sätt som för studsmeddelanden, de meddelanden som rapporteras av mottagarna som skräppost. Plattformen bör konfigureras för att blockera framtida leveranser till användare som har klagat. Det är viktigt att du inte längre kontaktar dem även om de inte använde rätt avanmälningslänk. Det baseras på dessa klagomål på att en Internet-leverantör lägger till en IP-adress till blockeringslista. Beroende på Internet-leverantören kommer en klagofrekvens på ungefär 1 % att leda till att en IP-adress blockeras.

En standard håller på att utarbetas för att definiera formatet för meddelanden med feedback-slingor: [Rapporteringsformat för missbruk av feedback (ARF)](https://tools.ietf.org/html/rfc6650).

Implementering av en feedbackslinga för en instans kräver:

* En postlåda som är dedikerad till instansen, som kan vara studspostlådan
* IP-adresser som är dedikerade till instansen

När du implementerar en enkel feedbackslinga i Adobe Campaign används funktionen för studsmeddelanden. Postlådan för feedbackslingan används som studspostlåda och en regel definieras för att identifiera dessa meddelanden. E-postadresserna till mottagarna som rapporterade meddelandet som skräppost läggs till i karantänlistan.

* Skapa eller ändra en studs-e-postregel, **Feedback_loop**, i **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Mail rule sets]** med orsaken **Refused** och typen **Hård**.
* Om en postlåda har definierats särskilt för feedbackslingan, definierar du parametrarna för att få åtkomst till den genom att skapa ett nytt externt studentkonto i **[!UICONTROL Administration > Platform > External accounts]**.

Mekanismen fungerar omedelbart för att behandla klagomål. Om du vill vara säker på att den här regeln fungerar som den ska kan du tillfälligt inaktivera kontona så att de inte samlar in dessa meddelanden och sedan kontrollera innehållet i feedbackloopens postlåda manuellt. Kör följande kommandon på servern:

```
nlserver stop inMail@instance,
nlserver inMail -instance:instance -verbose.
```

Om du tvingas använda en enda slingadress för feedback för flera instanser måste du:

* Replikera de meddelanden som tas emot på så många postlådor som det finns instanser av,
* få varje postlåda upphämtad i en enda instans,
* Konfigurera instanserna så att de endast bearbetar de meddelanden som berör dem: instansinformationen ingår i Message-ID-huvudet i meddelanden som skickas av Adobe Campaign och finns därför även i svarsslingmeddelandena. Ange bara parametern **checkInstanceName** i instanskonfigurationsfilen (instansen verifieras inte som standard och det kan leda till att en viss adress sätts i karantän på fel sätt):

   ```
   <serverConf>
     <inMail checkInstanceName="true"/>
   </serverConf>
   ```

Adobe Campaign Deliverability-tjänst hanterar din prenumeration på tjänster för feedbackloopar för följande Internet-leverantörer: AOL, BlueTie, Comcast, Cox, EarthLink, FastMail, Gmail, Hotmail, HostedEmail, Libero, Mail.ru, MailTrust, OpenSRS, QQ, RoadRunner, Synacor, Telenor, Terra, UnitedOnline, USA, XS4ALL, Yahoo, Yandex, Zoho.

## List-Unsubscribe {#list-unsubscribe}

### Om List-Unsubscribe {#about-list-unsubscribe}

Det är obligatoriskt att lägga till ett SMTP-huvud med namnet **List-Unsubscribe** för att säkerställa optimal leveranshantering.

Den här rubriken kan användas som ett alternativ till ikonen&quot;Rapportera som SPAM&quot;. Den visas som en länk för avprenumeration i e-postgränssnittet.

Om du använder den här funktionen kan du skydda ditt rykte och din feedback kommer att tas som en oprenumeration.

Om du vill använda List-Unsubscribe måste du ange en kommandorad som ser ut så här:

```
List-Unsubscribe: mailto: client@newsletter.example.com?subject=unsubscribe?body=unsubscribe
```

>[!CAUTION]
>
>Exemplet ovan baseras på mottagartabellen. Om databasimplementeringen görs från en annan tabell måste du skriva om kommandoraden med rätt information.

Följande kommandorad kan användas för att skapa en dynamisk **List-Unsubscribe**:

```
List-Unsubscribe: mailto: %=errorAddress%?subject=unsubscribe%=message.mimeMessageId%
```

Gmail, Outlook.com och Microsoft Outlook har stöd för den här metoden och en avanmälningsknapp är tillgänglig direkt i gränssnittet. Den här tekniken minskar antalet klagomål.

Du kan implementera **List-Unsubscribe** genom att antingen:

* Lägg till kommandoraden direkt [i leveransmallen](#adding-a-command-line-in-a-delivery-template)
* [Skapa en typologiregel](#creating-a-typology-rule)

### Lägga till en kommandorad i en leveransmall {#adding-a-command-line-in-a-delivery-template}

Kommandoraden måste läggas till i det extra avsnittet i e-postmeddelandets SMTP-huvud.

Detta kan göras i varje e-postmeddelande eller i befintliga leveransmallar. Du kan också skapa en ny leveransmall som innehåller den här funktionen.

### Skapa en typologiregel {#creating-a-typology-rule}

Regeln måste innehålla skriptet som genererar kommandoraden och den måste inkluderas i e-postrubriken.

>[!NOTE]
>
>Vi rekommenderar att du skapar en typologiregel: funktionen för att avbryta prenumerationen läggs automatiskt till i varje e-postmeddelande.

1. List-Unsubscribe: &lt;mailto:unsubscribe@domain.com>

   Om du klickar på länken **unsubscribe** öppnas användarens standardklient för e-post. Den här typologiregeln måste läggas till i en typologi som används för att skapa e-post.

1. List-Unsubscribe: `<https://domain.com/unsubscribe.jsp>`

   Om du klickar på länken **unsubscribe** dirigeras användaren om till ditt formulär.

   Exempel:

   ![](../assets/s_tn_del_unsubscribe_param.png)

>[!NOTE]
>
>Lär dig hur du skapar typologiregler i Adobe Campaign Classic i [det här avsnittet](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html#typology-rules).

## E-postoptimering {#email-optimization}

### SMTP {#smtp}

SMTP (Simple mail transfer protocol) är en Internetstandard för e-postöverföring.

SMTP-felen som inte kontrolleras av en regel visas i mappen **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]**. Dessa felmeddelanden tolkas som standard som ej nåbara felmeddelanden.

De vanligaste felen måste identifieras och en motsvarande regel läggas till i **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]** om du vill att feedback från SMTP-servrarna ska vara korrekt. Utan detta kommer plattformen att göra onödiga återförsök (okända användare) eller felaktigt placera vissa mottagare i karantän efter ett visst antal tester.

### Dedikerade IP-adresser {#dedicated-ips}

Adobe tillhandahåller en dedikerad IP-strategi för varje kund med en IP-förstärkning för att bygga upp ett anseende och optimera leveransresultaten.