.. role:: python(code)
   :language: python

.. |br| raw:: html

   <br/>

Player besturing
===================

In dit deel programmeren we de bewegingen van de speler. Met de pijltjestoetsen links, rechts en omhoog moet de speler horizontaal kunnen bewegen en springen. Daarbij moeten we ervoor zorgen dat de animaties van de speler overeenkomen met de bewegingen. 

Commentaar
-----------------

Inmiddels telt ons codebestand al meer dan 50 regels code. Om te voorkomen dat we straks door de bomen het bos niet meer zien, en om de code beter leesbaar te maken voor anderen is het een goed idee de code van commentaar te voorzien.  

.. code-block:: python
    :linenos:
    :emphasize-lines: 3, 8, 12, 17, 20, 25-27, 42-44, 51-53, 61-63, 65, 67, 73-75

    from pgzhelper import *

    # WINDOW SETTINGS
    WIDTH = 600
    HEIGHT = 400
    TITLE = 'Level Devil'

    # COLOR CONSTANTS
    COLOR_BACKGROUND = (153, 107, 7)
    COLOR_ROOM = (254, 184, 84)

    # ROOM AND TRAP VARIABLES
    room_rect = Rect((0, 0), (0, 0))
    trap_rects = []
    active_traps = set()

    # IMAGE LISTS FOR ANIMATED ACTORS
    player_img_idle = [f'player/idle/player_idle_{i:02d}' for i in range(11)]

    # PLAYER ACTOR
    player = Actor(player_img_idle[0], anchor = ('center', 'bottom'))
    player.images = player_img_idle
    player.fps = 20

    ################################
    # SETUP_LEVEL()
    ################################
    def setup_level(level):
        if level == 1:
            # ROOM SETUP
            room_rect.width = int(3/5 * WIDTH)
            room_rect.height = int(1/3 * HEIGHT)
            room_rect.center = (WIDTH / 2, HEIGHT / 2)
            # TRAPS SETUP
            trap_rect = Rect((room_rect.left + 100, room_rect.bottom), (50, 0))
            trap_rects.append(trap_rect)
            trap_rect = Rect((room_rect.left + 200, room_rect.bottom), (50, 0))
            trap_rects.append(trap_rect)
            # PLAYER SETUP
            player.pos = (room_rect.left + 20, room_rect.bottom)

    ################################
    # EVENT HANDLERS
    ################################
    def on_key_down(key):
        if key == keys.K_0:
            active_traps.add(0)
        elif key == keys.K_1:
            active_traps.add(1)

    ################################
    # DRAW()
    ################################
    def draw():
        screen.fill(COLOR_BACKGROUND)
        screen.draw.filled_rect(room_rect, COLOR_ROOM)
        for trap_rect in trap_rects:
            screen.draw.filled_rect(trap_rect, COLOR_ROOM)
        player.draw()
        
    ################################
    # UPDATE()
    ################################    
    def update():
        # PLAYER
        player.animate()
        # TRAPS
        for index, trap_rect in enumerate(trap_rects):
            if index in active_traps:
                if trap_rect.height < HEIGHT - room_rect.bottom:
                    trap_rect.height += 2
        
    ################################
    # MAIN PROGRAM
    ################################
    setup_level(1)

Kom je eigen code nog overeen met de hierboven getoonde code? Let daarbij ook op de regelnummers. Bij het toevoegen van nieuwe code kun je aan die regelnummers namelijk zien op welke positie in het bestand je de nieuwe code moet plaatsen.

Horizontale beweging
---------------------
Als de speler naar rechts loopt, moet het karakter ook naar rechts kijken. En als de speler naar links loopt, moet het karakter naar links kijken. Om dit te programmeren, maken we eerst twee constanten aan voor de richting van de speler:

.. code-block:: python
    :linenos:
    :lineno-start: 8
    :emphasize-lines: 5-7

    # COLOR CONSTANTS
    COLOR_BACKGROUND = (153, 107, 7)
    COLOR_ROOM = (254, 184, 84)

    # DIRECTION CONSTANTS
    LEFT = 1
    RIGHT = 2

    # ROOM AND TRAP VARIABLES

Voor de idle animatie hebben we al een lijst met sprites, maar voor de loopbeweging nog niet. Dat doen we nu:

.. code-block:: python
    :linenos:
    :lineno-start: 21
    :emphasize-lines: 3

    # IMAGE LISTS FOR ANIMATED ACTORS
    player_img_idle = [f'player/idle/player_idle_{i:02d}' for i in range(11)]
    player_img_run = [f'player/run/player_run_{i:02d}' for i in range(12)]

De idle animatie bestaat ui 11 frames, maar de run animatie uit 12 frames. Met de list comprehension in regel 23 vullen we de lijst :python:`player_img_run` met de bestandsnamen :python:`'player/run/player_run_00'`, :python:`'player/run/player_run_01'`, ..., :python:`'player/run/player_run_11'`.

Aan de :python:`player` Actor voegen we enkele nieuwe eigenschappen toe voor de beweging:

.. code-block:: python
    :linenos:
    :lineno-start: 25
    :emphasize-lines: 5-7

    # PLAYER ACTOR
    player = Actor(player_img_idle[0], anchor = ('center', 'bottom'))
    player.images = player_img_idle
    player.fps = 20
    player.speedx = 5
    player.dx = 0
    player.direction = RIGHT

De :python:`player.dx` eigenschap geeft aan hoe snel de speler beweegt in horizontale richting. In de wiskunde is :python:`dx` (*delta* x) een notatie voor de verandering van de x-coördinaat. Op dit moment is :python:`player.dx` ingesteld op 0, wat betekent dat de speler nog niet beweegt. Als de speler wél beweegt, moeten de sprites van de run animatie worden getoond in plaats van de sprites van de idle animatie. Tevens moet eventueel de waarde van :python:`player.direction` worden aangepast. We gaan deze logica programmeren in een aparte functie met de naam :python:`update_player_state()`:

.. code-block:: python
    :linenos:
    :lineno-start: 50
    :emphasize-lines: 1-12

    ################################
    # UPDATE_PLAYER_STATE()
    ################################
    def update_player_state():
        if player.dx < 0:
            player.direction = LEFT
            player.images = player_img_run
        elif player.dx > 0:
            player.direction = RIGHT
            player.images = player_img_run
        else:
            player.images = player_img_idle 

In de event handler functies :python:`on_key_down()` en :python:`on_key_up()` passen we de waarde van :python:`player.dx` aan op basis van welke toets is ingedrukt of losgelaten. Verwijder de huidige code uit :python:`on_key_down()` en voeg de volgende code toe:

.. code-block:: python
    :linenos:
    :lineno-start: 63
    :emphasize-lines: 4-9, 11-16

    ################################
    # EVENT HANDLERS
    ################################
    def on_key_down(key):
        if key == keys.RIGHT:
            player.dx += player.speedx
        elif key == keys.LEFT:
            player.dx -= player.speedx
        update_player_state()

    def on_key_up(key):
        if key == keys.RIGHT:
            player.dx -= player.speedx
        elif key == keys.LEFT:
            player.dx += player.speedx
        update_player_state()

Door zowel de :python:`on_key_down()` als de :python:`on_key_up()` functie te programmeren, zorgen we ervoor dat de speler beweegt zolang één toets is ingedrukt en dat de speler stopt met bewegen wanneer beide toetsen zijn losgelaten óf ingedrukt. Immers, als de speler de rechterpijltoets indrukt, wordt :python:`player.dx` verhoogd met :python:`player.speedx`, maar als de speler vervolgens ook de linkerpijltoets indrukt, wordt :python:`player.dx` weer verlaagd met :python:`player.speedx`, waardoor de speler stopt met bewegen. 

In beide gevallen wordt na het aanpassen van :python:`player.dx` de functie :python:`update_player_state()` aangeroepen om de animatie en richting van de speler bij te werken op basis van de nieuwe waarde van :python:`player.dx`.

In de :python:`update()` functie moeten we tenslotte nog de positie van de speler aanpassen op basis van de waarde van :python:`player.dx`. Ook checken we hier welke kant de speler op moet kijken door de waarde van :python:`player.direction` te gebruiken. Voeg de volgende code toe aan de :python:`update()` functie:

.. code-block:: python
    :linenos:
    :lineno-start: 90
    :emphasize-lines: 7-8

    ################################
    # UPDATE()
    ################################    
    def update():
        # PLAYER
        player.animate()
        player.x += player.dx
        player.flip_x = player.direction == LEFT
        # TRAPS
        for index, trap_rect in enumerate(trap_rects):
            if index in active_traps:
                if trap_rect.height < HEIGHT - room_rect.bottom:
                    trap_rect.height += 2

De :python:`player.flip_x` eigenschap is afkomstig van de pgzhelper module en kan de waarden `True` of `False` aannemen. Regel 97 zorgt ervoor dat de sprite horizontaal gespiegeld wordt wanneer :python:`player.direction` gelijk is aan :python:`LEFT`. Op deze manier kijkt de speler altijd in de richting waarin hij beweegt. Probeer het maar eens uit!

Op dit moment kan de speler nog door de muren van de kamer lopen. Met een eenvoudig :python:`if` statement kunnen we dit voorkomen:

.. code-block:: python
    :linenos:
    :lineno-start: 90
    :emphasize-lines: 9-13

    ################################
    # UPDATE()
    ################################    
    def update():
        # PLAYER
        player.animate()
        player.x += player.dx
        player.flip_x = player.direction == LEFT
        # keep player inside the room
        if player.left < room_rect.left:
            player.left = room_rect.left
        elif player.right > room_rect.right:
            player.right = room_rect.right
        # TRAPS
        for index, trap_rect in enumerate(trap_rects):
            if index in active_traps:
                if trap_rect.height < HEIGHT - room_rect.bottom:
                    trap_rect.height += 2

.. image:: images/player_run_in_room.gif
    :alt: Level Devil
    :align: center
    :width: 600px

Springen
-----------------

Om de speler op een natuurlijk manier te laten springen, moeten we zwaartekracht simuleren. Voeg daartoe een constante :python:`GRAVITY` toe aan de code:

