# Eyes ⭐⭐

![image](https://user-images.githubusercontent.com/4598641/223815678-2c97174e-578c-4df4-9f8a-d8764f0b2424.png)

Vi vill göra en animering där ögonen följer muspekaren.

## Jobba så här
Vägen till ett färdigt projekt är en pusselbit i taget 🧩. Gör därför ett avsnitt i taget uppifrån och ner.
- Få det att fungera innan ni går vidare till nästa avsnitt. 
- Testkör ofta, efter varje avsnitt eller ännu oftare.

Var det något i beskrivningen som var svårt att förstå? Ta med det i redovisningen. 📝

## Innehåll
[Rita ett öga](#rita-ett-öga)
&bull; [Räkna ut avståndet i X- och Y-led mellan muspekaren och ögat](#räkna-ut-avståndet-i-x--och-y-led-mellan-muspekaren-och-ögat)
&bull; [Räkna ut avståndet mellan muspekaren och ögat](#räkna-ut-avståndet-mellan-muspekaren-och-ögat)
&bull; [Rörlig pupill](#rörlig-pupill)
&bull; [Pupillen ska inte smita när muspekaren är utanför ögat](#pupillen-ska-inte-smita-när-muspekaren-är-utanför-ögat)
&bull; [Två ögon](#två-ögon)
&bull; [Uppgifter](#uppgifter)
&bull; [Källor](#källor)

# Rita ett öga
Ögat ritas med en vit cirkel och en lite mindre, mörkblå cirkel för pupillen.

![image](https://user-images.githubusercontent.com/4598641/223816876-1da49223-c7af-46d5-836c-9b1216eb52d4.png)

1. Öppna p5.js här: https://editor.p5js.org/
2. ✏️ Mata in och testa den här startkoden. [Här finns tips](https://github.com/coderdojolund/gunnesbo26?tab=readme-ov-file#var-hittar-jag-symbolerna-p%C3%A5-tangentbordet) hur man matar in specialtecken.

```javascript
const WIDTH = 530;
const HEIGHT = 400;

function setup() {
  createCanvas(WIDTH, HEIGHT);
  noStroke(); 
}

function draw() {
  background(0, 0, 0);

  fill(255, 255, 255);
  circle(200, 200, 100); 

  fill(0, 0, 100);
  circle(200, 200, 30);  
}
```

# Räkna ut avståndet i X- och Y-led mellan muspekaren och ögat
Vi lägger till kod så att avståndet mellan ögats mitt och muspekaren visas, räknat i pixlar.

- Ögats x- och y-koordinater återanvänder vi från när vi ritar ögat och därför sparar vi koordinaterna i variabler.
- Vi importerar modulen `pygame` för att kunna använda funktionen `pygame.mouse.get_pos`.
- Funktionen `update()` är tom så länge. Funktionen `draw()` ritar om skärmen vid varje uppdatering.

✏️ **Mata in** och testkör koden!

Koden så här långt &ndash; nya rader är markerade:

```javascript
const WIDTH = 530;
const HEIGHT = 400;

function setup() {
  createCanvas(WIDTH, HEIGHT);
  noStroke();
}

function draw() {
  background(0, 0, 0);

  let eyeX = 200;
  let eyeY = 200;

  let distanceX = mouseX - eyeX;
  let distanceY = mouseY - eyeY;

  fill(255, 255, 255);
  circle(eyeX, eyeY, 100); 

  fill(0, 0, 100);
  circle(200, 200, 30);

  fill(255, 255, 255); // Set text color to white
  textAlign(LEFT, TOP); // Align text like Pygame Zero
  
  // Backticks (`) allow for string interpolation, similar to Python's f-strings
  text(`pixlar, x: ${distanceX}\npixlar, y: ${distanceY}`, 0, 0);
}
```

Så här ser det ut när jag testkör. Fungerar det inte? Kolla koden igen.

![image](https://user-images.githubusercontent.com/4598641/223817639-1363643f-481d-44e3-979b-d0b48eb0c9da.png)


# Räkna ut avståndet mellan muspekaren och ögat
Avståndet, `distance`, kan vi räkna ut med Pythagoras sats. Se figuren.

![image](https://user-images.githubusercontent.com/4598641/224125785-ee2eedc9-2155-4508-9fc2-d8518bfdfe32.png)

Avståndet i pixlar är roten ur (antalet pixlar i X-led i kvadrat + antalet pixlar i Y-led i kvadrat).
- Operatorn `**2` betyder upphöjt till 2, alltså kvadraten
- Vi importerar mattemodulen `math` för att räkna roten ur med `math.sqrt()`.

:pencil2: **Uppdatera koden** och testkör den!

```javascript
const WIDTH = 530;
const HEIGHT = 400;

function setup() {
  createCanvas(WIDTH, HEIGHT);
  noStroke();
}

function draw() {
  background(0, 0, 0);

  let eyeX = 200;
  let eyeY = 200;

  let distanceX = mouseX - eyeX;
  let distanceY = mouseY - eyeY;
  let distance = Math.sqrt((distanceX * distanceX) + (distanceY * distanceY));

  fill(255, 255, 255);
  circle(eyeX, eyeY, 100); 

  fill(0, 0, 100);
  circle(200, 200, 30);

  fill(255, 255, 255); // Set text color to white
  textAlign(LEFT, TOP); // Align text like Pygame Zero
  
  // Backticks (`) allow for string interpolation, similar to Python's f-strings
  text(`pixlar, x: ${distanceX}\npixlar, y: ${distanceY}\navstånd: ${distance}`, 0, 0);
}
```

Verkar avståndet räknas ut rätt? Flytta muspekaren sakta och kolla.

# Rörlig pupill

Vi vill att pupillen ska följa muspekaren.

Om muspekaren är innanför ögat är det lätt. Då kan vi sätta pupillen till muspekarens koordinater.

Då kan koden för `draw()` se ut så här. Den övriga koden är samma som innan.

```javascript
# behåll resten av koden

function draw() {
  background(0, 0, 0);

  let eyeX = 200;
  let eyeY = 200;

  let distanceX = mouseX - eyeX;
  let distanceY = mouseY - eyeY;
  let distance = Math.sqrt(distanceX * distanceX + distanceY * distanceY);
  let pupilX = eyeX + distanceX
  let pupilY = eyeY + distanceY 

  fill(255, 255, 255);
  circle(eyeX, eyeY, 100); 

  fill(0, 0, 100);
  circle(pupilX, pupilY, 30);

# behåll resten av koden
```
:pencil2: **Uppdatera och testa** din kod. Fungerar den bra när muspekaren är innanför ögats cirkel?

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
import pygame
import math  

WIDTH, HEIGHT = 530, 400


def update():
    pass


def draw(): #funktionen är uppdaterad 👀
    screen.fill((0, 0, 0))

    mouse_x, mouse_y = pygame.mouse.get_pos()
    eye_x = 200
    eye_y = 200

    distance_x = mouse_x - eye_x
    distance_y = mouse_y - eye_y
    distance = math.sqrt(distance_x**2 + distance_y**2) # Pythagoras
    pupil_x = eye_x + distance_x #nyrad 👀
    pupil_y = eye_y + distance_y #nyrad 👀

    screen.draw.filled_circle((eye_x, eye_y), 50, color=(255, 255, 255))
    screen.draw.filled_circle((pupil_x, pupil_y), 15, color=(0, 0, 100))


pgzrun.go()  # måste vara sist
```
    
</details>

# Pupillen ska inte smita när muspekaren är utanför ögat

🤔 Hur vet vi att muspekaren är i ögat? Vi har ju räknat ut avståndet från ögats centrum till muspekaren i variabeln `distance` så vi använder ögats radie som en gräns, till exempel 30 pixlar. Det ska ju finnas lite plats att rita pupillen också.

Om muspekaren är mer än 30 pixlar från ögats centrum, placerar vi pupillen i kanten av ögat. Se bilden.

![image](https://user-images.githubusercontent.com/4598641/224125785-ee2eedc9-2155-4508-9fc2-d8518bfdfe32.png)

- Den streckade triangeln har hypotenusan 30 pixlar och den är likformig med den större triangeln med muspekaren i ena hörnet.
- Proportionen mellan den streckade triangeln och den stora triangeln är `30 / distance` när muspekaren är utanför ögat. Om t.ex. `distance` är 60 så är sidorna på den lilla streckade triangeln 30/60 = 1/2 av den stora triangeln.
- Vi får därför skala `distance_x` och `distance_y` med den skalfaktorn när vi ska räkna ut pupillens x- och y-koordinater.
- Då kommer pupillen att ritas innanför ögats cirkel, som vi vill.

Så här kan `draw()` se ut nu:

```python
# behåll resten av koden

def draw():
    screen.fill((0, 0, 0))

    mouse_x, mouse_y = pygame.mouse.get_pos()
    radius = 30 #nyrad
    eye_x = 200
    eye_y = 200

    distance_x = mouse_x - eye_x
    distance_y = mouse_y - eye_y
    distance = math.sqrt(distance_x**2 + distance_y**2) # Pythagoras
    
    if distance < radius: # innanför ögat # ändrat 👀
        pupil_x = eye_x + distance_x # ändrat 👀
        pupil_y = eye_y + distance_y # ändrat 👀
    else: # utanför ögat # ändrat 👀
        scale = radius / distance # se bilden # ändrat 👀
        pupil_x = eye_x + distance_x * scale # ändrat 👀
        pupil_y = eye_y + distance_y * scale # ändrat 👀

    screen.draw.filled_circle((eye_x, eye_y), 50, color=(255, 255, 255))
    screen.draw.filled_circle((pupil_x, pupil_y), 15, color=(0, 0, 100))

# behåll resten av koden
```
:pencil2: **Uppdatera de markerade raderna och testa koden.** Fungerar den bra även när muspekaren är utanför ögats cirkel?

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
import pygame
import math

WIDTH, HEIGHT = 530, 400

def update():
    pass

def draw():  # uppdaterad 👀
    screen.fill((0, 0, 0))

    mouse_x, mouse_y = pygame.mouse.get_pos()
    radius = 30  # nyrad
    eye_x = 200
    eye_y = 200

    distance_x = mouse_x - eye_x
    distance_y = mouse_y - eye_y
    distance = math.sqrt(distance_x**2 + distance_y**2)  # Pythagoras

    if distance < radius:  # innanför ögat
        pupil_x = eye_x + distance_x
        pupil_y = eye_y + distance_y
    else:  # utanför ögat
        scale = radius / distance  # se bilden
        pupil_x = eye_x + distance_x * scale
        pupil_y = eye_y + distance_y * scale

    screen.draw.filled_circle((eye_x, eye_y), 50, color=(255, 255, 255))
    screen.draw.filled_circle((pupil_x, pupil_y), 15, color=(0, 0, 100))


pgzrun.go()  # måste vara sist
```
    
</details>

# Två ögon
Vi vill ha *två* ögon som följer muspekaren. Vi kan återanvända samma kod.
Därför lägger vi ögats kod
i funktionen `draw_eye` som har ögats x- och y-koordinat som indata/parametrar.

Så här kan koden se ut nu:

```python
import pgzrun
import pygame
import math

WIDTH, HEIGHT = 530, 400

def update():
    pass

def draw():
    screen.fill((0, 0, 0))

    def draw_eye(eye_x, eye_y): # ändrat 👀
        radius = 30 # ändrat 👀 -- kom ihåg att ha fyra extra mellanslag före
        mouse_x, mouse_y = pygame.mouse.get_pos() # ändrat 👀

        distance_x = mouse_x - eye_x # ändrat 👀
        distance_y = mouse_y - eye_y # ändrat 👀
        distance = math.sqrt(distance_x**2 + distance_y**2) # ändrat 👀
        if distance < radius: # ändrat 👀
            pupil_x = eye_x + distance_x # ändrat 👀
            pupil_y = eye_y + distance_y # ändrat 👀
        else: # ändrat 👀
            scale = radius / distance # se bilden # ändrat 👀
            pupil_x = eye_x + distance_x * scale # ändrat 👀
            pupil_y = eye_y + distance_y * scale # ändrat 👀

        screen.draw.filled_circle((eye_x, eye_y), 50, color=(255, 255, 255)) # ändrat 👀
        screen.draw.filled_circle((pupil_x, pupil_y), 15, color=(0, 0, 100)) # ändrat 👀

    draw_eye(200, 200) # första ögat 👀
    draw_eye(330, 200) # andra ögat 👀

pgzrun.go()
```
:pencil2: **Uppdatera de ändrade raderna och testa koden.** Följer båda ögonen muspekaren på rätt sätt?
>Tänk på att justera indragen i koden när du lägger till funktionen `draw_eye()`.
    
# Uppgifter

## 1. Utvärdera ert eget arbete!
När ni svarar på detta, tänk er att *ni har tillgång till uppgiften* &ndash; ni behöver alltså inte kunna koden utantill.

**1A.** De här delarna av uppgiften har vi gjort. Vi förstår dem och kan förklara koden för Susanne eller inför klassen.

**1B.** De här delarna av uppgiften har vi gjort *men vi förstår dem inte till 100%*. Ge exempel på något ni inte förstår.

## Uppgift 2
Uträkningen av scale går att göra om så att vi får ännu färre kodrader. Just nu har vi den här koden, där det är väldigt lite skillnad mellan de två kodblocken under `if`  respektive `else`:
```python
    # inuti funktionen draw():
        if distance < radius: # fall 1: innanför ögat
            pupil_x = eye_x + distance_x
            pupil_y = eye_y + distance_y
        else: # fall 2: utanför ögat
            scale = radius / distance # se bilden
            pupil_x = eye_x + distance_x * scale 
            pupil_y = eye_y + distance_y * scale
```
Kan du förenkla koden så att vi inte behöver två olika uträkningar av `pupil_x` resp. `pupil_y`?[^1]                          
- Glöm inte kodexempel när du redovisar
    
## Extrauppgift 3
Kan du få ögonen att byta färg, försvinna eller flytta sig till en annan position på skärmen ibland?
- Glöm inte kodexempel när du redovisar
- Om du gör `from random import randint` kan du använda funktionen `randint(min, max)` för att få ett heltal mellan min och max. 
    
## Extrauppgift 4
Kan du lägga in någon annan slags figur och få ögonen att följa den figuren istället för muspekaren?
- Glöm inte kodexempel när du redovisar

# Mer om koden i Pygame Zero och Pygame, förklarat på engelska

**[draw(), update()](https://pygame-zero.readthedocs.io/en/stable/hooks.html#game-loop-hooks)**: https://pygame-zero.readthedocs.io/en/stable/hooks.html#game-loop-hooks
    
**[pygame.mouse.get_pos()](https://www.pygame.org/docs/ref/mouse.html#pygame.mouse.get_pos)**: https://www.pygame.org/docs/ref/mouse.html#pygame.mouse.get_pos

**[screen.draw(), screen.fill()](https://pygame-zero.readthedocs.io/en/stable/builtins.html#screen)**: https://pygame-zero.readthedocs.io/en/stable/builtins.html#screen
    
# Källor

Detta är en anpassning till repl.it av originalprojektet https://simplegametutorials.github.io/pygamezero/eyes/

[^1]:Vad kan du sätta `scale` till när `distance < radius`? Hur kan du använda det för att räkna ut `pupil_x` och `pupil_y` med samma kod?
