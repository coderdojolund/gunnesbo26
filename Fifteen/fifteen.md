# Fifteen ⭐⭐⭐

## En handledning för JavaScript och Processing (editor.p5js.org)

![image](https://user-images.githubusercontent.com/4598641/226436083-7b0b90b9-9c13-4b83-bfd8-3534ac8291c0.png)

## Innehåll
**[Regler](#regler)**
 [Kontroller](#kontroller)
 &bull;
**[Översikt](#översikt)**
&bull;
**[Kodning](#kodning)**
 [Rita bitarna](#rita-bitarna)
 &bull; [Rita siffrorna](#rita-siffrorna)
 &bull; [Skapa rutnätet](#skapa-rutnätet)
 &bull; [Rita inte den tomma rutan](#rita-inte-den-tomma-rutan)
 &bull; [Hitta den tomma rutans position](#hitta-den-tomma-rutans-position)
 &bull; [Flytta en bit neråt](#flytta-en-bit-neråt)
 &bull; [Flytta en bit uppåt](#flytta-en-bit-uppåt)
 &bull; [Flytta bitar åt vänster och höger](#flytta-bitar-åt-vänster-och-höger)
 &bull; [Blanda bitarna](#blanda-bitarna)
 &bull; [Förenkla koden](#förenkla-koden)
 &bull; [Gör den nedre högra rutan tom](#gör-den-nedre-högra-rutan-tom)
 &bull; [Återställa spelet](#återställa-spelet)
 &bull; [Kolla om vi är klara](#kolla-om-vi-är-klara)
 &bull; [Förenkla koden](#förenkla-koden-1)
 &bull; [Blanda igen om det råkar vara klart direkt efter blandningen](#blanda-igen-om-det-råkar-vara-klart-direkt-efter-blandningen)
&bull;
**[Uppgifter](#uppgifter)**
   [1. Utvärdera ert eget arbete!](#1-utvärdera-ert-eget-arbete)
   &bull; [2. Pröva att göra några ändringar eller tillägg](#2-pröva-att-göra-några-ändringar-eller-tillägg)
   &bull; [3. Varför kan vi inte blanda bitarna helt slumpmässigt?](#3-varför-kan-vi-inte-blanda-bitarna-helt-slumpmässigt)
&bull;
**[Källor](#källor)**

## Jobba så här
Vägen till ett färdigt projekt är en pusselbit i taget 🧩. Gör därför ett avsnitt i taget uppifrån och ner.
- Få det att fungera innan ni går vidare till nästa avsnitt.
- Testkör ofta, efter varje avsnitt eller ännu oftare.

Var det något i beskrivningen som var svårt att förstå? Ta med det i redovisningen. 📝

# Regler

Det finns en tavla med 15 bitar och en tom ruta.
Flytta runt bitarna tills de är i nummerordning genom att använda piltangenterna för att flytta en bit i taget till den tomma rutan.

## Kontroller

**Piltangenter** Flytta en bit

**R** Starta om med ny blandning

# Översikt
Bitarna lagras som ett rutnät med siffror.

Siffran 16 representerar den tomma rutan.

![image](https://user-images.githubusercontent.com/4598641/226436258-85719c97-8e01-4aca-85b0-82d3cc184876.png)

Grannbiten flyttas till den tomma rutan när en piltangent trycks ned.

I början av spelet är bitarna i stigande nummerordning och slumpmässiga drag görs för att blanda dem.

Efter att en bit har flyttats, gås bitarna igenom. Om alla har sina ursprungliga värden i nummerordning är spelet över.

# Kodning

## Rita bitarna

Bitarna ritas som rutor.

Just nu ritas en bit där den tomma rutan ska vara.

✏️ Öppna https://editor.p5js.org/, välj **New Sketch** och jobba i filen `sketch.js`.
>Du ska inte logga in på p5.js. Spara istället din kod i ett dokument på Google Drive mellan lektionerna.

Testkör!

```javascript
function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(0);
  const pieceSize = 100;

  for (let y = 0; y < 4; y++) {
    for (let x = 0; x < 4; x++) {
      const pieceDrawSize = pieceSize - 1;
      noStroke();
      fill(100, 20, 150);
      rect(x * pieceSize, y * pieceSize, pieceDrawSize, pieceDrawSize);
    }
  }
}
```

![image](https://user-images.githubusercontent.com/4598641/226436463-1d10dd82-ed1c-429b-b0bc-e855b4969551.png)

## Rita siffrorna

Numren ritas ovanpå bitarna.

Bitens nummer beräknas genom att addera Y-koordinaten (dvs. radnummer) multiplicerat med antalet bitar i en rad till X-koordinaten plus 1.

Exempel: på den första raden är Y-koordinaten 0, så ingenting läggs till varje X-koordinat, så den första siffran på den första raden är 1.
På den andra raden läggs 4 till varje X-koordinat, så det första numret på den andra raden är 5.

Tänk på att koordinatsystemet för våra rutor har origo `(0, 0)` i övre vänstra hörnet. Den här typen av koordinatsystem är vanligt i datorgrafik.

![image](https://user-images.githubusercontent.com/4598641/228945276-453d6676-74ea-428e-850b-8033a408033a.png)

✏️ Uppdatera koden och testkör.

```javascript
function setup() {
  createCanvas(400, 400);
  textSize(60);
}

function draw() {
  background(0);
  const pieceSize = 100;

  for (let y = 0; y < 4; y++) {
    for (let x = 0; x < 4; x++) {
      const pieceDrawSize = pieceSize - 1;
      noStroke();
      fill(100, 20, 150);
      rect(x * pieceSize, y * pieceSize, pieceDrawSize, pieceDrawSize);

      fill(255);
      textAlign(LEFT, TOP);
      text(String(y * 4 + x + 1), x * pieceSize + 8, y * pieceSize + 8);
    }
  }
}
```

![image](https://user-images.githubusercontent.com/4598641/226436562-731e3960-4198-4bef-8635-e239557be6c9.png)

## Skapa rutnätet
Ett rutnät skapas med varje bits nummer lagrat på sin plats på rutnätet, och detta nummer ritas.

Antalet bitar på X- och Y-axlarna återanvänds från att rita bitarna, så de görs till variabler.

✏️ Uppdatera koden och testkör. Ritas bitarna rätt?

```javascript
let gridXCount = 4;
let gridYCount = 4;
let grid = [];

function setup() {
  createCanvas(400, 400);
  textSize(60);

  for (let y = 0; y < gridYCount; y++) {
    grid.push([]);
    for (let x = 0; x < gridXCount; x++) {
      grid[y].push(y * gridXCount + x + 1);
    }
  }
}

function draw() {
  background(0);
  const pieceSize = 100;

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      const pieceDrawSize = pieceSize - 1;

      noStroke();
      fill(100, 20, 150);
      rect(x * pieceSize, y * pieceSize, pieceDrawSize, pieceDrawSize);

      fill(255);
      textAlign(LEFT, TOP);
      text(String(grid[y][x]), x * pieceSize + 8, y * pieceSize + 8);
    }
  }
}
```

## Rita inte den tomma rutan
Om vi multiplicerar antalet bitar på varje axel får vi totala antalet bitar (dvs. 4 gånger 4 betyder 16 bitar). En bit ritas bara om numret är skilt från 16.

✏️ Uppdatera koden och testkör. Blir det en tom ruta nu?

```javascript
function draw() {
  background(0);
  const pieceSize = 100;

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x] === gridXCount * gridYCount) {
        continue;
      }

      const pieceDrawSize = pieceSize - 1;
      noStroke();
      fill(100, 20, 150);
      rect(x * pieceSize, y * pieceSize, pieceDrawSize, pieceDrawSize);

      fill(255);
      textAlign(LEFT, TOP);
      text(String(grid[y][x]), x * pieceSize + 8, y * pieceSize + 8);
    }
  }
}
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>

```javascript
let gridXCount = 4;
let gridYCount = 4;
let grid = [];

function setup() {
  createCanvas(400, 400);
  textSize(60);

  for (let y = 0; y < gridYCount; y++) {
    grid.push([]);
    for (let x = 0; x < gridXCount; x++) {
      grid[y].push(y * gridXCount + x + 1);
    }
  }
}

function draw() {
  background(0);
  const pieceSize = 100;

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x] === gridXCount * gridYCount) {
        continue;
      }

      const pieceDrawSize = pieceSize - 1;
      noStroke();
      fill(100, 20, 150);
      rect(x * pieceSize, y * pieceSize, pieceDrawSize, pieceDrawSize);

      fill(255);
      textAlign(LEFT, TOP);
      text(String(grid[y][x]), x * pieceSize + 8, y * pieceSize + 8);
    }
  }
}
```

</details>

![image](https://user-images.githubusercontent.com/4598641/226437183-8e41b05e-77bc-488b-b400-4785be077050.png)

## Hitta den tomma rutans position
Det första steget i att flytta en bit är att hitta positionen för den tomma rutan.

När en tangent trycks ned loopar vi igenom rutnätet. Om en bit är lika med antalet bitar på varje axel multiplicerat med varandra så är det den tomma rutan. Positionen skrivs ut i konsolen.

✏️ Lägg till funktionen `keyPressed()` och testkör genom att klicka i spelfönstret och sedan trycka någon tangent. Utskriften visas i p5.js-konsolen.
>Kom ihåg att klicka i spelfönstret när du startar spelet. Då kan spelet fånga upp tangenttryckningarna.

```javascript
function keyPressed() {
  let emptyX = 0;
  let emptyY = 0;

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x] === gridXCount * gridYCount) {
        emptyX = x;
        emptyY = y;
      }
    }
  }

  print(`empty x: ${emptyX}, empty y: ${emptyY}`);
}
```

Så här ska det se ut i konsolen:
```
empty x: 3, empty y: 3
```

## Flytta en bit neråt
Om Y-koordinaten för den tomma rutan är större än 0, betyder det att det finns en bit ovanför den tomma rutan, så det är möjligt att flytta en bit neråt.

Den tomma rutan ändras till bitnumret ovanför rutan. Biten ovanför rutan ändras till den lediga rutan (16).

Just nu flyttar vilken tangent som helst en bit ner.

✏️ Uppdatera koden och testkör genom att trycka på någon tangent. Flyttas biten neråt som den ska?
>Kom ihåg att klicka i spelfönstret när du startar spelet.

```javascript
function keyPressed() {
  let emptyX = 0;
  let emptyY = 0;

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x] === gridXCount * gridYCount) {
        emptyX = x;
        emptyY = y;
      }
    }
  }

  if (emptyY > 0) {
    const changed = [grid[emptyY][emptyX], grid[emptyY - 1][emptyX]];
    grid[emptyY - 1][emptyX] = changed[0];
    grid[emptyY][emptyX] = changed[1];
  }
}
```

## Flytta en bit uppåt
Om Y-koordinaten för den tomma rutan är mindre än antalet rader i rutnätet, betyder det att det finns en bit under den tomma rutan.
Den biten kan då flyttas upp.

Y-koordinaten för den bit som den tomma rutan byter med görs till en variabel.
När upp-tangenten trycks in ställs den till positionen under den tomma rutan, alltså ett steg ner på Y-axeln.

✏️ Ändra koden i `keyPressed()` och testkör med pil upp och ner.

```javascript
function keyPressed() {
  let emptyX = 0;
  let emptyY = 0;

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x] === gridXCount * gridYCount) {
        emptyX = x;
        emptyY = y;
      }
    }
  }

  let newEmptyY = emptyY;

  if (keyCode === DOWN_ARROW) {
    newEmptyY -= 1;
  } else if (keyCode === UP_ARROW) {
    newEmptyY += 1;
  }

  if (0 <= newEmptyY && newEmptyY < gridYCount) {
    const changed = [grid[emptyY][emptyX], grid[newEmptyY][emptyX]];
    grid[newEmptyY][emptyX] = changed[0];
    grid[emptyY][emptyX] = changed[1];
  }
}
```

## Flytta bitar åt vänster och höger
X-koordinaten för den bit som den tomma rutan byter med görs till en variabel. Den ändras när vänster- eller högerpilen trycks ned.

✏️ Uppdatera koden i `keyPressed()` och testkör med alla fyra piltangenterna.

```javascript
function keyPressed() {
  let emptyX = 0;
  let emptyY = 0;

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x] === gridXCount * gridYCount) {
        emptyX = x;
        emptyY = y;
      }
    }
  }

  let newEmptyY = emptyY;
  let newEmptyX = emptyX;

  if (keyCode === DOWN_ARROW) {
    newEmptyY -= 1;
  } else if (keyCode === UP_ARROW) {
    newEmptyY += 1;
  } else if (keyCode === RIGHT_ARROW) {
    newEmptyX -= 1;
  } else if (keyCode === LEFT_ARROW) {
    newEmptyX += 1;
  }

  if (
    0 <= newEmptyY && newEmptyY < gridYCount &&
    0 <= newEmptyX && newEmptyX < gridXCount
  ) {
    const changed = [grid[emptyY][emptyX], grid[newEmptyY][newEmptyX]];
    grid[newEmptyY][newEmptyX] = changed[0];
    grid[emptyY][emptyX] = changed[1];
  }
}
```

![image](https://user-images.githubusercontent.com/4598641/226437516-322ed925-e617-41a9-94a7-fc3e9329aeeb.png)

## Blanda bitarna
I början av spelet görs ett antal slumpmässiga drag för att blanda brädet.

Ett slumptal mellan 0 och 3 genereras och ett drag görs i en av de fyra rörelseriktningarna baserat på detta nummer.

✏️ Uppdatera koden och testkör. Vi återanvänder kod från `keyPressed()`. Blandas bitarna?

```javascript
let gridXCount = 4;
let gridYCount = 4;
let grid = [];

function move(direction) {
  let emptyX = 0;
  let emptyY = 0;

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x] === gridXCount * gridYCount) {
        emptyX = x;
        emptyY = y;
      }
    }
  }

  let newEmptyY = emptyY;
  let newEmptyX = emptyX;

  if (direction === 'down') {
    newEmptyY -= 1;
  } else if (direction === 'up') {
    newEmptyY += 1;
  } else if (direction === 'right') {
    newEmptyX -= 1;
  } else if (direction === 'left') {
    newEmptyX += 1;
  }

  if (
    0 <= newEmptyY && newEmptyY < gridYCount &&
    0 <= newEmptyX && newEmptyX < gridXCount
  ) {
    const changed = [grid[emptyY][emptyX], grid[newEmptyY][newEmptyX]];
    grid[newEmptyY][newEmptyX] = changed[0];
    grid[emptyY][emptyX] = changed[1];
  }
}

function setup() {
  createCanvas(400, 400);
  textSize(60);

  for (let y = 0; y < gridYCount; y++) {
    grid.push([]);
    for (let x = 0; x < gridXCount; x++) {
      grid[y].push(y * gridXCount + x + 1);
    }
  }

  for (let moveNumber = 0; moveNumber < 1000; moveNumber++) {
    const directions = ['down', 'up', 'right', 'left'];
    move(random(directions));
  }
}

function keyPressed() {
  if (keyCode === DOWN_ARROW) {
    move('down');
  } else if (keyCode === UP_ARROW) {
    move('up');
  } else if (keyCode === RIGHT_ARROW) {
    move('right');
  } else if (keyCode === LEFT_ARROW) {
    move('left');
  }
}
```

![image](https://user-images.githubusercontent.com/4598641/226437586-c1a482c0-b465-4214-822a-68f8b2530839.png)

## Förenkla koden
Den enda skillnaden mellan blandningskoden och koden i `keyPressed()` är hur riktningen för förflyttningen bestäms.
Vi gör en funktion med riktningen som parameter.

✏️ Uppdatera koden och testkör. Fungerar den som innan?

```javascript
function move(direction) {
  // samma kod som i förra avsnittet
}

function keyPressed() {
  if (keyCode === DOWN_ARROW) {
    move('down');
  } else if (keyCode === UP_ARROW) {
    move('up');
  } else if (keyCode === RIGHT_ARROW) {
    move('right');
  } else if (keyCode === LEFT_ARROW) {
    move('left');
  }
}

for (let moveNumber = 0; moveNumber < 1000; moveNumber++) {
  move(random(['down', 'up', 'right', 'left']));
}
```

## Gör den nedre högra rutan tom
Den tomma rutan ska vara längst ner till höger när spelet börjar. Därför flyttas bitarna åt vänster och uppåt flera gånger.
Antalet bitar på en axel minus 1 är det maximala antalet drag det skulle ta för att flytta den tomma rutan från ena sidan till den andra.

✏️ Den nya koden gör samma sak som tre tryck på vänsterpil och tre på uppåtpil. Testa med piltangenterna. Hamnar den tomma rutan längst ner till höger?
Uppdatera koden och testkör.

```javascript
for (let moveNumber = 0; moveNumber < 1000; moveNumber++) {
  move(random(['down', 'up', 'right', 'left']));
}

for (let moveNumber = 0; moveNumber < gridXCount - 1; moveNumber++) {
  move('left');
}

for (let moveNumber = 0; moveNumber < gridYCount - 1; moveNumber++) {
  move('up');
}
```

![image](https://user-images.githubusercontent.com/4598641/226437694-caf5182b-39da-41bf-95d7-4ef3c098baf8.png)

## Återställa spelet
Vi gör en funktion som ställer in spelets startläge.

Den funktionen anropas innan spelet börjar och när vi trycker på tangenten `R`.

✏️ Uppdatera koden och testkör. Blir det en ny spelplan när du trycker på `R`?

```javascript
function reset() {
  grid = [];

  for (let y = 0; y < gridYCount; y++) {
    grid.push([]);
    for (let x = 0; x < gridXCount; x++) {
      grid[y].push(y * gridXCount + x + 1);
    }
  }

  for (let moveNumber = 0; moveNumber < 1000; moveNumber++) {
    move(random(['down', 'up', 'right', 'left']));
  }

  for (let moveNumber = 0; moveNumber < gridXCount - 1; moveNumber++) {
    move('left');
  }

  for (let moveNumber = 0; moveNumber < gridYCount - 1; moveNumber++) {
    move('up');
  }
}

function setup() {
  createCanvas(400, 400);
  textSize(60);
  reset();
}

function keyPressed() {
  if (keyCode === DOWN_ARROW) {
    move('down');
  } else if (keyCode === UP_ARROW) {
    move('up');
  } else if (keyCode === RIGHT_ARROW) {
    move('right');
  } else if (keyCode === LEFT_ARROW) {
    move('left');
  } else if (key === 'r' || key === 'R') {
    reset();
  }
}
```

## Kolla om vi är klara
Efter varje drag loopar vi genom bitarna. Om alla bitarna är på rätt position i nummerordning så återställs spelet.

✏️ Uppdatera koden i `keyPressed()` och testkör. Vad händer om du lägger alla bitarna i nummerordning?

```javascript
function keyPressed() {
  if (keyCode === DOWN_ARROW) {
    move('down');
  } else if (keyCode === UP_ARROW) {
    move('up');
  } else if (keyCode === RIGHT_ARROW) {
    move('right');
  } else if (keyCode === LEFT_ARROW) {
    move('left');
  } else if (key === 'r' || key === 'R') {
    reset();
  }

  let complete = true;

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x] !== y * gridXCount + x + 1) {
        complete = false;
      }
    }
  }

  if (complete) {
    reset();
  }
}
```

## Förenkla koden
Koden för att beräkna startvärdet för en bit återanvänds.
Vi gör den koden till en egen funktion.

✏️ Uppdatera koden och testkör! Lägg till funktionen `getInitialValue` och anropa den i funktionerna `reset` och `keyPressed`. Fungerar spelet som innan?

```javascript
function getInitialValue(x, y) {
  return y * gridXCount + x + 1;
}

function reset() {
  grid = [];

  for (let y = 0; y < gridYCount; y++) {
    grid.push([]);
    for (let x = 0; x < gridXCount; x++) {
      grid[y].push(getInitialValue(x, y));
    }
  }

  // etc.
}

function keyPressed() {
  // etc.

  let complete = true;
  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x] !== getInitialValue(x, y)) {
        complete = false;
      }
    }
  }

  if (complete) {
    reset();
  }
}
```

## Blanda igen om det råkar vara klart direkt efter blandningen

Om bitarna skulle råka hamna i rätt ordning efter att de har blandats, försöker vi igen.

Koden för att kontrollera om bitarna är i sin ursprungliga ordning återanvänds. Vi gör det till en funktion.

✏️ Uppdatera koden för `isComplete`, `reset` och `keyPressed` och testkör. Vad händer när du lagt alla bitarna i nummerordning?

```javascript
let gridXCount = 4;
let gridYCount = 4;
let grid = [];

