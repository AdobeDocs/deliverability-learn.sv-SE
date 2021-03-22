---
title: Allt om skräppostsvällning
description: Lär dig hur du förstår, identifierar och undviker skräppostsvällningar när du hanterar leveranser.
feature: Ytterligare resurser
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 3696ec013014ea41c634ac4829ec40977d224ff1
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---


# Allt om skräppostsvällning

En [skräppostsvällning](/help/metrics/spam-traps.md) är en giltig adress, utan felmeddelande när e-postmeddelanden skickas till. En skräppostfälla har huvudsyftet: identifiera skräppost eller avsändare utan datahygienprocess.

## Vem hanterar de här skräppostfällorna?

Den första typen av skräppostsvällningsadresser är IP- och Domain blockeringslista-företag som SpamHaus, Sorbs och SpamCop. Dessa företag har ett stort nätverk av adresser som skickas på olika Internet-sidor som webbplatser, bloggar och forum så att deras adresser samlas in av skräppost.

Den andra typen av skräppostsvällning baseras på gamla aktiva ISP-adresser. Dessa Internet-leverantörer har ett eget nätverk för skräppostsvällning som bygger på inaktiva adresser som konverteras i svällning och varje träff påverkar avsändarens IP-adress och domänens anseende.

## Hur fungerar det?

**En e-postadress utan slutanvändare**: Dessa adresser har inte och kommer aldrig att ha någon slutanvändare som kan registrera sig för nyhetsbrev eller andra typer av kommunikation.

**En e-postadress som överges av en användare**: Efter en tids inaktivitet inaktiveras adresser av Internet-leverantörer. Studentmeddelanden skickas till avsändare för att informera dem om den nya statusen. Avsändare måste skicka in dessa adresser i karantän eller ta bort dem från framtida kommunikation. Internetleverantörer använder dessa adresser som har omvandlats till&quot;skräppostfällor&quot; för att övervaka avsändare med dåliga rutiner.

## Hur känner du igen eller identifierar en skräppostfälla?

Det är svårt att identifiera skräppostfällor. Adresserna måste vara anonyma eftersom de används för att identifiera dåliga avsändare. De flesta internetleverantörer har inte något öppet system och klickar på systemet för att övervaka felaktiga avsändarträffar. Enligt tidigare definitioner är det möjligt att fastställa ett pod med misstänkta adresser och testa effektiviteten i podvalet.

## Varför din databas är infekterad av skräppostsvällningar?

Din databas för e-postadresser innehåller svällning för skräppost, hur skulle det kunna vara möjligt? De två främsta anledningarna till detta är att databasens hygienprocess saknas eller att nedsatt funktion samlas in.

Här följer några punkter som hjälper dig att kontrollera dina processer:

* Samla in funktionsstörningar:
   * Varifrån kommer dina e-postadresser? Hur många källor används för att samla in adresserna? Kan du identifiera dem? Intern/koregistrering?
   * Fungerar anmälningssystemet som det ska?
   * Kontrollerade du domäner och alias för dina adresser? Gör det med tabellen nedan!
* Databashygienprocess:
   * Hur ser du på inaktiv adress de senaste 12 månaderna?
   * Bearbetar du en karantän på mjuka studsar som&quot;inaktiv användare&quot;?
   * När tog du hand om din databas och försökte rensa den senast? Gör det regelbundet.

## Alias och domäner som ska undvikas

**Alias**

![](../../help/assets/aliases.png)

**Domäner**

![](../../help/assets/domains.png)