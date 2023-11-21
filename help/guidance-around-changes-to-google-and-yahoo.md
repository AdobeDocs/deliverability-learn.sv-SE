---
title: Vägledning om de aviserade ändringarna på [!DNL Google] och [!DNL Yahoo]
description: Vägledning om de aviserade ändringarna på [!DNL Google] och [!DNL Yahoo]
role: Admin
level: Experienced
doc-type: Article
last-substantial-update: 2023-11-06T00:00:00Z
jira: KT-14320
thumbnail: KT-14320.jpeg
source-git-commit: ce0ecaa7f62e8ba0bbf44dc180908b81475a225e
workflow-type: tm+mt
source-wordcount: '1312'
ht-degree: 0%

---


# Vägledning om de aviserade ändringarna på [!DNL Google] och [!DNL Yahoo]

3 oktober [!DNL Google] och [!DNL Yahoo] tillsammans presenterade planerade ändringar via sina bloggar. Dessa ändringar är utformade för att göra användarna säkrare och ge en bättre e-postupplevelse genom att följa några vanliga branschstandarder som nya krav från och med februari 2024.

[https://blog.google/products/gmail/gmail-security-authentication-spam-protection/](https://blog.google/products/gmail/gmail-security-authentication-spam-protection/){target="_blank"}

![[!DNL Google] Meddelande_](/help/assets/Gmail.png)

[https://blog.postmaster.yahooinc.com/post/730172167494483968/more-secure-less-spam](https://blog.postmaster.yahooinc.com/post/730172167494483968/more-secure-less-spam){target="_blank"}

![[!DNL Yahoo] Meddelande](/help/assets/Yahoo.png)

E-postexperterna på Adobe har läst igenom dessa blogginlägg och all länkad dokumentation så att du inte behöver göra det. Här är de viktigaste lösningarna.

## Så vad exakt är [!DNL Google] och [!DNL Yahoo] gör du?

I e-postvärlden finns det juridiska krav, praktiska krav och allmänna bästa metoder. Juridiska krav varierar mycket mellan olika platser och är inte en del av detta ämne. Istället [!DNL Google] och [!DNL Yahoo] använder bästa praxis och omvandlar dem till praktiska krav. Inget av objekten [!DNL Google] och [!DNL Yahoo] kommer att börja kräva nya erbjudanden i februari och har ofta varit rekommendationer om god praxis i åratal, men användningen har varit långsam och ojämn i branschen. Det här är [!DNL Google] och [!DNL Yahoo]Det är ett sätt att hjälpa till att utveckla den acceptansprocessen genom att säga&quot;Om du vill distribuera e-post till våra användare (detta kan utgöra en betydande del av din e-postlista, i vissa fall upp till 70 %, beroende på region och bransch) måste du göra det här&quot;.

## Vilka är detaljerna?

Om du är Adobe-kund är det mesta av det de behöver redan en del av din konfiguration, men det finns några nyckelobjekt som är tekniskt valfria, och du vill vara säker på att din organisation har implementerat och konfigurerat dessa objekt korrekt före februari 2024.

## DMARC:

[!DNL Google] och [!DNL Yahoo] kommer båda att kräva att du har en DMARC-post för alla domäner som du använder för att skicka e-post till dem. De kräver för närvarande INTE en inställning för p=avvisa eller p=karantän, så en inställning på p=none, som vanligtvis kallas &quot;övervakning&quot;, är helt godtagbar. Detta ändrar inte hur dina e-postmeddelanden behandlas, de gör som de normalt skulle göra utan DMARC. Att konfigurera detta är första steget mot att skydda dig med DMARC, och utöver den nya fördelen med att hjälpa dig att skicka e-post till [!DNL Google] och [!DNL Yahoo] kan det också hjälpa dig att se om det finns autentiseringsproblem någonstans i e-postens ekosystem.
DMARC stöds fullt ut i Adobe, men är inte obligatoriskt. Använd en kostnadsfri DMARC-kontroll för att se om du har DMARC-inställningar för dina underdomäner, och om du inte gör det, prata med ditt supportteam på Adobe för att se hur du bäst får tag i den konfigurationen.

Du kan även hitta mer information om DMARC och hur du implementerar det [här](https://experienceleague.adobe.com/docs/deliverability-learn/deliverability-best-practice-guide/additional-resources/technotes/implement-dmarc.html?lang=sv){target="_blank"} for Adobe Campaign and Adobe Journey Optimizer Adobe or [here](https://experienceleague.adobe.com/docs/marketo/using/getting-started-with-marketo/setup/configure-protocols-for-marketo.html){target="_blank"} för Marketo Engage.

## 1-klicka (lista) Avbeställ:

Bli inte panikslagen. [!DNL Google] och [!DNL Yahoo] talar inte om länkar för att avbryta prenumerationen i e-postmeddelandets brödtext eller sidfot som användaren kan klicka på av en säkerhetsrobot som bara gör sitt jobb eller av misstag. De är rubrikfunktionen List-Unsubscribe för någon av versionerna&quot;mailto&quot; eller&quot;http/URL&quot;. Det här är funktionen i [!DNL Yahoo] och Gmail-gränssnitt där användarna kan klicka på Avbeställ. Gmail uppmanar till och med användare som klickar på&quot;Rapportera skräppost&quot; att se om de hade för avsikt att avbeställa prenumerationen istället, vilket kan minska antalet klagomål du får (klagomål som skadar ditt rykte) genom att omvandla dem till att avbeställa prenumerationer istället (vilket inte skadar ditt rykte).
Det är viktigt att notera att [!DNL Google] och [!DNL Yahoo] båda syftar på&quot;http/URL&quot;-alternativet med namnet&quot;1-Click&quot;, vilket är avsiktligt. Med det ursprungliga alternativet&quot;http/URL&quot; kunde du tekniskt omdirigera mottagare till en webbplats. Det är inte fokus för [!DNL Yahoo] och [!DNL Google], som båda hänvisar till den uppdaterade RFC8058 som fokuserar på att bearbeta avbeställningen via en HTTPS-POST i stället för en webbplats, vilket gör den till&quot;1-klickning&quot;.
För Marketo Engage har Adobe redan aktiverat alternativet&quot;mailto&quot; och stöder för närvarande inte alternativet&quot;http/URL&quot;. Ytterligare uppdateringar om detta kommer att ske.
För Adobe Campaign och Adobe Journey Optimizer Adobe rekommenderar vi att du använder både&quot;mailto&quot; och&quot;1-Click&quot;.

Om du vill ha mer information om hur du implementerar en lista för att avbryta prenumerationen kan du kontrollera [här](https://experienceleague.adobe.com/docs/deliverability-learn/deliverability-best-practice-guide/additional-resources/campaign/acc-technical-recommendations.html?lang=en#list-unsubscribe){target="_blank"} for **[!DNL Adobe Campaign Classic]**, [here](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-14778.html?lang=en){target="_blank"}, or **[!DNL Adobe Campaign Standard]**, and [here](https://experienceleague.adobe.com/docs/journey-optimizer/using/email/email-opt-out.html?lang=en){target="_blank"} for **[!DNL Adobe Journey Optimizer]** eller kontakta Adobe kundsupport när som helst.

Behovet av rubriker för att avbryta prenumerationen på listor gäller inte transaktionsmeddelanden. Observera att utlösta meddelanden som övergiven kundvagn och liknande kommunikation som inte genererats av prenumeranten anses vara marknadsföring av postlådeleverantörer som [!DNL Google] och [!DNL Yahoo] och dessa behöver inte längre prenumerera.

## Bearbeta Avbeställ inom 2 dagar:

Detta har varit en rekommenderad metod en stund, eftersom varje e-postmeddelande som du distribuerar till någon som avbeställer normalt leder till skräppost, så ju tidigare du slutar skicka e-post desto bättre. Även här kan juridiska krav vara mycket längre i vissa fall, men [!DNL Google] och [!DNL Yahoo] kommer att veta att användaren avbeställer prenumerationen via List-Unsubscribe och att du fortfarande skickar e-post på dag 3, och de har angett att de inte tillåter avsändare som gör det att fortsätta skicka e-post till NÅGON av sina användare.
Detta tvådagarskrav gäller för alla avbeställningar via olika metoder för att avsluta prenumerationen. I vissa fall (som&quot;mailto&quot;) innebär det att Adobe kommer att bearbeta dem. Adobe bearbetar alla avbeställningar omedelbart efter det att begäran mottagits, inom tvådagarsgränsen. I andra fall kanske du bearbetar dem. Om du bearbetar dessa förfrågningar kan du behöva göra ändringar i slutet för att den här 2-dagars tidslinjen ska uppfyllas.

## Klagomålen:

Det har länge varit bra att behålla låga klagomål under 0,2 procent. [!DNL Google] är att sätta stapeln högre till 0,3 % under en längre period, men har tydligt angett att det finns fördelar med att hålla den under 0,1 %. Här är detaljerna de delade:

* Tänk dig att hålla din skräppostfrekvens under 0,10 %.
* Undvik en skräppostfrekvens på 0,30 procent eller högre, särskilt under en längre tidsperiod.
* Om du behåller en låg skräppostfrekvens blir avsändarna mer överkänsliga för tillfälliga toppar i användarfeedback.
* På samma sätt kommer en hög skräppostfrekvens att leda till ökad skräppostklassificering. Det kan ta tid för förbättringar av skräpposthastigheten att reflektera positivt på skräppostklassificeringen.
  [!DNL Yahoo] har uppgett att tröskelvärdet för deras klagomål också kommer att ligga i intervallet 0,30 %.

Om du behöver hjälp med att övervaka dina klagomål, eller vill ha hjälp med strategier för att minska antalet klagomål, kan du tala med Adobe Deliverability Consultant eller prata med ditt kontoteam om hur du lägger till en Deliverability Consultant om du inte redan har en sådan.

## Hur kommer det här att påverka mig som marknadsförare?

Gmail och [!DNL Yahoo] förväntas resultera i att e-postmeddelanden landar i skräppostmappen eller blockeras (d.v.s. att en studsa tillbaka från MBP som anger att e-postmeddelandet inte levererades).
Därför rekommenderar Adobe starkt att du går igenom de ändringar som beskrivs ovan och ser till att du börjar följa dem så snart som möjligt. Nu är det också ett bra tillfälle att testa prestandan på [!DNL Yahoo] och [!DNL Google] för att du ska kunna se om det finns några väsentliga förändringar i mätvärdena från februari.
Vi är här för att hjälpa dig, så om du har några frågor eller behöver support kan du prata med din Adobe-konsult eller prata med ditt kontoteam om hur du lägger till en slutproduktskonsult om du inte redan har en.

## Finns det sätt att kringgå det här?

Även om detta alltid är en fråga som kommer fram är verkligheten den att dessa ändringar är förnuftiga för slutanvändarna av [!DNL Google] och [!DNL Yahoo]på sina plattformar. De är motiverade av dessa användares förväntningar på att följa dessa regler. Vi rekommenderar inte att du försöker kringgå dessa ändringar, utan snarare att du tar ett steg tillbaka och tänker efter hur du ska hantera dessa ändringar.
