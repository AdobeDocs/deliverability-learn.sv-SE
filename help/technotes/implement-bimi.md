---
title: Implementera Gmail:s varumärkesidentifierare för meddelandeidentifiering (BIMI)
description: Lär dig implementera BIMI
topics: Deliverability
role: Admin
level: Beginner
jira: KT-14079
exl-id: 6b911bcc-a531-466a-8bd3-7fa469b96cc7
source-git-commit: b96539608acd51ce76ef5bdaf5afd07b5a4208b7
workflow-type: tm+mt
source-wordcount: '1125'
ht-degree: 0%

---

# Implementera [!DNL Brand Indicators for Message Identification] (BIMI)

[!DNL Brand Indicators for Message Identification] (BIMI) är en branschstandard som tillåter att en godkänd logotyp visas bredvid en avsändares e-postadress på deltagande plattformar.

Med den här standarden kan ett varumärke fastställa en logotyp som ska visas i postlådans leverantörers inkorgar. När den publicerats i en så kallad BIMI DNS-post (Domain Name System) kan en postlådeleverantör hämta den här logotypen och visa den i inkorgen om vissa villkor uppfylls.

Olika leverantörer gör olika implementeringar, men fördelarna är uppenbara: att sticka ut i inkorgen, bygga upp förtroende och ha kontroll över vad som visas.

BIMI förbättrar inte direkt leveransen eller ditt rykte. Men det kan hjälpa er att bygga upp större förtroende med era mottagare och därmed öka engagemanget.

## Hur ser det ut?

