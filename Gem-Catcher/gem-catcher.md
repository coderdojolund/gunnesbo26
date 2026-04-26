# Gem Catcher ⭐⭐

![image](image0.png)

## Innehåll
[Första koden](#första-koden) &bull;
[Figuren (Emoji)](#figuren-emoji) &bull;
[Rörelse](#rörelse) &bull;
[Lägga till en ädelsten](#lägga-till-en-ädelsten) &bull;
[Kollision och random()](#kollision-och-random) &bull;
[Räkna poäng](#räkna-poäng) &bull;
[Mus och händelser](#mus-och-händelser) &bull;
[Game Over](#game-over) &bull;
[Uppgifter och utmaningar](#uppgifter-och-utmaningar)

## Jobba så här
Vägen till ett färdigt projekt är en pusselbit i taget 🧩. Gör därför ett avsnitt i taget uppifrån och ner.
- Få det att fungera innan ni går vidare till nästa avsnitt. 
- Testkör ofta, efter varje avsnitt eller ännu oftare.

Var det något i beskrivningen som var svårt att förstå? Ta med det i redovisningen. 📝

# Första koden

✏️ Gå till Processing, https://editor.p5js.org 
>Du ska inte logga in på p5.js. Spara istället din kod i ett dokument på Google Drive mellan lektionerna.

I Processing använder vi två huvudfunktioner: 
- `setup()` körs en gång i början
- `draw()` körs hela tiden, precis som en film

✏️ Skriv in den här koden:
```javascript
function setup() {
  createCanvas(600, 450);
}

function draw() {
  background(80, 0, 70); // En mörklila bakgrund
}
```

`createCanvas(600, 450)` sätter storleken på spelfönstret. `background()` fyller hela skärmen med färg varje bildruta så att vi inte får "spår" efter våra figurer.

Provkör koden. 

Det som står inom parenteser, `(80, 0, 70)`, kallas för en RGB-kod. Det är tre värden som talar om vilken färgblandning vi vill ha. Det första talet (80) är för färgen rött, det andra (0) för grönt och det tredje (70) är för blått. Det största värdet man kan ha för någon färgkomponent är 255 och det minsta värdet är 0. 

✏️ Uppdatera koden, testa olika färgvärden och se vad du får!

>**Frivilligt:**
>I Chrome kan du öppna en färgväljare genom att trycka tangentkombinationen ctrl+shift+I och sedan klicka på en av de små kvadratikonerna som du kan se i fliken Styles. Se bilden!

![image](https://user-images.githubusercontent.com/4598641/223195080-7bda1b3d-89e9-479a-bebe-936a332b1408.png)


# Figuren (Emoji)
Det var inte så spännande, så vi lägger till ett rymdskepp.

Istället för en bildfil använder vi rymdskepps-emojin: 🚀.

✏️ Vi skapar variabler för skeppets position högst upp i koden. Uppdatera koden så att den blir så här:

```javascript
let shipX = 300;
let shipY = 400;

function setup() {
  createCanvas(600, 450);
}

function draw() {
  background(80, 0, 70);
  
  // Rita skeppet
  textAlign(CENTER, CENTER);
  textSize(50);
  text("🚀", shipX, shipY);
}
```

Vad händer här?
- `let shipX = 300` Skapar en variabel för skeppets (horisontella) X-koordinat.
- `textSize(50)` Bestämmer hur stor emojin ska vara.
- `text("🚀", shipX, shipY)` Ritar ut emojin på skärmen.

Så här ser koordinatsystemet ut i p5.js med WIDTH = 800 och HEIGHT = 600.
>I vårt fall kör vi med WIDTH = 600 och HEIGHT = 450 men du kan testa att ändra värdena. Du kan behöva justera storleken på spelfönstret (Output) uppe till höger i repl.it.

![image](https://user-images.githubusercontent.com/4598641/222978372-85be8781-e6fe-414e-9fb1-b7847dfc8b5f.png)

✏️ Testkör! Visas rymdskeppet? Du kan behöva göra ditt spelfönster (Output) större för att se rymdskeppet längst ner.

# Rörelse
Vi vill kunna styra skeppet med piltangenterna. I p5.js kollar vi detta inuti `draw()`-funktionen med `keyIsDown()`.

✏️ Lägg till rörelselogiken i `draw()` så att koden ser ut så här:

```javascript
function draw() {
  background(80, 0, 70);

  // Styrning
  if (keyIsDown(LEFT_ARROW)) {
    shipX -= 5;
  }
  if (keyIsDown(RIGHT_ARROW)) {
    shipX += 5;
  }

  // Rita skeppet
  textAlign(CENTER, CENTER);
  textSize(50);
  text("🚀", shipX, shipY);
}
```

- `-=` betyder "minska med"
- `+=` betyder "öka med"


✏️ Uppdatera koden och testa programmet med startknappen. Funkar koden som du vill när du trycker på vänster- och högerpil?
>Kom ihåg att **klicka i spelfönstret** så att din kod kan ta emot tangenttryckningarna.


# Lägga till en ädelsten

Nu behöver vi något att fånga! Vi använder en diamant-emoji: 💎.

✏️ Skapa variabler för ädelstenen högst upp:

```javascript
let gemX = 300;
let gemY = 0;
```

✏️ Uppdatera `draw()` så att ädelstenen faller och ritas ut:

```javascript
function draw() {
  background(80, 0, 70);

  // ... (skeppets kod här) ...

  // Ädelstenens logik
  gemY += 4; // Falla neråt
  
  if (gemY > height) { // underkanten
    gemY = 0; // högst upp
    gemX = random(20, width - 20);
  }

  // Rita ädelstenen
  textSize(40);
  text("💎", gemX, gemY);
}
```

- `random(20, width - 20)` gör att diamanten dyker upp på ett nytt ställe varje gång den börjar om från toppen.
- `gemY > height`: Vi vill att ädelstenen ska hoppa till toppen av fönstret när den nått underkanten.
För att göra det, sätter vi y-positionen till 0 (topp) när den är större än HEIGHT (underkanten).
Kolla koordinatsystemet högre upp på sidan. Där ser du att y-axeln är upp- och nervänd mot hur den brukar ritas. Större y-värden är längre ner på skärmen.


# Kollision och `random()`

## Känna av en kollision

Vi vill också att ädelstenen ska flytta högst upp om den rör rymdskeppet. Om du har använt Scratch tidigare, kanske du har använt blocket "rör vid sprajt".

I p5.js kan vi använda funktionen `dist()` för att se om avståndet mellan skeppet och diamanten är tillräckligt litet för en "fångst".

✏️ Skapa en variabel `let score = 0;` längst upp.

✏️ Lägg sedan till detta inuti `draw()`:

```javascript
let d = dist(shipX, shipY, gemX, gemY);

if (d < 40) { // Om avståndet är mindre än 40 pixlar
  gemY = 0;
  gemX = random(20, width - 20);
  score += 1;
}
```

✏️ Testkör! Går det att fånga ädelstenen med rymdskeppet?

- `random()`: 
Det är inte så spännande av låta ädelstenen falla från samma ställe varje gång.
Funktionen `random` ger oss slumptal, ungefär som att kasta tärning.
Här ska vi använda den för att slumpa fram ädelstenens x-position. Vi vill att den ska vara minst 20 och högst bredden&ndash;20

# Räkna poäng

Vi har en poängräknare men den syns inte.

✏️ För att skriva ut poängen på skärmen använder vi `text()` igen. Lägg till detta i `draw()`:

```javascript
fill(255); // Vit färg på texten
textSize(24);
textAlign(LEFT);
text("Score: " + score, 20, 30);
```


# Mus och händelser

## Musstyrning
Om du vill använda musen istället för tangentbordet är det väldigt enkelt i p5.js.

✏️ Byt ut din piltangent-kod mot denna rad:

```javascript
shipX = mouseX;
```

`mouseX` är en inbyggd variabel som alltid vet var din muspekare är.

Testkör!

# Game Over
Vi lägger till en variabel för att hålla koll på om spelet är slut.

✏️ Längst upp:

```javascript
let gameOver = false;
```

✏️ I `draw()` får du ändra koden så att spelet stannar om diamanten nuddar botten:

```javascript
if (gemY > height) {
  gameOver = true;
}

if (gameOver) {
  background(0);
  fill(255);
  textAlign(CENTER);
  textSize(60);
  text("GAME OVER", width/2, height/2);
  textSize(30);
  text("Final Score: " + score, width/2, height/2 + 50);
  noLoop(); // Stoppar draw-loopen
}
```

✏️ Testkör nu! Blir det Game Over som väntat?

Till slut kan din kod se ut så här:
```javascript
let shipX = 300;
let shipY = 400;
let gemX = 300;
let gemY = 0;
let score = 0;
let gameOver = false;

function setup() {
  createCanvas(600, 450);
}

function draw() {
  if (gameOver) {
    showGameOver();
    return;
  }

  background(80, 0, 70);

  // Styrning (Tangentbord)
  if (keyIsDown(LEFT_ARROW)) shipX -= 5;
  if (keyIsDown(RIGHT_ARROW)) shipX += 5;

  // Ädelsten faller (Blir snabbare med poäng)
  gemY += 4 + score / 5;

  // Missade diamanten?
  if (gemY > height) {
    gameOver = true;
  }

  // Kollision?
  if (dist(shipX, shipY, gemX, gemY) < 40) {
    gemY = 0;
    gemX = random(20, width - 20);
    score++;
  }

  // Rita figurer
  textAlign(CENTER, CENTER);
  textSize(50);
  text("🚀", shipX, shipY);
  textSize(40);
  text("💎", gemX, gemY);

  // Poäng
  textAlign(LEFT);
  fill(255);
  textSize(24);
  text("Score: " + score, 20, 40);
}

function showGameOver() {
  background(0);
  fill(255);
  textAlign(CENTER);
  textSize(50);
  text("GAME OVER", width / 2, height / 2);
  textSize(30);
  text("Final Score: " + score, width / 2, height / 2 + 60);
}
```

# Uppgifter och utmaningar

## Uppgift 1. Utvärdera ert eget arbete!
När ni svarar på detta, tänk er att *ni har tillgång till uppgiften* &ndash; ni behöver alltså inte kunna koden utantill.

**1A.** De här delarna av uppgiften har vi gjort. Vi förstår dem och kan förklara koden för Susanne eller inför klassen.

**1B.** De här delarna av uppgiften har vi gjort *men vi förstår dem inte till 100%*. Ge exempel på något ni inte förstår.

## Här är några utmaningar för att göra spelet ännu bättre

✏️ **Gör minst en av utmaningarna eller hitta på en egen utmaning/förbättring.**
- Beskriv kort vilken förbättringen är och hur den ska fungera, sett ut spelarens perspektiv.
- Beskriv hur du fick ändra koden för att göra ändringen. Glöm inte att redovisa din kod.
- Om det inte gick att genomföra, förklara med några meningar vad du försökte och vad som hände. Glöm inte kodexempel och förklara vad som hände.

### Utmaning 1. Tre liv
Ändra spelet så att du har tre liv. Spelet ska sluta när spelaren använt alla tre liven.

### Utmaning 2. Olika ädelstenar
Det finns andra emoji som kan föreställa ädelstenar. Gör så att olika ädelstenar kan dyka upp när den ska börja falla uppifrån

### Utmaning 3. Välja skepp
Låt spelaren välja bland olika emoji som kan fungera som skepp innan spelet börjar.

### Utmaning 4. Flera ädelstenar
Just nu är det bara en fallande ädelsten åt gången. Ändra spelet så att flera kan falla samtidigt.

### Utmaning 5. Multi-player
Lägg till en andra spelare och tävla om vem som får högst poäng!

# Källor/Sources

Det här projektet är översatt och anpassat till p5.js baserat på https://aposteriori.trinket.io/game-development-with-pygame-zero#/gem-catcher/first-program

This project is a translated and adapted to p5.js from https://aposteriori.trinket.io/game-development-with-pygame-zero#/gem-catcher/first-program
