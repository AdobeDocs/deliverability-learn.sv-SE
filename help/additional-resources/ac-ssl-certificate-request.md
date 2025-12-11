---
title: Begäran om SSL-certifikat
description: Lär dig hur du installerar SSL-certifikat på de underdomäner du har delegerat till Adobe.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 8a78abd3-afba-49a7-a2ae-8b2c75326749
source-git-commit: 0be68f5674904aa105985a6e5fc4771c41f7fe48
workflow-type: tm+mt
source-wordcount: '2124'
ht-degree: 1%

---

# Process för begäran av SSL-certifikat

När du har delegerat en domän till Adobe för att skicka e-post (se [Domännamnskonfiguration](/help/additional-resources/ac-domain-name-setup.md)) skapar och använder Adobe vissa underdomäner för specifika funktioner.

Om du till exempel har delegerat *email.example.com* till Adobe för att skicka e-post, kommer Adobe att skapa underdomäner som följande:
* *t.email.example.com* - för att spåra länkar
* *m.email.example.com* - för spegelsidor
* *res.email.example.com* - för värdbaserade resurser (till exempel bilder)

Vi rekommenderar att **skydda dessa domäner via SSL (HTTPS)**. Osäkra länkar (HTTP) är sårbara för avlyssning och kommer att flagga för varningar i moderna webbläsare.

Om du vill installera SSL-certifikat på dessa underdomäner måste du begära en CSR-fil och sedan köpa SSL-certifikat för Adobe för att kunna installera eller förnya.

