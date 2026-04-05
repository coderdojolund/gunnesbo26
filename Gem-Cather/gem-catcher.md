# Gem Catcher ⭐⭐

![image](https://user-images.githubusercontent.com/4598641/230712343-2ac82257-0f69-40e0-921a-c525adc833f6.png)

## Innehåll
[Första koden](#första-koden) &bull;
[Funktionen update()](#funktionen-update) &bull;
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

✏️ Se till att vara inloggad i repl.it. Öppna startprojektet här i repl.it: https://replit.com/@RobertStorlind/gem-catcher-starter

Spara en egen kopia med knappen **Fork**.

Skelettet för ett Pygame Zero-program ser ut så här:
```python
import pgzrun

WIDTH, HEIGHT = 600, 450

pgzrun.go() # Måste vara sista raden
```

Den första raden, `import pgzrun`, hämtar Pygame Zero-modulen och den sista raden, `pgzrun.go()`, är funktionen som startar Pygame Zero. 
I mitten har vi `WIDTH = 800` och `HEIGHT = 600` som sätter spelfönstrets bredd och höjd.

# Figuren (Actor)
Det var inte så spännande, så vi lägger till ett rymdskepp.
För att göra det behöver ha en bild för rymdskeppet.

Du kan använda vilka bilder du vill men för att göra livet enklare för dig, är startprojektet i repl.it förberett med en bild.
Bilden med rymdskeppet behöver ligger i mappen **images** i projektet. 

![image](https://user-images.githubusercontent.com/4598641/222978313-4661f08c-e673-40be-87be-5fa1d8d06de8.png)

Så här ser det ut i repl.it. Du kan klicka på bildfilen så visas den längst ner till höger i repl.it.

![image](https://user-images.githubusercontent.com/4598641/230603082-7fa56b41-2f85-4845-8d36-b35420eb2e49.png)

>**Frivilligt:** Det finns andra bilder om du vill byta. Ladda ner ZIP-arkivet, packa upp och ladda sen upp önskad bild till ditt projekt.
>Här finns ZIP-arkivet med bilder: https://www.aposteriori.com.sg/wp-content/uploads/2020/02/image_pack.zip.
>**Obs!** Bildens filnamn kan bara innehålla små bokstäver, siffror och understrykning `_`.

✏️ Nu ska du lägga till rymdskeppet till ditt Pythonprogram.

```python
import pgzrun

WIDTH, HEIGHT = 600, 450

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

def draw():
    ship.draw()

pgzrun.go() # Måste vara sista raden
```

Detta är vad raderna gör:

`ship = Actor('playership1_blue')` : Skapar en ny figur (Actor) med utseende enligt filen *playership1_blue*. Om du använder en annan bildfil behöver du ändra namnet här.

`ship.x = WIDTH / 2 - 30` : Sätter skeppets x-koordinat till att vara ungefär mitt på x-axeln. Pröva att ändra värdet!

`ship.y = HEIGHT - 50` : Sätter skeppets y-koordinat till en bit ovanför undre kanten. Pröva att ändra värdet!

`def draw():` : Detta är en specialfunktion. Vi behöver inte anropa den själva; Pygamze Zero kör den åt oss när det behövs.

`ship.draw()` : Talar om för skeppets Actor att rita sig på skärmen. Koden behöver vara indragen under `def draw():` så att den körs när Pygame Zero anropar funktionen `draw()`.

Så här ser koordinatsystemet ut i Pygame Zero med WIDTH = 800 och HEIGHT = 600.
>I vårt fall kör vi med WIDTH = 600 och HEIGHT = 450 men du kan testa att ändra värdena. Du kan behöva justera storleken på spelfönstret (Output) uppe till höger i repl.it.

![image](https://user-images.githubusercontent.com/4598641/222978372-85be8781-e6fe-414e-9fb1-b7847dfc8b5f.png)

✏️ Testkör! Visas rymdskeppet? Du kan behöva göra ditt spelfönster (Output) större för att se rymdskeppet längst ner.
>Du behöver ha bildfilen *playership1_blue* som ingår i startprojektet för att det ska fungera. Den bilden ligger i mappen *images* som finns till vänster i repl.it-fönstret.

# Funktionen `update()`

Vi la till funktionen `draw()` innan. Det är en speciell funktion som Pygame Zero kör väldigt ofta för att rita det du ser på skärmen. En annan speciell funktion är `update()`. Pygame Zero anropar regelbundet vår `update()`-funktion för att uppdatera/rita om de olika figurerna i spelet.

✏️ Vi lägger till kod i `update()` så att den kan reagera på tangentbordet, i vårt fall vänster- och högerpil.

```python
import pgzrun

WIDTH, HEIGHT = 600, 450

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

def update():
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

def draw():
    ship.draw()

pgzrun.go() # Måste vara sista raden
```

Detta la vi till:
```python
def update():
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5
```
Varje gång Pygame Zero kör vår funktion `update()` kontrollerar den om vänsterpilen är nertryckt.
Om den är det, minskar vi rymdskeppets x-koordinat med 5. Om högerpilen är nertryckt, ökar vi x med 5.

✏️ Uppdatera koden och testa programmet med gröna "Run"-knappen i repl.it. Funkar koden som du väntade dig när du trycker på vänster- och högerpil?
>Kom ihåg att klicka i spelfönstret så att din kod kan ta emot tangenttryckningarna.

## Radera skärmen

Om du har kört programmet så här långt, fick du något knasigt i den här stilen:

![image](https://user-images.githubusercontent.com/4598641/222978681-7aa59c8e-4b41-49ae-8415-e5df0db6b51d.png)

Det är för att koden talar om för Pygame Zero att uppdatera skeppets position och rita det på skärmen. 
Vi bad aldrig om att sudda ut det som redan var ritat på skärmen.

✏️ För att sudda skärmen, fyller vi hela skärmen med en färg. Det gör vi i `draw()`-funktionen så här:
```python
screen.fill((80, 0, 70))
```
Det som står inom parenteser, `(80, 0, 70)`, kallas i Python för *tuple*; det fungerar som en lista med värden som inte går att ändra och talar om vilken färgblandning vi vill ha. Det första talet (80) är för färgen rött, det andra (0) för grönt och det tredje (70) är för blått. Det största värdet man kan ha för någon färgkomponent är 255 och det minsta värdet är 0. 

✏️ Uppdatera koden, testa olika färgvärden och se vad du får!

>**Frivilligt:**
>I Chrome kan du öppna en färgväljare genom att trycka tangentkombinationen ctrl+shift+I och sedan klicka på en av de små kvadratikonerna som du kan se i fliken Styles. Se bilden!

![image](https://user-images.githubusercontent.com/4598641/223195080-7bda1b3d-89e9-479a-bebe-936a332b1408.png)

Nu ska ditt program se ut så här:
```
import pgzrun

WIDTH, HEIGHT = 600, 450

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

def update():
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

def draw():
    screen.fill((80, 0, 70))
    ship.draw()

pgzrun.go() # Måste vara sista raden
```

# Lägga till en ädelsten

Spelet kan ju inte heta *Gem Catcher* om det inte finns en ädelsten! Det finns redan en i startprojektet i mappen *images*.

![image](https://user-images.githubusercontent.com/4598641/222978860-59fdc056-ee3c-4d15-bca8-09b80051d3c9.png)

✏️ Uppdatera koden och lägg till en ny *Actor* som representerar ädelstenen:
```python
gem = Actor('gemgreen')
gem.x = WIDTH / 2 - 50
gem.y = 0
```

Det här värdet på x placerar den ungefär i mitten horisontellt. Att sätta y till 0 placerar den högst upp i fönstret. Glöm inte att också rita ädelstenen i funktionen `draw()`.

```python
gem.draw()
```

✏️ Testkör. Visas ädelstenen på rätt plats?

Din kod ska nu se ut så här:

```python
import pgzrun

WIDTH, HEIGHT = 600, 450

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

gem = Actor('gemgreen')
gem.x = WIDTH / 2 - 50
gem.y = 0

def update():
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

def draw():
    screen.fill((80, 0, 70))
    gem.draw()
    ship.draw()

pgzrun.go() # Måste vara sista raden
```

## Flytta ädelstenen

Tidigare skrev vi kod i funktionen `update()` för att få skeppet att ändra x-koordinat när vi trycker på vänster- eller högerpil.

Vi låter ädelstenen åka nerför skärmen genom att ändra y-koordinaten för den.

```python
gem.y += 4
```

Vi vill också att ädelstenen ska hoppa till toppen av fönstret när den nått underkanten.
För att göra det, sätter vi y-positionen till 0 (topp) när den är större än HEIGHT (underkanten).
>Kolla Pygames koordinatsystem högre upp på sidan. Där ser du att y-axeln är upp- och nervänd mot hur den brukar ritas. Större y-värden är längre ner på skärmen.

```python
if gem.y > HEIGHT:
    gem.y = 0
```

✏️ Lägg till det i din kod och testkör! Rör sig ädelstenen som du tänkte dig?

```python
import pgzrun

WIDTH, HEIGHT = 600, 450

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

gem = Actor('gemgreen')
gem.x = WIDTH / 2 - 50
gem.y = 0

def update():
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

    gem.y += 4
    if gem.y > HEIGHT:
        gem.y = 0

def draw():
    screen.fill((80, 0, 70))
    gem.draw()
    ship.draw()

pgzrun.go() # Måste vara sista raden
```

# Kollision och random()

## Känna av en kollision

Vi vill också att ädelstenen ska flytta högst upp om den rör rymdskeppet. Om du har använt Scratch tidigare, kanske du har använt blocket "rör vid sprajt".

I PygameZero använder vi antingen `if gem.colliderect(ship):` eller `if ship.colliderect(gem):` &ndash; båda fungerar likadant.

```python
if gem.colliderect(ship):
    gem.y = 0
```
eller

```python
if ship.colliderect(gem):
    gem.y = 0
```
✏️ Uppdatera din kod och testkör! Går det att fånga ädelstenen med rymdskeppet?

## Slumptal med `random()`

Det är inte så spännande av låta ädelstenen falla från samma ställe varje gång.
I tidigare Python-projekt har du förmodligen redan använt modulen `random` för slumptal.
Här ska vi använda den för att slumpa fram ädelstenens x-position.

✏️ Innan vi kan använda modulen, behöver vi importera den. Lägg till detta högst upp.
```python
import random
```

✏️ För att ge ädelstenen en slumpmässig x-koordinat använder vi
```python
gem.x = random.randint(20, WIDTH - 20)
```
Anropet `random.randint(20, WIDTH - 20)` ger ett slumptal mellan 20 och WIDTH - 20. Det här behövs varje gång ädelstenen ska börja om högst upp på skärmen.

✏️ Uppdatera koden och testkör. Flyttar sig ädelstenen till olika x-koordinater mellan varje fall?

Nu bör din kod se ut så här:
```python
import pgzrun
import random

WIDTH, HEIGHT = 600, 450

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

gem = Actor('gemgreen')
gem.x = WIDTH / 2 - 50
gem.y = 0

def update():
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

    gem.y += 4
    if gem.y > HEIGHT:
        gem.y = 0
    if gem.colliderect(ship):
        gem.x = random.randint(20, WIDTH - 20)
        gem.y = 0

def draw():
    screen.fill((80, 0, 70))
    gem.draw()
    ship.draw()

pgzrun.go() # Måste vara sista raden
```

# Räkna poäng

## En variabel som räknar poäng

Vi kan lätt hålla reda på poängen med en variabel.

✏️ Lägg till en variabel som heter `score` och sätt den till 0 i början av programmet.

```python
score = 0
```
✏️ Nu ökar vi `score` med 1 varje gång rymdskeppet fångar ädelstenen.

```python
if gem.colliderect(ship):
    gem.x = random.randint(20, WIDTH - 20)
    gem.y = 0
    score += 1
```

✏️ Uppdatera din kod. Om du testkör programmet nu, får du ett fel:

```python
UnboundLocalError: local variable 'score' referenced before assignment
```

Det är för att variabeln `score` är deklarerad utanför funktionen `update()`, men vi försöker ändra värdet på den inne i funktionen `update()`.

✏️ I Python kan vi *läsa* variabler som finns utanför funktionen &ndash; de kallas globala variabler. 
Men vi kan inte skriva (ändra) dem om vi inte har deklarerat dem som `global` inuti funktionen.

```python
def update():
    global score # ny rad
```

✏️ Testkör nu! Fungerar det att köra?
>Poängen visas inte än. Det är nästa steg.

Efter den ändringen, bör din `update()`-funktion se ut så här:
```python
def update():
    global score
    
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

    gem.y += 4
    if gem.y > HEIGHT:
        gem.y = 0
    if gem.colliderect(ship):
        gem.x = random.randint(20, WIDTH - 20)
        gem.y = 0
        score += 1
```

## Visa poäng

✏️ För att visa poängen använder vi funktionen `screen.draw.text()`.

```python
screen.draw.text(f"Score: {score}", (15, 10), color=(255, 255, 255), fontsize=30)
```
Parametrarna är så här:

`f"Score: {score}"` : Detta är texten (strängen) vi vill skriva, alltså texten Score följt av variabelns värde.

`(15, 10)` : Detta är koordinaterna där vi vill skriva: x=15 och y=10.

`color=(255, 255, 255)` : Detta är textens färg, vitt i det här fallet.

`fontsize=30` : Textens storlek.

Som med de andra ritfunktionerna, behöver vi lägga detta inuti funktionen `draw()`. 

✏️ Uppdatera din kod och testkör. När du har gjort det, bör din kod se ut så här:

```python
import pgzrun
import random

WIDTH, HEIGHT = 600, 450

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

gem = Actor('gemgreen')
gem.x = WIDTH / 2 - 50
gem.y = 0
score = 0

def update():
    global score
    
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

    gem.y += 4
    if gem.y > HEIGHT:
        gem.y = 0
    if gem.colliderect(ship):
        gem.x = random.randint(20, WIDTH - 20)
        gem.y = 0
        score += 1

def draw():
    screen.fill((80, 0, 70))
    gem.draw()
    ship.draw()
    screen.draw.text(f"Score: {score}", (15, 10), color=(255, 255, 255), fontsize=30)

pgzrun.go() # Måste vara sista raden
```

# Mus och händelser

## Musstyrning
Vi kan också ändra vårt spel så att det fungerar med mus.
Precis som Scratch så är Pygame Zero händelsebaserat.
Det betyder att när en viss händelse inträffar, t.ex. att en musknapp trycks ner, så anropar Pygame Zero motsvarande funktion, t.ex. `on_mouse_down`.

För att känna av när musen rör sig, kan vi använda funktionen `on_mouse_move(pos, rel, buttons)` function.

✏️ Pröva att lägga till det här i ditt spel och testkör!

```
def on_mouse_move(pos, rel, buttons):
    ship.x = pos[0]
```

Syftet med tre parametrarna är:

`pos` : Ger muspositionen. Du kan få fram x-koordinaten med `pos[0]` och y-koordinaten med `pos[1]`.

`rel` : Ger förändringen i position sedan senaste gången musen förflyttades. `rel[0]` är ändringen i x-led och `rel[1]` förändringen i y-led.

`buttons` : Ger en lista med vilka musknappar som är nertryckta. Om du till exempel vill kolla om vänster musknapp är nertryckt:

```python
def on_mouse_move(pos, rel, buttons):
    if mouse.LEFT in buttons:
        print('left click')
```
Detta är bara ett exempel. **Lägg inte till det i ditt spel just nu**

## Exempel på andra händelser

Förutom musrörelser, finns det också andra funktioner som Pygame Zero kör när motsvarande händelse inträffar. De här händelserna finns:

`on_mouse_down(pos, buttons)` : Körs när en musknapp klickas på.

`on_mouse_up(pos, buttons)` : Körs när man släpper en musknapp. Parametrar:
- `pos` : Ger musens position. x-koordinaten ges av `pos[0]` och y-koordinaten av `pos[1]`.
- `buttons` : Ger en lista med de musknapper som är nertryckta.

`on_key_down(key, mod, unicode)` : Körs när en tangent trycks ner på tangentbordet.

`on_key_up(key, mod)` : Körs när man släpper en tangent. Parametrar:
- key : Ett heltal som talar om vilken tangent som trycktes ner. Se Pygame Zeros hemsida för en fullständig lista över tangenter.
- mod : En lista (bitmask) med vilka specialtangenter som trycktes ner. Du kan läsa av dem så här:

```python
def on_key_down(key, mod, unicode):
    if mod & keymods.LSHIFT:
        print('Left shift button pressed')
```
Detta är bara ett exempel. **Lägg inte till det i ditt spel just nu**

`unicode` : Vilket tecken som skrevs, om något. Du kan läsa av det så här:
```python
def on_key_down(key, mod, unicode):
    if unicode == 'e':
        print('e button pressed')
```
Detta är bara ett exempel. **Lägg inte till det i ditt spel just nu**


# Game Over
## Gör spelet svårare
Just nu är spelet för lätt.
Utan att skryta kan jag lätt få över 300 poäng utan ansträngning.
Vi gör det svårare genom att låta ädelstenen falla fortare ju högre poängen är.

✏️ Leta upp den här raden:
```python
gem.y += 4
```
och ändra den till
```python
gem.y += 4 + score / 5
```
Detta kommer att öka fallhastigheten när poängen går upp. När du har noll poäng, faller ädelstenen med hastighet 4. När din poäng är 10, kommer ädelstenen att falla med hastigheten 6 (4 + 10 / 5).

✏️ Testkör. Är det rätt svårighetsgrad? Testa att ändra värdena 4 och 5 till andra värden.

## Game Over
Det är inte så värst kul att spela ett spel som man inte kan förlora.
Vi lägger till ett Game Over-läge.
Om ädelstenen rör vi skärmens underkant, avslutas spelet.

✏️ Lägg först till en ny variabel som heter `game_over` och sätt den till `False`.

```python
game_over = False
```

Leta upp de här raderna i funktionen `update()`:
```python
if gem.y > HEIGHT:
    gem.x = random.randint(20, WIDTH - 20)
    gem.y = 0
```
och ändra dem till det här:
```python
if gem.y > HEIGHT:
    game_over = True
```
Detta sätter variabeln `game_over` till `True` om ädelstenen rör skärmens underkant.

Du behöver också lägga till `global game_over` högst upp i funktionen `update()`.
Minns du varför? Om inte, läs på under avsnittet [Räkna poäng](#räkna-poäng).

Inuti funktionen `draw()` ändrar du de här raderna:
```python
screen.fill((80, 0, 70))
gem.draw()
ship.draw()
screen.draw.text(f"Score: {score}", (15, 10), color=(255, 255, 255), fontsize=30)
```
Ändra dem till
```python
screen.fill((80, 0, 70))
if game_over:
    screen.draw.text("Game Over", (360, 300), color=(255, 255, 255), fontsize=60)
    screen.draw.text(f"Final Score: {score}", (360, 350), color=(255, 255, 255), fontsize=60)
else:
    gem.draw()
    ship.draw()
    screen.draw.text(f"Score: {score}", (15, 10), color=(255, 255, 255), fontsize=30)
```
Det gör att Game Over-texten visas när variabeln `game_over` är satt till True; annars kommer ädelstenen och skeppet att ritas som innan.

✏️ Testkör nu! Blir det Game Over som väntat?

Till slut bör din kod se ut så här:
```python
import pgzrun
import random

WIDTH, HEIGHT = 600, 450

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

gem = Actor('gemgreen')
gem.x = WIDTH / 2 - 50
gem.y = 0

score = 0
game_over = False

def on_mouse_move(pos, rel, buttons):
    ship.x = pos[0]
    
def update():
    global score, game_over
    
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

    gem.y += 4
    if gem.y > HEIGHT:
        gem.y = 0
    if gem.colliderect(ship):
        gem.x = random.randint(20, WIDTH - 20)
        gem.y = 0
        score += 1

def draw():
    screen.fill((80, 0, 70))
    gem.draw()
    ship.draw()
    if game_over:
        screen.draw.text("Game Over", (360, 300), color=(255, 255, 255), fontsize=60)
        screen.draw.text(f"Final Score: {score}", (360, 350), color=(255, 255, 255), fontsize=60)
    else:
        gem.draw()
        ship.draw()
        screen.draw.text(f"Score: {score}", (15, 10), color=(255, 255, 255), fontsize=30)

pgzrun.go() # Måste vara sista raden
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
Det finns ädelstenar med flera olika färger i mediefilen https://www.aposteriori.com.sg/wp-content/uploads/2020/02/image_pack.zip. Gör så att ädelstenar av olika färg kan dyka upp när den ska börja falla uppifrån

### Utmaning 3. Välja skepp
Mediefilen https://www.aposteriori.com.sg/wp-content/uploads/2020/02/image_pack.zip har flera olika slags rymdskepp. Fråga användaren vilket rymdskepp den vill ha när den ska spela.

### Utmaning 4. Flera ädelstenar
Just nu är det bara en fallande ädelsten åt gången. Ändra spelet så att flera kan falla samtidigt.

### Utmaning 5. Multi-player
Lägg till en andra spelare och tävla om vem som får högst poäng!

# Källor/Sources

Det här projektet är översatt och anpassat till repl.it baserat på https://aposteriori.trinket.io/game-development-with-pygame-zero#/gem-catcher/first-program

This project is a translated and adapted to repl.it from https://aposteriori.trinket.io/game-development-with-pygame-zero#/gem-catcher/first-program
