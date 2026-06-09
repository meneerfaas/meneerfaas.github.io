.. role:: python(code)
   :language: python

.. |br| raw:: html

   <br/>

Car Actor
=================

Het autootje in Car Driver is een Actor.

.. figure:: images/car.gif
    :alt: Car
    :align: center

Om te beginnen maken we een Actorvariabele :python:`car` aan door de :python:`Actor` constructor aan te roepen, waarbij we de naam van de sprite :python:`'car_0'` meegeven. Vervolgens geven we de Actor een beginpositie door de :python:`pos` eigenschap in te stellen op een tuple met de x- en y-coördinaten. In dit geval plaatsen we het autootje in het midden van het venster, dus op de helft van de breedte en de hoogte.

.. code-block:: python
    :linenos:
    :emphasize-lines: 6-8
    :caption: car_driver.py

    # WINDOW SETTINGS
    WIDTH = 600
    HEIGHT = 400
    TITLE = 'Car Driver'

    # ACTORS
    car = Actor('car_0')
    car.pos = (WIDTH // 2, HEIGHT // 2)

    ################################
    # DRAW()
    ################################
    def draw():
        screen.fill('darkgreen')

    ################################
    # UPDATE()
    ################################
    def update():
        pass

Om het autootje zichtbaar te maken, moeten we het tekenen in de :python:`draw()` functie. Daarvoor roepen we de :python:`draw()` methode van de Actor aan.

.. code-block:: python
    :linenos:
    :emphasize-lines: 15
    :caption: car_driver.py

    # WINDOW SETTINGS
    WIDTH = 600
    HEIGHT = 400
    TITLE = 'Car Driver'

    # ACTORS
    car = Actor('car_0')
    car.pos = (WIDTH // 2, HEIGHT // 2)

    ################################
    # DRAW()
    ################################
    def draw():
        screen.fill('darkgreen')
        car.draw()

    ################################
    # UPDATE()
    ################################
    def update():
        pass

Run de code om te controleren of het autootje in het midden van het venster verschijnt.

Besturing
-----------------

We willen het autootje kunnen besturen met de pijltjestoetsen. Dat kan op verschillende manieren die elk hun eigen voor- en nadelen hebben. In deze tutorial gebruiken we niet de eenvoudigste manier, maar een manier die goed werkt en die je hopelijk beter leert begrijpen hoe je handig gebruik kunt maken van de event handler functies van Pygame Zero. We gaan namelijk de :python:`on_key_down()` en de :python:`on_key_up()` functies gebruiken, die worden aangeroepen wanneer een toets wordt ingedrukt of losgelaten.

Voordat we de event handlers programmeren, voegen we eerst een drietal eigenschappen toe aan de :python:`car` Actor om de rijsnelheid van het autootje en de verplaatsingen in de x- en y-richting bij te houden.

.. code-block:: python
    :linenos:
    :lineno-start: 6
    :emphasize-lines: 4-6
    :caption: car_driver.py

    # ACTORS
    car = Actor('car_0')
    car.pos = (WIDTH // 2, HEIGHT // 2)
    car.speed = 3
    car.dx = 0
    car.dy = 0

Nu kunnen we de event handlers maken. De :python:`on_key_down()` functie ziet er zo uit:

.. code-block:: python
    :linenos:
    :lineno-start: 6
    :emphasize-lines: 8-19
    :caption: car_driver.py

    # ACTORS
    car = Actor('car_0')
    car.pos = (WIDTH // 2, HEIGHT // 2)
    car.speed = 3
    car.dx = 0
    car.dy = 0

    ################################
    # EVENT HANDLERS
    ################################
    def on_key_down(key):
        if key == keys.LEFT:
            car.dx -= car.speed
        elif key == keys.RIGHT:
            car.dx += car.speed
        elif key == keys.UP:
            car.dy -= car.speed
        elif key == keys.DOWN:
            car.dy += car.speed

    ################################
    # DRAW()
    ################################

De variabelen :python:`car.dx` en :python:`car.dy` staan eigenlijk voor de snelheid van het autootje in de x- en y-richting. Wanneer je op een pijltjestoets drukt, wordt de snelheid in de bijbehorende richting verhoogd of verlaagd. Als je bijvoorbeeld op de linkerpijltjestoets drukt, wordt :python:`car.dx` met 3 verlaagd, waardoor het autootje naar links gaat bewegen. 

Wanneer de linkerpijltjestoets wordt losgelaten, moeten we :python:`car.dx` weer met 3 verhogen, zodat het autootje stopt met bewegen in de x-richting. De :python:`on_key_up()` functie is dus een spiegelbeeld van de :python:`on_key_down()` functie:

.. code-block:: python
    :linenos:
    :lineno-start: 13
    :emphasize-lines: 14-22
    :caption: car_driver.py

    ################################
    # EVENT HANDLERS
    ################################
    def on_key_down(key):
        if key == keys.LEFT:
            car.dx -= car.speed
        elif key == keys.RIGHT:
            car.dx += car.speed
        elif key == keys.UP:
            car.dy -= car.speed
        elif key == keys.DOWN:
            car.dy += car.speed

    def on_key_up(key):
        if key == keys.LEFT:
            car.dx += car.speed
        elif key == keys.RIGHT:
            car.dx -= car.speed
        elif key == keys.UP:
            car.dy += car.speed
        elif key == keys.DOWN:
            car.dy -= car.speed

Nu moeten we nog zorgen dat de snelheden :python:`car.dx` en :python:`car.dy` worden toegepast op de positie van het autootje. Dat doen we in de :python:`update()` functie,waar we nu het keyword :python:`pass` kunnen vervangen door echte code:

.. code-block:: python
    :linenos:
    :lineno-start: 43
    :emphasize-lines: 5-6
    :caption: car_driver.py

    ################################
    # UPDATE()
    ################################
    def update():
        car.x += car.dx
        car.y += car.dy

Run de code en controleer of je het autootje kunt besturen met de pijltjestoetsen. Test ook wat er gebeurt wanneer je twee pijltjestoetsen tegelijk indrukt en probeer te verklaren waarom dat gebeurt.

Flippen
-----------------

Wanneer je het autootje naar links laat bewegen, dan wil je dat het autootje ook daadwerkelijk naar links wijst. De :file:`pgzhelper` module voegt de handige eigenschap :python:`flip_x` toe aan de Actor klasse, waarmee je een sprite horizontaal kunt spiegelen. Deze eigenschap gebruiken we om het autootje te laten flippen wanneer het naar links beweegt.

We voegen helemaal bovenaan het codebestand de volgende importregel toe om de :file:`pgzhelper` module te importeren:

.. code-block:: python
    :linenos:
    :emphasize-lines: 1
    :caption: car_driver.py

    from pgzhelper import *

Daarna creëren we twee constanten om de richting van het autootje mee aan te geven, en we voegen een nieuwe eigenschap :python:`direction` toe aan de :python:`car` Actor om de huidige rijrichting bij te houden:

.. code-block:: python
    :linenos:
    :lineno-start: 3
    :emphasize-lines: 6-8, 16
    :caption: car_driver.py

    # WINDOW SETTINGS
    WIDTH = 600
    HEIGHT = 400
    TITLE = 'Car Driver'

    # DIRECTION CONSTANTS
    LEFT = 1
    RIGHT = 2

    # ACTORS
    car = Actor('car_0')
    car.pos = (WIDTH // 2, HEIGHT // 2)
    car.speed = 3
    car.dx = 0
    car.dy = 0
    car.direction = RIGHT

Het wisselen van richting gebeurt in de event handler functies. Voeg de volgende regels toe aan de :python:`on_key_down()` en :python:`on_key_up()` functies:

.. code-block:: python
    :linenos:
    :lineno-start: 20
    :emphasize-lines: 7-8, 11-12, 21-22, 25-26
    :caption: car_driver.py

    ################################
    # EVENT HANDLERS
    ################################
    def on_key_down(key):
        if key == keys.LEFT:
            car.dx -= car.speed
            if car.dx < 0:
                car.direction = LEFT
        elif key == keys.RIGHT:
            car.dx += car.speed
            if car.dx > 0:
                car.direction = RIGHT
        elif key == keys.UP:
            car.dy -= car.speed
        elif key == keys.DOWN:
            car.dy += car.speed
            
    def on_key_up(key):
        if key == keys.LEFT:
            car.dx += car.speed
            if car.dx > 0:
                car.direction = RIGHT
        elif key == keys.RIGHT:
            car.dx -= car.speed
            if car.dx < 0:
                car.direction = LEFT
        elif key == keys.UP:
            car.dy += car.speed
        elif key == keys.DOWN:
            car.dy -= car.speed

Begrijp je hoe deze toevoegingen werken? De richting van het autootje zou van rechts naar links kunnen veranderen wanneer je op de linkerpijltjestoets drukt, maar ook wanneer de rechterpijltjestoets wordt losgelaten terwijl de linkerpijltjestoets nog steeds ingedrukt is. Dus zowel de :python:`on_key_down()` als de :python:`on_key_up()` event handler krijgen een :python:`if` statement om te checken of :python:`car.dx` negatief is en zonodig de richting op :python:`LEFT` te zetten. Op een soortgelijke manier wordt de richting op :python:`RIGHT` gezet wanneer dat nodig is.

Nu hoeven we enkel nog de :python:`flip_x` eigenschap van de Actor in te stellen op basis van de huidige richting. Dat doen we in de :python:`draw()` functie:

.. code-block:: python
    :linenos:
    :lineno-start: 51
    :emphasize-lines: 6
    :caption: car_driver.py

    ################################
    # DRAW()
    ################################
    def draw():
        screen.fill('darkgreen')
        car.flip_x = (car.direction == LEFT)
        car.draw()

Als :python:`car.direction == LEFT` naar :python:`True` evalueert, dan krijgt :python:`car.flip_x` ook die waarde :python:`True`, waardoor de sprite horizontaal gespiegeld wordt. En als :python:`car.direction == LEFT` de waarde :python:`False` oplevert, dan krijgt :python:`car.flip_x` die waarde :python:`False`, waardoor de sprite in zijn normale staat blijft. Vind je regel 56 lastig te begrijpen, dan zou je het ook als volgt kunnen programmeren:

.. code-block:: python
    :linenos:
    :lineno-start: 51
    :emphasize-lines: 6-9
    :caption: car_driver.py

    ################################
    # DRAW()
    ################################
    def draw():
        screen.fill('darkgreen')
        if car.direction == LEFT:
            car.flip_x = True
        else:
            car.flip_x = False
        car.draw()

Maar hoe minder regels code, hoe beter, dus de eerste versie is eigenlijk het mooist.

Test de code om te zien of het autootje nu ook daadwerkelijk flipt wanneer je van richting verandert. 

Animatie
-----------------

In de :file:`images` map van dit project heb je vijf verschillende sprites geplaatst van het autootje, telkens in een iets andere houding. Door deze sprites snel achter elkaar te tonen, kunnen we een animatie-effect creëren waardoor het autootje lijkt te wiebelen terwijl het rijdt. 

Voor de animatie is het nodig dat we een lijst maken met de namen van de sprites die we willen gebruiken. Deze lijst geven we als waarde aan de eigenschap :python:`images` van de Actor.

.. code-block:: python
    :linenos:
    :lineno-start: 12
    :emphasize-lines: 2-5
    :caption: car_driver.py

    # ACTORS
    car_images = ['car_0', 'car_1', 'car_2', 'car_3', 'car_4']
    car = Actor(car_images[0])
    car.images = car_images
    car.fps = 15
    car.pos = (WIDTH // 2, HEIGHT // 2)
    car.speed = 3
    car.dx = 0
    car.dy = 0
    car.direction = RIGHT

In regel 13 maken we de lijst :python:`car_images` aan met de namen van de sprites. In regel 14 maken we de Actor aan met als beginbeeld de eerste sprite in de lijst (het eerste item in een lijst heeft index 0). In regel 15 kennen we de lijst :python:`car_images` toe aan de eigenschap :python:`images` van de Actor, zodat de Actor weet welke sprites hij moet gebruiken voor de animatie. Tenslotte stellen we in regel 16 in dat de animatie met 15 frames per seconde moet worden afgespeeld.

In de :python:`update()` functie moeten we nu nog de :python:`animate()` methode van de Actor aanroepen om de animatie te laten lopen.

.. code-block:: python
    :linenos:
    :lineno-start: 62
    :emphasize-lines: 7
    :caption: car_driver.py

    ################################
    # UPDATE()
    ################################
    def update():
        car.x += car.dx
        car.y += car.dy
        car.animate()

Als je nu de code runt, zou je moeten zien dat het autootje wiebelt. Echter, het wiebelt nu de hele tijd, ook wanneer het stilstaat. Dat is niet wat we willen. Het autootje moet alleen wiebelen wanneer het rijdt. Om dat te bereiken, moeten we de :python:`animate()` methode alleen aanroepen wanneer :python:`car.dx` of :python:`car.dy` niet gelijk is aan 0. Dat kan met een eenvoudig :python:`if` statement:

.. code-block:: python
    :linenos:
    :lineno-start: 62
    :emphasize-lines: 7-9
    :caption: car_driver.py

    ################################
    # UPDATE()
    ################################
    def update():
        car.x += car.dx
        car.y += car.dy
        if car.dx != 0 or car.dy != 0:
            car.animate()

Met nog geen 70 regels code hebben we nu een rijdend autootje gemaakt dat met de pijltjestoetsen bestuurd kan worden, dat van richting verandert wanneer je van rijrichting wisselt, en dat een leuke wiebelende animatie heeft terwijl het rijdt. Mooi werk!

.. figure:: images/car_driving.gif
    :alt: Car driving
    :align: center