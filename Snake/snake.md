# Snake ⭐⭐⭐

## En handledning för p5.js

**[Regler](#regler)**

**[Översikt](#översikt)**

**[Kodning](#kodning)**
  [Rita bakgrunden](#rita-bakgrunden)
  &bull; [Rita ormen](#rita-ormen)
  &bull; [Timer](#timer)
  &bull; [Flytta ormen åt höger](#flytta-ormen-åt-höger)
  &bull; [Flytta ormen i alla fyra riktningar](#flytta-ormen-i-alla-fyra-riktningar)
  &bull; [Hindra att ormen rör sig rakt bakåt](#hindra-att-ormen-rör-sig-rakt-bakåt)
  &bull; [Använd en kö för riktningarna](#använd-en-kö-för-riktningarna)
  &bull; [Hindra att lägga till samma riktning två gånger](#hindra-att-lägga-till-samma-riktning-två-gånger)
  &bull; [Slå över vid skärmkanten](#slå-över-vid-skärmkanten)
  &bull; [Rita maten](#rita-maten)
  &bull; [Förenkla koden](#förenkla-koden)
  &bull; [Äta maten](#äta-maten)
  &bull; [Förenkla koden](#förenkla-koden-1)
  &bull; [Flytta maten till en ledig ruta](#flytta-maten-till-en-ledig-ruta)
  &bull; [Game over](#game-over)
  &bull; [Återställa spelet](#återställa-spelet)
  &bull; [Pausa efter att ormen har kraschat](#pausa-efter-att-ormen-har-kraschat)
  &bull; [Ändra ormens färg när den är död](#ändra-ormens-färg-när-den-är-död)

**[Uppgifter](#uppgifter)**

**[Källor](#källor)**

![image](https://user-images.githubusercontent.com/4598641/226439115-c9800ff9-c916-406c-9efb-39407658988a.png)

## Jobba så här

Vägen till ett färdigt projekt är en pusselbit i taget 🧩. Gör därför ett avsnitt i taget uppifrån och ner.

  - Få det att fungera innan ni går vidare till nästa avsnitt.
  - Testkör ofta, efter varje avsnitt eller ännu oftare.

Var det något i beskrivningen som var svårt att förstå? Ta med det i redovisningen. 📝

✏️ Pennan visar något du ska göra. Mata in koden och testkör innan du går vidare.
**Fungerar det inte? Rätta koden innan du fortsätter till nästa ändring.**

# Regler

När ormen äter mat, växer den. När en matbit är uppäten, dyker det upp en ny på en slumpmässig position.

Ormen kommer att slå över till andra sidan skärmen när den går utanför kanten.

Spelet är över när ormen slingrar in i sig själv.

## Kontroller

**Piltangenter** Byt riktning

# Översikt
Ormen är en variabel som har en lista av X- och Y-koordinater.

Maten representeras av en X- och Y-koordinat.

Ormen rör sig i två steg:

1.  Vi tar bort det sista elementet i listan – det är den gamla svanspositionen.
2.  Sen lägger vi till ett nytt element först i listan. Det är den nya positionen för huvudet i den riktning som ormen ringlar.

Så här kan det se ut i p5.js koordinatsystem. Y-axeln är upp och ner jämfört med vanliga koordinataxlar. Det är vanligt i datorgrafik. [Scratch](https://scratch.mit.edu) är ett undantag.
<br>
<img src="https://github.com/user-attachments/assets/a771cf43-c8a9-4f59-82ae-16955d1fbd44" width="50%">

Jämför med hur det ser ut i koden. Verkar det stämma?<br>
![image](https://user-images.githubusercontent.com/4598641/226439258-020b4582-5409-448b-99e6-55cf6a4bbcdc.png)

![image](https://user-images.githubusercontent.com/4598641/226439284-599e5e4f-6987-4eea-8b79-f39d5a2d850a.png)

När ormen når maten är huvudpositionen samma som matens position.
Då rör vi inte svansen utan flyttar maten till en slumpmässig ruta där ormen inte är. Ormen har alltså blivit en ruta längre av maten.

![image](https://user-images.githubusercontent.com/4598641/226439323-b54bc813-62f2-49d5-ac3b-1002ba0de713.png)

Ormen har krockat med sig själv om den nya huvudpositionen är samma som något av ormens andra segment. Då är spelet över.

# Kodning

## Rita bakgrunden

Spelområdet är 20 celler brett och 15 celler högt, och varje cell har en sidolängd på 15 pixlar.

Spelytan (canvas) ritas upp i funktionen `setup()`; bakgrunden ritas i `draw()`.

✏️ Kör koden med Run i https://editor.p5.js. Ser det ut så här?

![image](https://user-images.githubusercontent.com/4598641/235364441-4af78ea9-4545-40d5-af52-ed10b6de87da.png)

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```javascript
// Globala variabler här nedanför
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;

// Funktioner här nedanför
function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  background(70, 70, 70);
}
```

</details>

## Rita ormen

Ormens segment lagras som objekt med X- och Y-koordinater och ritas som rutor.

✏️ Uppdatera funktionen `draw` och testkör! Ritas ormen rätt?

> Raderna som ska ändras är markerade med 🐍

```javascript
// Globala variabler här nedanför
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  background(70, 70, 70);

  let snakeSegments = [ // nytt 🐍
    {x: 2, y: 0}, // nytt 🐍
    {x: 1, y: 0}, // nytt 🐍 
    {x: 0, y: 0}  // nytt 🐍
  ]; // nytt 🐍

  fill(165, 255, 81); // nytt 🐍
  noStroke(); // nytt 🐍

  for (let segment of snakeSegments) { // nytt 🐍
    rect( // nytt 🐍
      segment.x * cellSize, segment.y * cellSize, // nytt 🐍
      cellSize - 1, cellSize - 1 // nytt 🐍
    ); // nytt 🐍
  } // nytt 🐍
}
```

✏️ Testkör koden. Så här ska det se ut.
>Fungerar det inte? Kolla dina ändringar noga.

![image](https://user-images.githubusercontent.com/4598641/226439469-a0bf9621-d2ff-4b38-810e-9a1be63b3324.png)


## Timer

Ormen kommer att röra sig var 0,15:e sekund.

En timervariabel börjar på 0 och ökar med `deltaTime` för varje bildruta. I p5.js mäts `deltaTime` i millisekunder, så vi dividerar med 1000 för att få det i sekunder.

När timern är större eller lika med 0.15 återställs den till 0.

För närvarande skrivs `tick` ut varje gång ormen ska röra sig.

✏️ Uppdatera koden. Testkör och kolla utskriften i terminalfönstret.

```javascript
// Globala variabler här nedanför
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0; // nytt 🐍

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  timer += deltaTime / 1000; // nytt 🐍
  if (timer >= 0.15) { // nytt 🐍
    timer = 0; // nytt 🐍
    // Tillfälligt 🐍
    console.log('tick'); // nytt 🐍
  } // nytt 🐍

  background(70, 70, 70);
  
  // etc.
```

✏️ Testkör och kolla utskriften i terminalfönstret. Ser det ut så här?

![image](https://user-images.githubusercontent.com/4598641/235365117-32e6a3e4-1427-450b-8f0b-8fddb4fb64ee.png)


<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    // Temporary
    console.log("tick");
  }

  background(70, 70, 70);

  let snakeSegments = [
    {x: 2, y: 0},
    {x: 1, y: 0},
    {x: 0, y: 0}
  ];

  fill(165, 255, 81);
  noStroke();

  for (let segment of snakeSegments) {
    rect(
      segment.x * cellSize, segment.y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }
}
```

</details>

## Flytta ormen åt höger

Nästa position för ormens huvud beräknas genom att öka den nuvarande X-koordinaten för ormens huvud med 1. Det är första elementet i listan med ormens segment.
Detta nya segment läggs till i början av listan (med `unshift`).

Det sista elementet i listan, ormens svans, tas bort (med `pop`).

Segmentlistan ändras nu över tid, så den måste flyttas ut ur `draw()` och bli en global variabel.

✏️ Uppdatera koden och testkör.

```javascript
// Globala variabler här nedanför
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;

let snakeSegments = [ // flyttad från draw() 🐍
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    let nextXPosition = snakeSegments[0].x + 1; // nytt 🐍
    let nextYPosition = snakeSegments[0].y;     // nytt 🐍

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition}); // nytt 🐍
    snakeSegments.pop(); // nytt 🐍
  }

  background(70, 70, 70);

  // snakeSegments har flyttat ut 🐍

  fill(165, 255, 81);
  noStroke();

  for (let segment of snakeSegments) {
    // etc.
```

✏️ Testkör! Flyttar ormen i X-led?

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;

let snakeSegments = [
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    let nextXPosition = snakeSegments[0].x + 1;
    let nextYPosition = snakeSegments[0].y;

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});
    snakeSegments.pop();
  }

  background(70, 70, 70);

  fill(165, 255, 81);
  noStroke();

  for (let segment of snakeSegments) {
    rect(
      segment.x * cellSize, segment.y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }
}
```

</details>

## Flytta ormen i alla fyra riktningar

Ormens nuvarande riktning lagras i en variabel och ändras med hjälp av piltangenterna via funktionen `keyPressed()`.

Ormens nästa huvudposition ska ställas in beroende på denna riktning.

✏️ Uppdatera koden så här. Testkör med piltangenterna.

> Kom ihåg att först klicka i spelfönstret så kan din kod fånga upp tangenterna.

```javascript
let timer = 0;
let direction = 'right'; // nytt 🐍

let snakeSegments = [
  // etc...
];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    let nextXPosition = snakeSegments[0].x; // ändrat 🐍
    let nextYPosition = snakeSegments[0].y;

    if (direction === 'right') { // nytt 🐍
      nextXPosition += 1; // nytt 🐍
    } else if (direction === 'left') { // nytt 🐍
      nextXPosition -= 1; // nytt 🐍
    } else if (direction === 'down') { // nytt 🐍
      nextYPosition += 1; // nytt 🐍
    } else if (direction === 'up') { // nytt 🐍
      nextYPosition -= 1; // nytt 🐍
    } // nytt 🐍

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});
    snakeSegments.pop();
  }

  // etc... draw background and rects
}

function keyPressed() { // nytt 🐍
  if (keyCode === RIGHT_ARROW) { // nytt 🐍
    direction = 'right'; // nytt 🐍
  } else if (keyCode === LEFT_ARROW) { // nytt 🐍
    direction = 'left'; // nytt 🐍
  } else if (keyCode === DOWN_ARROW) { // nytt 🐍
    direction = 'down'; // nytt 🐍
  } else if (keyCode === UP_ARROW) { // nytt 🐍
    direction = 'up'; // nytt 🐍
  } // nytt 🐍
}
```

✏️ Testkör med piltangenterna.

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let direction = 'right';

let snakeSegments = [
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (direction === 'right') {
      nextXPosition += 1;
    } else if (direction === 'left') {
      nextXPosition -= 1;
    } else if (direction === 'down') {
      nextYPosition += 1;
    } else if (direction === 'up') {
      nextYPosition -= 1;
    }

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});
    snakeSegments.pop();
  }

  background(70, 70, 70);

  fill(165, 255, 81);
  noStroke();

  for (let segment of snakeSegments) {
    rect(
      segment.x * cellSize, segment.y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }
}

function keyPressed() {
  if (keyCode === RIGHT_ARROW) {
    direction = 'right';
  } else if (keyCode === LEFT_ARROW) {
    direction = 'left';
  } else if (keyCode === DOWN_ARROW) {
    direction = 'down';
  } else if (keyCode === UP_ARROW) {
    direction = 'up';
  }
}
```

</details>

## Hindra att ormen rör sig rakt bakåt

Ormen ska inte kunna röra sig i motsatt riktning som den för närvarande går. När den t.ex. går åt höger ska den inte direkt gå åt vänster. Detta kontrolleras innan riktningen ställs in.

✏️ Uppdatera funktionen `keyPressed()`.

```javascript
function keyPressed() {
  if (keyCode === RIGHT_ARROW && direction !== 'left') { // ändrat 🐍
    direction = 'right';
  } else if (keyCode === LEFT_ARROW && direction !== 'right') { // ändrat 🐍
    direction = 'left';
  } else if (keyCode === DOWN_ARROW && direction !== 'up') { // ändrat 🐍
    direction = 'down';
  } else if (keyCode === UP_ARROW && direction !== 'down') { // ändrat 🐍
    direction = 'up';
  }
}
```

✏️ Testkör svängar med piltangenterna och kolla om det blev rätt!

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let direction = 'right';

let snakeSegments = [
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (direction === 'right') {
      nextXPosition += 1;
    } else if (direction === 'left') {
      nextXPosition -= 1;
    } else if (direction === 'down') {
      nextYPosition += 1;
    } else if (direction === 'up') {
      nextYPosition -= 1;
    }

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});
    snakeSegments.pop();
  }

  background(70, 70, 70);

  fill(165, 255, 81);
  noStroke();

  for (let segment of snakeSegments) {
    rect(
      segment.x * cellSize, segment.y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }
}

function keyPressed() {
  if (keyCode === RIGHT_ARROW && direction !== 'left') {
    direction = 'right';
  } else if (keyCode === LEFT_ARROW && direction !== 'right') {
    direction = 'left';
  } else if (keyCode === DOWN_ARROW && direction !== 'up') {
    direction = 'down';
  } else if (keyCode === UP_ARROW && direction !== 'down') {
    direction = 'up';
  }
}
```

</details>

## Använd en kö för riktningarna

Just nu kan ormen fortfarande gå bakåt om en annan riktning och sedan den motsatta riktningen trycks inom ett enda tick på timern.
Om ormen t.ex. slingrade åt höger på den sista ticket och du trycker neråtpil och sedan vänsterpil före nästa tick, kommer ormen att flytta åt vänster på nästa tick.

Dessutom kan spelaren vilja ge flera anvisningar inom ett enda tick. I exemplet ovan kan spelaren ha velat att ormen skulle flytta neråt ett tick och sen vänster nästa tick.

Därför behöver vi en riktningskö (array). Det första elementet i kön är riktningen som ormen kommer att röra sig vid nästa tick.

Om riktningskön har mer än ett element, tas det första elementet bort (med `shift`) vid varje tick.

När en knapp trycks ned läggs riktningen till i slutet av riktningskön (med `push`).

Det sista elementet i riktningskön, alltså den senast tryckta riktningen, kontrolleras för att se om det inte är i motsatt riktning mot den nya riktningen innan den nya riktningen läggs till i kön.

✏️ Uppdatera koden.

```javascript
// Globala variabler här nedanför
let timer = 0;
let directionQueue = ['right']; // ändrat 🐍

let snakeSegments = [
  // etc...
];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    if (directionQueue.length > 1) { // nytt 🐍
      directionQueue.shift(); // tar bort första elementet 🐍
    }

    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (directionQueue[0] === 'right') { // ändrat 🐍
      nextXPosition += 1;
    } else if (directionQueue[0] === 'left') { // ändrat 🐍
      nextXPosition -= 1;
    } else if (directionQueue[0] === 'down') { // ändrat 🐍
      nextYPosition += 1;
    } else if (directionQueue[0] === 'up') { // ändrat 🐍
      nextYPosition -= 1;
    }

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});
    snakeSegments.pop();
  }

  background(70, 70, 70);

  fill(165, 255, 81);
  noStroke();

  for (let segment of snakeSegments) {
    rect(
      segment.x * cellSize, segment.y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  // Tillfälligt nytt 🐍
  fill(255); // Vit text 🐍
  for (let i = 0; i < directionQueue.length; i++) { // nytt 🐍
    text(`directionQueue[${i}]: ${directionQueue[i]}`, 15, 25 + 15 * i); // nytt 🐍
  } // nytt 🐍
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1]; // hämtar sista elementet 🐍

  if (keyCode === RIGHT_ARROW && lastDirection !== 'left') { // ändrat 🐍
    directionQueue.push('right'); // nytt 🐍
  } else if (keyCode === LEFT_ARROW && lastDirection !== 'right') { // ändrat 🐍
    directionQueue.push('left'); // nytt 🐍
  } else if (keyCode === DOWN_ARROW && lastDirection !== 'up') { // ändrat 🐍
    directionQueue.push('down'); // nytt 🐍
  } else if (keyCode === UP_ARROW && lastDirection !== 'down') { // ändrat 🐍
    directionQueue.push('up'); // nytt 🐍
  }
}
```

✏️ Testkör – vad händer när du trycker snabbt på de olika piltangenterna?

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let directionQueue = ['right'];

let snakeSegments = [
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    if (directionQueue.length > 1) {
      directionQueue.shift();
    }

    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (directionQueue[0] === 'right') {
      nextXPosition += 1;
    } else if (directionQueue[0] === 'left') {
      nextXPosition -= 1;
    } else if (directionQueue[0] === 'down') {
      nextYPosition += 1;
    } else if (directionQueue[0] === 'up') {
      nextYPosition -= 1;
    }

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});
    snakeSegments.pop();
  }

  background(70, 70, 70);

  fill(165, 255, 81);
  noStroke();

  for (let segment of snakeSegments) {
    rect(
      segment.x * cellSize, segment.y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  fill(255);
  for (let i = 0; i < directionQueue.length; i++) {
    text(`directionQueue[${i}]: ${directionQueue[i]}`, 15, 25 + 15 * i);
  }
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

</details>

## Hindra att lägga till samma riktning två gånger

Om den senaste riktningen är samma som den nya riktningen, ska den nya riktningen inte läggas till i riktningskön.

✏️ Uppdatera 🐍-raderna i `keyPressed()`.

```javascript
function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW 
      && lastDirection !== 'right' // nytt 🐍
      && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW 
             && lastDirection !== 'left' // nytt 🐍
             && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW 
             && lastDirection !== 'down' // nytt 🐍
             && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW 
             && lastDirection !== 'up' // nytt 🐍
             && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

Testkör – vad händer när du  snabbt trycker på olika piltangenter, eller samma piltangent?

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let directionQueue = ['right'];

let snakeSegments = [
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    if (directionQueue.length > 1) {
      directionQueue.shift();
    }

    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (directionQueue[0] === 'right') {
      nextXPosition += 1;
    } else if (directionQueue[0] === 'left') {
      nextXPosition -= 1;
    } else if (directionQueue[0] === 'down') {
      nextYPosition += 1;
    } else if (directionQueue[0] === 'up') {
      nextYPosition -= 1;
    }

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});
    snakeSegments.pop();
  }

  background(70, 70, 70);

  fill(165, 255, 81);
  noStroke();

  for (let segment of snakeSegments) {
    rect(
      segment.x * cellSize, segment.y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  fill(255);
  for (let i = 0; i < directionQueue.length; i++) {
    text(`directionQueue[${i}]: ${directionQueue[i]}`, 15, 25 + 15 * i);
  }
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW 
      && lastDirection !== 'right' 
      && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW 
             && lastDirection !== 'left' 
             && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW 
             && lastDirection !== 'down' 
             && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW 
             && lastDirection !== 'up' 
             && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

</details>

## Slå över vid skärmkanten

Om nästa position skulle vara utanför rutnätet, slår vi över till motsatta sidan på skärmen.

✏️ Uppdatera raderna med 🐍 i `draw()` så att det blir så här:

```javascript
function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    // etc.
    
    if (directionQueue[0] === 'right') {
      nextXPosition += 1;
      if (nextXPosition >= gridXCount) { // nytt 🐍
        nextXPosition = 0; // nytt 🐍
      } // nytt 🐍
    } else if (directionQueue[0] === 'left') {
      nextXPosition -= 1;
      if (nextXPosition < 0) { // nytt 🐍
        nextXPosition = gridXCount - 1; // nytt 🐍
      } // nytt 🐍
    } else if (directionQueue[0] === 'down') {
      nextYPosition += 1;
      if (nextYPosition >= gridYCount) { // nytt 🐍
        nextYPosition = 0; // nytt 🐍
      } // nytt 🐍
    } else if (directionQueue[0] === 'up') {
      nextYPosition -= 1;
      if (nextYPosition < 0) { // nytt 🐍
        nextYPosition = gridYCount - 1; // nytt 🐍
      } // nytt 🐍
    }

    // etc.
```

✏️ Testkör – vad händer när ormen når kanten?

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let directionQueue = ['right'];

let snakeSegments = [
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    if (directionQueue.length > 1) {
      directionQueue.shift();
    }

    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (directionQueue[0] === 'right') {
      nextXPosition += 1;
      if (nextXPosition >= gridXCount) {
        nextXPosition = 0;
      }
    } else if (directionQueue[0] === 'left') {
      nextXPosition -= 1;
      if (nextXPosition < 0) {
        nextXPosition = gridXCount - 1;
      }
    } else if (directionQueue[0] === 'down') {
      nextYPosition += 1;
      if (nextYPosition >= gridYCount) {
        nextYPosition = 0;
      }
    } else if (directionQueue[0] === 'up') {
      nextYPosition -= 1;
      if (nextYPosition < 0) {
        nextYPosition = gridYCount - 1;
      }
    }

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});
    snakeSegments.pop();
  }

  background(70, 70, 70);

  fill(165, 255, 81);
  noStroke();

  for (let segment of snakeSegments) {
    rect(
      segment.x * cellSize, segment.y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  fill(255);
  for (let i = 0; i < directionQueue.length; i++) {
    text(`directionQueue[${i}]: ${directionQueue[i]}`, 15, 25 + 15 * i);
  }
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW 
      && lastDirection !== 'right' 
      && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW 
             && lastDirection !== 'left' 
             && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW 
             && lastDirection !== 'down' 
             && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW 
             && lastDirection !== 'up' 
             && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

</details>

## Rita maten

Maten lagras som ett objekt med X- och Y-koordinater. Maten ritas som en kvadrat.

p5.js inbyggda `random()` och `floor()` används för att slumpa fram positionen. Maten måste beräknas inuti `setup()` för p5.js funktioner finns tillgängliga först då.

✏️ Uppdatera koden.

```javascript
// etc.
let directionQueue = ['right'];
let foodPosition; // nytt 🐍

let snakeSegments = [
// etc.

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  
  foodPosition = { // nytt 🐍
    x: floor(random(gridXCount)), // nytt 🐍
    y: floor(random(gridYCount))  // nytt 🐍
  }; // nytt 🐍
}

function draw() {
  // etc.

  fill(255, 76, 76); // Röd färg för maten 🐍
  rect( // nytt 🐍
    foodPosition.x * cellSize, foodPosition.y * cellSize, // nytt 🐍
    cellSize - 1, cellSize - 1 // nytt 🐍
  ); // nytt 🐍 

  fill(255);
  // for-loopen för texten
}
```

✏️ Testkör – ritas maten?

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let directionQueue = ['right'];
let foodPosition;

let snakeSegments = [
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  
  foodPosition = {
    x: floor(random(gridXCount)),
    y: floor(random(gridYCount))
  };
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    if (directionQueue.length > 1) {
      directionQueue.shift();
    }

    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (directionQueue[0] === 'right') {
      nextXPosition += 1;
      if (nextXPosition >= gridXCount) {
        nextXPosition = 0;
      }
    } else if (directionQueue[0] === 'left') {
      nextXPosition -= 1;
      if (nextXPosition < 0) {
        nextXPosition = gridXCount - 1;
      }
    } else if (directionQueue[0] === 'down') {
      nextYPosition += 1;
      if (nextYPosition >= gridYCount) {
        nextYPosition = 0;
      }
    } else if (directionQueue[0] === 'up') {
      nextYPosition -= 1;
      if (nextYPosition < 0) {
        nextYPosition = gridYCount - 1;
      }
    }

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});
    snakeSegments.pop();
  }

  background(70, 70, 70);

  fill(165, 255, 81);
  noStroke();

  for (let segment of snakeSegments) {
    rect(
      segment.x * cellSize, segment.y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  fill(255, 76, 76);
  rect(
    foodPosition.x * cellSize, foodPosition.y * cellSize,
    cellSize - 1, cellSize - 1
  );

  fill(255);
  for (let i = 0; i < directionQueue.length; i++) {
    text(`directionQueue[${i}]: ${directionQueue[i]}`, 15, 25 + 15 * i);
  }
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW 
      && lastDirection !== 'right' 
      && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW 
             && lastDirection !== 'left' 
             && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW 
             && lastDirection !== 'down' 
             && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW 
             && lastDirection !== 'up' 
             && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

</details>

## Förenkla koden

Koden för att rita en orms segment och rita maten är samma, förutom färgen.
Vi gör det till en funktion med färgen som parameter. I p5.js kan vi skicka in färgen som en array av rgb-värden.

✏️ Uppdatera de markerade raderna i `draw()`.

```javascript
function draw() {
  // etc...
  
  background(70, 70, 70);

  function drawCell(x, y, color) { // nytt 🐍
    fill(color[0], color[1], color[2]); // nytt 🐍
    rect( // nytt 🐍
      x * cellSize, y * cellSize, // nytt 🐍
      cellSize - 1, cellSize - 1 // nytt 🐍
    ); // nytt 🐍
  } // nytt 🐍

  noStroke();

  for (let segment of snakeSegments) {
    drawCell(segment.x, segment.y, [165, 255, 81]); // nytt 🐍
  }

  drawCell(foodPosition.x, foodPosition.y, [255, 76, 76]); // nytt 🐍

  // etc. text array
}
```

✏️ Testkör – fungerar koden som innan vi förenklade den?

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let directionQueue = ['right'];
let foodPosition;

let snakeSegments = [
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  
  foodPosition = {
    x: floor(random(gridXCount)),
    y: floor(random(gridYCount))
  };
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    if (directionQueue.length > 1) {
      directionQueue.shift();
    }

    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (directionQueue[0] === 'right') {
      nextXPosition += 1;
      if (nextXPosition >= gridXCount) {
        nextXPosition = 0;
      }
    } else if (directionQueue[0] === 'left') {
      nextXPosition -= 1;
      if (nextXPosition < 0) {
        nextXPosition = gridXCount - 1;
      }
    } else if (directionQueue[0] === 'down') {
      nextYPosition += 1;
      if (nextYPosition >= gridYCount) {
        nextYPosition = 0;
      }
    } else if (directionQueue[0] === 'up') {
      nextYPosition -= 1;
      if (nextYPosition < 0) {
        nextYPosition = gridYCount - 1;
      }
    }

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});
    snakeSegments.pop();
  }

  background(70, 70, 70);

  function drawCell(x, y, color) {
    fill(color[0], color[1], color[2]);
    rect(
      x * cellSize, y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  noStroke();

  for (let segment of snakeSegments) {
    drawCell(segment.x, segment.y, [165, 255, 81]);
  }

  drawCell(foodPosition.x, foodPosition.y, [255, 76, 76]);

  fill(255);
  for (let i = 0; i < directionQueue.length; i++) {
    text(`directionQueue[${i}]: ${directionQueue[i]}`, 15, 25 + 15 * i);
  }
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW 
      && lastDirection !== 'right' 
      && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW 
             && lastDirection !== 'left' 
             && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW 
             && lastDirection !== 'down' 
             && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW 
             && lastDirection !== 'up' 
             && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

</details>

## Äta maten

Om ormens nya huvudposition är samma som matens position, tas inte ormens svans bort och maten får en ny slumpmässig position.

På så vis blir ormen en ruta längre.

✏️ Uppdatera logiken inuti din timer-block i `draw()`.

```javascript
function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    // etc... fram till unshift
    
    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});

    if (snakeSegments[0].x === foodPosition.x // nytt 🐍
        && snakeSegments[0].y === foodPosition.y) { // nytt 🐍
      foodPosition = { // nytt 🐍
        x: floor(random(gridXCount)), // nytt 🐍
        y: floor(random(gridYCount))  // nytt 🐍
      }; // nytt 🐍
    } else { // nytt 🐍
      snakeSegments.pop(); // dra in raden 🐍
    } // nytt 🐍
  }
```

✏️ Testkör – vad händer när ormen äter?

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let directionQueue = ['right'];
let foodPosition;

let snakeSegments = [
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  
  foodPosition = {
    x: floor(random(gridXCount)),
    y: floor(random(gridYCount))
  };
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    if (directionQueue.length > 1) {
      directionQueue.shift();
    }

    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (directionQueue[0] === 'right') {
      nextXPosition += 1;
      if (nextXPosition >= gridXCount) {
        nextXPosition = 0;
      }
    } else if (directionQueue[0] === 'left') {
      nextXPosition -= 1;
      if (nextXPosition < 0) {
        nextXPosition = gridXCount - 1;
      }
    } else if (directionQueue[0] === 'down') {
      nextYPosition += 1;
      if (nextYPosition >= gridYCount) {
        nextYPosition = 0;
      }
    } else if (directionQueue[0] === 'up') {
      nextYPosition -= 1;
      if (nextYPosition < 0) {
        nextYPosition = gridYCount - 1;
      }
    }

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});

    if (snakeSegments[0].x === foodPosition.x 
        && snakeSegments[0].y === foodPosition.y) {
      foodPosition = {
        x: floor(random(gridXCount)),
        y: floor(random(gridYCount))
      };
    } else {
      snakeSegments.pop();
    }
  }

  background(70, 70, 70);

  function drawCell(x, y, color) {
    fill(color[0], color[1], color[2]);
    rect(
      x * cellSize, y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  noStroke();

  for (let segment of snakeSegments) {
    drawCell(segment.x, segment.y, [165, 255, 81]);
  }

  drawCell(foodPosition.x, foodPosition.y, [255, 76, 76]);

  fill(255);
  for (let i = 0; i < directionQueue.length; i++) {
    text(`directionQueue[${i}]: ${directionQueue[i]}`, 15, 25 + 15 * i);
  }
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW 
      && lastDirection !== 'right' 
      && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW 
             && lastDirection !== 'left' 
             && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW 
             && lastDirection !== 'down' 
             && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW 
             && lastDirection !== 'up' 
             && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

</details>

## Förenkla koden

Koden för att lägga maten på en slumpmässig position behövs på fler ställen. Vi gör en global funktion för det.

✏️ Lägg till funktionen utanför `draw()`, anropa den i `setup()` och uppdatera logiken.

```javascript
function moveFood() { // nytt 🐍
  foodPosition = { // flyttat 🐍
    x: floor(random(gridXCount)), // flyttat 🐍
    y: floor(random(gridYCount)) // flyttat 🐍
  }; // flyttat 🐍
}

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  moveFood(); // nytt 🐍
}

function draw() {
  // etc.

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});

    if (snakeSegments[0].x === foodPosition.x 
        && snakeSegments[0].y === foodPosition.y) {
      moveFood(); // nytt 🐍
    } else {
      snakeSegments.pop();
    }

  // etc.
```

✏️ Testkör – fungerar koden lika bra som innan?

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let directionQueue = ['right'];
let foodPosition;

let snakeSegments = [
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function moveFood() {
  foodPosition = {
    x: floor(random(gridXCount)),
    y: floor(random(gridYCount))
  };
}

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  moveFood();
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    if (directionQueue.length > 1) {
      directionQueue.shift();
    }

    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (directionQueue[0] === 'right') {
      nextXPosition += 1;
      if (nextXPosition >= gridXCount) {
        nextXPosition = 0;
      }
    } else if (directionQueue[0] === 'left') {
      nextXPosition -= 1;
      if (nextXPosition < 0) {
        nextXPosition = gridXCount - 1;
      }
    } else if (directionQueue[0] === 'down') {
      nextYPosition += 1;
      if (nextYPosition >= gridYCount) {
        nextYPosition = 0;
      }
    } else if (directionQueue[0] === 'up') {
      nextYPosition -= 1;
      if (nextYPosition < 0) {
        nextYPosition = gridYCount - 1;
      }
    }

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});

    if (snakeSegments[0].x === foodPosition.x 
        && snakeSegments[0].y === foodPosition.y) {
      moveFood();
    } else {
      snakeSegments.pop();
    }
  }

  background(70, 70, 70);

  function drawCell(x, y, color) {
    fill(color[0], color[1], color[2]);
    rect(
      x * cellSize, y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  noStroke();

  for (let segment of snakeSegments) {
    drawCell(segment.x, segment.y, [165, 255, 81]);
  }

  drawCell(foodPosition.x, foodPosition.y, [255, 76, 76]);

  fill(255);
  for (let i = 0; i < directionQueue.length; i++) {
    text(`directionQueue[${i}]: ${directionQueue[i]}`, 15, 25 + 15 * i);
  }
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW 
      && lastDirection !== 'right' 
      && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW 
             && lastDirection !== 'left' 
             && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW 
             && lastDirection !== 'down' 
             && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW 
             && lastDirection !== 'up' 
             && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

</details>

## Flytta maten till en ledig ruta

Istället för att flytta maten till en slumpmässig plats, flyttar den till en plats där ormen inte är just nu.

Alla positioner i rutnätet loopas igenom. För varje rutnätsposition loopar vi igenom alla ormens segment.
Om inga ormsegment är på en viss rutnätsposition, läggs den positionen till i en lista över möjliga matpositioner.
Nästa matposition väljs sen slumpmässigt från den listan. I p5.js använder vi `random()` på en array för att få ut en slumpmässig matbit.

✏️ Uppdatera kodraderna i `moveFood()`.

```javascript
function moveFood() {
  let possibleFoodPositions = []; // nytt 🐍

  for (let foodX = 0; foodX < gridXCount; foodX++) { // nytt 🐍
    for (let foodY = 0; foodY < gridYCount; foodY++) { // nytt 🐍
      let possible = true; // nytt 🐍

      for (let segment of snakeSegments) { // nytt 🐍
        if (foodX === segment.x && foodY === segment.y) { // nytt 🐍
          possible = false; // nytt 🐍
        } // nytt 🐍
      } // nytt 🐍

      if (possible) { // nytt 🐍
        possibleFoodPositions.push({x: foodX, y: foodY}); // nytt 🐍
      } // nytt 🐍
    } // nytt 🐍
  } // nytt 🐍

  foodPosition = random(possibleFoodPositions); // nytt 🐍
}
```

✏️ Testkör – vad händer när ormen äter?

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let directionQueue = ['right'];
let foodPosition;

let snakeSegments = [
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function moveFood() {
  let possibleFoodPositions = [];

  for (let foodX = 0; foodX < gridXCount; foodX++) {
    for (let foodY = 0; foodY < gridYCount; foodY++) {
      let possible = true;

      for (let segment of snakeSegments) {
        if (foodX === segment.x && foodY === segment.y) {
          possible = false;
        }
      }

      if (possible) {
        possibleFoodPositions.push({x: foodX, y: foodY});
      }
    }
  }

  foodPosition = random(possibleFoodPositions);
}

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  moveFood();
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    if (directionQueue.length > 1) {
      directionQueue.shift();
    }

    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (directionQueue[0] === 'right') {
      nextXPosition += 1;
      if (nextXPosition >= gridXCount) {
        nextXPosition = 0;
      }
    } else if (directionQueue[0] === 'left') {
      nextXPosition -= 1;
      if (nextXPosition < 0) {
        nextXPosition = gridXCount - 1;
      }
    } else if (directionQueue[0] === 'down') {
      nextYPosition += 1;
      if (nextYPosition >= gridYCount) {
        nextYPosition = 0;
      }
    } else if (directionQueue[0] === 'up') {
      nextYPosition -= 1;
      if (nextYPosition < 0) {
        nextYPosition = gridYCount - 1;
      }
    }

    snakeSegments.unshift({x: nextXPosition, y: nextYPosition});

    if (snakeSegments[0].x === foodPosition.x 
        && snakeSegments[0].y === foodPosition.y) {
      moveFood();
    } else {
      snakeSegments.pop();
    }
  }

  background(70, 70, 70);

  function drawCell(x, y, color) {
    fill(color[0], color[1], color[2]);
    rect(
      x * cellSize, y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  noStroke();

  for (let segment of snakeSegments) {
    drawCell(segment.x, segment.y, [165, 255, 81]);
  }

  drawCell(foodPosition.x, foodPosition.y, [255, 76, 76]);

  fill(255);
  for (let i = 0; i < directionQueue.length; i++) {
    text(`directionQueue[${i}]: ${directionQueue[i]}`, 15, 25 + 15 * i);
  }
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW 
      && lastDirection !== 'right' 
      && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW 
             && lastDirection !== 'left' 
             && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW 
             && lastDirection !== 'down' 
             && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW 
             && lastDirection !== 'up' 
             && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

</details>

## Game over

Ormens segment loopas igenom. Om något av dem – förutom det sista – är i samma position som ormens nya huvudposition, så har ormen kraschat in i sig själv.

> Det sista segmentet på ormen ska inte kollas eftersom det kommer att tas bort inom samma tick. I JavaScript kan vi få ut alla element förutom sista med `snakeSegments.slice(0, -1)`.

För närvarande skrivs `collision` ut när ormen kraschar in i sig själv.

✏️ Uppdatera i funktionen `draw()`.

```javascript
function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    // etc.
    } else if (directionQueue[0] === 'up') {
      nextYPosition -= 1;
      if (nextYPosition < 0) {
        nextYPosition = gridYCount - 1;
      }
    }
    
    let canMove = true; // nytt 🐍

    for (let segment of snakeSegments.slice(0, -1)) { // nytt 🐍
      if (nextXPosition === segment.x // nytt 🐍
          && nextYPosition === segment.y) { // nytt 🐍
        canMove = false; // nytt 🐍
      } // nytt 🐍
    } // nytt 🐍

    if (canMove) { // nytt 🐍
      snakeSegments.unshift({x: nextXPosition, y: nextYPosition});

      if (snakeSegments[0].x === foodPosition.x 
          && snakeSegments[0].y === foodPosition.y) {
        moveFood();
      } else {
        snakeSegments.pop();
      }
    } else { // nytt 🐍
      console.log('collision'); // nytt 🐍
    } // nytt 🐍
  }
```

✏️ Testkör – vad händer när ormen kraschar in sig själv? Kolla i terminalfönstret.

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let directionQueue = ['right'];
let foodPosition;

let snakeSegments = [
  {x: 2, y: 0},
  {x: 1, y: 0},
  {x: 0, y: 0}
];

function moveFood() {
  let possibleFoodPositions = [];

  for (let foodX = 0; foodX < gridXCount; foodX++) {
    for (let foodY = 0; foodY < gridYCount; foodY++) {
      let possible = true;

      for (let segment of snakeSegments) {
        if (foodX === segment.x && foodY === segment.y) {
          possible = false;
        }
      }

      if (possible) {
        possibleFoodPositions.push({x: foodX, y: foodY});
      }
    }
  }

  foodPosition = random(possibleFoodPositions);
}

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  moveFood();
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    if (directionQueue.length > 1) {
      directionQueue.shift();
    }

    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (directionQueue[0] === 'right') {
      nextXPosition += 1;
      if (nextXPosition >= gridXCount) {
        nextXPosition = 0;
      }
    } else if (directionQueue[0] === 'left') {
      nextXPosition -= 1;
      if (nextXPosition < 0) {
        nextXPosition = gridXCount - 1;
      }
    } else if (directionQueue[0] === 'down') {
      nextYPosition += 1;
      if (nextYPosition >= gridYCount) {
        nextYPosition = 0;
      }
    } else if (directionQueue[0] === 'up') {
      nextYPosition -= 1;
      if (nextYPosition < 0) {
        nextYPosition = gridYCount - 1;
      }
    }

    let canMove = true;

    for (let segment of snakeSegments.slice(0, -1)) {
      if (nextXPosition === segment.x 
          && nextYPosition === segment.y) {
        canMove = false;
      }
    }

    if (canMove) {
      snakeSegments.unshift({x: nextXPosition, y: nextYPosition});

      if (snakeSegments[0].x === foodPosition.x 
          && snakeSegments[0].y === foodPosition.y) {
        moveFood();
      } else {
        snakeSegments.pop();
      }
    } else {
      console.log('collision');
    }
  }

  background(70, 70, 70);

  function drawCell(x, y, color) {
    fill(color[0], color[1], color[2]);
    rect(
      x * cellSize, y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  noStroke();

  for (let segment of snakeSegments) {
    drawCell(segment.x, segment.y, [165, 255, 81]);
  }

  drawCell(foodPosition.x, foodPosition.y, [255, 76, 76]);

  fill(255);
  for (let i = 0; i < directionQueue.length; i++) {
    text(`directionQueue[${i}]: ${directionQueue[i]}`, 15, 25 + 15 * i);
  }
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW 
      && lastDirection !== 'right' 
      && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW 
             && lastDirection !== 'left' 
             && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW 
             && lastDirection !== 'down' 
             && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW 
             && lastDirection !== 'up' 
             && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

</details>

## Återställa spelet

Vi gör en funktion som ställer in spelets startläge.

Funktionen anropas innan spelet börjar och när ormen kraschar.

✏️ Lägg till funktionen `reset()` och gör de andra småändringarna på markerade rader.

```javascript
let directionQueue; // ta bort = ['right'] härifrån
let snakeSegments;  // ta bort den ursprungliga arrayen härifrån

function moveFood() {
  // etc.
}

function reset() { // nytt 🐍
  timer = 0; // nytt 🐍
  directionQueue = ['right']; // nytt 🐍
  snakeSegments = [ // nytt 🐍
    {x: 2, y: 0}, // nytt 🐍
    {x: 1, y: 0}, // nytt 🐍
    {x: 0, y: 0}  // nytt 🐍
  ]; // nytt 🐍
  moveFood(); // nytt 🐍
} // nytt 🐍

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  reset(); // ändrat 🐍
}

function draw() {
  // etc.

    if (canMove) {
      // etc.
    } else {
      reset(); // nytt 🐍
    }

// etc.
```

✏️ Testkör – vad händer när ormen kraschar?

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let directionQueue;
let foodPosition;
let snakeSegments;

function moveFood() {
  let possibleFoodPositions = [];

  for (let foodX = 0; foodX < gridXCount; foodX++) {
    for (let foodY = 0; foodY < gridYCount; foodY++) {
      let possible = true;

      for (let segment of snakeSegments) {
        if (foodX === segment.x && foodY === segment.y) {
          possible = false;
        }
      }

      if (possible) {
        possibleFoodPositions.push({x: foodX, y: foodY});
      }
    }
  }

  foodPosition = random(possibleFoodPositions);
}

function reset() {
  timer = 0;
  directionQueue = ['right'];
  snakeSegments = [
    {x: 2, y: 0},
    {x: 1, y: 0},
    {x: 0, y: 0}
  ];
  moveFood();
}

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  reset();
}

function draw() {
  timer += deltaTime / 1000;
  if (timer >= 0.15) {
    timer = 0;
    
    if (directionQueue.length > 1) {
      directionQueue.shift();
    }

    let nextXPosition = snakeSegments[0].x;
    let nextYPosition = snakeSegments[0].y;

    if (directionQueue[0] === 'right') {
      nextXPosition += 1;
      if (nextXPosition >= gridXCount) {
        nextXPosition = 0;
      }
    } else if (directionQueue[0] === 'left') {
      nextXPosition -= 1;
      if (nextXPosition < 0) {
        nextXPosition = gridXCount - 1;
      }
    } else if (directionQueue[0] === 'down') {
      nextYPosition += 1;
      if (nextYPosition >= gridYCount) {
        nextYPosition = 0;
      }
    } else if (directionQueue[0] === 'up') {
      nextYPosition -= 1;
      if (nextYPosition < 0) {
        nextYPosition = gridYCount - 1;
      }
    }

    let canMove = true;

    for (let segment of snakeSegments.slice(0, -1)) {
      if (nextXPosition === segment.x 
          && nextYPosition === segment.y) {
        canMove = false;
      }
    }

    if (canMove) {
      snakeSegments.unshift({x: nextXPosition, y: nextYPosition});

      if (snakeSegments[0].x === foodPosition.x 
          && snakeSegments[0].y === foodPosition.y) {
        moveFood();
      } else {
        snakeSegments.pop();
      }
    } else {
      reset();
    }
  }

  background(70, 70, 70);

  function drawCell(x, y, color) {
    fill(color[0], color[1], color[2]);
    rect(
      x * cellSize, y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  noStroke();

  for (let segment of snakeSegments) {
    drawCell(segment.x, segment.y, [165, 255, 81]);
  }

  drawCell(foodPosition.x, foodPosition.y, [255, 76, 76]);

  fill(255);
  for (let i = 0; i < directionQueue.length; i++) {
    text(`directionQueue[${i}]: ${directionQueue[i]}`, 15, 25 + 15 * i);
  }
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW 
      && lastDirection !== 'right' 
      && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW 
             && lastDirection !== 'left' 
             && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW 
             && lastDirection !== 'down' 
             && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW 
             && lastDirection !== 'up' 
             && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

</details>

## Pausa efter att ormen har kraschat

En global variabel används för att lagra om ormen är vid liv eller inte. Om den är `false` (falskt) så har ormen kraschat.

Om ormen är död, väntar timern i 2 sekunder innan den anropar `reset()` för att starta om spelet.

✏️ Uppdatera koden på de markerade raderna.

```javascript
let snakeSegments;
let snakeAlive; // nytt 🐍

function reset() {
  // etc.
  moveFood();
  snakeAlive = true; // nytt 🐍
}

function draw() {
  timer += deltaTime / 1000;
  
  if (snakeAlive) { // nytt 🐍
    if (timer >= 0.15) {
      timer = 0;

      // etc.

      if (canMove) {
        // etc.
      } else {
        snakeAlive = false; // nytt 🐍
      }
    } // Stänger timer-if-satsen
  } else if (timer >= 2) { // nytt 🐍
    reset(); // nytt 🐍
  } // nytt 🐍
  
  // etc. drawing code
```

✏️ Testkör – vad händer när ormen kraschar?

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let directionQueue;
let foodPosition;
let snakeSegments;
let snakeAlive;

function moveFood() {
  let possibleFoodPositions = [];

  for (let foodX = 0; foodX < gridXCount; foodX++) {
    for (let foodY = 0; foodY < gridYCount; foodY++) {
      let possible = true;

      for (let segment of snakeSegments) {
        if (foodX === segment.x && foodY === segment.y) {
          possible = false;
        }
      }

      if (possible) {
        possibleFoodPositions.push({x: foodX, y: foodY});
      }
    }
  }

  foodPosition = random(possibleFoodPositions);
}

function reset() {
  timer = 0;
  directionQueue = ['right'];
  snakeSegments = [
    {x: 2, y: 0},
    {x: 1, y: 0},
    {x: 0, y: 0}
  ];
  moveFood();
  snakeAlive = true;
}

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  reset();
}

function draw() {
  timer += deltaTime / 1000;
  
  if (snakeAlive) {
    if (timer >= 0.15) {
      timer = 0;
      
      if (directionQueue.length > 1) {
        directionQueue.shift();
      }

      let nextXPosition = snakeSegments[0].x;
      let nextYPosition = snakeSegments[0].y;

      if (directionQueue[0] === 'right') {
        nextXPosition += 1;
        if (nextXPosition >= gridXCount) {
          nextXPosition = 0;
        }
      } else if (directionQueue[0] === 'left') {
        nextXPosition -= 1;
        if (nextXPosition < 0) {
          nextXPosition = gridXCount - 1;
        }
      } else if (directionQueue[0] === 'down') {
        nextYPosition += 1;
        if (nextYPosition >= gridYCount) {
          nextYPosition = 0;
        }
      } else if (directionQueue[0] === 'up') {
        nextYPosition -= 1;
        if (nextYPosition < 0) {
          nextYPosition = gridYCount - 1;
        }
      }

      let canMove = true;

      for (let segment of snakeSegments.slice(0, -1)) {
        if (nextXPosition === segment.x 
            && nextYPosition === segment.y) {
          canMove = false;
        }
      }

      if (canMove) {
        snakeSegments.unshift({x: nextXPosition, y: nextYPosition});

        if (snakeSegments[0].x === foodPosition.x 
            && snakeSegments[0].y === foodPosition.y) {
          moveFood();
        } else {
          snakeSegments.pop();
        }
      } else {
        snakeAlive = false;
      }
    }
  } else if (timer >= 2) {
    reset();
  }

  background(70, 70, 70);

  function drawCell(x, y, color) {
    fill(color[0], color[1], color[2]);
    rect(
      x * cellSize, y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  noStroke();

  for (let segment of snakeSegments) {
    drawCell(segment.x, segment.y, [165, 255, 81]);
  }

  drawCell(foodPosition.x, foodPosition.y, [255, 76, 76]);
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW 
      && lastDirection !== 'right' 
      && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW 
             && lastDirection !== 'left' 
             && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW 
             && lastDirection !== 'down' 
             && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW 
             && lastDirection !== 'up' 
             && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

</details>

## Ändra ormens färg när den är död

Ormens färg ändras beroende på om den är vid liv eller inte.

✏️ Uppdatera kodraderna i `draw()`.

```javascript
function draw() {
  // etc.

  for (let segment of snakeSegments) {
    let color = [165, 255, 81]; // nytt 🐍
    if (!snakeAlive) { // nytt 🐍
      color = [140, 140, 140]; // nytt 🐍
    } // nytt 🐍
    drawCell(segment.x, segment.y, color); // nytt 🐍
  }

  // etc.   
```

✏️ Testkör – vad händer när ormen dör?

<details>
<summary>📝 Så här kan koden se ut nu</summary>

```javascript
let gridXCount = 20;
let gridYCount = 15;
let cellSize = 15;
let timer = 0;
let directionQueue;
let foodPosition;
let snakeSegments;
let snakeAlive;

function moveFood() {
  let possibleFoodPositions = [];

  for (let foodX = 0; foodX < gridXCount; foodX++) {
    for (let foodY = 0; foodY < gridYCount; foodY++) {
      let possible = true;

      for (let segment of snakeSegments) {
        if (foodX === segment.x && foodY === segment.y) {
          possible = false;
        }
      }

      if (possible) {
        possibleFoodPositions.push({x: foodX, y: foodY});
      }
    }
  }

  foodPosition = random(possibleFoodPositions);
}

function reset() {
  timer = 0;
  directionQueue = ['right'];
  snakeSegments = [
    {x: 2, y: 0},
    {x: 1, y: 0},
    {x: 0, y: 0}
  ];
  moveFood();
  snakeAlive = true;
}

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  reset();
}

function draw() {
  timer += deltaTime / 1000;
  
  if (snakeAlive) {
    if (timer >= 0.15) {
      timer = 0;
      
      if (directionQueue.length > 1) {
        directionQueue.shift();
      }

      let nextXPosition = snakeSegments[0].x;
      let nextYPosition = snakeSegments[0].y;

      if (directionQueue[0] === 'right') {
        nextXPosition += 1;
        if (nextXPosition >= gridXCount) {
          nextXPosition = 0;
        }
      } else if (directionQueue[0] === 'left') {
        nextXPosition -= 1;
        if (nextXPosition < 0) {
          nextXPosition = gridXCount - 1;
        }
      } else if (directionQueue[0] === 'down') {
        nextYPosition += 1;
        if (nextYPosition >= gridYCount) {
          nextYPosition = 0;
        }
      } else if (directionQueue[0] === 'up') {
        nextYPosition -= 1;
        if (nextYPosition < 0) {
          nextYPosition = gridYCount - 1;
        }
      }

      let canMove = true;

      for (let segment of snakeSegments.slice(0, -1)) {
        if (nextXPosition === segment.x 
            && nextYPosition === segment.y) {
          canMove = false;
        }
      }

      if (canMove) {
        snakeSegments.unshift({x: nextXPosition, y: nextYPosition});

        if (snakeSegments[0].x === foodPosition.x 
            && snakeSegments[0].y === foodPosition.y) {
          moveFood();
        } else {
          snakeSegments.pop();
        }
      } else {
        snakeAlive = false;
      }
    }
  } else if (timer >= 2) {
    reset();
  }

  background(70, 70, 70);

  function drawCell(x, y, color) {
    fill(color[0], color[1], color[2]);
    rect(
      x * cellSize, y * cellSize,
      cellSize - 1, cellSize - 1
    );
  }

  noStroke();

  for (let segment of snakeSegments) {
    let color = [165, 255, 81];
    if (!snakeAlive) {
      color = [140, 140, 140];
    }
    drawCell(segment.x, segment.y, color);
  }

  drawCell(foodPosition.x, foodPosition.y, [255, 76, 76]);
}

function keyPressed() {
  let lastDirection = directionQueue[directionQueue.length - 1];

  if (keyCode === RIGHT_ARROW 
      && lastDirection !== 'right' 
      && lastDirection !== 'left') {
    directionQueue.push('right');
  } else if (keyCode === LEFT_ARROW 
             && lastDirection !== 'left' 
             && lastDirection !== 'right') {
    directionQueue.push('left');
  } else if (keyCode === DOWN_ARROW 
             && lastDirection !== 'down' 
             && lastDirection !== 'up') {
    directionQueue.push('down');
  } else if (keyCode === UP_ARROW 
             && lastDirection !== 'up' 
             && lastDirection !== 'down') {
    directionQueue.push('up');
  }
}
```

</details>

# Uppgifter

## 1. Utvärdera ert eget arbete!

När ni svarar på detta, tänk på att *ni har tillgång till uppgiften* – ni behöver alltså inte kunna koden utantill.

**1A.** De här delarna av uppgiften har vi gjort. Vi förstår dem och kan förklara koden för Susanne eller inför klassen.

**1B.** De här delarna av uppgiften har vi gjort *men vi förstår dem inte till 100%*. Ge exempel på något ni inte förstår.

## 2. Gör spelet ännu bättre

Pröva att göra några ändringar eller tillägg. Det kan vara en poängräknare för ormens längd, high score, roligare game over, en introskärm eller något annat som är standard i den här typen av spel.

  - Beskriv kort vad förändringen är och hur den ska fungera.
  - Beskriv hur du fick ändra koden för att göra ändringen. Glöm inte kodexempel.
  - Om det inte gick att genomföra, förklara med några meningar vad du försökte och vad som hände. Glöm inte kodexempel.

## 3. Be någon annan att testa ditt projekt

  - Be om konstruktiv kritik på spelet. Skriv ner
  - Be om konstruktiv kritik på koden. Skriv ner
  - Har du något tips att ge någon som inte har gjort det här projektet innan? Skriv ner

# Källor

[https://simplegametutorials.github.io/pygamezero/snake/](https://simplegametutorials.github.io/pygamezero/snake/)

[https://web.archive.org/web/20140820192218/http://www.realtid.se/ArticlePages/200603/01/20060301132710\_Realtid437/20060301132710\_Realtid437.dbp.asp?Action=Print](https://web.archive.org/web/20140820192218/http://www.realtid.se/ArticlePages/200603/01/20060301132710_Realtid437/20060301132710_Realtid437.dbp.asp?Action=Print)

-----

# TODO: Links That Likely Require Updates

1.  **Python Dictionary Screenshots:**
      * `https://user-images.githubusercontent.com/4598641/226439258...png`
      * `https://user-images.githubusercontent.com/4598641/226439284...png`
      * These images display Python dictionary syntax (`{'x': 2, 'y': 0}`). In the translated tutorial, standard JavaScript object syntax is used (`{x: 2, y: 0}`).
2.  **Game Window Screenshots:**
      * `https://user-images.githubusercontent.com/4598641/235364441...png` (and all subsequent gameplay screenshots).
      * These images show the distinct Pygame Zero window frame. A p5.js sketch renders inside a web browser window or the p5.js web editor canvas.
3.  **Terminal Output Screenshot:**
      * `https://user-images.githubusercontent.com/4598641/235365117...png`
      * This image displays a Python terminal. A p5.js user will view `console.log()` output in the browser developer tools console or the web editor console.
4.  **Source Link:**
      * `https://simplegametutorials.github.io/pygamezero/snake/`
      * This link directs the reader to the Pygame Zero version of the tutorial. You may wish to replace it with a native p5.js resource or add a note clarifying it as the origin of the Python version.