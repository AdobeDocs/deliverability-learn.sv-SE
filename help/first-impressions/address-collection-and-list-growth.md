---
title: Adressinsamling och listökning
description: 'Lär dig vilka de bästa källorna för nya e-postadresser är, hur du säkerställer hög datakvalitet och hur du anpassar dig till juridiska riktlinjer. '
topics: Deliverability
kt: 7063
thumbnail: kt7063.jpg
doc-type: article
activity: understand
team: TM
exl-id: 350950dc-4703-402a-8e22-3862f4e21d52
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '1622'
ht-degree: 3%

---

# Adressinsamling och listökning

De bästa källorna till nya e-postadresser är direktkällor som registreringar på er webbplats eller i fysiska butiker. I sådana situationer kan ni styra upplevelsen för att säkerställa att den är positiv och att prenumeranten är intresserad av att få e-post från ert varumärke.

Några anteckningar om dessa registreringsmetoder:

**Insamlingen av fysiska** arkivlistor kan medföra problem på grund av muntliga eller skriftliga adressindata som orsakar felstavning i adresserna. Du bör skicka ett bekräftelsemeddelande via e-post så fort som möjligt efter det att du registrerat dig i butik.

Den vanligaste formen för **webbplatsregistrering** är&quot;single opt-in&quot;. Det är den absoluta minimistandarden som du bör använda för att hämta e-postadresser. Ett fristående deltagande är när innehavaren av en viss e-postadress ger en avsändare tillstånd att skicka marknadsföring via e-post till dem, vanligen genom att skicka adressen via ett webbformulär eller butiksregistreringar. Det går att köra en lyckad e-postkampanj med den här metoden, men det kan bero på vissa problem.

* Obekräftade e-postadresser kan innehålla stavfel, vara felaktiga eller ha skadligt format. Typos och felformaterade adresser orsakar höga studsfrekvenser, vilket kan och gör att blockeringar som utfärdas av Internet-leverantörer eller IP-adresser inte fungerar som de ska.

* Skadlig inlämning av kända skräppostfällor (kallas ibland&quot;listförgiftning&quot;) kan orsaka stora problem med leveransen och anseendet om ägaren till fällan vidtar åtgärder. Det är omöjligt att veta om mottagaren verkligen vill bli medlem i en marknadsföringslista utan att bekräfta det. Detta gör det lika omöjligt att ställa in mottagarens förväntningar och kan leda till fler skräppostklagomål - och ibland blocklist om det insamlade e-postmeddelandet råkar vara en skräppostfälla.