Du kan hitta några exempel på implementeringar från olika leverantörer och mer information om vilka leverantörer som visar logotypen på [BIMI-gruppens sida](https://bimigroup.org/where-is-my-bimi-logo-displayed/){target="_blank"}.

## Vem är BIMI-gruppen?

BIMI-gruppen är en arbetsgrupp som utvecklar BIMI eftersom den inte bara omfattar logotypen utan också tekniska, rättsliga och efterlevnadsmässiga krav.

BIMI Group består av flera intressenter från olika delar av branschen: Google, Yahoo, Fastmail, Proofpoint, Mailchimp, Sendgrid, Valimail och Validity.

## Vem stöder BIMI?

Postlådeleverantörernas lista som stöder BIMI växer stadigt. En uppdaterad lista finns [här](https://bimigroup.org/bimi-infographic/){target="_blank"} både för stödleverantörer och för leverantörer som överväger BIMI.

Från och med april 2023 innehåller listan Gmail, Yahoo, La Poste, Fastmail, Onet.pl och Zone, Proofpoint som antispam-enhet och Apple Mail (från och med iOS 16).

De mest framträdande namnen på den listan är uppenbarligen Yahoo, Gmail och en nybörjare: Apple med iOS 16. Apple har en speciell roll i mixen eftersom de inte är postlådeleverantör, men de har lagt till BIMI-stöd i sin e-postapp. E-post som är kompatibel med BIMI visas som&quot;digitalt certifierad e-post&quot;, vilket ökar förtroendet för varumärket.

## Implementering

Att implementera BIMI går i flera steg:

1. DMARC-implementering (domänbaserad meddelandeautentisering, rapportering och överensstämmelse) på efterlevnadsnivå för både den sändande domänen och dess organisationsdomän - [Läs mer](#dmarc)

1. Skapande av din logotyp i formatet SVG TinyPS - [Läs mer](#create-brand-logo)

1. Registrera dig för ett verifierat varumärkescertifikat (krävs endast för vissa leverantörer) - [Läs mer](#vmc)

1. Publish en BIMI DNS-post med logotypen och certifikatet - [Läs mer](#publish-bimi-record)

1. Har gott rykte - [Läs mer](#good-reputation)

>[!NOTE]
>
>Observera att alla steg måste vara avmarkerade.


### DMARC {#dmarc}

DMARC är en standard som gör att varumärket kan bestämma vad en postlådeleverantör ska göra med ett e-postmeddelande som inte kan [autentiseras](../additional-resources/authentication.md). De så kallade profilerna sträcker sig från&quot;ingen&quot; över&quot;karantän&quot; (placering av skräppostmappen) till&quot;avvisa&quot; (blockera e-post helt). Det är bara de senare två policyerna som kallas&quot;genomförande&quot; och som är kvalificerade för BIMI. E-post som skickas av Adobe skickar autentisering eftersom SPF (Sender Policy Framework) och DKIM (Domain Keys Identified Mail) är inställda som standard. Adobe konfigurerar DMARC på din sändande domän på begäran.

Förutom DMARC på den sändande domänen måste DMARC även användas på efterlevnadsnivå för organisationsdomänen (om den sändande domänen är news.example.com är example.com organisationsdomänen).

### Skapa din logotyp {#create-brand-logo}

Logotypen måste uppfylla kraven till 100 %. Se alltid [BIMI-gruppens riktlinjer](https://bimigroup.org/creating-bimi-svg-logo-files/){target="_blank"}.

Logotypen måste lagras på en säker plats (HTTPS) om ett leveransnätverk (CDN) används för att skydda postlådeprovidrar från att hämta logotypen (t.ex. punktskydd).

Förutom de tekniska kraven finns det några praktiska rekommendationer, som att ha en fyrkantig logotyp, en enfärgad bakgrundsfärg och andra. Rekommendationerna är till för bästa visualisering. Vissa leverantörer har egna krav som är utöver dem som ställs av BIMI-arbetsgruppen. [Gmail](https://support.google.com/a/answer/10911027?sjid=903725605955621707-EU){target="_blank"} kräver till exempel att logotypen ska vara minst 96 x 96 pixlar.
Observera att bristande efterlevnad kan leda till att logotypen inte visas.

### Verifierat varumärkescertifikat (VMC) {#vmc}

Ett Verified Mark Certificate (VMC) behövs bara för vissa postlådeproviders som Gmail och Apple, och är därför valfritt. Vi rekommenderar att du skaffar en VMC för att verkligen utnyttja BIMI.

Ett verifierat varumärkescertifikat är en giltig validering av att varumärket kan använda logotypen. En certifikatutfärdare kommer att kontrollera detta via det varumärkeskontor där varumärkeslogotypen är registrerad. Denna process innefattar flera juridiska valideringar och kontroller, och kan ta en stund. För närvarande utfärdar två certifikatutfärdare (certifikatutfärdare) VMC: Digicert och Entrust. Den första uppsättningen varumärkeskontor är USA, Kanada, EU, Storbritannien, Tyskland, Japan, Australien och Spanien.

Som regel behöver du en VMC per logotyp. En VMC för din organisationsdomän omfattar underdomäner och har en tillagd funktion som till och med skiljer sig åt. Om du har olika logotyper behövs mer än en VMC. Den certifikatutfärdare eller den partner du väljer att arbeta med hjälper dig att konfigurera detta.

>[!NOTE]
>
>Observera att VMC har en årsavgift.

### Publicera BIMI-posten {#publish-bimi-record}

När de andra stegen är klara kan BIMI-DNS-posten publiceras. Posten ser ut så här:

```
default._bimi.[domain] IN TXT "v=BIMI1; l=[SVG URL]; a=[PEM URL]
```

&quot;PEM URL&quot; är den plats där det verifierade märkescertifikatet finns.

För den sändande domänen måste detta göras av Adobe.

### Bra rykte {#good-reputation}

Förtroende är avgörande för BIMI. Användaren litar på sin postlådeleverantör för att endast visa logotypen för berättigade avsändare, så postlådeprovidern måste lita på varumärket, och detta görs av avsändarens rykte. Om du har ett gott rykte är allt bra, men om du inte gör det visas inte logotypen. Tyvärr finns det inga uppgifter eller mätvärden som vi kan undersöka för att ta reda på om ryktet är tillräckligt högt.

Även om en VMC går igenom arbetet och kostnaderna tar inte bort den här delen. Om postlådeprovidern inte litar på varumärket visas inte logotypen.

## Tips och tricks

* BIMI-gruppen har ett praktiskt valideringsverktyg för BIMI. Om du vill dubbelkontrollera om allt är konfigurerat och klart, eller bara vill se om logotypen är kompatibel, går du till [den här länken](https://bimigroup.org/bimi-generator/){target="_blank"}. För den senare klickar du bara på **[!UICONTROL Generate BIMI]** och anger en platshållardomän men rätt logotypadress. Inspektören talar om för dig om logotypen är kompatibel.

* Du kan börja utan en VMC utan problem, men det skadar inte ditt rykte om BIMI-posten inte innehåller någon VMC-URL, men logotypen kan redan visas i Yahoo.

* Att införa DMARC på organisationsnivå är ett stort företag. Vissa företag är specialiserade på att hjälpa varumärken att uppnå en fullständig DMARC-användning.

* En omfattande lista med vanliga frågor och svar publiceras [här](https://bimigroup.org/faqs-for-senders-esps/){target="_blank"}.
