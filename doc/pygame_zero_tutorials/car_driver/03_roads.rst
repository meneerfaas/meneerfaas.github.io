.. role:: python(code)
   :language: python

.. |br| raw:: html

   <br/>

Op de weg blijven
===================

Het idee van Car Driver is dat je met je autootje op de weg blijft. In dit deel van de tutorial gaan we de weg tekenen en detecteren wanneer de auto van de weg af raakt.

Rectangles
----------------

Voor het tekenen van de wegen gebruiken we rechthoeken. Voor rechthoeken gebruik je in Pygame Zero :python:`Rect` objecten. Uitgebreide informatie over deze objecten vind je in de `Pygame documentatie <https://www.pygame.org/docs/ref/rect.html>`_. Omdat een weg uit meerdere rechthoeken kan bestaan, gebruiken we een lijst om de rechthoeken van de weg op te slaan. 

.. code-block:: python
    :linenos:
    :lineno-start: 12
    :emphasize-lines: 12-14
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

    # ROADS
    ROAD_WIDTH = 60
    road_rects = []

    ################################
    # EVENT HANDLERS
    ################################

In regel 24 definiëren we een constante :python:`ROAD_WIDTH` die de breedte van de weg aangeeft. In regel 25 maken we een lege lijst :python:`road_rects` aan waarin we later de rechthoeken van de weg gaan opslaan.

Het is handig om voor het creëren van de rechthoeken die de weg vormen een aparte functie te maken. We noemen deze functie :python:`build_roads()`. 

.. code-block:: python
    :linenos:
    :lineno-start: 23
    :emphasize-lines: 5-14
    :caption: car_driver.py

    # ROADS
    ROAD_WIDTH = 60
    road_rects = []

    ################################
    # BUILD ROADS
    ################################
    def build_roads():
        road_rect = Rect((0, 100), (200, ROAD_WIDTH))
        road_rects.append(road_rect)
        road_rect = Rect((200, 100), (ROAD_WIDTH, 150))
        road_rects.append(road_rect)
        road_rect = Rect((200, 250), (WIDTH - 200, ROAD_WIDTH))
        road_rects.append(road_rect)

    ################################
    # EVENT HANDLERS
    ################################

De functie :python:`build_roads()` maakt drie rechthoeken aan die samen de weg vormen. Uiteraard kun je zelf andere rechthoeken maken om een andere weg te creëren; gebruik je fantasie! Elke rechthoek wordt toegevoegd aan de lijst :python:`road_rects` zodat we er later mee kunnen werken.

In de :python:`draw()` functie tekenen we alle rechthoeken in de lijst op de volgende manier:

.. code-block:: python
    :linenos:
    :lineno-start: 69
    :emphasize-lines: 6-7
    :caption: car_driver.py

    ################################
    # DRAW()
    ################################
    def draw():
        screen.fill('darkgreen')
        for road_rect in road_rects:
            screen.draw.filled_rect(road_rect, 'darkgrey')
        car.flip_x = (car.direction == LEFT)
        car.draw()

Deze regels 74 en 75 kun je vrij letterlijk naar het Nederlands vertalen: 'voor elke rechthoek in de lijst van wegrechthoeken, teken een gevulde rechthoek op het scherm met de kleur donkergrijs'.

Wanneer je nu de code zou runnen, dan zie je nog geen wegen. Dat komt doordat we de functie :python:`build_roads()` nog niet hebben aangeroepen. Die aanroep moet bij aanvang van het spel als eerste gebeuren, dus we plaatsen hem onderaan de code in het hoofdprogramma:

.. code-block:: python
    :linenos:
    :lineno-start: 79
    :emphasize-lines: 10-13
    :caption: car_driver.py

    ################################
    # UPDATE()
    ################################
    def update():
        car.x += car.dx
        car.y += car.dy
        if car.dx != 0 or car.dy != 0:
            car.animate()

    ################################
    # MAIN
    ################################
    build_roads()

Collision detection
-------------------------

Om te detecteren of het autootje zich op de weg bevindt, moeten we controleren of het midden van de auto binnen een van de rechthoeken in de lijst :python:`road_rects` ligt. Dat doen we met behulp van de methode :python:`collidepoint()` van de :python:`Rect` objecten. Deze methode controleert of een bepaald punt binnen de rechthoek ligt en geeft :python:`True` terug als dat het geval is, en :python:`False` als dat niet het geval is.

Eerst doen we een kleine aanpassing in regel 14, waarin de :python:`car` Actor wordt gedefinieerd:

