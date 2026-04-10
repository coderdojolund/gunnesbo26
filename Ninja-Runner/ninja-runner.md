**Innehåll:** [Bakgrund](#bakgrund) &bull;
[Ninjan](#ninjan) &bull;
[Hoppa](#hoppa) &bull;
[Hinder](#hinder) &bull;
[Poängräkning](#poängräkning) &bull;
[Game Over](#game-over) &bull;
[Uppgifter](#uppgifter) &bull;
[Källor](#källor)

# Infinite Runner ⭐⭐

![image](https://www.aposteriori.com.sg/wp-content/uploads/2020/02/google-runner.gif)

Ett spel av typen Infinite/Endless Runner tar aldrig slut. Målet är att överleva så långt som möjligt och ju längre, desto högre poäng får du.

För att göra vårt spel ska vi använda vad vi lärt oss från spelet [Gem Catcher](https://github.com/coderdojolund/Python-8/blob/main/Gem-Catcher/gem-catcher.md) och lägga till
- ritning
- animering
- fysik
- listor med figurer (aktörer).

## Jobba så här
Vägen till ett färdigt projekt är en pusselbit i taget 🧩. Gör därför ett avsnitt i taget uppifrån och ner.
- Få det att fungera innan ni går vidare till nästa avsnitt. 
- Testkör ofta, efter varje avsnitt eller ännu oftare.

Var det något i beskrivningen som var svårt att förstå? Ta med det i redovisningen. 📝

## Nytt projekt
Gå till https://editor.p5.js och ta bort exempelkoden.

# Bakgrund
## Första steget
Precis som i [Gem Catcher](https://github.com/coderdojolund/Python-8/blob/main/Gem-Catcher/gem-catcher.md) börjar vi koda vårt spel genom att sätta grafikfönstrets bredd och höjd.

```javascript
function setup() {
    createCanvas(600, 450);
}
```

✏️ Testkör!

## Rita
Vi kan rita figurer med olika funktioner. Innan vi ritar en form måste vi tala om vilken färg den ska ha med fill(). Här är några vanliga funktioner:
- `line()`
- `circle()`
- `rect()`
- `ellipse()`
- `triangle()`

För att lära dig mer kan du läsa i [dokumentationen för p5.js](https://p5js.org/reference/).

Vi ska börja med `rect()`. Den ritar en rektangel på skärmen. Vi lägger till koden i funktionen `draw()`.

✏️ Uppdatera koden så att den blir så här:

```javascript
function setup() {
  createCanvas(600, 450);
}

function draw() {
  noStroke(); // Tar bort kanten runt formerna
  fill(163, 232, 254);
  rect(0, 0, width, height - 200);
}
```

Det här gör de nya raderna:

- `function draw() { ... }`: Detta är en funktion som p5.js kör automatiskt cirka 60 gånger i sekunden. Allt som ska visas på skärmen skrivs här.
- `fill(163, 232, 254)`: Detta sätter färgen för de former som ritas efteråt. Det är en blandning av 163 delar röd, 232 grön och 254 blå.
- `rect(0, 0, width, height - 200)`: Detta ritar en rektangel. De två första siffrorna är koordinaterna för det övre vänstra hörnet (0, 0). `width` är ritytans totala bredd (600). Den sista delen sätter höjden till ritytans höjd minus 200 pixlar.

✏️ **Testa att experimentera med olika färger och se vad du får. Kom ihåg att varje färgkomponent ska vara mellan 0 och 255.**

Så här ser det ut när jag testar:

![image](https://user-images.githubusercontent.com/4598641/235426777-440c48fd-7882-43e0-af99-1b1327ebb411.png)

## Marken
Steget innan ritade himlen. Vi lägger till en andra rektangel för att rita marken.

```javascript
function setup() {
  createCanvas(600, 450);
}

function draw() {
  noStroke();
  
  // Himlen
  fill(163, 232, 254);
  rect(0, 0, width, height - 200);
  
  // Marken
  fill(88, 242, 152);
  rect(0, height - 200, width, 200);
}
```
Det här gör den nya raden:

- `rect(0, height - 200, width, 200)`: Den här rektangeln börjar där himlen slutar. Y-koordinaten är `height - 200`. Den är lika bred som hela ritytan och är 200 pixlar hög.

✏️ Mata in detta och kör din kod. Din skärm kan se ut så här:

![image](https://user-images.githubusercontent.com/4598641/223218919-66ee7ecf-043f-4c08-a775-074ff487bb3c.png)



# Ninjan

## Animering
Vi gör en enkel animering genom att byta mellan två olika emoji. 
Jämför med din kod och lägg till det nya.
- Du behöver inte lägga till förklaringarna som börjar med `//` 

```javascript
let WIDTH = 600;
let HEIGHT = 450;
let groundY;

// Ninja-variabler
let runnerX = 100;
let runnerY;
let emojiSize = 80;

// NYTT: Lista med emojis för animeringen
let runEmojis = ["🏃", "💨"]; 

function setup() {
  createCanvas(WIDTH, HEIGHT);
  groundY = height - 200;
  runnerY = groundY; 
  
  textSize(emojiSize);
  textAlign(CENTER, CENTER);
}

function draw() {
  noStroke();
  
  // Himlen
  fill(163, 232, 254);
  rect(0, 0, width, groundY);
  
  // Marken
  fill(88, 242, 152);
  rect(0, groundY, width, 200);
  
  // --- NYTT: Animeringslogik ---
  
  // Vi ritar figuren på skärmen.
  // Vi använder frameCount för att räkna ut vilken emoji som ska visas.
  
  // frameCount ökar med 1 för varje bildruta (ca 60 gånger per sekund).
  // Genom att dividera med 10 och avrunda nedåt (floor) byter vi emoji långsammare.
  let frameIndex = floor(frameCount / 10); 
  
  // % (modulo) ser till att indexet växlar mellan 0 och 1 (längden på listan).
  let currentEmojiIndex = frameIndex % runEmojis.length;
  
  // Hämta den aktuella emojin från listan
  let currentEmoji = runEmojis[currentEmojiIndex];
  
  // Rita den aktuella emojin
  text(currentEmoji, runnerX, runnerY);
}
```

✏️ Testkör!

### Vill du använda egna bilder?

* Leta upp andra emoji som du vill använda. Du kan använda fler än två. Då behöver animeringslogiken ändras lite.

### Justera placeringen

Du kan justera ninjas placering med `runner.x` och `runner.y`.

✏️ Pröva detta:

```javascript
runnerX = 100
runnerY = HEIGHT - 200
```
✏️ **Pröva att justera ninjans position genom att ändra `runner.x` och `runner.y` tills hen är där du vill.**

# Hoppa

## Fysik

Vilken av de här ser mest realistisk ut?

![image](https://www.aposteriori.com.sg/wp-content/uploads/2020/02/jump.gif)

I verkliga livet påverkas föremål av gravitationen. För att få vår ninja att hoppa realistiskt, behöver vi simulera gravitationens effekter i vårt spel.

✏️ Vi börjar med att lägga till variabler för `velocity_y` och `gravity`, alltså hastigheten i y-led och gravitationen.

```python
velocity_y = 0
gravity = 1
```

Förklaring av kodraderna:

`velocity_y = 0` : Håller reda på hur snabbt ninjan ska röra sig uppåt eller neråt. Hastigheten börjar med 0 eftersom ninjan inte hoppar än.

`gravity = 1` : Gravitationen påverkar hastigheten. Vi kan ändra detta senare och se effekten av det, men just nu låter vi värdet vara 1. 

✏️ I `update()` ska vi sen ändra hastigheten när uppåtpilen trycks ner.

```python
def update():
  global velocity_y
  runner.next_image()

  if keyboard.up:
    velocity_y = -15

  runner.y += velocity_y
```

Det här gör raderna:

`global velocity_y` : Vi behöver använda `global` om vi ändrar på en variabel som finns utanför funktionen.

`if keyboard.up:` : När tangenten med uppåtpil trycks ner …

`velocity_y = -15` : Sätt hastigheten upp/ner till &ndash;15. Ett negativt värde betyder att den rör sig uppåt.

`runner.y += velocity_y` :  Ändra vår ninjas position baserat på hastigheten. Operatorn `+=` betyder att vi ökar `runner.y` med värdet i `velocity_y`.

✏️ Testa det! Om du programmerat rätt, bör ninjan flyga upp i himlen när du trycker uppåtpil. Det är för att vi inte har lagt till någon gravitation än!
>Funkar inte uppåtpil? Kom ihåg att klicka i spelfönstret när du startat spelet med *Run*.

## Gravitation
Gravitationen ändrar ninjans hastighet. 

✏️ Under raden `runner.y += velocity_y` lägger vi till gravitionen med `velocity_y += gravity`.

Nu ramlar vår ninja rakt ner! Vi har inte talat om för ninjan när den ska sluta falla! Vi lägger till det nu:

```python
if runner.y > HEIGHT - 200:
  velocity_y = 0
  runner.y = HEIGHT - 200
```
Här bestämmer vi att `HEIGHT - 200` är där marken börjar och om ninjan är på en y-koordinat som är större än `HEIGHT - 200` så sätter vi hennes `velocity_y` till 0 och y-koordinaten till `HEIGHT - 200`. Detta hindrar att hon faller igenom marken.

✏️ Testkör!

Ditt program bör nu se ut så här:
```python
import pgzrun
from pgzhelper import *

WIDTH, HEIGHT = 600, 450

runner = Actor('run__000')
run_images = ['run__000', 'run__001', 'run__002', 'run__003', 'run__004', 'run__005', 'run__006', 'run__007', 'run__008', 'run__009']
runner.images = run_images
runner.x = 100
runner.y = HEIGHT - 200

velocity_y = 0
gravity = 1

def update():
  global velocity_y
  runner.next_image()

  if keyboard.up:
    velocity_y = -15

  runner.y += velocity_y
  velocity_y += gravity
  if runner.y > HEIGHT - 200:
    velocity_y = 0
    runner.y = HEIGHT - 200

def draw():
  screen.draw.filled_rect(Rect(0, 0, WIDTH, HEIGHT - 200), (163, 232, 254))
  screen.draw.filled_rect(Rect(0, HEIGHT - 200, WIDTH, 200), (88, 242, 152))
  runner.draw()

pgzrun.go() # Måste vara sista raden
```

# Hinder
## En lista med figurer

I vårt [Gem Catcher-spel](https://github.com/coderdojolund/Python-8/blob/main/Gem-Catcher/gem-catcher.md) har vi bara en ädelsten i taget och den flyttar sig till toppen av skärmen varje gång vi fångar den.
I ninjaspelet kommer vi att flera hinder på skärmen samtidigt.
För att klara det behöver vi använda **listor**.

![image](https://user-images.githubusercontent.com/4598641/223222660-26ee39e0-5420-47ba-8ef9-d4cb855636fe.png)

✏️ Först skapar vi en tom lista som heter `obstacles` och en heltalsvariabel `obstacles_timeout`.

```python
obstacles = []
obstacles_timeout = 0
```
✏️ I vår funktion `update()` ska vi räkna upp `timeout` med 1 varje gång.
```python
obstacles_timeout += 1
```
✏️ Och om `timeout` är större än 50 lägger vi till ett hinder och sätter `timeout` till 0.

```python
if obstacles_timeout > 50:
  actor = Actor('cactus')
  actor.x = WIDTH + 50
  actor.y = HEIGHT - 170
  obstacles.append(actor)
  obstacles_timeout = 0
```

Det enda nya här är `obstacles.append(actor)`. Det lägger till figuren `actor` i hinderlistan, `obstacles`.

**VIKTIGT: Du behöver ha bilden med kaktusen i mappen images. Om du vill använda en annan bild, glöm inte att byta bildnamnet i koden också.**

✏️ För att få hindren att röra sig över skärmen:
```python
for actor in obstacles:
  actor.x -= 8
```
Detta går igenom hela listan `obstacles` och minskar x-koordinaten för varje hinder. Att minska *x* gör att hindret rör sig åt vänster.

✏️ Till slut behöver vi rita hindren på skärmen. Lägg till det här i `draw()`-funktionen:
```python
for actor in obstacles:
  actor.draw()
```

✏️ Testkör din kod!

Nu ska ditt program se ut ungefär så här:

```python
import pgzrun
from pgzhelper import *

WIDTH, HEIGHT = 600, 450

runner = Actor('run__000')
run_images = ['run__000', 'run__001', 'run__002', 'run__003', 'run__004', 'run__005', 'run__006', 'run__007', 'run__008', 'run__009']
runner.images = run_images
runner.x = 100
runner.y = HEIGHT - 200

velocity_y = 0
gravity = 1

obstacles = []
obstacles_timeout = 0

def update():
  global velocity_y, obstacles_timeout
  runner.next_image()

  obstacles_timeout += 1
  if obstacles_timeout > 50:
    actor = Actor('cactus')
    actor.x = WIDTH + 50
    actor.y = HEIGHT - 170
    obstacles.append(actor)
    obstacles_timeout = 0

  for actor in obstacles:
    actor.x -= 8

  if keyboard.up:
    velocity_y = -15

  runner.y += velocity_y
  velocity_y += gravity
  if runner.y > HEIGHT - 200:
    velocity_y = 0
    runner.y = HEIGHT - 200

def draw():
  screen.draw.filled_rect(Rect(0, 0, WIDTH, HEIGHT - 200), (163, 232, 254))
  screen.draw.filled_rect(Rect(0, HEIGHT - 200, WIDTH, 200), (88, 242, 152))
  runner.draw()
  for actor in obstacles:
    actor.draw()

pgzrun.go() # Måste vara sista raden
```

# Poängräkning

## Poängen

Precis som i [Gem Catcher](https://github.com/coderdojolund/Python-8/blob/main/Gem-Catcher/gem-catcher.md), använder vi en variabel `score` för att hålla reda på  poängställningen.

✏️ Uppdatera koden.
```python
score = 0
```
✏️ Vi ökar poängen varje gång ett hinder försvinner ut åt vänster. Ändra det här inuti `update()`, från
```python
  for actor in obstacles:
    actor.x -= 8
```
… till detta:
```python
  for actor in obstacles:
    actor.x -= 8
    if actor.x < -50:
      obstacles.remove(actor)
      score += 1
```
Detta är vad raderna betyder:

`if actor.x < -50` : När x-koordinaten är mindre än &ndash;50 är figuren förmodligen utanför skärmen. Då kommer vi att …

`obstacles.remove(actor)` : ta bort figuren från listan `obstacles` och sen

`score += 1` : öka poängen med 1. Kom ihåg att deklarera `score` som global variabel!

✏️ Testkör!

## Rita hinder och skriva ut poängen
Hindren ritas inte om vi glömmer att göra det i `draw()`. 

✏️ Vi lägger till detta:
```python
for actor in obstacles:
  actor.draw()
```
Det går igenom listan med hinder och ritar dem ett i taget.

✏️ Vi behöver också visa poängen på skärmen så här:
```python
screen.draw.text(f"Score: {score}", (15, 10), color=(0,0,0), fontsize=30)
```
✏️ Ändra gärna färg, placering eller teckenstorleken.


# Game Over
Just nu gör spelet inget även om vår ninja krockar med kaktusen. Vi lägger till ett game over-läge. Om ninjan rör något av hindren så avslutar vi spelet.

✏️ Först lägger vi till en variabel `game_over` och sätter den till `False` i början.

```python
game_over = False
```

✏️ Inuti funktionen `update()` ska vi upptäcka om vår ninja har krockat med något av hindren. Om hon gjorde det så sätter vi `game_over` till `True`.

```python
if runner.collidelist(obstacles) != -1:
  game_over = True
```
✏️ **Viktigt: kom ihåg att deklarera `game_over` som global i funktionen `update()`**

Frågan `runner.collidelist(obstacles)` kollar om ninjan har krockat med något av hindren i listan `obstacles`.
Om hon inte gjorde det, ger funktionen `collidelist` värdet &ndash;1.

Sen behöver vi skriva texten *Game over* inuti `draw()` genom att ändra från
```python
  runner.draw()
  for actor in obstacles:
    actor.draw()
  screen.draw.text(f"Score: {score}", (15, 10), color=(0,0,0), fontsize=30)
```
✏️ till detta:
```python
if game_over:
  screen.draw.text('Game Over', centerx=WIDTH/2, centery=HEIGHT - 330, color=(255, 255, 255), fontsize=60)
  screen.draw.text(f"Score: {score}", centerx=WIDTH/2, centery=330, color=(255, 255, 255), fontsize=60)
else:
  runner.draw()
  for actor in obstacles:
    actor.draw()
  screen.draw.text(f"Score: {score}", (15, 10), color=(0, 0, 0), fontsize=30)
```

✏️ Uppdatera och testkör!

Ditt spel bör se ut så här till slut:

```python
import pgzrun
from pgzhelper import *

WIDTH, HEIGHT = 600, 450

runner = Actor('run__000')
run_images = ['run__000', 'run__001', 'run__002', 'run__003', 'run__004', 'run__005', 'run__006', 'run__007', 'run__008', 'run__009']
runner.images = run_images
runner.x = 100
runner.y = HEIGHT - 200

velocity_y = 0
gravity = 1

obstacles = []
obstacles_timeout = 0

score = 0
game_over = False

def update():
  global velocity_y, obstacles_timeout, score, game_over
  runner.next_image()

  obstacles_timeout += 1
  if obstacles_timeout > 50:
    actor = Actor('cactus')
    actor.x = WIDTH + 50
    actor.y = HEIGHT - 170
    obstacles.append(actor)
    obstacles_timeout = 0

  for actor in obstacles:
    actor.x -= 8
    if actor.x < -50:
      obstacles.remove(actor)
      score += 1

  if keyboard.up:
    velocity_y = -15

  runner.y += velocity_y
  velocity_y += gravity
  if runner.y > HEIGHT - 200:
    velocity_y = 0
    runner.y = HEIGHT - 200

  if runner.collidelist(obstacles) != -1:
    game_over = True

def draw():
  screen.draw.filled_rect(Rect(0, 0, WIDTH, HEIGHT - 200), (163, 232, 254))
  screen.draw.filled_rect(Rect(0, HEIGHT - 200, WIDTH, 200), (88, 242, 152))
  if game_over:
    screen.draw.text('Game Over', centerx=WIDTH/2, centery=HEIGHT - 330, color=(255, 255, 255), fontsize=60)
    screen.draw.text(f"Score: {score}", centerx=WIDTH/2, centery=330, color=(255, 255, 255), fontsize=60)
  else:
    runner.draw()
    for actor in obstacles:
      actor.draw()
    screen.draw.text(f"Score: {score}", (15, 10), color=(0, 0, 0), fontsize=30)

pgzrun.go() # Måste vara sista raden
```

# Uppgifter

Det är vanligt att program har buggar. Jag har avsiktligt lämnat kvar ett par buggar i vårt ninjaspel. Har du hittat några än? Pröva att rätta dem! Redovisa hur du gjorde.

## Uppgift 1: Buggfix 1. Game Over-poäng
Spela spelet, låt din ninja krocka med en kaktus och kolla sen poängräknaren efter game over … Såg du att poängen fortsatte öka? Det händer eftersom vi fortsätter lägga till kaktusar i hinderlistan efter game over. Kan du fixa det?

## Uppgift 2: Buggfix 2. Multihopp
Pröva att snabbt trycka på hoppknappen flera gånger. Hoppade din ninja upp ovanför det som syns på skärmen? Ninjan ska bara kunna hoppa när hon är på marken och inte i luften. Kan du fixa det?

## Uppgift 3: Utvärdera ert eget arbete!
När ni svarar på detta, tänk er att *ni har tillgång till uppgiften* &ndash; ni behöver alltså inte kunna koden utantill.

**3A.** De här delarna av uppgiften har vi gjort. Vi förstår dem och kan förklara koden för Susanne eller inför klassen.

**3B.** De här delarna av uppgiften har vi gjort *men vi förstår dem inte till 100%*. Ge exempel på något ni inte förstår.

## Uppgift 4
Välj och gör minst en av utmaningarna 1, 2, 3 här nedanför. Redovisa så här:
- Beskriv kort vad förändringen är och hur den ska fungera.
- Beskriv hur du fick ändra koden för att göra ändringen.
- Om det inte gick att genomföra, förklara med några meningar vad du försökte och vad som hände. Glöm inte kodexempel. 

### Utmaning 1. Glidande ninja
Kan du ändra ninjabilden till en glidande ninja när hon hoppar? Om du använder en annan figur så har de flesta en liknande bild med hopp som du kan använda:

![image](https://user-images.githubusercontent.com/4598641/223225545-64334e74-b6e3-4511-8f25-8128512012d8.png)
![image](https://user-images.githubusercontent.com/4598641/223225411-f124b452-0956-44a9-bbe2-1e9936c87552.png)
![image](https://user-images.githubusercontent.com/4598641/223225445-7663895c-45e2-4237-95c4-7dfaef60d46f.png)


### Utmaning 2. Olika hinder
Det enda hindret just nu är kaktusen. Kan du göra så att programmet väljer bland olika slags hinder varje gång?

![image](https://user-images.githubusercontent.com/4598641/223225610-600aa71d-9f7d-4d51-bc0d-745b8442470b.png)
![image](https://user-images.githubusercontent.com/4598641/223225631-7523def5-ca93-4bb9-a7d1-df6ad8b6e06c.png)
![image](https://user-images.githubusercontent.com/4598641/223225654-7930549a-1186-4d7e-8ae7-572e8670cd5d.png)

### Utmaning 3. Sprid ut hindren
Som spelet är nu dyker hindren alltid upp på samma avstånd från varann. Kan du slumpa till det så att hindren ibland är närmare och ibland längre från varann?
>Kom ihåg att göra `import random` så kan du använda t.ex. `random.randint(min, max + 1)` för att få ett slumptal mellan `min` och `max`.

# Källor
Projektet är en översättning och anpassning till p5.js av originalet på https://aposteriori.trinket.io/game-development-with-pygame-zero#/ninja-runner/infinite-runner

This is a Swedish translation and p5.js adaptation of https://aposteriori.trinket.io/game-development-with-pygame-zero#/ninja-runner/infinite-runner