>[!CAUTION]
>
>Innan du installerar ett SSL-certifikat bör du kontrollera att du är medveten om de krav som anges på [den här sidan](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html?lang=sv#installing-ssl-certificate).
>
>Adobe stöder endast upp till 2 048-bitars certifikat. 4096-bitars certifikat stöds ännu inte.

## Ordlista

| Villkor | Beskrivning |
|--- |--- |
| CA (certifikatutfärdare) | En SSL-certifikatleverantör som utfärdar digitala certifikat till organisationer eller enskilda efter att ha verifierat deras identitet, som DigiCert, Symantec osv.<ul><li>En betrodd certifikatutfärdare betraktas vanligtvis som en tredjepartscertifikatutfärdare som utfärdar ett rotcertifikat.</li><li>Om certifikatet är signerat av samma organisation/företag som använder certifikatet, klassificeras det som ej betrodd certifikatutfärdare även när de är SSL-certifikat, till exempel självsignerade certifikat.</li></ul> |
| Kedjecertifikat | Ett certifikat som innehåller ett rotcertifikat och ett eller flera mellanliggande certifikat kallas ett kedjecertifikat (eller kedjat). |
| CSR (Certificate Signing Request) | Ett block med kodad text som ges till en certifikatutfärdare när en ansökan om ett SSL-certifikat görs. Den genereras vanligtvis på den server där certifikatet är installerat. |
| DER (Särskilda kodningsregler) | En certifikattilläggstyp. Tillägget .der används för binära DER-kodade certifikat. Dessa filer kan även ha stöd för filnamnstillägget .cer eller .crt. |
| EV-certifikat (Extended Validation) | Ett EV-certifikat är en ny typ av certifikat som är utformat för att förhindra nätfiskeattacker. Det kräver utökad validering av din verksamhet och av personen som beställer certifikatet. |
| Certifikat för hög säkerhet | Certifikat med hög säkerhet utfärdas av certifikatutfärdaren efter verifiering av ägarskap av domännamnet och giltig företagsregistrering. |
| Mellanliggande CA | En certifikatutfärdare av mellanliggande certifikat som ingår i ett kedjecertifikat. |
| Mellanliggande certifikat | En certifikatutfärdare utfärdar certifikat i form av en trädstruktur. Rotcertifikatet är det översta certifikatet för trädet. Alla certifikat mellan ditt certifikat och rotcertifikatet kallas en kedja eller ett mellanliggande certifikat. |
| Lågkvalitetscertifikat | Ett certifikat med låg säkerhet, som även kallas domänvaliderat certifikat, innehåller endast domännamnet i certifikatet (och inte namnet på företaget/organisationen). |
| PEM (Privacy Enhanced Mail) | Ett certifikat med tillägget .pem som innehåller ASCII-data (Base64). Sådana certifikat börjar med raden &quot; - - - - - - - BEGIN CERTIFICATE - - - - -&quot;. |
| Rotcertifikat | En certifikatutfärdare utfärdar certifikat i form av en trädstruktur. Rotcertifikatet är det översta certifikatet för trädet. |
| SAN (alternativt ämnesnamn) | Alternativa ämnesnamn är ytterligare värdnamn (platser, IP-adresser, vanliga namn osv.) som ska signeras som en del av ett enda SSL-certifikat. |
| Självsignerat certifikat | Ett certifikat som signeras av den person som skapar det i stället för en betrodd certifikatutfärdare. Självsignerade certifikat kan aktivera samma krypteringsnivå som ett certifikat signerat av en certifikatutfärdare, men det finns två stora nackdelar:<ul><li>En besökares anslutning kan kapas så att en angripare kan se alla data som skickas (vilket kan motverka syftet med krypteringen)</li><li> Certifikatet kan inte återkallas på samma sätt som ett betrott certifikat.</li></ul> |
| SSL (Secure Sockets Layer) | Standardsäkerhetstekniken för att skapa en krypterad länk mellan en webbserver och en webbläsare. |
| Jokertecken | Ett jokertecken kan skydda ett obegränsat antal underdomäner på första nivån på ett domännamn, till exempel *.adobe.com. |

## Huvudsteg

1. Be om en CSR-fil (Certificate Signing Request) och lämna nödvändig information (land, stat, ort, organisationsnamn, organisationsenhetsnamn osv.) till Adobe.
1. Validera CSR-filen som genererats av Adobe och verifiera att all information du angett är korrekt.
1. Använd CSR-informationen för att skapa ett certifikat som signerats av en betrodd certifikatutfärdare <!--taking care of asking for using the subjectAltName SSL extension (SAN) if it is for several domain names, and get/purchase the resulting certificate (ideally) in PEM format for Apache server-->.
1. Validera SSL-certifikatet och verifiera att det matchar CSR.
1. Ge SSL-certifikatet till Adobe, som ska installera det.
1. Testa att SSL-certifikatet har installerats för varje skyddad underdomän.
1. Övervaka giltighetsperioden för SSL-certifikatet.
1. Uppdatera en specifik konfiguration i Adobe Campaign.

## Detaljerad process

### Förhandskrav

Du måste identifiera domännamnen och funktionerna (spårning, spegelsidor, webbprogram osv.) för att kunna skydda dem.
>[!NOTE]
>
>Adobe kan hjälpa dig att definiera de domännamn och funktioner som ska ingå. Kontakta Adobe Account Team om du vill ha mer information.

### Steg 1 - Hämta en CSR-fil

Följ stegen nedan för att få en CSR-fil (Certificate Signing Request).

* Om du har tillgång till [Kontrollpanelen](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=sv) följer du instruktionerna på [den här sidan](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html?lang=sv#subdomains-and-certificates) för att generera och hämta en CSR-fil från Kontrollpanelen.

* I annat fall skapar du en supportanmälan via https://adminconsole.adobe.com/ för att få en CSR-fil från Adobe kundtjänst för de underdomäner som krävs.

Här följer några metodtips:

* Generera en begäran per delegerad underdomän.
* Det går att kombinera flera underdomäner till en enda CSR-begäran, men bara inom samma miljö. I Campaign Classic är till exempel marknadsföringsservern, [mellankällservern](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/install-campaign-on-prem/mid-sourcing-server.html?lang=sv-SE) och [körningsinstansen](https://experienceleague.adobe.com/docs/campaign-classic/using/transactional-messaging/configure-transactional-messaging/configuring-instances.html?lang=sv-SE#execution-instance) tre separata miljöer.
* Du måste skaffa en ny CSR innan du kan förnya SSL-certifikat. Använd inte en gammal CSR-fil från ett år sedan eller senare.

Du måste ange följande information.

>[!CAUTION]
>
>Alla fält som anges i tabellerna nedan måste fyllas i. Annars kan CSR-begäran inte behandlas.

**Information som ska tillhandahållas med hjälp av Adobe team:**

| Information att lämna | Exempelvärde | Obs |
|--- |--- |--- |
| Klientnamn | My Company Inc. | Organisationens namn. Det här fältet används av Adobe för att spåra din begäran (det ingår inte i CSR/SSL-certifikatet). |
| Adobe Campaign Environment URL | https://client-mid-prod1.campaign.adobe.com | Adobe Campaign instans-URL. |
| Gemensamt namn [CN] | t.subdomain.customer.com | Detta kan vara någon av de relevanta domänerna, men vanligtvis spårningsdomänen. |
| Alternativt namn för ämne [SAN] | t.subdomain.customer.com | Se till att inkludera spårningsunderdomän som ett SAN. |
| Alternativt namn för ämne [SAN] | m.subdomain.customer.com | |
| Alternativt namn för ämne [SAN] | res.subdomain.customer.com | |

**Information som ska tillhandahållas av ditt interna IT/SSL-team:**

| Information att lämna | Exempelvärde | Obs |
|--- |--- |--- |
| Land [C] | USA | Det här måste vara en kod med två bokstäver. Få åtkomst till den fullständiga landslistan [här](https://www.ssl.com/csrs/country_codes/).</br>*Obs! Använd GB (inte UK) för Storbritannien.* |
| Delstat (eller provinsnamn) [ST] | Illinois | Om tillämpligt. Värdet måste vara ett fullständigt namn, inte förkortat. |
| Ort/platsnamn [L] | Chicago | |
| Organisationsnamn [O] | KOM | |
| Organisationsenhetsnamn [OU] | IT | |

>[!NOTE]
>
>Ersätt&quot;subdomain.customer.com&quot; med din delegerade underdomän och de andra exempelvärdena med rätt värden.

### Steg 2 - Validera CSR-filen

När du har skickat in din begäran med relevant information, genererar Adobe en CSR-fil (Certificate Signing Request).

Texten i den resulterande CSR-filen måste börja med **—BEGIN CERTIFICATE REQUEST—**.

När du har fått CSR-filen från Adobe följer du stegen nedan:

1. Kopiera och klistra in CSR-filens text i en onlineavkodare som https://www.sslshopper.com/csr-decoder.html, <!--https://www.certlogik.com/decoder/,--> eller https://www.entrust.net/ssl-technical/csr-viewer.cfm.
Du kan också använda kommandot *OpenSSL* lokalt på en Linux-dator.
1. Kontrollera att alla kontroller är slutförda.
1. Kontrollera att rätt parametrar och domännamn finns med.
1. Kontrollera att alla andra data överensstämmer med de uppgifter du angav när du skickade din begäran.

### Steg 3 - Generera SSL-certifikatet

När CSR-filen har angetts måste du köpa och generera ett SSL-certifikat för rätt domäner med hjälp av CSR-filen.

* SSL-certifikatet:
   * måste vara i Apache PEM-format.
   * får inte vara längre än 2048 bitar.
   * ska vara signerat av en giltig certifikatutfärdare (certifikatutfärdare),
   * måste innehålla alla SAN-nätverk (Subject Alternative Names) som anges i CSR-filen.
* Om det finns ett eller flera mellanliggande certifikat måste du ange rotcertifikatet och alla mellanliggande certifikat till Adobe.
* Du kan ange vilken giltighetsperiod som helst, men Adobe rekommenderar att du väljer den tillräckligt lång (till exempel två år).

>[!NOTE]
>
>Om du använder dina egna interna verktyg eller en portal som tillhandahålls av en certifikatutfärdare för att begära certifikatet måste du använda samma information som i CSR-begäran för att undvika förseningar eller avvikelser i certifikatgenereringsprocessen.

### Steg 4 - Validera SSL-certifikatet

När SSL-certifikatet har skapats måste du validera det innan du skickar det till Adobe. För att göra detta, följ nedanstående steg:

1. Kontrollera att certifikatet har filnamnstillägget .pem. Om så inte är fallet konverterar du det till PEM-format. Du kan konvertera med *OpenSSL*.
1. Bekräfta att certifikatet börjar med **—BEGIN CERTIFICATE—**.
1. Kopiera certifikattexten till en onlineavkodare, till exempel https://www.sslshopper.com/certificate-decoder.html eller https://www.entrust.net/ssl-technical/csr-viewer.cfm.
Du kan också använda kommandot *OpenSSL* lokalt på en Linux-dator. Mer information finns på [den här externa sidan](https://www.shellhacks.com/decode-ssl-certificate/).
1. Kontrollera att certifikatet löses korrekt, inklusive Common Name, SAN, Issuer och Validity Period.
1. Om SSL-certifikatverifieringen lyckas kontrollerar du att certifikatet matchar CSR med [den här webbplatsen](https://www.sslshopper.com/certificate-key-matcher.html): välj **Kontrollera om en CSR och ett certifikat matchar** och ange ditt certifikat och din CSR i motsvarande fält. De borde matcha.

### Steg 5 - Begär installation av SSL-certifikat

* Om du har tillgång till [Kontrollpanelen](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=sv) följer du instruktionerna på [den här sidan](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html?lang=sv#installing-ssl-certificate) för att överföra certifikatet till Kontrollpanelen.

* I annat fall skapar du en ny supportanmälan via https://adminconsole.adobe.com/ för att begära att Adobe installerar certifikatet på Adobe-servrar.

Du måste ange:

* Certifikatfilen, rotcertifikatet och eventuella mellanliggande certifikat (bifogade till biljetten), helst i Apache PEM-format.
* Antalet tidigare supportärenden som tagits upp för CSR.
* Samma data som angavs för CSR-biljetten (inklusive gemensamt namn, instans-URL, delstat, ort/plats, organisationsnamn, organisationsenhetsnamn osv.).

### Steg 6 - Testa installationen av SSL-certifikatet

När SSL-certifikatet har installerats och bekräftats av Adobe kundtjänst kontrollerar du att det har installerats korrekt för alla URL:er.

Utför testerna nedan innan du stänger installationsbiljetten för SSL. Se även till att du uppdaterar någon specifik konfiguration enligt anvisningarna i [det här avsnittet](#update-configuration).

Navigera till följande URL:er i webbläsaren (ersätt&quot;subdomain.customer.com&quot; med din underdomän):

* https://subdomain.customer.com/r/test (endast för [webbprogram](https://experienceleague.adobe.com/docs/campaign-classic/using/designing-content/web-applications/about-web-applications.html?lang=sv-SE) underdomäner - gäller inte e-postunderdomäner)
* https://t.subdomain.customer.com/r/test
* https://m.subdomain.customer.com/r/test
* https://res.subdomain.customer.com/r/test

Resultatet ger miljöinformation och adressfältet i URL:en anger att anslutningen är säker. Du kan till exempel se följande meddelande i Google Chrome:

![](../../help/assets/ssl-google-successful-result.png)

Om SSL-certifikatet inte är korrekt installerat visas följande varning:

![](../../help/assets/ssl-google-unsuccessful-result.png)

### Steg 7 - Kontrollera certifikatets giltighetsperiod

Du kan kontrollera certifikatets giltighetsperiod i webbläsaren. I Google Chrome klickar du till exempel på **Skydda** > **Certifikat**.

Det är ditt ansvar att kontrollera giltighetsperioden. Adobe rekommenderar att du implementerar en process för att övervaka certifikatets förfallodatum. Läs mer om vad som händer när ditt SSL-certifikat upphör att gälla i [den här artikeln](https://www.thesslstore.com/blog/what-happens-when-your-ssl-certificate-expires/).

* Skapa en supportanmälan om du vill begära ett uppdaterat certifikat minst två veckor före certifikatets förfallodatum. Du behöver inte begära ytterligare en CSR, såvida inte CSR-informationen har ändrats.

* Om du har tillgång till [Kontrollpanelen](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=sv), och om din miljö hanteras av Adobe i en AWS-miljö, kan du använda Kontrollpanelen för att förnya certifikatet innan det upphör att gälla. Läs mer i [det här avsnittet](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-ssl-certificates.html?lang=sv-SE#monitoring-certificates).

### Steg 8 - Uppdatera en specifik konfiguration {#update-configuration}

När du är säker på att de begärda SSL-certifikaten är korrekt installerade kan du uppdatera alla referenser i Adobe Campaign från HTTP till HTTPS.

>[!NOTE]
>
>För Campaign Classic finns de URL:er som ska uppdateras huvudsakligen i [distributionsguiden](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/initial-configuration/deploying-an-instance.html#deployment-wizard) och i [externa konton](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/accessing-external-database/external-accounts.html?lang=sv-SE) (spårnings-, spegelsidesdomäner och offentliga resursdomäner). För Campaign Standard, se [Varumärkningskonfiguration](https://experienceleague.adobe.com/docs/campaign-standard/using/administrating/application-settings/branding.html?lang=sv-SE#about-brand-identity).

När konfigurationerna har uppdaterats skickas nya e-postmeddelanden med HTTPS-URL:er i stället för HTTP. Om du vill kontrollera att webbadresserna nu är säkra kan du snabbt utföra följande tester:

* Överför en bild från Adobe Campaign. När bilden har överförts ska URL:en som returneras vara HTTPS.
* Skapa ett test-e-postmeddelande som innehåller en länk till en spegelsida, bilder, text och en länk för att avbryta prenumerationen. Skicka ut e-postmeddelandet till ett externt e-post-ID (till exempel din Gmail-adress). Öppna e-postmeddelandet när det tagits emot och kontrollera att alla länkar i e-postmeddelandet öppnas korrekt i HTTPS-formuläret (inte HTTP), utan några SSL-certifikatvarningar eller fel.

## Produktspecifika resurser

**Campaign Classic**

* [Kontrollpanelen: Lägger till SSL-certifikat (självstudiekurs)](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html?lang=sv-SE) - Lär dig hur du lägger till SSL-certifikat för att skydda dina underdomäner.

**Campaign Standard**

* [Kontrollpanelen: Lägger till SSL-certifikat (självstudiekurs)](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html?lang=sv-SE) - Lär dig hur du lägger till SSL-certifikat för att skydda dina underdomäner.
