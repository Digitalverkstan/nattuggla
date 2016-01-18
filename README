# Nattuggla

Ett enkelt projekt där en Arduino i form av ett djur reagerar på att omgivningens ljus slocknar genom att lysa med ögonen. Ett bra första projekt med lite kod, lite elektronik och lite detektivarbete.

### Använder:

 - Arduino, programmering
 - LED & Resistor
 - LDR (Light Dependent Resistor, fotoresistor)
 - Prototyping

### Beskrivning

Ugglor är vakna på natten, och när en uggla är vaken lyser ögonen. Med hjälp av en arduino och lite elektronikkomponenter kan du få din uggla att lysa med ögonen när det blir mörkt.

### Bygg, del 1

Bygg ihop kretsen enligt schemat. Kom ihåg vilka portar (hål) på arduinon som används, och var noga med de sladdar som går till 5V och GND (jord). Det finns två resistorer i kretsen, en på ca 220 ohm för lysdioderna och en på 10k ohm som tillhör fotoresistorn. Var noga med att använda rätt resistor på rätt ställe, 10k ohm är 50x så starkt som 220 ohm. På ritningen ser resistorerna lika stora ut, men den starka är mycket mindre än den svaga i verkligheten.

Starta en ny sketch i Arduino, skapa två variabler av typen `int` som pekar på portarna för LEDs och fotoresistorn.

    int led = 5; // led betyder nu 5
    int ldr = 0;

Se även till att portarna startas i rätt läge, börja med `led`, detta gör du i `setup()`-metoden:

    void setup() {
        pinMode(led, OUTPUT);
    }

Testa att lysdioderna fungerar genom att slå på dem i `loop()`-metoden

    void loop() {
        digitalWrite(led, HIGH);
    }

Skicka till Arduinon och Lysdioderna bör nu lysa.

### Detektivarbete

Nu ska vi ta reda på hur arduinon tolkar ljusnivåerna. Fotoresistorn kan skicka en siffra mellan 0 och 1023 till analog-porten, men vi vet inte om ett högre tal betyder starkare eller svagare ljus, och dessutom vill vi ta reda på vad värdet är när det börjar bli mörkt i rummet.

Arduinon kan hjälpa oss genom att tala om vilket tal den får, om och om igen flera gånger per sekund. Det gör den genom att skriva till serieporten (Serial) som i själva verket är USB-kabeln som är kopplad till datorn.

Börja med att lägga till följande kod i `setup()`-metoden:

    void setup() {
        pinMode(led, OUTPUT);
        Serial.begin(9600);
    }

Det betyder att arduinon ska börja använda serieporten, och med vilken hastighet den ska läsa och skriva data. Det liknar lite att man ringer någon med telefonen, men inte börjar prata än.

Sedan ser vi till att arduinon hela tiden skickar talet som vi läser in från analog-porten. Först hämtar vi värdet och sparar i en variabel, sedan skickar vi variabeln till serieporten.

    void loop() {
        digitalWrite(led, HIGH);
        int value = analogRead(ldr); // value betyder värde
        Serial.println(value);
    }

Skicka programmet till arduinon och öppna det som heter "Serial Monitor" (knappen till höger ovanför koden i arduino-fönstret). Nu ska en stor vit ruta dyka upp med en massa siffror som rullar. Siffrorna är det som arduinon får från fotoresistorn.

Prova att skugga fotoresistorn med handen och se om siffrorna ändrar sig. Om det hela tiden står samma sak (exempelvis 0 eller 1023) så är någonting fel med kretsen, kolla alla sladdar och komponenter! Normalt sett borde värdet minska eller öka med ett par hundratal, exempelvis från 300 till 700 eller tvärtom när man ökar eller minskar ljuset. Skriv ner ungefär vad det blir när det är ljust, och vad det blir när det blir mörkt.

### Ändra i koden

Nattugglor är vakna på natten, och när de är vakna lyser deras ögon. Det är mörkt på natten, alltså ska ögonen lysa när det är mörkt och vara släckta när det är ljust. Därför ska arduinon tända eller släcka lysdioderna beroende på om fotoresistorn tycker att det är mörkt eller inte.

Du har säkert sett if-block i Scratch tidigare, och det finns likadana i arduino-språket också, men man får skriva texten själv. De ser ut ungefär såhär:

    if(<något som kan vara true eller false>) {
        // Kod som körs om det var true
    } else {
        // Kod som körs om det var false
    }

Den sista biten (else med tillhörande kod) behöver man inte ha med, men det är ganska praktiskt. Om vi tänker efter så ska vi ju göra en sak om ett påstående stämmer, och en annan sak om samma påstående inte stämmer, påståendet är "Det är mörkt", och om det stämmer så tänder vi lysdioderna, annars släcker vi dem. Vi gör om det till kod i flera steg, kommer du ihåg hur man tänder och släcker lysdioder?

    Om "Det är mörkt":
        Tänd lysdioder
    Annars:
        Släck lysdioder

    ==>

    if("Det är mörkt") {
        digitalWrite(led, HIGH);
    } else {
        digitalWrite(led, LOW); // LOW släcker dioderna
    }

Den här koden fungerar nästan, arduinon förstår inte vad "Det är mörkt" betyder. Det är dags att ta fram det du skrev ner under detektivarbetet. Titta på siffrorna och se om siffran för mörker är större eller mindre än den för ljuset, försök även hitta siffran mitt emellan - det borde vara en bra gräns mellan ljust och mörkt. Om det är mörkt borde värdet vi får från fotoresistorn vara mindre (eller större) än gränsen, och om det är ljust så borde det vara tvärtom.

if-blocket kan jämföra två siffror och säga om det är sant eller falskt att den ena siffran är större än den andra, exempelvis:

    2 < 3 // två är mindre än tre, sant.
    4 < 3 // fyra är mindre än tre, falskt.

Och nu är det bara att smälta ihop vad vi vet om fotoresistorns siffror och hur if-blocket kan jämföra mellan värden, stoppa sedan in det istället för "Det är mörkt" i koden ovan.

    int value = analogRead(ldr);

    if(value < 600) {
        digitalWrite(led, HIGH);
    } else {
        digitalWrite(led, LOW); // LOW släcker dioderna
    }

Alltså, hämta siffran från fotoresistorn, och jämför med gränsen mellan mörkt och ljust. I mitt fall när jag testade så var 800 ljust och 200 mörkt, alltså borde det vara mörkt under 600.
