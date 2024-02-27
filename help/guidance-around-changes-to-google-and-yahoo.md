---
title: Vägledning om de aviserade ändringarna på [!DNL Google] och [!DNL Yahoo]
description: Vägledning om de aviserade ändringarna på [!DNL Google] och [!DNL Yahoo]
role: Admin
level: Experienced
doc-type: Article
last-substantial-update: 2023-11-06T00:00:00Z
jira: KT-14320
thumbnail: KT-14320.jpeg
exl-id: 879e9124-3cfe-4d85-a7d1-64ceb914a460
source-git-commit: e2c2fbfee5e404e1eef25dd0068a6bdd560ed977
workflow-type: tm+mt
source-wordcount: '1770'
ht-degree: 0%

---

# Vägledning om de aviserade ändringarna på [!DNL Google] och [!DNL Yahoo]

3 oktober [!DNL Google] och [!DNL Yahoo] tillsammans presenterade planerade ändringar via sina bloggar. Dessa ändringar är utformade för att skydda sina användare och ge dem en bättre e-postupplevelse genom att följa några vanliga branschstandarder som nya krav från och med 1 februari 2024.

[https://blog.google/products/gmail/gmail-security-authentication-spam-protection/](https://blog.google/products/gmail/gmail-security-authentication-spam-protection/){target="_blank"}

![[!DNL Google] Meddelande_](/help/assets/Gmail.png)

[https://blog.postmaster.yahooinc.com/post/730172167494483968/more-secure-less-spam](https://blog.postmaster.yahooinc.com/post/730172167494483968/more-secure-less-spam){target="_blank"}

![[!DNL Yahoo] Meddelande](/help/assets/Yahoo.png)

E-postexperterna på Adobe har läst igenom dessa blogginlägg och all länkad dokumentation så att du inte behöver göra det. Här är de viktigaste lösningarna.

## Så vad exakt är [!DNL Google] och [!DNL Yahoo] gör du?

I e-postvärlden finns det juridiska krav, praktiska krav och allmänna bästa metoder. Juridiska krav varierar mycket mellan olika platser och är inte en del av detta ämne. Istället [!DNL Google] och [!DNL Yahoo] använder bästa praxis och omvandlar dem till praktiska krav.

Inget av objekten [!DNL Google] och [!DNL Yahoo] kommer att börja kräva nya erbjudanden i februari och har ofta varit rekommendationer om god praxis i åratal, men användningen har varit långsam och ojämn i branschen. Det här är [!DNL Google] och [!DNL Yahoo]Det är ett sätt att hjälpa till att utveckla den acceptansprocessen genom att säga&quot;Om du vill distribuera e-post till våra användare (detta kan utgöra en betydande del av din e-postlista, i vissa fall upp till 70 %, beroende på region och bransch) måste du göra det här&quot;.

## Vilka är detaljerna?

Om du är Adobe-kund är det mesta av det de behöver redan en del av din konfiguration, men det finns några nyckelobjekt som är tekniskt valfria, och du vill vara säker på att din organisation har implementerat och konfigurerat dessa objekt korrekt före februari 2024.

## DMARC:

[!DNL Google] och [!DNL Yahoo] kommer båda att kräva att du har en DMARC-post för alla domäner som du använder för att skicka e-post till dem. De kräver för närvarande INTE en inställning för p=avvisa eller p=karantän, så en inställning på p=none, som vanligtvis kallas &quot;övervakning&quot;, är helt acceptabel för tillfället. Detta ändrar inte hur dina e-postmeddelanden behandlas, de gör som de normalt skulle göra utan DMARC. Att konfigurera detta är första steget mot att skydda dig med DMARC, och utöver den nya fördelen med att hjälpa dig att skicka e-post till [!DNL Google] och [!DNL Yahoo] kan det också hjälpa dig att se om det finns autentiseringsproblem någonstans i e-postens ekosystem.

Reglerna för DMARC ändras inte, vilket innebär att om de inte har konfigurerats för att förhindra det ärvs en DMARC-post på den överordnade domänen (adobe.com som exempel) och täcker alla underdomäner (som email.adobe.com). Du behöver inte olika DMARC-poster för dina underdomäner, såvida du inte vill eller behöver lägga till dem av olika affärsskäl.

Konfigurering av DMARC TXT-poster stöds för närvarande helt i Adobe för Campaign och AJO, men är inte obligatoriskt. Använd en kostnadsfri DMARC-kontroll för att se om du har DMARC-inställningar för dina underdomäner, och om du inte gör det, prata med ditt supportteam på Adobe för att se hur du bäst får tag i den konfigurationen.

Du kan även hitta mer information om DMARC och hur du implementerar det [här](https://experienceleague.adobe.com/docs/deliverability-learn/deliverability-best-practice-guide/additional-resources/technotes/implement-dmarc.html?lang=sv){target="_blank"} for Adobe Campaign, [here](https://experienceleague.adobe.com/docs/journey-optimizer/using/reporting/deliverability/dmarc-record-update.html?lang=en){target="_blank"} for AJO, or [here](https://experienceleague.adobe.com/docs/marketo/using/getting-started-with-marketo/setup/configure-protocols-for-marketo.html){target="_blank"} för Marketo Engage.

## 1-klicka (lista) Avbeställ:

Bli inte panikslagen. [!DNL Google] och [!DNL Yahoo] talar inte om länkar för att avbryta prenumerationen i e-postmeddelandets brödtext eller sidfot som användaren kan klicka på av en säkerhetsrobot som bara gör sitt jobb eller av misstag. Vad de menar är rubrikfunktionen List-Unsubscribe för någon av versionerna&quot;mailto&quot; eller&quot;http/URI&quot;. Det här är funktionen i [!DNL Yahoo] och Gmail-gränssnitt där användarna kan klicka på Avbeställ. Gmail uppmanar till och med användare som klickar på&quot;Rapportera skräppost&quot; att se om de hade för avsikt att avbeställa prenumerationen istället, vilket kan minska antalet klagomål du får (klagomål som skadar ditt rykte) genom att omvandla dem till att avbeställa prenumerationer istället (vilket inte skadar ditt rykte).

Det är viktigt att notera att [!DNL Google] och [!DNL Yahoo] båda hänvisar till alternativet &quot;http/URI&quot; med namnet &quot;1-Click&quot;, och detta är avsiktligt. Tekniskt sett kunde du med det ursprungliga alternativet &quot;http/URI&quot; dirigera om mottagare till en webbplats. Det är inte fokus för [!DNL Yahoo] och [!DNL Google], som båda refererar till den uppdaterade [RFC8058](https://datatracker.ietf.org/doc/html/rfc8058){target="_blank"} som fokuserar på att bearbeta avbeställningen via en HTTPS-POST istället för en webbplats, vilket innebär att den&quot;1-klickas&quot;.

Idag accepterar Gmail alternativet &quot;mailto&quot; list-unsubscribe. Gmail har sagt att&quot;mailto&quot; inte uppfyller deras förväntningar och avsändarna måste ha alternativet&quot;post&quot; list-unsubscribe aktiverat. Avsändare som redan har en lista över avbrutna prenumerationer av någon typ kommer fram till 1 juni 2024 att ha en lista över avbeställningar.

[!DNL Yahoo] har sagt att de kommer att fortsätta att respektera&quot;mailto&quot;-alternativet, för tillfället, men att de också kommer att behöva&quot;post&quot; i framtiden.

Adobe rekommenderar att du använder alternativen&quot;mailto&quot; och&quot;post/1-Click&quot; för att avbryta prenumerationen. Adobe arbetar med att aktivera&quot;postsupport&quot; på alla våra e-postsändningsplattformar för att ge stöd åt våra användare så att de uppfyller dessa krav. Se informationen nedan.

Behovet av rubriker för att avbryta prenumerationen på listor gäller inte transaktionsmeddelanden. Observera att utlösta meddelanden som övergiven kundvagn och liknande kommunikation som inte genererats av prenumeranten anses vara marknadsföring av postlådeleverantörer som [!DNL Google] och [!DNL Yahoo] och dessa behöver inte längre prenumerera.

[!DNL Google] och [!DNL Yahoo] är båda medvetna om att en mottagare i vissa fall kommer att avbryta prenumerationen och sedan återprenumerera vid ett senare datum. De är inte villiga att dela den hemliga såsen om hur de identifierar dessa situationer, men de arbetar med metoder för att undvika att straffa avsändare felaktigt i dessa fall.

>[!INFO]
> Adobe arbetar med att aktivera&quot;post&quot;-support på alla våra e-postsändningsplattformar för att ge stöd åt våra användare så att de uppfyller dessa krav:
>
> * [!DNL Adobe Campaign v7/v8]: Fullt stöd för POST 1-klickning idag, instruktioner finns [här](https://experienceleague.adobe.com/docs/deliverability-learn/deliverability-best-practice-guide/additional-resources/campaign/acc-technical-recommendations.html?lang=en#list-unsubscribe){target="_blank"}.
>* [!DNL Adobe Campaign Standard]: Från och med den 19 februari stöder POST 1-Click fullt ut. Mer information finns [här](https://experienceleague.adobe.com/docs/campaign-standard/using/administrating/configuring-channels/configuring-email-channel.html#email-channel-parameters){target="_blank"}.
>* [!DNL Adobe Journey Optimizer]: Stöder POST 1-klickning idag, men vissa viktiga förbättringar pågår och förväntas äga rum i mars 2024. Uppdateringar av dokumentationen publiceras [här](https://experienceleague.adobe.com/docs/journey-optimizer/using/email/email-opt-out.html?lang=en){target="_blank"} en gång.
> * [!DNL Marketo]: Från och med den 31 januari 2024 stöds POST 1-Click List-Unsubscribe. Ingen åtgärd krävs av användaren.

## Bearbeta Avbeställ inom 2 dagar:

Detta har varit en rekommenderad metod en stund, eftersom varje e-postmeddelande som du distribuerar till någon som avbeställer normalt leder till skräppost, så ju tidigare du slutar skicka e-post desto bättre. Även här kan juridiska krav vara mycket längre i vissa fall, men [!DNL Google] och [!DNL Yahoo] kommer att veta att användaren avbeställer prenumerationen via List-Unsubscribe och att du fortfarande skickar e-post på dag 3, och de har angett att de inte tillåter avsändare som gör det att fortsätta skicka e-post till NÅGON av sina användare.

Detta tvådagarskrav gäller för alla avbeställningar via olika metoder för att avsluta prenumerationen. I vissa fall (som&quot;mailto&quot;) innebär det att Adobe kommer att bearbeta dem. Adobe bearbetar alla avbeställningar omedelbart efter det att begäran mottagits, inom tvådagarsgränsen. I andra fall kanske du bearbetar dem. Om du bearbetar dessa förfrågningar kan du behöva göra ändringar i slutet för att den här 2-dagars tidslinjen ska uppfyllas.

## Klagomålen:

Det har länge varit bra att behålla låga klagomål under 0,2 procent. [!DNL Google] är att sätta stapeln högre till 0,3 % under en längre period, men har tydligt angett att det finns fördelar med att hålla den under 0,1 %. Här är detaljerna de delade:

* Tänk dig att hålla din skräppostfrekvens under 0,10 %.
* Undvik en skräppostfrekvens på 0,30 procent eller högre, särskilt under en längre tidsperiod.
* Om du behåller en låg skräppostfrekvens blir avsändarna mer överkänsliga för tillfälliga toppar i användarfeedback.
* På samma sätt kommer en hög skräppostfrekvens att leda till ökad skräppostklassificering. Det kan ta tid för förbättringar av skräpposthastigheten att reflektera positivt på skräppostklassificeringen.

[!DNL Yahoo] har uppgett att tröskelvärdet för deras klagomål också kommer att vara 0,30 %.

[!DNL Google] och [!DNL Yahoo]Målet är inte att straffa avsändare för en enda dålig dag eller ett misstag som orsakar en tillfällig topp i klagomål. I stället fokuserar de på avsändare som har höga klagomål under en längre period eller ett mönster av dåligt sändningsbeteende.

Om du behöver hjälp med att övervaka dina klagomål, eller vill ha hjälp med strategier för att minska antalet klagomål, kan du tala med Adobe Deliverability Consultant eller prata med ditt kontoteam om hur du lägger till en Deliverability Consultant om du inte redan har en sådan.

## Vilka tidslinjer tittar vi på?

Uppdateringar av tidslinjer har gjorts sedan det ursprungliga meddelandet i oktober. De senaste tidslinjerna ser ut så här:

[!DNL Gmail]:

Februari 2024 - Tillfälliga studsar avsedda att varna för bristande efterlevnad börjar. E-postmeddelanden kommer fortfarande att levereras som vanligt efter en kort fördröjning om du ännu inte uppfyller kraven. Om ni uppfyller alla krav kommer det inte att finnas några tillfälliga studsar och ni kommer inte att märka någonting.

April 2024 - Blocken börjar för avsändare som inte uppfyller allt utom List-Unsubscribe 1-Click. Endast en del av e-postmeddelandet som inte uppfyller kraven blockeras först, där procentandelen blockeras med tiden.

1 juni 2024 - Alla avsändare som inte uppfyller alla krav, inklusive List-Unsubscribe 1-Click, blockeras.

[!DNL Yahoo]:

Februari 2024 - Den successiva utrullningen av indrivningen för alla krav utom 1-klicksregistrering börjar i februari 2024.

Juni 2024 - 1-Click List-Unsubscribe enforcement will begin in juni 2024.

## Hur kommer det här att påverka mig som marknadsförare?

Gmail och [!DNL Yahoo] förväntas resultera i att e-postmeddelanden landar i skräppostmappen eller blockeras (d.v.s. att en studsa tillbaka från MBP som anger att e-postmeddelandet inte levererades).

Därför rekommenderar Adobe starkt att du går igenom de ändringar som beskrivs ovan och ser till att du börjar följa dem så snart som möjligt. Nu är det också ett bra tillfälle att testa prestandan på [!DNL Yahoo] och [!DNL Google] för att du ska kunna se om det finns några väsentliga förändringar i mätvärdena från februari.

Vi är här för att hjälpa dig, så om du har några frågor eller behöver support kan du prata med din Adobe-konsult eller prata med ditt kontoteam om hur du lägger till en slutproduktskonsult om du inte redan har en.

## Finns det sätt att kringgå det här?

Även om detta alltid är en fråga som kommer fram är verkligheten den att dessa ändringar är förnuftiga för slutanvändarna av [!DNL Google] och [!DNL Yahoo]på sina plattformar. De är motiverade av dessa användares förväntningar på att följa dessa regler. Vi rekommenderar inte att du försöker kringgå dessa ändringar, utan snarare att du tar ett steg tillbaka och tänker efter hur du ska hantera dessa ändringar.

## Slutlig anteckning:

Detta gäller för närvarande inte e-postmeddelanden som skickas till [!DNL Yahoo].JP eller [!DNL Gmail] Konton på arbetsytan gäller dock e-post som kommer från dessa platser.

## Ytterligare resurser (inte specifikt för dessa ändringar):

[Google Sender Guidelines](https://support.google.com/mail/answer/81126){target="_blank"}

[GOOGLE FAQ](https://support.google.com/a/answer/14229414?sjid=2864589551334481470-NC){target="_blank"}

[Yahoo Sender Guidelines](https://senders.yahooinc.com/best-practices/){target="_blank"}

[Vanliga frågor om Yahoo](https://senders.yahooinc.com/faqs/){target="_blank"}
