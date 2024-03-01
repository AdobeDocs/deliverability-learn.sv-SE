---
source-git-commit: d105a5b7d81aa14144b9d01f28a5e24c1110ae6c
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 4%

---
# Skapar en typologiregel som stöder ett klick för att avbryta prenumeration:

**1. Skapa den nya typologiregeln:**

* Klicka på &quot;ny&quot; i navigeringsträdet för att skapa en ny typ

![bild](/help/assets/CreatingTypologyRules1.png)

**2. Fortsätt med att konfigurera typologiregeln:**

* Regeltyp: Kontroll
* Fas: Vid början av målinriktningen
* Kanal: E-post
* Nivå: Ditt val
* Aktiv


![bild](/help/assets/CreatingTypologyRules2.png)


**Koda javascript-koden för typologiregeln:**


>[!NOTE]
>
>Koden som beskrivs nedan ska endast refereras som exempel.
>I det här exemplet beskrivs hur du:
>* Konfigurera en URL List-Unsubscribe och lägger till rubrikerna eller lägger till den befintliga mailto-parametern och ersätter den med: &lt;mailto..>>, https://..
>* Lägg till i sidhuvudet List-Unsubscribe-Post
>I exemplet med post-URL används var headerUnsubUrl = &quot;https://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=&lt;%= mottagare.cryptedId %>&quot; max
>* Du kan lägga till andra parametrar (som &amp;service = ...)
>


```
// Function to add or replace a header in the provided headers 
function addHeader(headers, header, value)  { 
    
  // Create the new header line 
  var headerLine = header + ": " + value; 
    
  // Create a regular expression to find the specified header 
  var regExp = new RegExp(header + ":(.*)$", "i") 
    
  // Split the headers into individual lines 
  var headerLines = headers.split("\n"); 
    
  // Loop through each line 
  for (var i=0; i < headerLines.length; i++) { 
      
    // Check if the specified header exists 
    var match = headerLines[i].match(regExp) 
      
    // If it exists 
    if ( match != null ) { 
        
      // Replace the existing header line 
      headerLines[i] = headerLine; 
        
      // Return the modified headers 
      return headerLines.join("\n"); 
    } 
  } 
    
  // If the header does not exist, add the new header line 
  headerLines.push(headerLine); 
    
  // Return the modified headers 
  return headerLines.join("\n"); 
} 
  
// Function to get the value of a specified header from the provided headers 
function getHeader(headers, header) { 
    
  // Create a regular expression to find the specified header 
  var regExp = new RegExp(header + ":(.*)$", "i") 
    
  // Split the headers into individual lines 
  var headerLines = headers.split("\n"); 
    
  // Loop each line 
  for each (line in headerLines) { 
      
    // Check if the specified header exists 
    var match = line.match(regExp); 
      
    // If it exists 
    if ( match != null ) { 
        
      // Return the header value, removing leading whitespace 
      return match[1].replace(/^\s*/, ""); 
    } 
  } 
    
  // If the header does not exist, return an empty string 
  return ""; 
} 
  
  
// Define the unsubscribe URL 
var headerUnsubUrl = "https://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=<%= recipient.cryptedId %>"; 
  
// Get the value of the List-Unsubscribe header 
var headerUnsub = getHeader(delivery.mailParameters.headers, "List-Unsubscribe"); 
  
// If the List-Unsubscribe header does not exist 
if ( headerUnsub === "" ) { 
  // Add the List-Unsubscribe header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe", "<"+headerUnsubUrl+">"); 
} 
// If the List-Unsubscribe header exists and contains 'mailto' 
else if(headerUnsub.search('mailto')){ 
  // Replace the existing List-Unsubscribe header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe", "<"+headerUnsubUrl+">"); 
} 
  
// Get the value of the List-Unsubscribe-Post header 
var headerUnsubPost = getHeader(delivery.mailParameters.headers, "List-Unsubscribe-Post"); 
  
// If the List-Unsubscribe-Post header does not exist 
if ( headerUnsubPost === "" ) { 
  // Add the List-Unsubscribe-Post header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe-Post", "List-Unsubscribe=One-Click"); 
} 
  
// Return true to indicate success 
return true; 
```


![bild](/help/assets/CreatingTypologyRules3.png)

**3. Lägg till din nya regel i en typologi i ett e-postmeddelande (standardtypologin är OK):**

![bild](/help/assets/CreatingTypologyRules4.png)

**4. Förbered en ny leverans (kontrollera att ytterligare SMTP-huvuden i leveransegenskapen är tomma)**

![bild](/help/assets/CreatingTypologyRules5.png)

**5. Kontrollera under leveransförberedelsen att din nya typologiregel används.**

![bild](/help/assets/CreatingTypologyRules6.png)



**6. Verifiera att List-Unsubscribe finns.**

![bild](/help/assets/CreatingTypologyRules7.png)