function getInitialValue(x, y) {
  return y * gridXCount + x + 1;
}

function move(direction) {
  let emptyX = 0;
  let emptyY = 0;

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x] === gridXCount * gridYCount) {
        emptyX = x;
        emptyY = y;
      }
    }
  }

  let newEmptyY = emptyY;
  let newEmptyX = emptyX;

  if (direction === 'down') {
    newEmptyY -= 1;
  } else if (direction === 'up') {
    newEmptyY += 1;
  } else if (direction === 'right') {
    newEmptyX -= 1;
  } else if (direction === 'left') {
    newEmptyX += 1;
  }

  if (
    0 <= newEmptyY && newEmptyY < gridYCount &&
    0 <= newEmptyX && newEmptyX < gridXCount
  ) {
    const changed = [grid[emptyY][emptyX], grid[newEmptyY][newEmptyX]];
    grid[newEmptyY][newEmptyX] = changed[0];
    grid[emptyY][emptyX] = changed[1];
  }
}

function isComplete() {
  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x] !== getInitialValue(x, y)) {
        return false;
      }
    }
  }

  return true;
}

function reset() {
  grid = [];

  for (let y = 0; y < gridYCount; y++) {
    grid.push([]);
    for (let x = 0; x < gridXCount; x++) {
      grid[y].push(getInitialValue(x, y));
    }
  }

  while (true) {
    for (let moveNumber = 0; moveNumber < 1000; moveNumber++) {
      move(random(['down', 'up', 'right', 'left']));
    }

    for (let moveNumber = 0; moveNumber < gridXCount - 1; moveNumber++) {
      move('left');
    }

    for (let moveNumber = 0; moveNumber < gridYCount - 1; moveNumber++) {
      move('up');
    }

    if (!isComplete()) {
      break;
    }
  }
}