.. code-block:: python
    :linenos:
    :lineno-start: 12
    :emphasize-lines: 5-6

    # DIRECTION CONSTANTS
    LEFT = 1
    RIGHT = 2

    # GRAVITY
    GRAVITY = 0.3

In de :file:`images/player/jump/` map staan de sprites voor de jump animatie. Het zijn er slechts twee: één voor de beweging omhoog en één voor omlaag. We vullen de lijst :python:`player_img_jump` met deze sprites:

.. code-block:: python
    :linenos:
    :lineno-start: 24
    :emphasize-lines: 4

    # IMAGE LISTS FOR ANIMATED ACTORS
    player_img_idle = [f'player/idle/player_idle_{i:02d}' for i in range(11)]
    player_img_run = [f'player/run/player_run_{i:02d}' for i in range(12)]
    player_img_jump = ['player/jump/player_jump', 'player/jump/player_fall']

De :python:`player` Actor moet worden uitgebreid met enkele eigenschappen voor de verticale beweging:

.. code-block:: python
    :linenos:
    :lineno-start: 29
    :emphasize-lines: 6, 8, 10

    # PLAYER ACTOR
    player = Actor(player_img_idle[0], anchor = ('center', 'bottom'))
    player.images = player_img_idle
    player.fps = 20
    player.speedx = 5
    player.speedy = 6
    player.dx = 0
    player.dy = 0
    player.direction = RIGHT
    player.in_air = False

De :python:`player.speedy` eigenschap bevat de sprongsnelheid van de speler. De :python:`player.dy` eigenschap geeft aan hoe snel de speler beweegt in verticale richting. De :python:`player.in_air` eigenschap is een boolean die bijhoudt of de speler zich in de lucht bevindt of niet. Dat is belangrijk om bij te houden, teneinde te voorkomen dat de speler meerdere keren kan springen terwijl hij in de lucht is. 

De speler moet springen wanneer op de toets pijltje omhoog wordt gedrukt. Voeg de volgende code toe aan de :python:`on_key_down()` functie:

.. code-block:: python
    :linenos:
    :lineno-start: 70
    :emphasize-lines: 9-11

    ################################
    # EVENT HANDLERS
    ################################
    def on_key_down(key):
        if key == keys.RIGHT:
            player.dx += player.speedx
        elif key == keys.LEFT:
            player.dx -= player.speedx
        elif key == keys.UP and not player.in_air:
            player.dy = -player.speedy
            player.in_air = True
        update_player_state()

Wanneer de pijltje omhoog toets wordt ingedrukt en de speler zich niet in de lucht bevindt, wordt :python:`player.dy` ingesteld op de negatieve waarde van :python:`player.speedy`, waardoor de speler met de sprongsnelheid omhoog beweegt. Tevens wordt :python:`player.in_air` ingesteld op `True`, zodat de speler niet nogmaals kan springen voordat hij weer op de grond staat.

In de :python:`update()` functie moeten we tenslotte nog de verticale beweging van de speler programmeren. Voeg de volgende code toe aan de :python:`update()` functie:

.. code-block:: python
    :linenos:
    :lineno-start: 100
    :emphasize-lines: 9-19

    ################################
    # UPDATE()
    ################################    
    def update():
        # PLAYER
        player.animate()
        player.x += player.dx
        player.flip_x = player.direction == LEFT
        if player.in_air:
            player.y += player.dy
            player.dy += GRAVITY
            if player.dy < 0:
                player.image = player_img_jump[0]
            else:
                player.image = player_img_jump[1]
            if player.y > room_rect.bottom:
                player.y = room_rect.bottom
                player.dy = 0
                player.in_air = False
        # keep player inside the room

Regels 109 t/m 118 worden alleen uitgevoerd als de speler zich in de lucht bevindt. In regel 110 zie je dat de verticale snelheid van de speler toeneemt door de zwaartekracht. Op het moment van springen is de snelheid negatief (coderegel 79), maar door regel 110 wordt deze steeds minder negatief, totdat de speler het hoogste punt van de sprong heeft bereikt en weer naar beneden begint te vallen.

In de regels 111 t/m 114 wordt de sprite van de speler aangepast op basis van de verticale snelheid. Zolang de speler omhoog beweegt, wordt de eerste sprite van de jump animatie getoond. Zodra de speler begint te vallen, wordt de tweede sprite van de jump animatie getoond.

In regel 115 wordt gecontroleerd of de speler onder de grond is gezakt. Als dat het geval is, wordt de speler weer op de grond geplaatst en worden de verticale snelheid en de :python:`in_air` status van de speler gereset, zodat de speler weer kan springen.

Op dit moment zijn de waarden van :python:`player.speedy` en :python:`GRAVITY` zodanig, dat de speler niet hoger dan het plafond van de kamer springt. Maar als je de waarde van :python:`player.speedy` in regel 34 verhoogt naar bijvoorbeeld :python:`player.speedy = 10`, dan zal de speler wél door het plafond van de kamer heen springen. Als je wilt, kun je in de :python:`update()` functie een extra check toevoegen om dit te voorkomen. Dat is aan jou!

