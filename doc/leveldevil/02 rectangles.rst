.. role:: python(code)
   :language: python

.. |br| raw:: html

   <br/>

Kamer en valkuilen
===================

In dit deel programmeren we de kamer en de valkuilen van het eerste level van onze versie van Level Devil. We maken gebruik van het :python:`Rect` object om rechthoeken te definiëren die de kamer en de valkuilen voorstellen. De kamer is een statische rechthoek die altijd zichtbaar is, terwijl de valkuilen dynamische rechthoeken zijn die in hoogte toenemen wanneer ze actief worden. We gebruiken een lijst om de valkuilen op te slaan en een set om bij te houden welke valkuilen actief zijn.

Rectangles
-----------------

De kamer en de valkuilen in onze versie van Level Devil zijn rechthoeken. Voor het werken met rechthoeken gebruik je in Pygame Zero :python:`Rect` objecten. Uitgebreide informatie over deze objecten vind je in de `Pygame documentatie <https://www.pygame.org/docs/ref/rect.html>`_.

Breid de code als volgt uit:

.. code-block:: python
    :linenos:
    :lineno-start: 6
    :emphasize-lines: 4, 8
    :caption: level_devil.py

    COLOR_BACKGROUND = (153, 107, 7)
    COLOR_ROOM = (254, 184, 84)

    room_rect = Rect((10, 10), (200, 100))

    def draw():
        screen.fill(COLOR_BACKGROUND)
        screen.draw.filled_rect(room_rect, COLOR_ROOM)
        
    def update():
        pass

In regel 9 maken we een variabele :python:`room_rect` aan die een :python:`Rect` object bevat. Om een :python:`Rect` object te maken, geef je eerst de coördinaten van de linkerbovenhoek van de rechthoek op als een tuple (x, y) en daarna de breedte en hoogte van de rechthoek als een tweede tuple (width, height). In dit geval maken we dus een rechthoek met een breedte van 200 en een hoogte van 100, waarvan de linkerbovenhoek zich bevindt op positie (10, 10).

In regel 13 in de functie :python:`draw()` tekenen we de rechthoek met de methode :python:`screen.draw.filled_rect()`, die een :python:`Rect` object en een kleur als argumenten verwacht.

.. image:: images/level_devil_01.png
    :align: center
    :width: 300px

De huidige positie en afmetingen van :python:`room_rect` zijn nog niet goed. Het is mooier als de kamer wat groter is en zich meer in het midden van het venster bevindt. Echter, dit kan natuurlijk per level verschillen. Daarom gaan we een aparte functie :python:`setup_level()` maken waarin we de positie en afmetingen van de kamer kunnen aanpassen. Wijzig de code als volgt:

.. code-block:: python
    :linenos:
    :lineno-start: 6
    :emphasize-lines: 4, 6-10, 19-20
    :caption: level_devil.py

    COLOR_BACKGROUND = (153, 107, 7)
    COLOR_ROOM = (254, 184, 84)

    room_rect = Rect((0, 0), (0, 0))

    def setup_level(level):
        if level == 1:
            room_rect.width = int(3/5 * WIDTH)
            room_rect.height = int(1/3 * HEIGHT)
            room_rect.center = (WIDTH / 2, HEIGHT / 2)

    def draw():
        screen.fill(COLOR_BACKGROUND)
        screen.draw.filled_rect(room_rect, COLOR_ROOM)
        
    def update():
        pass
        
    # MAIN PROGRAM
    setup_level(1)

Regel 9 is gewijzigd zodat :python:`room_rect` nu een rechthoek is met een breedte en hoogte van 0. In de functie :python:`setup_level()` worden de breedte en hoogte van de rechthoek aangepast op basis van de waarde van het argument :python:`level`. Vooralsnog is er slechts één level, maar in de toekomst kunnen we hier eenvoudig meer levels toevoegen. De breedte van de kamer wordt ingesteld op 3/5 van de breedte van het venster en de hoogte op 1/3 van de hoogte van het venster. De positie van de kamer wordt ingesteld op het midden van het venster.

Denk eraan dat de functie :python:`setup_level()` onderaan, in het hoofdprogramma, moet worden aangeroepen om de instellingen daadwerkelijk toe te passen. 

.. image:: images/level_devil_02.png
    :align: center
    :width: 300px

Een lijst met rectangles
---------------------------

Elk level heeft slechts één kamer, maar er kunnen meerdere valkuilen zijn. Daarom gaan we een lijst maken waarin we de rechthoeken van de valkuilen kunnen opslaan. Voeg de volgende code toe:

.. code-block:: python
    :linenos:
    :lineno-start: 9
    :emphasize-lines: 2, 9-12, 17-18
    :caption: level_devil.py

    room_rect = Rect((0, 0), (0, 0))
    trap_rects = []

    def setup_level(level):
        if level == 1:
            room_rect.width = int(3/5 * WIDTH)
            room_rect.height = int(1/3 * HEIGHT)
            room_rect.center = (WIDTH / 2, HEIGHT / 2)
            trap_rect = Rect((room_rect.left + 100, room_rect.bottom), (50, 0))
            trap_rects.append(trap_rect)
            trap_rect = Rect((room_rect.left + 200, room_rect.bottom), (50, 0))
            trap_rects.append(trap_rect)

    def draw():
        screen.fill(COLOR_BACKGROUND)
        screen.draw.filled_rect(room_rect, COLOR_ROOM)
        for trap_rect in trap_rects:
            screen.draw.filled_rect(trap_rect, COLOR_ROOM)

In de regels 17 t/m 20 worden twee valkuilen toegevoegd aan de lijst :python:`trap_rects`. Valt je iets op aan de afmetingen van de valkuilen? Ze hebben een breedte van 50, maar een hoogte van 0. Dat klopt, want bij aanvang van het level moeten de valkuilen nog niet zichtbaar zijn.

Wanneer de speler een valkuil nadert, moet die valkuil 'actief' worden. Om bij te houden welke valkuilen actief zijn, maken we een set aan met de naam :python:`active_traps`:

.. code-block:: python
    :linenos:
    :lineno-start: 9
    :emphasize-lines: 3
    :caption: level_devil.py

    room_rect = Rect((0, 0), (0, 0))
    trap_rects = []
    active_traps = set()

Net als een *list* is een *set* een datastructuur die meerdere waarden kan bevatten. In tegenstelling tot een lijst kunnen de waarden in een set echter niet meerdere keren voorkomen en is er geen volgorde. In dit geval willen we alleen bijhouden welke valkuilen actief zijn, dus we hebben geen behoefte aan een volgorde of aan dubbele waarden. Daarom is een set hier een betere keuze dan een lijst.

In de set :python:`active_traps` zullen we de indices van de actieve valkuilen opslaan. De eerste valkuil in de lijst :python:`trap_rects` heeft index 0, de tweede valkuil heeft index 1, enzovoort. Wanneer een valkuil actief wordt, voegen we de bijbehorende index toe aan de set. Vervolgens kunnen we in de functie :python:`update()` controleren welke valkuilen actief zijn en de hoogte van die valkuilen laten toenemen totdat ze de grond hebben bereikt.

We programmeren nu eerst de code in de functie :python:`update()`. Verwijder het keyword :python:`pass` uit de functie :python:`update()` en vervang het door de volgende code:

.. code-block:: python
    :linenos:
    :lineno-start: 29
    :emphasize-lines: 2-6
    :caption: level_devil.py

    def update():
        for index, trap_rect in enumerate(trap_rects):
            if index in active_traps:
                if trap_rect.height < HEIGHT - room_rect.bottom:
                    trap_rect.height += 2
                    print(trap_rect.height)

In regel 30 wordt de :python:`enumerate()` functie gebruikt om zowel de index als het valkuil rechthoek object te krijgen bij het itereren door de lijst van valkuilen (zie ook de uitleg :ref:`hier <Enumerate>`). In regel 31 wordt gecontroleerd of de index van de valkuil in de set van actieve valkuilen zit. Als dat het geval is, wordt de hoogte van die valkuil verhoogd totdat deze de grond heeft bereikt. Het tijdelijke :python:`print` statement in regel 34 stelt je als programmeur in staat om te checken hoe de hoogte van de valkuil verandert.

We hebben nog geen speler geprogrammeerd die valkuilen kan activeren. Om de nieuwe functionaliteit toch te kunnen testen, gaan we de valkuilen tijdelijk handmatig activeren via de :python:`on_key_down()` functie. Voeg de volgende code toe boven de :python:`draw()` functie:

.. code-block:: python
    :linenos:
    :lineno-start: 23
    :emphasize-lines: 1-5
    :caption: level_devil.py

    def on_key_down(key):
        if key == keys.K_0:
            active_traps.add(0)
        elif key == keys.K_1:
            active_traps.add(1)

Wanneer je nu op de :kbd:`0` of :kbd:`1` toets drukt, wordt de bijbehorende valkuil actief: de index van de valkuil wordt met de :python:`add()` methode toegevoegd aan de set :python:`active_traps`. Probeer het maar eens uit. In de shell van Mu editor kun je dankzij het :python:`print` statement in de :python:`update()` functie tevens zien hoe de hoogte van de valkuilen toeneemt totdat ze de onderrand van het venster hebben bereikt.

.. image:: images/level_devil_traps.gif
    :alt: Level Devil
    :align: center
    :width: 300px