function setup() {
  createCanvas(400, 400);
  textSize(60);
  reset();
}

function keyPressed() {
  if (keyCode === DOWN_ARROW) {
    move('down');
  } else if (keyCode === UP_ARROW) {
    move('up');
  } else if (keyCode === RIGHT_ARROW) {
    move('right');
  } else if (keyCode === LEFT_ARROW) {
    move('left');
  } else if (key === 'r' || key === 'R') {
    reset();
  }

  if (isComplete()) {
    reset();
  }
}

function draw() {
  background(0);
  const pieceSize = width / gridXCount;

  for (let y = 0; y < gridYCount; y++) {
    for (let x = 0; x < gridXCount; x++) {
      if (grid[y][x] === gridXCount * gridYCount) {
        continue;
      }

      const pieceDrawSize = pieceSize - 1;
      noStroke();
      fill(100, 20, 150);
      rect(x * pieceSize, y * pieceSize, pieceDrawSize, pieceDrawSize);

      fill(255);
      textAlign(LEFT, TOP);
      textSize(pieceSize * 0.55);
      text(String(grid[y][x]), x * pieceSize + pieceSize * 0.08, y * pieceSize + pieceSize * 0.08);
    }
  }
}
```

# Uppgifter
## 1. Utvärdera ert eget arbete!
När ni svarar på detta, tänk på att *ni har tillgång till uppgiften* – ni behöver alltså inte kunna koden utantill.

A. De här delarna av uppgiften har vi gjort. Vi förstår dem och kan förklara koden för Susanne eller inför klassen.

B. De här delarna av uppgiften har vi gjort *men vi förstår dem inte till 100%*. Ge exempel på något ni inte förstår.

## 2. Pröva att göra några ändringar eller tillägg
- Beskriv kort med ord eller bilder vad förändringen är och hur den ska fungera.
- Beskriv hur du fick ändra koden för att göra ändringen.
- Om det inte gick att genomföra, förklara med några meningar vad du försökte och vad som hände. Glöm inte kodexempel.

Kanske programmet kan ta tiden för att lösa pusslet? När alla bitar är på plats kan man visa tiden och gratulera spelaren.

## 3. Varför kan vi inte blanda bitarna helt slumpmässigt?
I funktionen `reset()` har vi den här koden:
```
for (let moveNumber = 0; moveNumber < 1000; moveNumber++) {
  move(random(['down', 'up', 'right', 'left']));
}
```

Varför behöver vi göra så när vi ställer upp brickorna på spelplanen? Kan vi inte bara slumpa fram vilken position som helst?

# Källor

Översatt och bearbetat baserat på originalet: https://simplegametutorials.github.io/pygamezero/fifteen/
