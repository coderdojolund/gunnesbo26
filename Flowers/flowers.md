# Flowers ⭐⭐⭐
## En handledning för JavaScript och Processing i editor.p5js.org

<img src="image-hela.png" width="400" title="Flowers">


## TODO
- Restore the descriptions -- they are incomplete
- Update screenshots to reflect the p5.js version
- Describe how to type the Unicode emojis and their names for searching


## Innehåll
**[Regler](#regler)** [Kontroller](#kontroller)

**[Översikt](#översikt)**

**[Kodning](#kodning)**
  [Rita celler](#rita-celler)
  &bull; [Markera celler](#markera-celler)
  &bull; [Bara celler inom rutnätet ska gå att välja](#bara-celler-inom-rutnätet-ska-gå-att-välja)
  &bull; [Markera celler](#markera-celler-1)
  &bull; [Ändra cellens utseende när vänster musknapp klickas](#ändra-cellens-utseende-när-vänster-musknapp-klickas)
  &bull; [Rita blommor](#rita-blommor)
  &bull; [Förenkla koden](#förenkla-koden)
  &bull; [Växla blommor](#växla-blommor)
  &bull; [Visa antalet blommor runt cellen](#visa-antalet-blommor-runt-cellen)
  &bull; [Slumpa blomplanteringen](#slumpa-blomplanteringen)
  &bull; [Återställa spelet](#återställa-spelet)
  &bull; [Att avtäcka celler](#att-avtäcka-celler)
  &bull; [En lista som sparar celler som ska avtäckas](#en-lista-som-sparar-celler-som-ska-avtäckas)
  &bull; [Lägg till fler celler på stacken](#lägg-till-fler-celler-på-stacken)
  &bull; [Ta hänsyn till antalet grannblommor när vi avtäcker](#ta-hänsyn-till-antalet-grannblommor-när-vi-avtäcker)
  &bull; [Rita flaggor och frågetecken](#rita-flaggor-och-frågetecken)
  &bull; [Byta cellens status mellan blank, flagga och frågetecken](#byta-cellens-status-mellan-blank-flagga-och-frågetecken)
  &bull; [Hindra att flaggade celler avtäcks](#hindra-att-flaggade-celler-avtäcks)
  &bull; [Celler med frågetecken får avtäckas](#celler-med-frågetecken-får-avtäckas)
  &bull; [Ändra grafiken när vänster musknapp klickar på en flaggad cell](#ändra-grafiken-när-vänster-musknapp-klickar-på-en-flaggad-cell)
  &bull; [Slut på spelet](#slut-på-spelet)
  &bull; [Att vinna spelet](#att-vinna-spelet)
  &bull; [Nytt spel vid nästa klick](#nytt-spel-vid-nästa-klick)
  &bull; [Sluta markera celler när spelet är slut](#sluta-markera-celler-när-spelet-är-slut)
  &bull; [Göm blommorna tills spelet är slut](#göm-blommorna-tills-spelet-är-slut)
  &bull; [Göm antalet blomgrannar för täckta celler](#göm-antalet-blomgrannar-för-täckta-celler)
  &bull; [Hindra att man klickar på en blomma vid första klicket](#hindra-att-man-klickar-på-en-blomma-vid-första-klicket)

**[Uppgifter](#uppgifter)**

**[Källor](#källor)**

## Jobba så här
Bygg spelet bit för bit.
- Gör ett avsnitt i taget.
- Testa ofta.
- Gå vidare först när det fungerar.

Om något var svårt: skriv ner det till redovisningen. 📝
- Samma om du ber om hjälp från generativt 🤖 AI: skriv ner frågan och en sammanfattning av svaret *med dina egna ord* i en mening. Ta med det i redovisningen. Det påverkar bedömningen och hjälper oss också att förbättra uppgiften.


# Regler
Spelet börjar med ett rutnät av täckta rutor. Under några rutor finns blommor.

Om du klickar på en blomma är spelet slut.

När du vänsterklickar avtäcks en ruta. Om rutan saknar blomgrannar så avtäcks fler rutor automatiskt.

Högerklick växlar mellan:
- flagga
- frågetecken
- tom ruta

En flagga stoppar vänsterklick. Frågetecken är bara en markering.

Du vinner när alla rutor utan blommor är avtäckta.

## Kontroller

**Vänsterklick med musen** Avtäck en ruta

**Högerklick med musen** Växla dold ruta mellan flagga, frågetecken och tom

# Översikt

Vi använder ett rutnät. Varje ruta har ett objekt:
- `flower` (true/false)
- `state` (`covered`, `uncovered`, `flag`, `question`)

Vi ritar med:
- symboler för rutor: `⬜` = täckt, `🟧` = avtäckt)
- emoji (`🌷`, `🚩`, `❓`)
- siffror för antalet blomgrannar, `1` till `8`

# Kodning
## Rita celler
Vi börjar med att rita täckta rutor.

Skriv in koden på https://editor.p5js.org. 
>Du ska inte logga in. Kom ihåg att spara din kod på Google Drive.

✏️ Testkör. Ser det ut som på bilden?

```javascript
let cellSize = 24;
let gridXCount = 19;
let gridYCount = 14;

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  textAlign(CENTER, CENTER);
  textSize(18);
}

function drawCellSymbol(symbol, x, y, col = 20) {
  fill(col);
  noStroke();
  text(symbol, x * cellSize + cellSize / 2, y * cellSize + cellSize / 2 + 1);
}

function draw() {
  background(235);

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      drawCellSymbol('⬜', x, y, 30);
    }
  }
}
```

<img src="image0.png" width="400" title="Rita celler">

## Markera celler
Nu följer vi musen och skriver ut koordinaterna till en början.
Så här ser koden ut nu. Vi har lagt till de två variablerna och uppdaterat funktionen `draw()`.

✏️ Uppdatera och testkör!  Ser det ut som i bildexemplet?

```javascript
let selectedX = 0;
let selectedY = 0;

function draw() {
  background(235);

  selectedX = floor(mouseX / cellSize);
  selectedY = floor(mouseY / cellSize);

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      drawCellSymbol('⬜', x, y, 30);
    }
  }

  fill(0);
  textSize(12);
  text(`x: ${selectedX} y: ${selectedY}`, 44, 12);
  textSize(18);
}
```

<img src="image-markera-celler.png" width="400" title="Markera celler">


## Bara celler inom rutnätet ska gå att välja
Om muspositionen är större än rutnätets storlek i X- eller Y-led, alltså om vi pekar utanför rutnätet, så ställs den valda positionen in på den sista cellen på den axeln.

Rutnätets storlek i X- och Y-led återanvänds från att rita cellerna. Vi gör därför variabler med storlekarna.

✏️ Uppdatera funktionen `draw()` och testkör. Vad händer när du pekar innanför och utanför spelplanen med muspekaren?

📝 Så här ser funktionen `draw()` ut nu:

TODO: This code is incomplete

```javascript
function draw() {
  background(235);

  selectedX = constrain(floor(mouseX / cellSize), 0, gridXCount - 1);
  selectedY = constrain(floor(mouseY / cellSize), 0, gridYCount - 1);

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      drawCellSymbol('⬜', x, y, 30);
    }
  }
}
```

![image](https://user-images.githubusercontent.com/4598641/226451363-15f4d3b2-c3f1-4187-9d11-949fd2691b7d.png)

## Markera celler
Vi visar markerad ruta med en annan symbol. Uppdatera funktionen `draw()` så att den blir så här.

```javascript
function draw() {
  background(235);

  selectedX = constrain(floor(mouseX / cellSize), 0, gridXCount - 1);
  selectedY = constrain(floor(mouseY / cellSize), 0, gridYCount - 1);

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (x === selectedX && y === selectedY) {
        drawCellSymbol('🟧', x, y, 20);
      } else {
        drawCellSymbol('⬜', x, y, 30);
      }
    }
  }
}
```

![image](https://user-images.githubusercontent.com/4598641/226451429-c6b5e111-f945-47a4-bfe6-5005c372f603.png)

## Ändra cellens utseende när vänster musknapp klickas
När vänster musknapp hålls nere visar vi rutan som avtäckt. Uppdatera `draw()` och testkör.

```javascript
function draw() {
  background(235);

  selectedX = constrain(floor(mouseX / cellSize), 0, gridXCount - 1);
  selectedY = constrain(floor(mouseY / cellSize), 0, gridYCount - 1);

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (x === selectedX && y === selectedY) {
        if (mouseIsPressed && mouseButton === LEFT) {
          drawCellSymbol('🟨', x, y, 20);
        } else {
          drawCellSymbol('🟧', x, y, 20);
        }
      } else {
        drawCellSymbol('⬜', x, y, 30);
      }
    }
  }
}
```

![image](https://user-images.githubusercontent.com/4598641/226451476-56697739-2fbd-436c-8baa-06d9e22be4ad.png)

## Rita blommor
Nu skapar vi rutnätet.

Uppdatera och testkör.

```javascript
let grid = [];

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  textAlign(CENTER, CENTER);
  textSize(18);

  for (let y = 0; y < gridYCount; y++) {
    grid.push([]);
    for (let x = 0; x < gridXCount; x++) {
      grid[y].push({ flower: false });
    }
  }

  // Tillfälligt test
  grid[0][0].flower = true;
  grid[0][1].flower = true;
}

function draw() {
  background(235);

  selectedX = constrain(floor(mouseX / cellSize), 0, gridXCount - 1);
  selectedY = constrain(floor(mouseY / cellSize), 0, gridYCount - 1);

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (x === selectedX && y === selectedY) {
        if (mouseIsPressed && mouseButton === LEFT) {
          drawCellSymbol('🟨', x, y, 20);
        } else {
          drawCellSymbol('🟧', x, y, 20);
        }
      } else {
        drawCellSymbol('⬜', x, y, 30);
      }

      if (grid[y][x].flower) {
        drawCellSymbol('🌷', x, y, 20);
      }
    }
  }
}
```

![image](https://user-images.githubusercontent.com/4598641/226451517-df57e52b-abe0-4c91-b75c-fd350bc0ef44.png)


## Förenkla koden
Vi gör hjälpfunktioner för att slippa onödiga upprepningar. 

```javascript
function drawCovered(x, y) {
  drawCellSymbol('⬜', x, y, 30);
}

function drawHighlighted(x, y) {
  drawCellSymbol('🟧', x, y, 20);
}

function drawUncovered(x, y) {
  drawCellSymbol('🟨', x, y, 20);
}
```

## Växla blommor
Tillfälligt test: högerklick växlar blomma.

Lägg till funktionen `mouseReleased()` och testa.

```javascript
  let canvas = createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  canvas.elt.addEventListener("contextmenu", (e) => e.preventDefault());

function mouseReleased() {
  if (mouseButton === RIGHT) {
    grid[selectedY][selectedX].flower = !grid[selectedY][selectedX].flower;
  }
  return false;
}
```

## Visa antalet blommor runt cellen
Vi räknar blomgrannar.

```javascript
function getSurroundingFlowerCount(x, y) {
  let count = 0;

  for (let dy = -1; dy <= 1; dy++) {
    for (let dx = -1; dx <= 1; dx++) {
      if (
        !(dx === 0 && dy === 0) &&
        y + dy >= 0 && y + dy < grid.length &&
        x + dx >= 0 && x + dx < grid[y + dy].length &&
        grid[y + dy][x + dx].flower
      ) {
        count++;
      }
    }
  }

  return count;
}

function drawNumber(x, y, value) {
  fill(10);
  text(String(value), x * cellSize + cellSize / 2, y * cellSize + cellSize / 2 + 1);
}
```

![image](https://user-images.githubusercontent.com/4598641/226451615-217a10c0-cf61-41f0-80fd-df17ef8c238e.png)


## Slumpa blomplanteringen

```javascript
function plantFlowersRandomly() {
  const possible = [];

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      possible.push({ x, y });
    }
  }

  for (let i = 0; i < 40; i++) {
    const idx = floor(random(possible.length));
    const p = possible.splice(idx, 1)[0];
    grid[p.y][p.x].flower = true;
  }
}
```

![image](https://user-images.githubusercontent.com/4598641/226451656-7aac2925-cd35-488f-a397-0a2efc8d269b.png)


## Återställa spelet

Vi gör en funktion som heter `reset()`. Den ska ställa in spelets startläge. 
Nästan all kod som just nu ligger längst ner under `# Kod för att starta appen` flyttar vi dit.

`reset()` anropas innan spelet börjar och när någon knapp trycks ned.

✏️ Uppdatera koden. Testkör och se om det fungerar som innan!

```javascript
let gameOver = false;
let firstClick = true;

function reset() {
  grid = [];

  for (let y = 0; y < gridYCount; y++) {
    grid.push([]);
    for (let x = 0; x < gridXCount; x++) {
      grid[y].push({
        flower: false,
        state: 'covered' // covered, uncovered, flag, question
      });
    }
  }

  gameOver = false;
  firstClick = true;
}

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  textAlign(CENTER, CENTER);
  textSize(18);
  reset();
}

function keyPressed() {
  reset();
}
```

## Att avtäcka celler

```javascript
function mouseReleased() {
  if (mouseButton === LEFT) {
    grid[selectedY][selectedX].state = 'uncovered';
  }
  return false;
}
```

![image](https://user-images.githubusercontent.com/4598641/226451742-496e7414-d3d4-4be4-b251-df824a393c09.png)

## En lista som sparar celler som ska avtäckas

En lista över cellpositioner skapas. Så småningom kommer alla cellpositioner som ska avtäckas att läggas till i denna lista.

För närvarande kommer denna "avtäckningsstack" bara att innehålla den valda positionen, så den kommer bara att avtäcka den valda cellen som tidigare.

Så länge det finns positioner i avtäckningsstacken, tas en position bort från den och cellen vid denna position på rutnätet avtäcks.

✏️ Uppdatera hela funktionen `mouseReleased`. Testkör vad som händer när du klickar på olika ställen i rutnätet!

```javascript
function mouseReleased() {
  if (mouseButton === LEFT) {
    const stack = [{ x: selectedX, y: selectedY }];

    while (stack.length > 0) {
      const current = stack.pop();
      const x = current.x;
      const y = current.y;

      grid[y][x].state = 'uncovered';
    }
  }
  return false;
}
```

## Lägg till fler celler på stacken

```javascript
if (getSurroundingFlowerCount(x, y) === 0) {
  for (let dy = -1; dy <= 1; dy++) {
    for (let dx = -1; dx <= 1; dx++) {
      if (
        !(dx === 0 && dy === 0) &&
        y + dy >= 0 && y + dy < grid.length &&
        x + dx >= 0 && x + dx < grid[y + dy].length &&
        grid[y + dy][x + dx].state === 'covered'
      ) {
        stack.push({ x: x + dx, y: y + dy });
      }
    }
  }
}
```

![image](https://user-images.githubusercontent.com/4598641/226451814-b25ebd8c-a36b-445d-9512-a291112a56f4.png)


## Ta hänsyn till antalet grannblommor när vi avtäcker
Använd funktionen `getSurroundingFlowerCount(x, y)` både vid klick och ritning.

```javascript
if (getSurroundingFlowerCount(x, y) > 0 && grid[y][x].state === 'uncovered') {
  drawNumber(x, y, getSurroundingFlowerCount(x, y));
}
```

![image](https://user-images.githubusercontent.com/4598641/226451877-3422c1a8-a0ff-49d3-8df1-f46e50c52424.png)

## Rita flaggor och frågetecken

En cells status kan också vara en flagga eller ett frågetecken.

Om en cells status är flagga eller frågetecken, ritas flagga eller frågetecken över cellen.

För att testa detta ändrar vi tillfälligt status för två celler till att ha en flagga och ett frågetecken.

✏️ Uppdatera koden. Testkör och se om det ritas en flagga och ett frågetecken på rätt celler!

```javascript
if (grid[y][x].state === 'flag') {
  drawCellSymbol('🚩', x, y, 20);
} else if (grid[y][x].state === 'question') {
  drawCellSymbol('❓', x, y, 20);
}
```

![image](https://user-images.githubusercontent.com/4598641/226451916-0675c6bd-8039-4926-b164-3cf556ff3a08.png)

## Byta cellens status mellan blank, flagga och frågetecken

När man högerklickar på en cell ska statusen ändras mellan blank, flagga och frågetecken.

✏️ Uppdatera koden. Testkör och se om det fungerar att byta statusen på några olika celler!

```javascript
if (mouseButton === RIGHT) {
  if (grid[selectedY][selectedX].state === 'covered') {
    grid[selectedY][selectedX].state = 'flag';
  } else if (grid[selectedY][selectedX].state === 'flag') {
    grid[selectedY][selectedX].state = 'question';
  } else if (grid[selectedY][selectedX].state === 'question') {
    grid[selectedY][selectedX].state = 'covered';
  }
}
```

Högerklick gör att vi får upp en meny som inte hör till spelet. 

Vi behöver ändra hur vi skapar canvasen i `setup()` också:
```javascript
function setup() {
  let canvas = createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  canvas.elt.addEventListener("contextmenu", (e) => e.preventDefault());
  // behåll resten som det är
```

## Hindra att flaggade celler avtäcks

```javascript
if (mouseButton === LEFT && grid[selectedY][selectedX].state !== 'flag') {
  // avtäckningskod
}
```

## Celler med frågetecken får avtäckas

```javascript
if (
  ['covered', 'question'].includes(grid[y + dy][x + dx].state)
) {
  stack.push({ x: x + dx, y: y + dy });
}
```

## Ändra grafiken när vänster musknapp klickar på en flaggad cell

```javascript
if (x === selectedX && y === selectedY && !gameOver) {
  if (mouseIsPressed && mouseButton === LEFT) {
    if (grid[y][x].state === 'flag') {
      drawCovered(x, y);
    } else {
      drawUncovered(x, y);
    }
  } else {
    drawHighlighted(x, y);
  }
}
```

## Slut på spelet

```javascript
if (gameOver) {
  return false;
}

if (grid[selectedY][selectedX].flower) {
  grid[selectedY][selectedX].state = 'uncovered';
  gameOver = true;
}
```

## Att vinna spelet

```javascript
let complete = true;

for (let y = 0; y < gridYCount; y++) {
  for (let x = 0; x < gridXCount; x++) {
    if (grid[y][x].state !== 'uncovered' && !grid[y][x].flower) {
      complete = false;
    }
  }
}

if (complete) {
  gameOver = true;
}
```

## Nytt spel vid nästa klick

```javascript
if (gameOver) {
  reset();
  return false;
}
```

## Sluta markera celler när spelet är slut

```javascript
if (x === selectedX && y === selectedY && !gameOver) {
  drawHighlighted(x, y);
}
```

## Göm blommorna tills spelet är slut

```javascript
if (grid[y][x].flower && gameOver) {
  drawCellSymbol('🌷', x, y, 20);
}
```

![image](https://user-images.githubusercontent.com/4598641/226452171-3df8c25a-b72c-4d16-9ff4-c654bb0e6db3.png)


## Göm antalet blomgrannar för täckta celler
Om en cell är täckt, ska vi inte visa antalet grannceller med blommor.

✏️ Uppdatera koden. Testkör och se om det fungerar på rätt sätt!

```javascript
if (getSurroundingFlowerCount(x, y) > 0 && grid[y][x].state === 'uncovered') {
  drawNumber(x, y, getSurroundingFlowerCount(x, y));
}
```

## Hindra att man klickar på en blomma vid första klicket
![image](https://user-images.githubusercontent.com/4598641/226452196-f8755175-df82-4650-be3a-73491516082d.png)

Plantera blommor först efter första vänsterklick.

För att det första klicket inte ska avtäcka en blomma, flyttar vi koden för att placera blommor så att den körs **efter** att vänster musknapp klickas **för första gången**.

Blomplanteringen får en egen funktion, `plantFlowersAvoiding(x, y)` där en del av koden från `reset()` hamnar.
`mouseReleased()` kan då anropa den koden vid första klicket.

Den första cellen vi klickade på får då ingen blomma.

Vi skapar en variabel för att hålla reda på om ett klick är det första klicket i spelet.

✏️ Uppdatera koden och testkör! Fungerar spelet som du tänker dig nu?

```javascript
function plantFlowersAvoiding(avoidX, avoidY) {
  const possible = [];

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (x !== avoidX || y !== avoidY) {
        possible.push({ x, y });
      }
    }
  }

  for (let i = 0; i < 40; i++) {
    const idx = floor(random(possible.length));
    const p = possible.splice(idx, 1)[0];
    grid[p.y][p.x].flower = true;
  }
}
```

## Komplett version

```javascript
let cellSize = 24;
let gridXCount = 19;
let gridYCount = 14;

let grid = [];
let selectedX = 0;
let selectedY = 0;

let gameOver = false;
let firstClick = true;

function setup() {
  createCanvas(gridXCount * cellSize, gridYCount * cellSize);
  textAlign(CENTER, CENTER);
  textSize(18);
  reset();
}

function reset() {
  grid = [];

  for (let y = 0; y < gridYCount; y++) {
    grid.push([]);
    for (let x = 0; x < gridXCount; x++) {
      grid[y].push({
        flower: false,
        state: 'covered'
      });
    }
  }

  gameOver = false;
  firstClick = true;
}

function drawCellSymbol(symbol, x, y, col = 20) {
  fill(col);
  noStroke();
  text(symbol, x * cellSize + cellSize / 2, y * cellSize + cellSize / 2 + 1);
}

function drawCovered(x, y) {
  drawCellSymbol('⬜', x, y, 30);
}

function drawHighlighted(x, y) {
  drawCellSymbol('🟧', x, y, 20);
}

function drawUncovered(x, y) {
  drawCellSymbol('🟨', x, y, 20);
}

function drawNumber(x, y, value) {
  fill(10);
  noStroke();
  text(String(value), x * cellSize + cellSize / 2, y * cellSize + cellSize / 2 + 1);
}

function plantFlowersAvoiding(avoidX, avoidY) {
  const possible = [];

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (x !== avoidX || y !== avoidY) {
        possible.push({ x, y });
      }
    }
  }

  for (let i = 0; i < 40; i++) {
    const idx = floor(random(possible.length));
    const p = possible.splice(idx, 1)[0];
    grid[p.y][p.x].flower = true;
  }
}

function getSurroundingFlowerCount(x, y) {
  let count = 0;

  for (let dy = -1; dy <= 1; dy++) {
    for (let dx = -1; dx <= 1; dx++) {
      if (
        !(dx === 0 && dy === 0) &&
        y + dy >= 0 && y + dy < grid.length &&
        x + dx >= 0 && x + dx < grid[y + dy].length &&
        grid[y + dy][x + dx].flower
      ) {
        count++;
      }
    }
  }

  return count;
}

function draw() {
  background(235);

  selectedX = constrain(floor(mouseX / cellSize), 0, gridXCount - 1);
  selectedY = constrain(floor(mouseY / cellSize), 0, gridYCount - 1);

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x].state === 'uncovered') {
        drawUncovered(x, y);
      } else {
        if (x === selectedX && y === selectedY && !gameOver) {
          if (mouseIsPressed && mouseButton === LEFT) {
            if (grid[y][x].state === 'flag') {
              drawCovered(x, y);
            } else {
              drawUncovered(x, y);
            }
          } else {
            drawHighlighted(x, y);
          }
        } else {
          drawCovered(x, y);
        }
      }

      if (grid[y][x].flower && gameOver) {
        drawCellSymbol('🌷', x, y, 20);
      } else if (getSurroundingFlowerCount(x, y) > 0 && grid[y][x].state === 'uncovered') {
        drawNumber(x, y, getSurroundingFlowerCount(x, y));
      }

      if (grid[y][x].state === 'flag') {
        drawCellSymbol('🚩', x, y, 20);
      } else if (grid[y][x].state === 'question') {
        drawCellSymbol('❓', x, y, 20);
      }
    }
  }
}

function mouseReleased() {
  if (gameOver) {
    reset();
    return false;
  }

  if (mouseButton === LEFT && grid[selectedY][selectedX].state !== 'flag') {
    if (firstClick) {
      firstClick = false;
      plantFlowersAvoiding(selectedX, selectedY);
    }

    if (grid[selectedY][selectedX].flower) {
      grid[selectedY][selectedX].state = 'uncovered';
      gameOver = true;
    } else {
      const stack = [{ x: selectedX, y: selectedY }];

      while (stack.length > 0) {
        const current = stack.pop();
        const x = current.x;
        const y = current.y;

        if (grid[y][x].state === 'uncovered') {
          continue;
        }

        grid[y][x].state = 'uncovered';

        if (getSurroundingFlowerCount(x, y) === 0) {
          for (let dy = -1; dy <= 1; dy++) {
            for (let dx = -1; dx <= 1; dx++) {
              if (
                !(dx === 0 && dy === 0) &&
                y + dy >= 0 && y + dy < grid.length &&
                x + dx >= 0 && x + dx < grid[y + dy].length &&
                ['covered', 'question'].includes(grid[y + dy][x + dx].state)
              ) {
                stack.push({ x: x + dx, y: y + dy });
              }
            }
          }
        }
      }

      let complete = true;
      for (let y = 0; y < gridYCount; y++) {
        for (let x = 0; x < gridXCount; x++) {
          if (grid[y][x].state !== 'uncovered' && !grid[y][x].flower) {
            complete = false;
          }
        }
      }

      if (complete) {
        gameOver = true;
      }
    }
  } else if (mouseButton === RIGHT) {
    if (grid[selectedY][selectedX].state === 'covered') {
      grid[selectedY][selectedX].state = 'flag';
    } else if (grid[selectedY][selectedX].state === 'flag') {
      grid[selectedY][selectedX].state = 'question';
    } else if (grid[selectedY][selectedX].state === 'question') {
      grid[selectedY][selectedX].state = 'covered';
    }
  }

  return false;
}

function keyPressed() {
  reset();
}
```


# Uppgifter
## 1. Utvärdera ert eget arbete!
När ni svarar: tänk att ni får titta på uppgiften.

A. De här delarna har vi gjort. Vi förstår dem och kan förklara för Susanne eller klassen.

B. De här delarna har vi gjort, men vi förstår dem inte helt. Ge exempel på vad som är svårt.

## 2. Gör spelet ännu bättre
Lägg till något nytt. Till exempel:
- timer
- poäng
- ljud
- fler nivåer

Beskriv:
- vad ni ville göra
- hur spelaren märker ändringen
- vilken kod ni ändrade (med exempel)
- om det inte gick: vad ni testade och vad som hände

# Källor
https://simplegametutorials.github.io/pygamezero/flowers/