.. code-block:: python
    :linenos:
    :lineno-start: 12
    :emphasize-lines: 3
    :caption: car_driver.py

    # ACTORS
    car_images = ['car_0', 'car_1', 'car_2', 'car_3', 'car_4']
    car = Actor(car_images[0], anchor = ('center', 'bottom'))
    car.images = car_images
    car.fps = 15
    car.pos = (WIDTH // 2, HEIGHT // 2)
    car.speed = 3
    car.dx = 0
    car.dy = 0
    car.direction = RIGHT

Hiermee stellen we het :python:`midbottom` punt van de sprite in als ankerpunt. Vanaf nu verwijzen de :python:`car.pos` eigenschap en de :python:`car.x` en :python:`car.y` eigenschappen niet meer naar het midden van de sprite maar naar dit nieuwe ankerpunt. Dat is een logischer punt om te gebruiken voor de collision detection, omdat dat punt zich op de weg bevindt wanneer de auto op de weg staat.

.. figure:: images/car_anchor.png
    :alt: Car anchor point
    :align: center

Voor de collision detection maken we een nieuwe functie :python:`is_on_road()` die controleert of het ankerpunt van de auto binnen een van de rechthoeken in de lijst :python:`road_rects` ligt. Deze functie geeft :python:`True` terug als dat het geval is, en :python:`False` als dat niet het geval is.

.. code-block:: python
    :linenos:
    :lineno-start: 27
    :emphasize-lines: 12-19
    :caption: car_driver.py

    ################################
    # BUILD ROADS
    ################################
    def build_roads():
        road_rect = Rect((0, 100), (200, ROAD_WIDTH))
        road_rects.append(road_rect)
        road_rect = Rect((200, 100), (ROAD_WIDTH, 150))
        road_rects.append(road_rect)
        road_rect = Rect((200, 250), (WIDTH - 200, ROAD_WIDTH))
        road_rects.append(road_rect)

    ################################
    # IS ON ROAD?
    ################################
    def is_on_road():
        for road_rect in road_rects:
            if road_rect.collidepoint(car.pos):
                return True
        return False

De :python:`is_on_road()` functie doorloopt alle rechthoeken in de lijst :python:`road_rects` en controleert of het ankerpunt van de auto binnen een van deze rechthoeken ligt. Als dat het geval is, geeft de functie :python:`True` terug. Als geen van de rechthoeken het ankerpunt bevat, geeft de functie :python:`False` terug.

Om deze functionaliteit zichtbaar te maken in het spel, voegen we een :python:`emergency_light` Actor toe die groen of rood is, afhankelijk van of de auto op de weg staat of niet. 

.. code-block:: python
    :linenos:
    :lineno-start: 15
    :emphasize-lines: 4-5
    :caption: car_driver.py

    # ACTORS
    car_images = ['car_0', 'car_1', 'car_2', 'car_3', 'car_4']
    car = Actor(car_images[0], anchor = ('center', 'bottom'))
    car.images = car_images
    car.fps = 15
    car.pos = (WIDTH // 2, HEIGHT // 2)
    car.speed = 3
    car.dx = 0
    car.dy = 0
    car.direction = RIGHT

    emergency_light = Actor('emergency_light_green')
    emergency_light.pos = (WIDTH - 40, HEIGHT - 40)

In de :python:`draw()` functie tekenen we deze Actor als volgt:

.. code-block:: python
    :linenos:
    :lineno-start: 81
    :emphasize-lines: 10-14
    :caption: car_driver.py

    ################################
    # DRAW()
    ################################
    def draw():
        screen.fill('darkgreen')
        for road_rect in road_rects:
            screen.draw.filled_rect(road_rect, 'darkgrey')
        car.flip_x = (car.direction == LEFT)
        car.draw()
        if is_on_road():
            emergency_light.image = 'emergency_light_green'
        else:
            emergency_light.image = 'emergency_light_red'
        emergency_light.draw()

En daarmee is Car Driver zo goed als klaar. Om het helemaal af te maken, wijzigen we de startpositie van het autootje zodat hij op de weg staat. Het is namelijk wat vreemd als je het spel start en de rode lamp al brandt omdat de auto van de weg af staat.

.. code-block:: python
    :linenos:
    :lineno-start: 12
    :emphasize-lines: 6
    :caption: car_driver.py

    # ACTORS
    car_images = ['car_0', 'car_1', 'car_2', 'car_3', 'car_4']
    car = Actor(car_images[0], anchor = ('center', 'bottom'))
    car.images = car_images
    car.fps = 15
    car.pos = (50, 150)
    car.speed = 3
    car.dx = 0
    car.dy = 0
    car.direction = RIGHT

.. figure:: images/car_driver.gif
    :alt: Car Driver
    :align: center

Zoals gezegd is Car Driver nog geen echte game, maar je zou dat er wel van kunnen maken door meer game elementen toe te voegen. Je zou bijvoorbeeld kunnen programmeren dat de auto binnen een bepaalde tijd een doel moet bereiken zonder van de weg af te raken. Of overstekende voetgangers die je moet ontwijken.