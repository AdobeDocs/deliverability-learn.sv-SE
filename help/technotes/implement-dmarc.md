---
title: Implementera Gmail:s varumärkesidentifierare för meddelandeidentifiering (BIMI)
description: Lär dig implementera BIMI
topics: Deliverability
role: Admin
level: Beginner
exl-id: f1c14b10-6191-4202-9825-23f948714f1e
source-git-commit: 2a78db97a46150237629eef32086919cacf4998c
workflow-type: tm+mt
source-wordcount: '1284'
ht-degree: 8%

---

# Implementera [!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC)

Syftet med det här dokumentet är att ge läsaren ytterligare information om e-postautentiseringsmetoden, DMARC. Genom att förklara hur DMARC fungerar och dess olika politiska alternativ kommer läsarna att få en bättre förståelse för DMARC:s påverkan på e-postleveransen.

## Vad är DMARC? {#about}

Domänbaserad meddelandeautentisering, rapportering och överensstämmelse är en e-postautentiseringsmetod som gör att domänägare kan skydda sin domän från obehörig användning. DMARC ger också feedback om e-postautentiseringsstatus och tillåter avsändare att styra vad som händer med e-postmeddelanden som inte kan autentiseras. Detta inkluderar alternativ för att övervaka, sätta i karantän eller avvisa e-post beroende på vilken DMARC-policy som har implementerats.

DMARC har tre politiska alternativ:

* **Monitor (p=none):** Instruerar postlådeprovidern/ISP att göra vad de normalt skulle göra med meddelandet.
* **Karantän (p=karantän):** Instruerar postlådeprovidern/ISP att leverera e-post som inte skickar DMARC till mottagarens skräppostmapp.
* **Avvisa (p=avvisa):** Instruerar postlådeprovidern/ISP att blockera e-post som inte godkänns av DMARC, vilket resulterar i ett studs.

## Hur fungerar DMARC? {#how}

SPF och DKIM används båda för att associera ett e-postmeddelande med en domän och fungerar tillsammans för att autentisera e-post. DMARC tar detta steg längre och hjälper till att förhindra förfalskning genom att matcha den domän som kontrolleras av DKIM och SPF. För att skicka DMARC måste ett meddelande skicka SPF eller DKIM. Om båda dessa misslyckas kommer DMARC att misslyckas och e-postmeddelandet levereras enligt din valda DMARC-policy.

>[!NOTE]
>
>DMARC kräver justering mellan adressen&quot;Från&quot; och&quot;Retursökväg&quot;.

## Varför ska DMARC implementeras? {#why}

DMARC är valfritt, och även om det inte krävs är det kostnadsfritt och gör det möjligt för e-postmottagare att enkelt identifiera autentiseringen av e-postmeddelanden, vilket kan förbättra leveransen. En av de största fördelarna med DMARC är att det erbjuder rapportering om vilka meddelanden som inte godkänns i SPF och/eller DKIM. Det ger också avsändarna en viss kontroll över vad som händer med e-post som inte godkänns på någon av dessa autentiseringsmetoder. Genom DMARC-rapportering får avsändarna insyn i vilka meddelanden som misslyckas med DMARC, vilket gör det möjligt att vidta åtgärder för att minska ytterligare fel.

>[!NOTE]
>
>Om du vill implementera BIMI krävs en p=karantän eller p=avvisa DMARC-princip.

## Bästa metoder för att implementera DMARC {#best-practice}

Eftersom DMARC är valfritt kommer det inte att konfigureras som standard på någon ESP:s plattform. En DMARC-post måste skapas i DNS för din domän för att den ska fungera. Dessutom krävs en e-postadress som du väljer för att ange var DMARC-rapporter ska finnas inom organisationen. Det är en god praxis att
Vi rekommenderar att du långsamt implementerar DMARC genom att trahera din DMARC-policy från p=none till p=karantän, till p=reject när du får DMARC-förståelse för DMARC:s potentiella effekt.

1. Analysera den feedback du får och använder (p=none), som instruerar mottagaren att inte utföra några åtgärder mot meddelanden som inte kan autentiseras, men ändå skicka e-postrapporter till avsändaren. Granska och åtgärda även problem med SPF/DKIM om giltiga meddelanden inte kan autentiseras.
1. Kontrollera om SPF och DKIM är justerade och skickar autentisering för alla giltiga e-postmeddelanden, och flytta sedan principen till (p=karantän), vilket anger att den mottagande e-postservern ska placera e-postmeddelanden som inte kan autentiseras (detta innebär vanligtvis att meddelandena placeras i skräppostmappen).
1. Justera princip till (p=avvisa). Principen p=reject om att avvisa innebär att mottagaren helt nekar (studsar) alla e-postmeddelanden för domänen som inte kan autentiseras. När den här principen är aktiverad är det bara e-postmeddelanden som verifieras som 100 % autentiserade av din domän som har en chans att skickas till Inkorgen.

   >[!NOTE]
   >
   >Använd denna policy med försiktighet och fastställ om den är lämplig för din organisation.

## DMARC-rapportering {#reporting}

DMARC kan ta emot rapporter om e-postmeddelanden som saknar SPF/DKIM. Det finns två olika rapporter som genereras av ISP-tjänstleverantörer som en del av autentiseringsprocessen och som avsändare kan ta emot via RUA/RUF-taggarna i deras DMARC-policy:

* **Aggregate Reports (RUA):** innehåller inte någon PII (Personally Identiitable Information) som skulle vara GDPR-känslig.
* **Forensiska rapporter (RUF):** Innehåller e-postadresser som är GDPR-känsliga. Innan informationen används är det bäst att kontrollera internt hur man hanterar information som måste uppfylla GDPR.

Det viktigaste användningsområdet för dessa rapporter är att få en översikt över e-postmeddelanden som försöker förfalskas. Det här är mycket tekniska rapporter som är bäst sammanställda via ett verktyg från tredje part. Några företag som specialiserar sig på DMARC-övervakning är:

* [ValiMail](https://www.valimail.com/products/#automated-delivery)
* [Agari](https://www.agari.com/)
* [Dmarcier](https://dmarcian.com/)
* [Korrekturpunkt](https://www.proofpoint.com/us)

>[!CAUTION]
>
>Om e-postadresserna som du lägger till för att ta emot rapporter ligger utanför domänen som DMARC-posten skapas för, måste du auktorisera e-postadressernas externa domän för att ange för DNS:en att du äger den här domänen. Gör det här genom att följa stegen som beskrivs på [dmarc.org](https://dmarc.org/2015/08/receiving-dmarc-reports-outside-your-domain)

### Exempel på DMARC-post {#example}

```
v=DMARC1; p=reject; fo=1; rua=mailto:dmarc_rua@emaildefense.proofpoint.com;ruf=mailto:dmarc_ruf@emaildefense.proofpoint.co
```

## DMARC-taggar och vad de gör {#tags}

DMARC-poster har flera komponenter som kallas DMARC-taggar. Varje tagg har ett värde som anger en viss aspekt av DMARC.

| Märkordsnamn | Obligatoriskt/valfritt | Funktion | Exempel | Standardvärde |
|  ---  |  ---  |  ---  |  ---  |  ---  |
| v | Obligatoriskt | Den här DMARC-taggen anger versionen. Det finns bara en version från och med nu, så det här har ett fast värde på v=DMARC1 | V=DMARC1 DMARC1 | DMARC1 |
| p | Obligatoriskt | Visar den valda DMARC-principen och instruerar mottagaren att rapportera, karantän eller avvisa e-post som inte kan autentiseras. | p=ingen, karantän eller avvisad | – |
| fo | Valfritt | Låter domänägaren ange rapportalternativ. | 0: Generera en rapport om allt misslyckas<br/>1: Generera en rapport om något misslyckas<br/>d: Generera en rapport om DKIM misslyckas<br/>s: Generera en rapport om SPF misslyckas | 1 (rekommenderas för DMARC- rapporter) |
| pct | Valfritt | Anger procentandelen meddelanden som ska filtreras. | pct=20 | 100 |
| rua | Valfritt (rekommenderas) | Identifierar var aggregerade rapporter kommer att levereras. | `rua=mailto:aggrep@example.com` | – |
| ruf | Valfritt (rekommenderas) | Identifierar var kriminaltekniska rapporter kommer att levereras. | `ruf=mailto:authfail@example.com` | – |
| sp | Valfritt | Anger DMARC-princip för underdomäner till den överordnade domänen. | sp=avvisa | – |
| adkim | Valfritt | Kan vara Strikt (Strikt) eller Avspänd (r). Avlastad justering innebär att domänen som används i DKIM-signaturen kan vara en underdomän till Från-adressen. Strikta justeringar innebär att domänen som används i DKIM-signaturen måste vara en exakt matchning av domänen som används i formuläradressen. | adkim=r | r |
| aspf | Valfritt | Kan vara Strikt (Strikt) eller Avspänd (r). Avspänd justering innebär att ReturnPath-domänen kan vara en underdomän till Från adress. Strikta justeringar innebär att domänen Return-Path måste vara exakt densamma som Från-adressen. | aspf=r | r |

## DMARC &amp; Adobe Campaign {#campaign}

>[!NOTE]
>
>Om din Campaign-instans finns på AWS kan du implementera DMARC för dina underdomäner med Kontrollpanelen. [Lär dig hur du implementerar DMARC-poster med Kontrollpanelen](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/txt-records/dmarc.html?lang=sv-SE).

En vanlig orsak till DMARC-fel är felpassning mellan adressen&quot;Från&quot; och&quot;Fel till&quot; eller&quot;Retursökväg&quot;. För att undvika detta bör du kontrollera adressinställningarna&quot;Från&quot; och&quot;Fel till&quot; i leveransmallarna när du konfigurerar DMARC.

1. Granska i leveransmallen vilken adress som är angiven som din&quot;Från&quot;-adress.

   ![](../assets/dmarc1.png)

1. Här väljer du Egenskaper som gör att du kan redigera leveransmallen ytterligare. I det här fönstret väljer du SMTP och avmarkerar Använd standardfeladressen som definierats för plattformen om det är markerat. Leveransmallar i Adobe Campaign markerar den här kryssrutan som standard. Standardfeladressen får inte vara den adress som är associerad med Från adress i den här leveransmallen.

   ![](../assets/dmarc2.png)

1. När den här rutan är avmarkerad visas ett textfält där du kan ange en unik feladress som använder samma domän som anges i Från adress.

   ![](../assets/dmarc3.png)

När dessa ändringar har sparats kan du gå vidare med din DMARC-implementering med korrekt domänjustering.

## Användbara länkar {#links}

* [DMARC.org](https://dmarc.org/){target="_blank"}
* [M3AWG-e-postautentisering](https://www.m3aawg.org/sites/default/files/document/M3AAWG_Email_Authentication_Update-2015.pdf){target="_blank"}
