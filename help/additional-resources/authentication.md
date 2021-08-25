---
title: Autentisering
description: Läs mer om autentiseringsmetoderna SPF, DKIM och DMARC.
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 03609139-b39b-4051-bcde-9ac7c5358b87
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Autentisering

## SPF {#spf}

SPF (Sender Policy Framework) är en standard för e-postautentisering som gör att ägaren av en domän kan ange vilka e-postservrar som får skicka e-post för den domänens räkning. I den här standarden används domänen i e-postens &quot;Return-Path&quot;-huvud (kallas även för &quot;Envelope From&quot;-adress).

>[!NOTE]
>
>Du kan använda [det här externa verktyget](https://www.kitterman.com/spf/validate.html) för att verifiera en SPF-post.

SPF är en teknik som i viss utsträckning gör att du kan kontrollera att domännamnet som används i ett e-postmeddelande inte är falskt. När ett meddelande tas emot från en domän tillfrågas domänens DNS-server. Svaret är en kort post (SPF-posten) som anger vilka servrar som har behörighet att skicka e-post från den här domänen. Om vi antar att bara ägaren av domänen har möjlighet att ändra den här posten, kan vi tänka oss att den här metoden inte tillåter att avsändaradressen förfalskas, åtminstone inte delen från höger om&quot;@&quot;.

I den sista [RFC 4408-specifikationen](https://www.rfc-editor.org/info/rfc4408) används två element i meddelandet för att avgöra vilken domän som betraktas som avsändare: den domän som anges av SMTP-kommandot &quot;HELO&quot; (eller &quot;EHLO&quot;) och den domän som anges av adressen för huvudet &quot;Return-Path&quot; (eller &quot;MAIL FROM&quot;), som också är studsadressen. Olika överväganden gör det möjligt att endast ta hänsyn till ett av dessa värden. vi rekommenderar att du ser till att båda källorna anger samma domän.

Om du kontrollerar SPF-filen utvärderas giltigheten för avsändarens domän:

* **Ingen**: Ingen utvärdering kunde utföras.
* **Neutral**: Domänen som efterfrågas aktiverar inte utvärdering.
* **Godkänd**: Domänen anses vara autentisk.
* **Misslyckades**: Domänen är förfalskad och meddelandet bör avvisas.
* **SoftFail**: Domänen är antagligen förfalskad, men meddelandet bör inte avvisas enbart baserat på det här resultatet.
* **TempError**: Ett tillfälligt fel stoppade utvärderingen. Meddelandet kan avvisas.
* **PermError**: SPF-posterna för domänen är ogiltiga.

Det är värt att notera att det kan ta upp till 48 timmar att ta hänsyn till poster gjorda på DNS-servernivå. Den här fördröjningen beror på hur ofta DNS-cachen för de mottagande servrarna uppdateras.

## DKIM {#dkim}

DKIM-autentisering (DomainKeys Identified Mail) är en efterföljare till SPF. Den använder kryptografi med publika nycklar som gör att den mottagande e-postservern kan verifiera att ett meddelande verkligen skickades av den person eller enhet som den hävdar att det skickades av, och om meddelandeinnehållet ändrades mellan den tidpunkt då det ursprungligen skickades (och DKIM &quot;signerade&quot;) och den tidpunkt då det togs emot. Den här standarden använder vanligtvis domänen i huvudet Från eller Avsändare.

DKIM kommer från en kombination av DomainKeys, Yahoo! och Cisco identifierade autentiseringsprinciper för Internet Mail och används för att kontrollera avsändardomänens autenticitet och garantera meddelandets integritet.

DKIM ersatte **DomainKeys**-autentisering.

DKIM kräver vissa förutsättningar:

* **Säkerhet**: Kryptering är ett nyckelelement i DKIM. För att säkerställa att DKIM:s säkerhetsnivå är 1024b den rekommenderade krypteringsstorleken. Lägre DKIM-nycklar anses inte giltiga av de flesta åtkomstleverantörer.
* **Anseende**: Anseendet baseras på IP och/eller domänen, men den mindre transparenta DKIM-väljaren är också ett nyckelelement som ska beaktas. Det är viktigt att du väljer väljaren: Undvik att behålla&quot;standardinställningen&quot; som kan användas av vem som helst och därför har ett svagt anseende. Du måste implementera en annan väljare för **kvarhållning jämfört med förvärvsinformation** och för autentisering.

Läs mer om DKIM-krav när du använder Campaign Classic i [det här avsnittet](/help/additional-resources/acc-technical-recommendations.md#dkim-acc).

## DMARC {#dmarc}

DMARC (Domain-based Message Authentication, Reporting and Conformance) är den senaste formen av e-postautentisering, och den förlitar sig på både SPF- och DKIM-autentisering för att avgöra om ett e-postmeddelande godkänns eller misslyckas. DMARC är unikt och kraftfullt på två viktiga sätt:

* Överensstämmelse - Avsändaren kan instruera Internet-leverantörer om vad de ska göra med meddelanden som inte kan autentiseras (till exempel: acceptera det inte).
* Rapportering - Den ger avsändaren en detaljerad rapport som visar alla meddelanden som inte kunde verifieras av DMARC, tillsammans med domänen Från och IP-adressen som används för varje. Detta gör att ett företag kan identifiera giltig e-post som inte kan autentiseras och som behöver någon typ av &quot;fix&quot; (till exempel att lägga till IP-adresser i sin SPF-post) samt källorna till och förekomsten av nätfiskeförsök i sina e-postdomäner.

>[!NOTE]
>
>DMARC kan utnyttja rapporter som skapats av [250ok](https://250ok.com/).
