---
title: Implementera Gmail:s varumärkesidentifierare för meddelandeidentifiering (BIMI)
description: Lär dig implementera BIMI
topics: Deliverability
exl-id: 6b911bcc-a531-466a-8bd3-7fa469b96cc7
source-git-commit: a4d2a75e85f37f48aa3246707b98e473682e13f6
workflow-type: tm+mt
source-wordcount: '686'
ht-degree: 0%

---

# Implementera Gmail [!DNL Brand Indicators for Message Identification] (BIMI)

Gmail meddelade nyligen att de skulle [lansera allmänt stöd för BIMI](https://cloud.google.com/blog/products/identity-security/bringing-bimi-to-gmail-in-google-workspace). Det finns ett antal saker du måste ta itu med innan du kan dra nytta av detta, bland annat: Verifierade Mark Certificates, varumärkesskyddade logotyper, logotyper med korrekt format, DMARC-inställningar och publicerar slutligen en BIMI-post till din DNS. Vi kommer att granska alla dessa steg i den här artikeln.

[!DNL Brand Indicators for Message Identification] (BIMI) är en branschstandard som tillåter att en godkänd logotyp visas bredvid en avsändares e-postadress på deltagande plattformar. Det är inte bara det här som kan öka engagemanget, det bidrar också till att bekräfta avsändarens autenticitet och minskar risken för nätfiske och andra skräptaktiker.

## Verifierat märkescertifikat

En av huvudkomponenterna i Gmail:s BIMI-program är kravet att avsändarna har ett verifierat Mark Certificate (VMC) utfärdat av en giltig certifikatutfärdare. För närvarande är de här VMC:erna bara tillgängliga från Entrust och DigiCert, men listan över leverantörer kommer sannolikt att växa efter Gmail:s meddelande.

VMC liknar SSL-certifikat på vissa sätt. Du behöver en VMC för varje logotyp som du vill visa, så om du har många varumärken bör du planera för att behöva flera VMC. Varje VMC kan dock vara giltigt över flera domäner om du får en VMC med flera SAN. Om du vill att en logotyp ska visas i flera sändande domäner behöver du bara en VMC.

## Varumärke för logotyp

Innan du kan hämta din VMC finns det ett annat nyckelsteg som måste slutföras. För att få en VMC måste den logotyp som du vill visa vara registrerad med ett av åtta godkända globala varumärken och patentbyråer.

* United States Patent and Trademark Office (USPTO)
* Canadian Intelligence Property Office
* Europeiska unionens byrå för immaterialrätt
* UK Intelligence Property Office
* Deutsches Patent- und Markenamt
* Japan Trademark Office
* Spansk patentbyrå och varumärkesbyrå
* IP Australia

Om logotypen som du vill visa inte är registrerad eller inte är registrerad hos någon av dessa åtta organisationer måste du arbeta med ditt juridiska team för att åtgärda det innan du ansöker om VMC.

## Bildformat för logotyp

Det är också ett bra tillfälle att se till att din logotyp uppfyller kraven för BIMI-logotypen.

Det måste vara i SVG-format och följa SVG Portable/Secure-profil (SVG-P/S). Vägledning om hur du gör detta finns på [BIMI Working Group](https://bimigroup.org/svg-conversion-tools-released).

## DMARC

När din varumärkeslogotyp är korrekt formaterad och ditt verifierade Mark-certifikat måste du också se till att DMARC är fullständigt konfigurerat för alla sändande domäner som du vill att BIMI ska fungera för.

Detta inkluderar att se till att P= är inställt på antingen Karantän eller Avvisa. Om din DMARC använder P=None är den inte berättigad till BIMI. Inställningen P=Ingen rekommenderas för att säkerställa att du vet vilken e-post som kommer ut från en domän och att inget skulle blockeras av misstag om du ändrade till antingen &quot;Karantän&quot; eller &quot;Avvisa&quot;, tänk på det som testnings- och informationsinsamlingsfasen. Du måste gå steget längre än så till att genomdriva BIMI innan du har tillgång till det.

## DNS-post

När allt annat är klart är det dags att uppdatera DNS-posten med BIMI.

Det här är ett enkelt inlägg som ska se ut ungefär så här:

```
default._bimi.[domain] IN TXT “v=BIMI1; l=[SVG URL] 
```

Du kan få information om inlägget och till och med använda en kostnadsfri BIMI-kontroll på [BIMI - arbetsgruppsplats](https://bimigroup.org/implementation-guide).


## Viktiga uppgifter

Om du är en [!DNL Adobe Campaign] För Marketo-klienter kan Adobe hjälpa dig att skapa DNS-uppdateringen för BIMI: kontakta Adobe kundtjänst för att beställa en. Adobe kan även hjälpa dig med felsökning om BIMI inte fungerar som det ska för dig.

Om du behöver hjälp med varumärken eller verifierade varumärkescertifikat kan du samarbeta med ditt juridiska team och en auktoriserad VMC-leverantör.

Det kanske inte går snabbt att få BIMI-inställningar för Gmail, men det kan ha betydande fördelar både ur ett marknadsförings- och säkerhetsperspektiv.