Mer information om hur du minimerar problemen i både fysisk lagring och fristående deltagande finns i [Datakvalitet och hygien](#data-quality-and-hygiene)-avsnittet i den här handboken där du hittar information om och fördelar med dubbel anmälan.

>[!NOTE]
>
>Prenumeranter använder ofta bortkastadresser, utgångna adresser eller adresser som inte är deras för att få det de vill ha från en webbplats, men undviker också att läggas till i marknadsföringslistor. När detta händer resulterar marknadsförarnas listor i ett stort antal hårda studsar, höga andelen skräppost och prenumeranter som inte klickar, öppnar eller engagerar sig positivt i e-postmeddelanden. Den kan ses som en röd flagga för postlådeleverantörer och Internet-leverantörer.

## Anmälningsformulär

Förutom att lägga till fälten för data, vill du samla in information om dina nya prenumeranter, finns det några andra saker du bör göra med registreringsformuläret på webbplatsen.

* Ange tydliga förväntningar med prenumeranten på att de godkänner att få e-post, vad de får och hur ofta de får det.
* Lägg till alternativ så att abonnenten kan välja frekvens eller typ av kommunikation som han/hon får. Dessa alternativ gör att du kan känna till prenumerantens preferenser redan från början så att du kan ge den bästa möjliga upplevelsen för din nya kund.
* Balansera risken att förlora abonnentens intresse under registreringsprocessen genom att be om så mycket information som möjligt. Det som är deras födelsedag, plats eller intressen hjälper dig att skicka mer anpassat innehåll. Varje varumärkes prenumeranter har olika förväntningar och toleransnivåer, så tester är avgörande för att hitta rätt balans för er situation.

>[!NOTE]
>
> Använd inte förkryssade rutor under registreringsprocessen. Även om det kan förorsaka problem juridiskt sett är det också en negativ kundupplevelse.

## Datakvalitet och hygien

Att samla in data är bara en del av utmaningen. Du måste också se till att informationen är både korrekt och användbar. Du bör ha grundläggande formatfilter på plats. En e-postadress är inte giltig om den inte innehåller&quot;@&quot; eller&quot;.&quot; till exempel. Se till att du inte tillåter vanliga aliasadresser, som också kallas rollkonton (som &quot;info&quot;, &quot;admin&quot;, &quot;sales&quot;, &quot;support&quot;). Rollkonton kan utgöra en risk eftersom mottagaren till sin natur innehåller en grupp personer i motsats till en enskild prenumerant. Förväntningarna och toleransen kan variera inom en grupp, vilket kan leda till klagomål, olika engagemang, avanmälan och allmän förvirring.

Här är några lösningar på vanliga problem som du kan stöta på med dina e-postadressdata:

**[!DNL Double opt-in (DOI)]**
[!DNL Double opt-in (DOI)] anses vara den bästa leveransmetoden för de flesta e-postexperter. Om du har problem med skräppostfällor eller klagomål på dina välkomstmeddelanden är DOI ett bra sätt att se till att den prenumerant som tar emot dina e-postmeddelanden verkligen har registrerat sig för ditt e-postprogram och vill få dina e-postmeddelanden.

DOI består av att skicka ett bekräftelsemeddelande via e-post till prenumerantens e-postadress, som har registrerat sig för ditt e-postprogram och som innehåller en länk som måste klickas för att bekräfta samtycke. Med den här förvärvsmetoden kan avsändaren inte skicka fler e-postmeddelanden om prenumeranten inte bekräftar det. Informera nya prenumeranter om att du gör detta på webbplatsen och uppmuntra dem att slutföra registreringen innan de fortsätter. Den här metoden ser en minskning av antalet registreringar, men de som registrerar sig tenderar att vara mycket engagerade och stanna på lång sikt. Det ger oftast en mycket högre avkastning för avsändaren.

**Dolt**
fältAtt använda ett dolt fält i registreringsformuläret är ett bra sätt att skilja automatiserade startsregistreringar från verkliga humanprenumeranter. Eftersom datafältet inte är synligt, dolt i HTML-koden, kommer en robot att ange data där en människa inte skulle göra det. Med den här metoden kan du skapa regler för att inaktivera registreringar som innehåller data i det dolda fältet.

**[!DNL re-CAPTCHA] är en valideringsmetod som du kan använda för att minska riskerna för att prenumeranten är en robot och inte en riktig person. Det finns olika versioner, varav vissa innehåller nyckelordsidentifiering eller bilder. Vissa versioner är effektivare än andra, och det ni får när det gäller att förhindra problem med säkerhet och leverans är mycket större än någon negativ effekt på konverteringarna.

## Juridiska riktlinjer

Kontakta era advokater för att tolka lokal och nationell lagstiftning om e-post. Kom ihåg att e-postlagstiftningen varierar mycket mellan olika länder och ibland mellan olika lokala regioner i ett land.

* Se till att samla in en abonnents platsinformation så att du följer abonnentens landslagstiftning. Utan den detaljrikedomen kan ni begränsa hur ni kan marknadsföra till abonnenten.
* Alla lagar som är relevanta bestäms av var mottagaren befinner sig, inte avsändaren. Så ni måste känna till och följa lagarna i alla länder där ni kan ha en abonnent.
* Det är ofta svårt att med total säkerhet veta i vilket land abonnenten bor. Data som tillhandahålls av kunden kan vara inaktuella och pixelplatsdata kan vara felaktiga på grund av VPN eller bildlagring, som med Gmail och Yahoo. Det är säkrast att tillämpa striktaste möjliga lagar och riktlinjer.

## Andra icke-rekommenderade metoder för listsamling

Det finns många andra sätt att samla in adresser, vart och ett med sina egna möjligheter, utmaningar och nackdelar. Adobe rekommenderar inte dessa i allmänhet, eftersom användningen ofta begränsas via leverantörens policy för godtagbar användning. Vi ska titta på några vanliga exempel så att du kan lära dig farorna som hjälper dig att begränsa eller undvika riskerna:

**Köp eller hyra en**
listaDet finns många typer av e-postadresser där ute. Primär e-post, e-post till arbetet, e-post till skolan, sekundära e-postmeddelanden och inaktiva e-postmeddelanden för att nämna några. De typer av adresser som samlas in och delas ut via köpta eller hyrda listor är sällan primära e-postkonton, där nästan all engagemangs- och inköpsaktivitet sker.

Om du har tur får du ett sekundärt konto, där folk letar efter erbjudanden när de är redo att köpa något. Detta resulterar vanligtvis i låga engagemangsnivåer - om sådana finns. Om du inte har tur är listan full av inaktiva e-postmeddelanden som nu kan vara skräppostsvällningar. Du får ofta en blandning av både sekundära och inaktiva e-postmeddelanden. I allmänhet är kvaliteten på dessa typer av listor mer skadlig än bra för e-postprogram. Detta är förbjudet enligt [Adobe Campaign policy för godtagbar användning](https://www.adobe.com/legal/terms/aup.html).

**Bifogade listor**
Det här är kunder som har valt att interagera med ert varumärke, vilket är bra. Men de valde att engagera sig via en annan metod än e-post (butiker, sociala medier osv.). De kunde inte vara mottagliga för att få ett oönskat e-postmeddelande från dig och kanske också bekymra sig om hur du fick deras e-postadress eftersom de inte tillhandahöll den. Den här metoden riskerar att förvandla en kund eller potentiell kund som interagerade med ert varumärke till en traktor som inte längre litar på ert varumärke och som istället går in i er konkurrens. Detta är förbjudet enligt [Adobe Campaign policy för godtagbar användning](https://www.adobe.com/legal/terms/aup.html).

**Det kan vara praktiskt att**
samla in tävlingsadresser både vid ett eller flera officiella och tydligt märkta mässor. Risken är att många händelser, som detta, samlar in alla adresser och distribuerar dem via eventleverantören eller värden. Det innebär att ägarna av dessa e-postadresser aldrig har begärt e-post från ert varumärke. Dessa prenumeranter kommer troligen att klaga och markera din e-post som skräppost, och de kanske inte har angett korrekt kontaktinformation.

**Lotteriet**

Lotteriet erbjuder snabbt ett stort antal e-postadresser. Men de här prenumeranterna vill ha priset, inte era e-postmeddelanden. De kanske inte ens uppmärksammat namnet på dem som skulle komma att kontakta dem. De kommer troligen att klaga och markera din post som skräppost, och det är inte troligt att de någonsin kommer att genomföra något köp.

## Produktspecifika resurser

**Adobe Campaign Classic**

* [Skapa ett prenumerationsformulär med dubbel anmälan](https://experienceleague.adobe.com/docs/campaign-classic/using/designing-content/web-forms/use-cases—web-forms.html?lang=sv#create-a-subscription—form-with-double-opt-in)

**Adobe Campaign Standard**

* [Dubbel anmälningsprocess](https://experienceleague.adobe.com/docs/campaign-standard/using/communication-channels/landing-pages/setting-up-a-double-opt-in-process.html?lang=sv#communication-channels)
