# Fifteen ⭐⭐⭐

## En handledning för Python och Pygame Zero 1.2

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
   [1. Vad tyckte du var svårt med uppgiften? Ge ett par exempel.](#1-vad-tyckte-du-var-svårt-med-uppgiften-ge-ett-par-exempel)
   &bull; [2. Pröva att göra några ändringar eller tillägg.](#2-pröva-att-göra-några-ändringar-eller-tillägg)
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

**Piltangenter**	Flytta en bit

# Översikt
Bitarna lagras som ett rutnät med siffror.

Siffran 16 representerar den tomma rutan.

![image](https://user-images.githubusercontent.com/4598641/226436258-85719c97-8e01-4aca-85b0-82d3cc184876.png)

Grannbiten flyttas till den tomma rutan när en piltangent trycks ned.

I början av spelet är bitarna i stigande nummerordning och slumpmässiga drag görs för att blanda det. 
>Om bitarnas position blandas helt slumpmässigt kan det resultera i en uppställning som inte går att lösa.

Efter att en bit har flyttats, gås bitarna igenom. Om alla har sina ursprungliga värden i nummerordning är spelet över.

# Kodning

## Rita bitarna

Bitarna ritas som rutor.

Just nu ritas en bit där den tomma rutan ska vara.

✏️ Se till att du är inloggad i repl.it. Öppna startprojektet https://replit.com/@RobertStorlind/fifteen-starter och spara en egen kopia med knappen "Fork".
Testkör!

```python
import pgzrun
# Globala variabler här under
WIDTH, HEIGHT = 400, 400

# Funktioner (def) här under

def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(4):
        for x in range(4):
            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

# Kod för att starta appen här under

pgzrun.go()  # Ska alltid vara sist
```

![image](https://user-images.githubusercontent.com/4598641/226436463-1d10dd82-ed1c-429b-b0bc-e855b4969551.png)

## Rita siffrorna

Numren ritas ovanpå bitarna.

Bitens nummer beräknas genom att addera Y-koordinaten (dvs. radnummer) multiplicerat med antalet bitar i en rad till X-koordinaten plus 1.

Exempel: på den första raden är Y-koordinaten 0, så ingenting läggs till varje X-koordinat, så den första siffran på den första raden är 1.
På den andra raden läggs 4 till varje X-koordinat, så den första nummer på andra raden är 5.

Tänk på att koordinatsystemet för våra rutor är upp och ner och att (x, y) = (0, 0) är i övre högra hörnet. Kolla bilden!

![image](https://user-images.githubusercontent.com/4598641/228945276-453d6676-74ea-428e-850b-8033a408033a.png)


✏️ Uppdatera koden och testkör.

```python
def draw():
    screen.fill((0, 0, 0))

    for y in range(4):
        for x in range(4):
            piece_size = 100
            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )
            
            screen.draw.text( #nyrad 🔲
                str(y * 4 + x + 1), #nyrad 🔲
                (x * piece_size, y * piece_size), #nyrad 🔲
                fontsize=60 #nyrad 🔲
            ) #nyrad 🔲
            

# Kod för att starta appen här under

pgzrun.go() # Ska alltid vara sist
```

![image](https://user-images.githubusercontent.com/4598641/226436562-731e3960-4198-4bef-8635-e239557be6c9.png)

## Skapa rutnätet
Ett rutnät skapas med varje bits nummer lagrat på sin plats på rutnätet, och detta nummer ritas.

Antalet bitar på X- och Y-axlarna återanvänds från att rita bitarna, så de görs till variabler.

✏️ Uppdatera koden och testkör. Ritas bitarna rätt?

```python
import pgzrun
# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4 #nyrad 🔲
grid_y_count = 4 #nyrad 🔲
grid = [] #nyrad 🔲

# Funktioner (def) här under
def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count): #ändrad 🔲
        for x in range(grid_x_count): #ändrad 🔲
            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]), #ändrad 🔲
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
for y in range(grid_y_count): #nyrad 🔲
    grid.append([]) #nyrad 🔲
    for x in range(grid_x_count): #nyrad 🔲
        grid[y].append(y * grid_x_count + x + 1) #nyrad 🔲

pgzrun.go()  # Ska alltid vara sist
```

## Rita inte den tomma rutan
Om vi multiplicerar antalet bitar på varje axel får vi totala antalet bitar (dvs. 4 gånger 4 betyder 16 bitar). En bit ritas bara om numret är skilt från 16.

✏️ Uppdatera koden och testkör. Blir det en tom ruta nu?

```python
# etc.
def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count: #ändrad 🔲
                continue  # hoppa över detta x och gå till nästa värde i "for x" #ändrad 🔲
# etc.
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4
grid = []

# Funktioner (def) här under


def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                continue  # hoppa över detta x och gå till nästa värde i "for x"

            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]),
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
for y in range(grid_y_count):
    grid.append([])
    for x in range(grid_x_count):
        grid[y].append(y * grid_x_count + x + 1)

pgzrun.go()  # Ska alltid vara sist
```
  
</details>

![image](https://user-images.githubusercontent.com/4598641/226437183-8e41b05e-77bc-488b-b400-4785be077050.png)

## Hitta den tomma rutans position
Det första steget i att flytta en bit är att hitta positionen för den tomma rutan.

När en tangent trycks ner, loopar vi igenom rutnätet. Om en bit är lika med antalet bitar på varje axel multiplicerat med varandra så är det den tomma rutan. Positionen ut för närvarande. Kolla i terminalfönstret.

✏️ Lägg till funktionen `on_key_down()` och testkör genom att klicka i spelfönstret och sen trycka någon tangent på tangentbordet. Utskriften kommer i det svarta terminalfönstret.
>Klicka på Run för att starta och klicka sen var som helst i spelfönstret. Då kan din kod fånga upp tangenttryckningarna.
 
```python
def on_key_down(key): #
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    # Tillfälligt
    print(f"empty x: {empty_x}, empty y: {empty_y}")
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4
grid = []

# Funktioner (def) här under


def on_key_down(key):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    # Tillfälligt
    print(f"empty x: {empty_x}, empty y: {empty_y}")


def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                continue  # hoppa över detta x och gå till nästa värde i "for x"

            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]),
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
for y in range(grid_y_count):
    grid.append([])
    for x in range(grid_x_count):
        grid[y].append(y * grid_x_count + x + 1)

pgzrun.go()  # Ska alltid vara sist
```
  
</details>

Så här ska det se ut i det svarta konsollfönstret:
```
empty x: 3, empty y: 3
```

## Flytta en bit neråt
Om Y-koordinaten för den tomma rutan är större än 0, betyder det att det finns en bit ovanför den tomma rutan, så det är möjligt att flytta en bit neråt.

Den tomma rutan ändras till bitnumret ovanför rutan. Biten ovanför rutan ändras till den lediga rutan (16).

Just nu flyttar vilken tangent som helst en bit ner.

✏️ Uppdatera koden och testkör genom att trycka på någon tangent. Flyttas biten neråt som den ska?

```python
def on_key_down(key):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    # Tillfälligt
    print(f"empty x: {empty_x}, empty y: {empty_y}")

    if empty_y > 0: #nyrad 🔲
        changed = (grid[empty_y][empty_x], grid[empty_y - 1][empty_x]) #nyrad 🔲
        grid[empty_y - 1][empty_x], grid[empty_y][empty_x] = changed #nyrad 🔲
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4
grid = []

# Funktioner (def) här under

def on_key_down(key):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    # Tillfälligt
    print(f"empty x: {empty_x}, empty y: {empty_y}")

    if empty_y > 0:
        changed = (grid[empty_y][empty_x], grid[empty_y - 1][empty_x])
        grid[empty_y - 1][empty_x], grid[empty_y][empty_x] = changed


def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                continue  # hoppa över detta x och gå till nästa värde i "for x"

            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]),
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
for y in range(grid_y_count):
    grid.append([])
    for x in range(grid_x_count):
        grid[y].append(y * grid_x_count + x + 1)

pgzrun.go()  # Ska alltid vara sist
```
  
</details>

![image](https://user-images.githubusercontent.com/4598641/226437400-e5f88975-05ce-4b80-80ca-50862059eb21.png)

## Flytta en bit uppåt
Om Y-koordinaten för den tomma rutan är mindre än antalet rader i rutnätet, betyder det att det finns en bit under den tomma rutan. 
Den biten kan då flyttas upp.

Y-koordinaten för den bit som det tomma rutan byter med görs till en variabel. 
När upp-tangenten trycks in ställs den till positionen under den tomma rutan, alltså ett steg ner på Y-axeln.

✏️ Ändra koden i `on_key_down()` och testkör med pil upp och ner.

```python
def on_key_down(key):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    new_empty_y = empty_y #nyrad 🔲

    if key == keys.DOWN: #nyrad 🔲
        new_empty_y -= 1 #nyrad 🔲
    elif key == keys.UP: #nyrad 🔲
        new_empty_y += 1 #nyrad 🔲

    if 0 <= new_empty_y < grid_y_count: #nyrad 🔲
        changed = (grid[empty_y][empty_x], grid[new_empty_y][empty_x]) #ändrad 🔲
        grid[new_empty_y][empty_x], grid[empty_y][empty_x] = changed #ändrad 🔲
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4
grid = []

# Funktioner (def) här under


def on_key_down(key):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    new_empty_y = empty_y

    if key == keys.DOWN:
        new_empty_y -= 1
    elif key == keys.UP:
        new_empty_y += 1

    if 0 <= new_empty_y < grid_y_count:
        changed = (grid[empty_y][empty_x], grid[new_empty_y][empty_x])
        grid[new_empty_y][empty_x], grid[empty_y][empty_x] = changed

def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                continue  # hoppa över detta x och gå till nästa värde i "for x"

            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]),
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
for y in range(grid_y_count):
    grid.append([])
    for x in range(grid_x_count):
        grid[y].append(y * grid_x_count + x + 1)

pgzrun.go()  # Ska alltid vara sist
```
  
</details>


## Flytta bitar åt vänster och höger
X-koordinaten för den bit som den tomma rutan byter med görs till en variabel. Den ändras när vänster- eller högerpilen trycks ned.

✏️ Uppdatera koden i `on_key_down()` och testkör med alla fyra piltangenterna.

```python
def on_key_down(key):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    new_empty_y = empty_y
    new_empty_x = empty_x #nyrad 🔲

    if key == keys.DOWN:
        new_empty_y -= 1
    elif key == keys.UP:
        new_empty_y += 1
    elif key == keys.RIGHT: #nyrad 🔲
        new_empty_x -= 1 #nyrad 🔲
    elif key == keys.LEFT: #nyrad 🔲
        new_empty_x += 1 #nyrad 🔲

    if ( #nyrad 🔲
        0 <= new_empty_y < grid_y_count and #ändrad 🔲
        0 <= new_empty_x < grid_x_count #nyrad 🔲
    ): #nyrad 🔲
        changed = (grid[empty_y][empty_x], grid[new_empty_y][new_empty_x]) #ändrad 🔲
        grid[new_empty_y][new_empty_x], grid[empty_y][empty_x] = changed #ändrad 🔲
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4
grid = []

# Funktioner (def) här under


def on_key_down(key):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    new_empty_y = empty_y
    new_empty_x = empty_x

    if key == keys.DOWN:
        new_empty_y -= 1
    elif key == keys.UP:
        new_empty_y += 1
    elif key == keys.RIGHT:
        new_empty_x -= 1
    elif key == keys.LEFT:
        new_empty_x += 1

    if (
        0 <= new_empty_y < grid_y_count and
        0 <= new_empty_x < grid_x_count
    ):
        changed = (grid[empty_y][empty_x], grid[new_empty_y][new_empty_x])
        grid[new_empty_y][new_empty_x], grid[empty_y][empty_x] = changed


def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                continue  # hoppa över detta x och gå till nästa värde i "for x"

            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]),
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
for y in range(grid_y_count):
    grid.append([])
    for x in range(grid_x_count):
        grid[y].append(y * grid_x_count + x + 1)

pgzrun.go()  # Ska alltid vara sist    
```
  
</details>


![image](https://user-images.githubusercontent.com/4598641/226437516-322ed925-e617-41a9-94a7-fc3e9329aeeb.png)



## Blanda bitarna
I början av spelet görs ett antal slumpmässiga drag för att blanda brädet.

Ett slumptal mellan 1 och 4 genereras och ett drag görs i en av de fyra rörelseriktningarna baserat på detta nummer.

Slumpmodulen importeras så att `random.randint` kan användas.

✏️ Uppdatera koden och testkör. Vi återanvänder kod från `on_key_down()`. Blandas bitarna?

```python
import pgzrun
import random #nyrad 🔲

# etc.

# Kod för att starta appen här under
for y in range(grid_y_count):
    grid.append([])
    for x in range(grid_x_count):
        grid[y].append(y * grid_x_count + x + 1)

# Koden är kopierad från on_key_down med en del småändringar
for move_number in range(1000): #nyrad
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y
    
    new_empty_y = empty_y
    new_empty_x = empty_x
    
    roll = random.randint(0, 3) #nyrad 🔲
    if roll == 0: #nyrad 🔲
        new_empty_y -= 1
    elif roll == 1: #nyrad 🔲
        new_empty_y += 1
    elif roll == 2: #nyrad 🔲
        new_empty_x -= 1
    elif roll == 3: #nyrad 🔲
        new_empty_x += 1
    
    if (
        0 <= new_empty_y < grid_y_count and
        0 <= new_empty_x < grid_x_count
    ):
        changed = (grid[empty_y][empty_x], grid[new_empty_y][new_empty_x])
        grid[new_empty_y][new_empty_x], grid[empty_y][empty_x] = changed

pgzrun.go()  # Ska alltid vara sist
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
import random

# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4
grid = []

# Funktioner (def) här under


def on_key_down(key):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    new_empty_y = empty_y
    new_empty_x = empty_x

    if key == keys.DOWN:
        new_empty_y -= 1
    elif key == keys.UP:
        new_empty_y += 1
    elif key == keys.RIGHT:
        new_empty_x -= 1
    elif key == keys.LEFT:
        new_empty_x += 1

    if (
        0 <= new_empty_y < grid_y_count and
        0 <= new_empty_x < grid_x_count
    ):
        changed = (grid[empty_y][empty_x], grid[new_empty_y][new_empty_x])
        grid[new_empty_y][new_empty_x], grid[empty_y][empty_x] = changed


def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                continue  # hoppa över detta x och gå till nästa värde i "for x"

            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]),
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
for y in range(grid_y_count):
    grid.append([])
    for x in range(grid_x_count):
        grid[y].append(y * grid_x_count + x + 1)


for move_number in range(1000):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y
    
    new_empty_y = empty_y
    new_empty_x = empty_x
    
    roll = random.randint(0, 3)
    if roll == 0:
        new_empty_y -= 1
    elif roll == 1:
        new_empty_y += 1
    elif roll == 2:
        new_empty_x -= 1
    elif roll == 3:
        new_empty_x += 1
    
    if (
        0 <= new_empty_y < grid_y_count and
        0 <= new_empty_x < grid_x_count
    ):
        changed = (grid[empty_y][empty_x], grid[new_empty_y][new_empty_x])
        grid[new_empty_y][new_empty_x], grid[empty_y][empty_x] = changed

pgzrun.go()  # Ska alltid vara sist
```
  
</details>


![image](https://user-images.githubusercontent.com/4598641/226437586-c1a482c0-b465-4214-822a-68f8b2530839.png)

## Förenkla koden
Den enda skillnaden mellan den nya blandningskoden (längst ner) och koden i `on_key_down()` är hur riktningen för förflyttningen bestäms.
Vi gör en funktion med riktningen som parameter.

✏️ Uppdatera koden och testkör. Fungerar den som innan?

```python
# etc.

# Funktioner (def) här under

def move(direction): #nyrad 🔲
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    new_empty_y = empty_y
    new_empty_x = empty_x

    if direction == 'down': #nyrad 🔲
        new_empty_y -= 1
    elif direction == 'up': #nyrad 🔲
        new_empty_y += 1
    elif direction == 'right': #nyrad 🔲
        new_empty_x -= 1
    elif direction == 'left': #nyrad 🔲
        new_empty_x += 1

    if (
        0 <= new_empty_y < grid_y_count and
        0 <= new_empty_x < grid_x_count
    ):
        changed = (grid[empty_y][empty_x], grid[new_empty_y][new_empty_x])
        grid[new_empty_y][new_empty_x], grid[empty_y][empty_x] = changed


def on_key_down(key):
    if key == keys.DOWN:
        move('down') #nyrad 🔲
    elif key == keys.UP:
        move('up') #nyrad 🔲
    elif key == keys.RIGHT:
        move('right') #nyrad 🔲
    elif key == keys.LEFT:
        move('left') #nyrad 🔲

# etc.

# Kod för att starta appen här under
for y in range(grid_y_count):
    grid.append([])
    for x in range(grid_x_count):
        grid[y].append(y * grid_x_count + x + 1)

for move_number in range(1000):
    move(random.choice(('down', 'up', 'right', 'left'))) #nyrad 🔲

pgzrun.go()  # Ska alltid vara sist
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
import random

# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4
grid = []

# Funktioner (def) här under

def move(direction):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    new_empty_y = empty_y
    new_empty_x = empty_x

    if direction == 'down':
        new_empty_y -= 1
    elif direction == 'up':
        new_empty_y += 1
    elif direction == 'right':
        new_empty_x -= 1
    elif direction == 'left':
        new_empty_x += 1

    if (
        0 <= new_empty_y < grid_y_count and
        0 <= new_empty_x < grid_x_count
    ):
        changed = (grid[empty_y][empty_x], grid[new_empty_y][new_empty_x])
        grid[new_empty_y][new_empty_x], grid[empty_y][empty_x] = changed


def on_key_down(key):
    if key == keys.DOWN:
        move('down')
    elif key == keys.UP:
        move('up')
    elif key == keys.RIGHT:
        move('right')
    elif key == keys.LEFT:
        move('left')


def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                continue  # hoppa över detta x och gå till nästa värde i "for x"

            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]),
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
for y in range(grid_y_count):
    grid.append([])
    for x in range(grid_x_count):
        grid[y].append(y * grid_x_count + x + 1)

for move_number in range(1000):
    move(random.choice(('down', 'up', 'right', 'left')))

pgzrun.go()  # Ska alltid vara sist
```
  
</details>



## Gör den nedre högra rutan tom
Den tomma rutan ska vara längst ner till höger när spelet börjar. Därför flyttas bitarna åt vänster och uppåt flera gånger. 
Antalet bitar på en axel minus 1 är det maximala antalet drag det skulle ta för att flytta den tomma rutan från ena sidan till den andra.

✏️ Den nya koden gör samma sak som tre tryck på vänsterpil och tre på uppåtpil. Testa med piltangenterna. Hamnar den tomma rutan längst ner till höger?
Uppdatera koden och testkör.

```python
# etc.
# Kod för att starta appen här under
for y in range(grid_y_count):
    grid.append([])
    for x in range(grid_x_count):
        grid[y].append(y * grid_x_count + x + 1)

for move_number in range(1000):
    move(random.choice(('down', 'up', 'right', 'left')))

for move_number in range(grid_x_count - 1): #nyrad 🔲
    move('left') #nyrad 🔲

for move_number in range(grid_y_count - 1): #nyrad 🔲
    move('up') #nyrad 🔲
    
pgzrun.go()  # Ska alltid vara sist
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
import random

# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4
grid = []

# Funktioner (def) här under

def move(direction):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    new_empty_y = empty_y
    new_empty_x = empty_x

    if direction == 'down':
        new_empty_y -= 1
    elif direction == 'up':
        new_empty_y += 1
    elif direction == 'right':
        new_empty_x -= 1
    elif direction == 'left':
        new_empty_x += 1

    if (
        0 <= new_empty_y < grid_y_count and
        0 <= new_empty_x < grid_x_count
    ):
        changed = (grid[empty_y][empty_x], grid[new_empty_y][new_empty_x])
        grid[new_empty_y][new_empty_x], grid[empty_y][empty_x] = changed


def on_key_down(key):
    if key == keys.DOWN:
        move('down')
    elif key == keys.UP:
        move('up')
    elif key == keys.RIGHT:
        move('right')
    elif key == keys.LEFT:
        move('left')


def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                continue  # hoppa över detta x och gå till nästa värde i "for x"

            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]),
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
for y in range(grid_y_count):
    grid.append([])
    for x in range(grid_x_count):
        grid[y].append(y * grid_x_count + x + 1)

for move_number in range(1000):
    move(random.choice(('down', 'up', 'right', 'left')))

for move_number in range(grid_x_count - 1):
    move('left')

for move_number in range(grid_y_count - 1):
    move('up')
    
pgzrun.go()  # Ska alltid vara sist
```
  
</details>


![image](https://user-images.githubusercontent.com/4598641/226437694-caf5182b-39da-41bf-95d7-4ef3c098baf8.png)


## Återställa spelet
Vi gör en funktion som ställer in spelets startläge.

Den funktionen anropas innan spelet börjar och när vi trycker på tangenten `R`.

✏️ Uppdatera koden och testkör. Blir det en ny spelplan när du trycker på `R`?

```python
import pgzrun
import random

# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4

# Funktioner (def) här under

def move(direction):
    # etc.

def reset():
    global grid #nyrad 🔲
    grid = [] #flyttad 🔲

    for y in range(grid_y_count):
        grid.append([])
        for x in range(grid_x_count):
            grid[y].append(y * grid_x_count + x + 1)

    for move_number in range(1000):
        move(random.choice(('down', 'up', 'right', 'left')))

    for move_number in range(grid_x_count - 1):
        move('left')

    for move_number in range(grid_y_count - 1):
        move('up')

def on_key_down(key):
    if key == keys.DOWN:
        move('down')
    elif key == keys.UP:
        move('up')
    elif key == keys.RIGHT:
        move('right')
    elif key == keys.LEFT:
        move('left')
    elif key == keys.R: #nyrad 🔲
        reset() #nyrad 🔲


def draw():
    # etc.

# Kod för att starta appen här under
reset() #nyrad 🔲

pgzrun.go()  # Ska alltid vara sist
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
import random

# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4

# Funktioner (def) här under

def move(direction):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    new_empty_y = empty_y
    new_empty_x = empty_x

    if direction == 'down':
        new_empty_y -= 1
    elif direction == 'up':
        new_empty_y += 1
    elif direction == 'right':
        new_empty_x -= 1
    elif direction == 'left':
        new_empty_x += 1

    if (
        0 <= new_empty_y < grid_y_count and
        0 <= new_empty_x < grid_x_count
    ):
        changed = (grid[empty_y][empty_x], grid[new_empty_y][new_empty_x])
        grid[new_empty_y][new_empty_x], grid[empty_y][empty_x] = changed

def reset():
    global grid
    grid = []

    for y in range(grid_y_count):
        grid.append([])
        for x in range(grid_x_count):
            grid[y].append(y * grid_x_count + x + 1)

    for move_number in range(1000):
        move(random.choice(('down', 'up', 'right', 'left')))

    for move_number in range(grid_x_count - 1):
        move('left')

    for move_number in range(grid_y_count - 1):
        move('up')

def on_key_down(key):
    if key == keys.DOWN:
        move('down')
    elif key == keys.UP:
        move('up')
    elif key == keys.RIGHT:
        move('right')
    elif key == keys.LEFT:
        move('left')
    elif key == keys.R:
        reset()


def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                continue  # hoppa över detta x och gå till nästa värde i "for x"

            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]),
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
reset()

pgzrun.go()  # Ska alltid vara sist
```
  
</details>


## Kolla om vi är klara
Efter varje drag loopar vi genom bitarna. Om alla bitarna är på rätt position i nummerordning så återställs spelet.

✏️ Uppdatera koden i `on_key_down()` och testkör. Vad händer om du lägger alla bitarna i nummerordning?

```python
def on_key_down(key):
    if key == keys.DOWN:
        move('down')
    elif key == keys.UP:
        move('up')
    elif key == keys.RIGHT:
        move('right')
    elif key == keys.LEFT:
        move('left')
    elif key == keys.R:
        reset()

    complete = True

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] != y * grid_x_count + x + 1:
                complete = False

    if complete:
        reset()
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
import random

# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4

# Funktioner (def) här under


def move(direction):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    new_empty_y = empty_y
    new_empty_x = empty_x

    if direction == 'down':
        new_empty_y -= 1
    elif direction == 'up':
        new_empty_y += 1
    elif direction == 'right':
        new_empty_x -= 1
    elif direction == 'left':
        new_empty_x += 1

    if (
        0 <= new_empty_y < grid_y_count and
        0 <= new_empty_x < grid_x_count
    ):
        changed = (grid[empty_y][empty_x], grid[new_empty_y][new_empty_x])
        grid[new_empty_y][new_empty_x], grid[empty_y][empty_x] = changed


def reset():
    global grid
    grid = []

    for y in range(grid_y_count):
        grid.append([])
        for x in range(grid_x_count):
            grid[y].append(y * grid_x_count + x + 1)

    for move_number in range(1000):
        move(random.choice(('down', 'up', 'right', 'left')))

    for move_number in range(grid_x_count - 1):
        move('left')

    for move_number in range(grid_y_count - 1):
        move('up')


def on_key_down(key):
    if key == keys.DOWN:
        move('down')
    elif key == keys.UP:
        move('up')
    elif key == keys.RIGHT:
        move('right')
    elif key == keys.LEFT:
        move('left')
    elif key == keys.R:
        reset()

    complete = True

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] != y * grid_x_count + x + 1:
                complete = False

    if complete:
        reset


def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                continue  # hoppa över detta x och gå till nästa värde i "for x"

            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]),
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
reset()

pgzrun.go()  # Ska alltid vara sist
```
  
</details>

## Förenkla koden
Koden för att beräkna startvärdet för en bit återanvänds.
Vi gör den koden till en egen funktion.

✏️ Uppdatera koden och testkör! Lägg till funktionen `get_initial_value` och anropa den i funktionerna `reset` och `on_key_down`. Fungerar spelet som innan?

```python
def get_initial_value(x, y): #nyrad 🔲
    return y * grid_x_count + x + 1 #nyrad 🔲

def reset():
    global grid

    grid = []

    for y in range(grid_y_count):
        grid.append([])
        for x in range(grid_x_count):
            grid[y].append(get_initial_value(x, y)) #ändrad 🔲

    # etc.

def on_key_down(key):
    # etc.

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] != get_initial_value(x, y): #ändrad 🔲
                complete = False

    # etc.
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
import random

# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4

# Funktioner (def) här under


def move(direction):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    new_empty_y = empty_y
    new_empty_x = empty_x

    if direction == 'down':
        new_empty_y -= 1
    elif direction == 'up':
        new_empty_y += 1
    elif direction == 'right':
        new_empty_x -= 1
    elif direction == 'left':
        new_empty_x += 1

    if (
        0 <= new_empty_y < grid_y_count and
        0 <= new_empty_x < grid_x_count
    ):
        changed = (grid[empty_y][empty_x], grid[new_empty_y][new_empty_x])
        grid[new_empty_y][new_empty_x], grid[empty_y][empty_x] = changed

def get_initial_value(x, y):
    return y * grid_x_count + x + 1

def reset():
    global grid
    grid = []

    for y in range(grid_y_count):
        grid.append([])
        for x in range(grid_x_count):
            grid[y].append(get_initial_value(x, y))

    for move_number in range(1000):
        move(random.choice(('down', 'up', 'right', 'left')))

    for move_number in range(grid_x_count - 1):
        move('left')

    for move_number in range(grid_y_count - 1):
        move('up')


def on_key_down(key):
    if key == keys.DOWN:
        move('down')
    elif key == keys.UP:
        move('up')
    elif key == keys.RIGHT:
        move('right')
    elif key == keys.LEFT:
        move('left')
    elif key == keys.R:
        reset()

    complete = True

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] != get_initial_value(x, y):
                complete = False

    if complete:
        reset


def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                continue  # hoppa över detta x och gå till nästa värde i "for x"

            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]),
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
reset()

pgzrun.go()  # Ska alltid vara sist    
```

</details>

## Blanda igen om det råkar vara klart direkt efter blandningen

Om bitarna skulle råka hamna i rätt ordning efter att de har blandats, försöker vi igen.
    
Koden för att kontrollera om bitarna är i sin ursprungliga ordning återanvänds. Vi gör det till en funktion.

✏️ Uppdatera koden för `is_complete`, `reset` och `on_key_down` och testkör. Vad händer när du lagt alla brickorna i nummerordning?

```python
def is_complete(): #nyrad 🔲
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] != get_initial_value(x, y):
                return False #nyrad 🔲

    return True #nyrad 🔲

def reset():
    global grid

    grid = []

    for y in range(grid_y_count):
        grid.append([])
        for x in range(grid_x_count):
            grid[y].append(get_initial_value(x, y))

    while True: #nyrad 🔲
        for move_number in range(1000):
            move(random.choice(('down', 'up', 'right', 'left')))

        for move_number in range(grid_x_count - 1):
            move('left')

        for move_number in range(grid_y_count - 1):
            move('up')

        if not is_complete(): #nyrad 🔲
            break #nyrad 🔲

def on_key_down(key):
    if key == keys.DOWN:
        move('down')
    elif key == keys.UP:
        move('up')
    elif key == keys.RIGHT:
        move('right')
    elif key == keys.LEFT:
        move('left')
    elif key == keys.R:
        reset()

    if is_complete(): #nyrad 🔲
        reset()
```

<details>
  <summary>📝 Så här ser hela koden ut nu</summary>
  
```python
import pgzrun
import random

# Globala variabler här under
WIDTH, HEIGHT = 400, 400

grid_x_count = 4
grid_y_count = 4

# Funktioner (def) här under


def move(direction):
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                empty_x = x
                empty_y = y

    new_empty_y = empty_y
    new_empty_x = empty_x

    if direction == 'down':
        new_empty_y -= 1
    elif direction == 'up':
        new_empty_y += 1
    elif direction == 'right':
        new_empty_x -= 1
    elif direction == 'left':
        new_empty_x += 1

    if (
        0 <= new_empty_y < grid_y_count and
        0 <= new_empty_x < grid_x_count
    ):
        changed = (grid[empty_y][empty_x], grid[new_empty_y][new_empty_x])
        grid[new_empty_y][new_empty_x], grid[empty_y][empty_x] = changed


def get_initial_value(x, y):
    return y * grid_x_count + x + 1


def is_complete():
    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] != get_initial_value(x, y):
                return False

    return True


def reset():
    global grid
    grid = []

    for y in range(grid_y_count):
        grid.append([])
        for x in range(grid_x_count):
            grid[y].append(get_initial_value(x, y))

    while True:
        for move_number in range(1000):
            move(random.choice(('down', 'up', 'right', 'left')))

        for move_number in range(grid_x_count - 1):
            move('left')

        for move_number in range(grid_y_count - 1):
            move('up')

        if not is_complete():
            break


def on_key_down(key):
    if key == keys.DOWN:
        move('down')
    elif key == keys.UP:
        move('up')
    elif key == keys.RIGHT:
        move('right')
    elif key == keys.LEFT:
        move('left')
    elif key == keys.R:
        reset()

    if is_complete():
        reset()


def draw():
    screen.fill((0, 0, 0))
    piece_size = 100

    for y in range(grid_y_count):
        for x in range(grid_x_count):
            if grid[y][x] == grid_x_count * grid_y_count:
                continue  # hoppa över detta x och gå till nästa värde i "for x"

            piece_draw_size = piece_size - 1

            screen.draw.filled_rect(
                Rect(x * piece_size, y * piece_size, piece_draw_size, piece_draw_size),
                color=(100, 20, 150)
            )

            screen.draw.text(
                str(grid[y][x]),
                (x * piece_size, y * piece_size),
                fontsize=60
            )


# Kod för att starta appen här under
reset()

pgzrun.go()  # Ska alltid vara sist
```
  
</details>

# Uppgifter
## 1. Utvärdera ert eget arbete!
När ni svarar på detta, tänk er att *ni har tillgång till uppgiften* &ndash; ni behöver alltså inte kunna koden utantill.

A. De här delarna av uppgiften har vi gjort. Vi förstår dem och kan förklara koden för Susanne eller inför klassen.

B. De här delarna av uppgiften har vi gjort *men vi förstår dem inte till 100%*. Ge exempel på något ni inte förstår.

## 2. Pröva att göra några ändringar eller tillägg
- Beskriv kort med ord eller bilder vad förändringen är och hur den ska fungera.
- Beskriv hur du fick ändra koden för att göra ändringen.
- Om det inte gick att genomföra, förklara med några meningar vad du försökte och vad som hände. Glöm inte kodexempel.

## 3. Varför kan vi inte blanda bitarna helt slumpmässigt? 
I funktionen `reset()` har vi den här koden:
```
    for move_number in range(1000):
        move(random.choice(('down', 'up', 'right', 'left')))
```

Varför behöver vi göra så när vi ställer upp brickorna på spelplanen? Kan vi inte bara slumpa fram vilken position som helst?

# Källor

Översatt och bearbetat för repl.it baserat på originalet: https://simplegametutorials.github.io/pygamezero/fifteen/
