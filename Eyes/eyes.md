
# Eyes ⭐⭐
![image](https://user-images.githubusercontent.com/4598641/223815678-2c97174e-578c-4df4-9f8a-d8764f0b2424.png)

Vi vill göra en animering där ögonen följer muspekaren.

## Jobba så här

Vägen till ett färdigt projekt är en pusselbit i taget 🧩. Gör därför ett avsnitt i taget uppifrån och ner.

  - Få det att fungera innan ni går vidare till nästa avsnitt.
  - Testkör ofta, efter varje avsnitt eller ännu oftare.

Var det något i beskrivningen som var svårt att förstå? Ta med det i redovisningen. 📝

## Innehåll

[Rita ett öga](https://www.google.com/search?q=%23rita-ett-%C3%B6ga)
• [Räkna ut avståndet i X- och Y-led mellan muspekaren och ögat](https://www.google.com/search?q=%23r%C3%A4kna-ut-avst%C3%A5ndet-i-x--och-y-led-mellan-muspekaren-och-%C3%B6gat)
• [Räkna ut avståndet mellan muspekaren och ögat](https://www.google.com/search?q=%23r%C3%A4kna-ut-avst%C3%A5ndet-mellan-muspekaren-och-%C3%B6gat)
• [Rörlig pupill](https://www.google.com/search?q=%23r%C3%B6rlig-pupill)
• [Pupillen ska inte smita när muspekaren är utanför ögat](https://www.google.com/search?q=%23pupillen-ska-inte-smita-n%C3%A4r-muspekaren-%C3%A4r-utanf%C3%B6r-%C3%B6gat)
• [Två ögon](https://www.google.com/search?q=%23tv%C3%A5-%C3%B6gon)
• [Uppgifter](https://www.google.com/search?q=%23uppgifter)
• [Källor](https://www.google.com/search?q=%23k%C3%A4llor)

# Rita ett öga

Ögat ritas med en vit cirkel och en lite mindre, mörkblå cirkel för pupillen.
> Vi använder p5.js-funktioner för att rita på canvasen.

![image](https://user-images.githubusercontent.com/4598641/223816876-1da49223-c7af-46d5-836c-9b1216eb52d4.png)

Koden så här långt:

```javascript
function setup() {
  createCanvas(530, 400);
}

function draw() {
  background(0);

  fill(255);
  circle(200, 200, 100); // Ögonvitan (diameter 100)
  
  fill(0, 0, 100);
  circle(200, 200, 30); // Pupillen (diameter 30)
}
```

✏️ Gå till https://editor.p5js.org/ för att mata in koden. 

Provkör din kod innan du går vidare. Den behöver fungera innan du fortsätter.

# Räkna ut avståndet i X- och Y-led mellan muspekaren och ögat

Vi lägger till kod så att avståndet mellan ögats mitt och muspekaren visas, räknat i pixlar.

  - Ögats x- och y-koordinater återanvänder vi från när vi ritar ögat och därför sparar vi koordinaterna i variabler.
  - Vi använder de inbyggda variablerna `mouseX` och `mouseY`.
  - Funktionen `draw()` körs automatiskt i en loop och ritar om skärmen vid varje uppdatering.

✏️ **Mata in** och testkör koden\!

Koden så här långt – nya rader är markerade:

```javascript
function setup() {
  createCanvas(530, 400);
}

function draw() {
  background(0);

  let mouseXPos = mouseX; // nyrad 👀
  let mouseYPos = mouseY; // nyrad 👀
  let eyeX = 200; // nyrad 👀
  let eyeY = 200; // nyrad 👀

  let distanceX = mouseXPos - eyeX; // nyrad 👀
  let distanceY = mouseYPos - eyeY; // nyrad 👀

  fill(255);
  circle(eyeX, eyeY, 100); // lite ändrad 👀
  
  fill(0, 0, 100);
  circle(200, 200, 30);

  fill(255);
  text(`distance x: ${distanceX}\ndistance y: ${distanceY}`, 10, 20); // nyrad 👀
}
```

Så här ser det ut när jag testkör. Fungerar det inte? Kolla koden igen.

![image](https://user-images.githubusercontent.com/4598641/223817639-1363643f-481d-44e3-979b-d0b48eb0c9da.png)

# Räkna ut avståndet mellan muspekaren och ögat

Avståndet, `distance`, kan vi räkna ut med Pythagoras sats. Se figuren.

![image](https://user-images.githubusercontent.com/4598641/224125785-ee2eedc9-2155-4508-9fc2-d8518bfdfe32.png)

Avståndet i pixlar är roten ur (antalet pixlar i X-led i kvadrat + antalet pixlar i Y-led i kvadrat).

  - Operatorn `** 2` betyder upphöjt till 2, alltså kvadraten.
  - Vi använder `Math.sqrt()` för att räkna ut kvadratroten.

:pencil2: **Uppdatera koden** och testkör den\!

```javascript
function setup() {
  createCanvas(530, 400);
}

function draw() {
  background(0);

  let mouseXPos = mouseX;
  let mouseYPos = mouseY;
  let eyeX = 200;
  let eyeY = 200;

  let distanceX = mouseXPos - eyeX;
  let distanceY = mouseYPos - eyeY;
  let distance = Math.sqrt(distanceX ** 2 + distanceY ** 2); // nyrad 👀

  fill(255);
  circle(eyeX, eyeY, 100);
  
  fill(0, 0, 100);
  circle(200, 200, 30);

  fill(255);
  text(`distance x: ${distanceX}\ndistance y: ${distanceY}\ndistance: ${distance}`, 10, 20); // ändrad 👀
}
```

Verkar avståndet räknas ut rätt? Flytta muspekaren sakta och kolla.

# Rörlig pupill

Vi vill att pupillen ska följa muspekaren.

Om muspekaren är innanför ögat är det lätt. Då kan vi sätta pupillen till muspekarens koordinater.

Då kan koden för `draw()` se ut så här. Den övriga koden är samma som innan.

```javascript
// behåll resten av koden

function draw() { // funktionen är uppdaterad 👀
  background(0);

  let mouseXPos = mouseX;
  let eyeX = 200;
  let eyeY = 200;

  let distanceX = mouseXPos - eyeX;
  let distanceY = mouseY - eyeY;
  let distance = Math.sqrt(distanceX ** 2 + distanceY ** 2); // Pythagoras
  
  let pupilX = eyeX + distanceX; // nyrad 👀
  let pupilY = eyeY + distanceY; // nyrad 👀

  fill(255);
  circle(eyeX, eyeY, 100);
  
  fill(0, 0, 100);
  circle(pupilX, pupilY, 30); // nyrad 👀
}
```

:pencil2: **Uppdatera och testa** din kod. Fungerar den bra när muspekaren är innanför ögats cirkel?

<details>
<summary>📝 Så här ser hela koden ut nu:</summary>

```javascript
function setup() {
  createCanvas(530, 400);
}

function draw() {
  background(0);

  let mouseXPos = mouseX;
  let mouseYPos = mouseY;
  let eyeX = 200;
  let eyeY = 200;

  let distanceX = mouseXPos - eyeX;
  let distanceY = mouseYPos - eyeY;
  let distance = Math.sqrt(distanceX ** 2 + distanceY ** 2); // Pythagoras
  
  let pupilX = eyeX + distanceX; // nyrad 👀
  let pupilY = eyeY + distanceY; // nyrad 👀

  fill(255);
  circle(eyeX, eyeY, 100);
  
  fill(0, 0, 100);
  circle(pupilX, pupilY, 30);
}
```

</details>

# Pupillen ska inte smita när muspekaren är utanför ögat

🤔 Hur vet vi att muspekaren är i ögat? Vi har ju räknat ut avståndet från ögats centrum till muspekaren i variabeln `distance` så vi använder ögats radie som en gräns, till exempel 30 pixlar. Det ska ju finnas lite plats att rita pupillen också.

Om muspekaren är mer än 30 pixlar från ögats centrum, placerar vi pupillen i kanten av ögat. Se bilden.

![image](https://user-images.githubusercontent.com/4598641/224125785-ee2eedc9-2155-4508-9fc2-d8518bfdfe32.png)

  - Den streckade triangeln har hypotenusan 30 pixlar och den är likformig med den större triangeln med muspekaren i ena hörnet.
  - Proportionen mellan den streckade triangeln och den stora triangeln är `30 / distance` när muspekaren är utanför ögat.
  - Vi får därför skala `distanceX` och `distanceY` med den skalfaktorn när vi ska räkna ut pupillens koordinater.

Så här kan `draw()` se ut nu:

```javascript
// behåll resten av koden

function draw() {
  background(0);

  let mouseXPos = mouseX;
  let mouseYPos = mouseY;
  let maxRadius = 30; // nyrad 👀
  let eyeX = 200;
  let eyeY = 200;

  let distanceX = mouseXPos - eyeX;
  let distanceY = mouseYPos - eyeY;
  let distance = Math.sqrt(distanceX ** 2 + distanceY ** 2); // Pythagoras
  
  let pupilX, pupilY; // nyrad 👀

  if (distance < maxRadius) { // innanför ögat # ändrat 👀
    pupilX = eyeX + distanceX; // ändrat 👀
    pupilY = eyeY + distanceY; // ändrat 👀
  } else { // utanför ögat # ändrat 👀
    let scale = maxRadius / distance; // se bilden # ändrat 👀
    pupilX = eyeX + distanceX * scale; // ändrat 👀
    pupilY = eyeY + distanceY * scale; // ändrat 👀
  }

  fill(255);
  circle(eyeX, eyeY, 100);
  
  fill(0, 0, 100);
  circle(pupilX, pupilY, 30);
}
```

:pencil2: **Uppdatera de markerade raderna och testa koden.** Fungerar den bra även när muspekaren är utanför ögats cirkel?

<details>
<summary>📝 Så här ser hela koden ut nu:</summary>

```javascript
function setup() {
  createCanvas(530, 400);
}

function draw() {
  background(0);

  let mouseXPos = mouseX;
  let mouseYPos = mouseY;
  let maxRadius = 30;
  let eyeX = 200;
  let eyeY = 200;

  let distanceX = mouseXPos - eyeX;
  let distanceY = mouseYPos - eyeY;
  let distance = Math.sqrt(distanceX ** 2 + distanceY ** 2);

  let pupilX, pupilY;

  if (distance < maxRadius) {
    pupilX = eyeX + distanceX;
    pupilY = eyeY + distanceY;
  } else {
    let scale = maxRadius / distance;
    pupilX = eyeX + distanceX * scale;
    pupilY = eyeY + distanceY * scale;
  }

  fill(255);
  circle(eyeX, eyeY, 100);
  fill(0, 0, 100);
  circle(pupilX, pupilY, 30);
}
```

</details>

# Två ögon

Vi vill ha *två* ögon som följer muspekaren. Vi kan återanvända samma kod.
Därför lägger vi ögats kod i funktionen `drawEye` som har ögats koordinater som parametrar.

Så här kan koden se ut nu:

```javascript
function setup() {
  createCanvas(530, 400);
}

function draw() {
  background(0);

  drawEye(200, 200); // första ögat 👀
  drawEye(330, 200); // andra ögat 👀
}

function drawEye(eyeX, eyeY) { // ändrat 👀
  let maxRadius = 30; // ändrat 👀
  let mouseXPos = mouseX; 
  let mouseYPos = mouseY;

  let distanceX = mouseXPos - eyeX; // ändrat 👀
  let distanceY = mouseYPos - eyeY; // ändrat 👀
  let distance = Math.sqrt(distanceX ** 2 + distanceY ** 2); // ändrat 👀
  
  let pupilX, pupilY;

  if (distance < maxRadius) { // ändrat 👀
    pupilX = eyeX + distanceX; // ändrat 👀
    pupilY = eyeY + distanceY; // ändrat 👀
  } else { // ändrat 👀
    let scale = maxRadius / distance; // ändrat 👀
    pupilX = eyeX + distanceX * scale; // ändrat 👀
    pupilY = eyeY + distanceY * scale; // ändrat 👀
  }

  fill(255);
  circle(eyeX, eyeY, 100); // ändrat 👀
  fill(0, 0, 100);
  circle(pupilX, pupilY, 30); // ändrat 👀
}
```

:pencil2: **Uppdatera de ändrade raderna och testa koden.** Följer båda ögonen muspekaren på rätt sätt?

# Uppgifter

## 1\. Utvärdera ert eget arbete\!

När ni svarar på detta, tänk er att *ni har tillgång till uppgiften* – ni behöver alltså inte kunna koden utantill.

**1A.** De här delarna av uppgiften har vi gjort. Vi förstår dem och kan förklara koden för läraren eller inför klassen.

**1B.** De här delarna av uppgiften har vi gjort *men vi förstår dem inte till 100%*. Ge exempel på något ni inte förstår.

## Uppgift 2

Uträkningen av `scale` går att göra om så att vi får ännu färre kodrader. Just nu har vi en `if/else` där det är väldigt lite skillnad mellan de två kodblocken:

```javascript
    if (distance < maxRadius) { 
        pupilX = eyeX + distanceX;
        pupilY = eyeY + distanceY;
    } else { 
        let scale = maxRadius / distance;
        pupilX = eyeX + distanceX * scale; 
        pupilY = eyeY + distanceY * scale;
    }
```

Kan du förenkla koden så att vi inte behöver två olika uträkningar av `pupilX` respektive `pupilY`?[^1]

  - Glöm inte kodexempel när du redovisar.

## Extrauppgift 3

Kan du få ögonen att byta färg, försvinna eller flytta sig till en annan position på skärmen ibland?

  - Glöm inte kodexempel när du redovisar.
  - I p5.js kan du använda `random(min, max)` för att få ett tal mellan min och max.

## Extrauppgift 4

Kan du lägga in någon annan slags figur och få ögonen att följa den figuren istället för muspekaren?

  - Glöm inte kodexempel när du redovisar.

# Mer om koden i p5.js, förklarat på engelska

**[p5.js Reference](https://www.google.com/search?q=https://p5js.org/reference/)**: Hitta alla funktioner för att rita cirklar, text och hantera musen.

**[mouseX / mouseY](https://www.google.com/search?q=https://p5js.org/reference/%23/p5/mouseX)**: Variabler som håller koll på musens position.

**[circle()](https://www.google.com/search?q=https://p5js.org/reference/%23/p5/circle)**: Hur man ritar cirklar (notera att p5.js använder diameter).

# Källor

Detta är en anpassning till p5.js av originalprojektet [https://simplegametutorials.github.io/pygamezero/eyes/](https://simplegametutorials.github.io/pygamezero/eyes/)

[^1]:
    Vad kan du sätta `scale` till när `distance < maxRadius`? Hur kan du använda det för att räkna ut `pupilX` och `pupilY` med samma kod?
