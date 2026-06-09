.. role:: python(code)
   :language: python

.. |br| raw:: html

   <br/>

Valkuilen
===================

In dit deel programmeren we de collisions tussen de speler en de valkuilen. We zorgen ervoor dat de valkuilen actief worden wanneer de speler in de buurt komt en dat de speler daadwerkelijk in de valkuil valt wanneer hij er overheen loopt. Ook moet de speler doodgaan wanneer hij in de valkuil valt. 

Activeren
-----------------

Een valkuil moet actief worden zodra de speler hem nadert. In de :python:`update()` zou je kunnen berekenen hoe dicht de speler bij een valkuil is en die actief maken wanneer de afstand kleiner is dan een bepaalde drempel. Een wellicht gemakkelijkere manier is het creëren van een triggerzone rondom elke valkuil. Wanneer de speler die triggerzone betreedt, wordt de bijbehorende valkuil actief. We kunnen zo'n triggerzone maken door een rechthoek te definiëren die iets groter is dan de valkuil zelf. Vervolgens kunnen we controleren of de speler deze triggerzone raakt.

.. figure:: images/level_devil_trigger_zones.png
    :align: center
    :width: 600px

Voor het maken van de trigger zone rectangles kunnen we de :python:`inflate()` methode van de :python:`Rect` class gebruiken. Deze methode maakt een nieuwe rechthoek die is vergroot (of verkleind) ten opzichte van de originele rechthoek. Meer info vind je in de `Pygame documentatie <https://www.pygame.org/docs/ref/rect.html#pygame.Rect.inflate>`_. 

Breid de :python:`for` loop in de :python:`update()` functie als volgt uit:

.. code-block:: python
    :linenos:
    :lineno-start: 124
    :emphasize-lines: 3, 7-10

        # TRAPS
        for index, trap_rect in enumerate(trap_rects):
            # animate active traps
            if index in active_traps:
                if trap_rect.height < HEIGHT - room_rect.bottom:
                    trap_rect.height += 2
            # activate trap
            trigger_zone = trap_rect.inflate(40, 10)
            if player.colliderect(trigger_zone):
                active_traps.add(index)

In regel 131 maken we een triggerzone door de valkuil rechthoek 'op te blazen' met 40 pixels in de breedte (20 naar links en 20 naar rechts) en 10 pixels in de hoogte (5 naar boven en 5 naar beneden). Met de :python:`colliderect()` methode controleren we of de speler de triggerzone raakt. Als dat het geval is, voegen we de index van die valkuil toe aan de set van actieve valkuilen.

In de val vallen
-----------------

Op dit moment kan de speler nog over de valkuilen lopen zonder af te gaan. In de :python:`update()` functie moeten we controleren of de speler een actieve valkuil raakt. Als dat het geval is, moet de speler naar beneden vallen. Om het 'stervensproces' van de speler bij te houden, voegen we eerst twee nieuwe eigenschappen toe aan de :python:`player` Actor:

.. code-block:: python
    :linenos:
    :lineno-start: 29
    :emphasize-lines: 11-12

    # PLAYER ACTOR
    player = Actor(player_img_idle[0], anchor = ('center', 'bottom'))
    player.images = player_img_idle
    player.fps = 20
    player.speedx = 2
    player.speedy = 6
    player.dx = 0
    player.dy = 0
    player.direction = RIGHT
    player.in_air = False
    player.dying = False
    player.dead = False

In de :python:`update()` checken we of de speler een actieve valkuil raakt.

.. code-block:: python
    :linenos:
    :lineno-start: 126
    :emphasize-lines: 3, 7-11

        # TRAPS
        for index, trap_rect in enumerate(trap_rects):
            # animate active traps and check if player hits trap
            if index in active_traps:
                if trap_rect.height < HEIGHT - room_rect.bottom:
                    trap_rect.height += 2
                trap_zone = trap_rect.inflate(-30, 2)
                if player.colliderect(trap_zone):
                    player.dying = True
                    player.in_air = True
                    player.dx = 0
            # activate trap
            trigger_zone = trap_rect.inflate(40, 10)
            if player.colliderect(trigger_zone):
                active_traps.add(index)

Om ervoor te zorgen dat de speler als hij op de rand van de valkuil staat niet meteen afgaat, maken we de valkuilzone iets kleiner dan de valkuil zelf. In regel 132 passen we wederom de :python:`inflate()` methode toe, maar deze keer met een negatieve waarde in x-richting en een kleine positieve waarde in y-richting. Met die positieve waarde in y-richting laten we de valkuil eigenlijk een stukje boven de kamervloer uitsteken. Dat is nodig om de collision detection in regel 133 goed te laten werken.

Als de speler de valkuilzone raakt, zetten we de :python:`dying` en :python:`in_air` eigenschappen van de speler op :python:`True`. Tevens zetten we de horizontale snelheid :python:`dx` van de speler op 0. Dat is een gemakkelijke manier om te voorkomen dat de speler door de wand van de valkuil valt.

Om de speler daadwerkelijk te laten vallen, hoeven we nu alleen nog maar regel 117 uit te breiden met een extra voorwaarde:

.. code-block:: python
    :linenos:
    :lineno-start: 110
    :emphasize-lines: 8

        if player.in_air:
            player.y += player.dy
            player.dy += GRAVITY
            if player.dy < 0:
                player.image = player_img_jump[0]
            else:
                player.image = player_img_jump[1]
            if player.y > room_rect.bottom and not player.dying:
                player.y = room_rect.bottom
                player.dy = 0
                player.in_air = False

Door de extra voorwaarde :python:`not player.dying` zakt de speler tóch door de vloer wanneer hij doodgaat.

Van dying naar dead
----------------------

Wanneer de speler in de valkuil valt, moet hij net voordat hij uit het venster verdwijnt op een 'spectaculaire' manier sterven. Daartoe voegen we allereerst een nieuwe lijst met sprites toe:

.. code-block:: python
    :linenos:
    :lineno-start: 24
    :emphasize-lines: 5

    # IMAGE LISTS FOR ANIMATED ACTORS
    player_img_idle = [f'player/idle/player_idle_{i:02d}' for i in range(11)]
    player_img_run = [f'player/run/player_run_{i:02d}' for i in range(12)]
    player_img_jump = ['player/jump/player_jump', 'player/jump/player_fall']
    player_img_disappear = [f'player/disappear/player_disappear_{i:02d}' for i in range(8)]

In de :python:`update()` functie checken we vervolgens of de speler aan het sterven is en of hij de onderrand van het venster nadert. Als dat het geval is, zetten we de :python:`dead` eigenschap op :python:`True`. We zetten ook de animatie van de speler op de 'disappear' sprites.

.. code-block:: python
    :linenos:
    :lineno-start: 106
    :emphasize-lines: 22-25

    def update():
        # PLAYER
        player.animate()
        player.x += player.dx
        player.flip_x = player.direction == LEFT
        if player.in_air and not player.dead:
            player.y += player.dy
            player.dy += GRAVITY
            if player.dy < 0:
                player.image = player_img_jump[0]
            else:
                player.image = player_img_jump[1]
            if player.y > room_rect.bottom and not player.dying:
                player.y = room_rect.bottom
                player.dy = 0
                player.in_air = False
        # keep player inside the room
        if player.left < room_rect.left:
            player.left = room_rect.left
        elif player.right > room_rect.right:
            player.right = room_rect.right
        # dying player
        if player.dying and HEIGHT - player.bottom < 30:
            player.dead = True
            player.images = player_img_disappear

Als je nu de code runt, zul je zien dat er vreemde dingen gebeuren. De speler valt, maar springt een vreemde kant op en reageert ook nog steeds op de besturing. Dat laatste gaan we eerst verhelpen:

.. code-block:: python
    :linenos:
    :lineno-start: 73
    :emphasize-lines: 5-6, 17-18

    ################################
    # EVENT HANDLERS
    ################################
    def on_key_down(key):
        if player.dying or player.dead:
            return
        if key == keys.RIGHT:
            player.dx += player.speedx
        elif key == keys.LEFT:
            player.dx -= player.speedx
        elif key == keys.UP and not player.in_air:
            player.dy = -player.speedy
            player.in_air = True
        update_player_state()

    def on_key_up(key):
        if player.dying or player.dead:
            return
        if key == keys.RIGHT:
            player.dx -= player.speedx
        elif key == keys.LEFT:
            player.dx += player.speedx
        update_player_state()

Als de speler stervende of dood is, worden de event handler functies meteen verlaten. Zo voorkomen we dat de speler nog steeds op de besturing reageert.

Vervolgens willen we de disappear animatie goed laten zien. Daartoe zorgen we ervoor de de speler niet verder valt wanneer hij dood is:

.. code-block:: python
    :linenos:
    :lineno-start: 110
    :emphasize-lines: 6

    def update():
        # PLAYER
        player.animate()
        player.x += player.dx
        player.flip_x = player.direction == LEFT
        if player.in_air and not player.dead:
            player.y += player.dy
            player.dy += GRAVITY
        
Dit is een verbetering, maar de animatie lijkt niet af te spelen. De speler blijft op dezelfde sprite staan.

.. figure:: images/level_devil_dead_01.png
    :align: center
    :width: 600px

De oorzaak zit in regel 132. In het statement :python:`if player.dying and HEIGHT - player.bottom < 30:` zijn beide voorwaarden waar en dat verandert niet wanneer de speler dood is. Deze code wordt dus elke update frame opnieuw uitgevoerd, waardoor de animatie steeds weer opnieuw begint bij de eerste sprite. De volgende toevoeging lost dit op:

.. code-block:: python
    :linenos:
    :lineno-start: 131
    :emphasize-lines: 2

        # dying player
        if player.dying and HEIGHT - player.bottom < 30 and not player.dead:
            player.dead = True
            player.images = player_img_disappear

We zijn er bijna. De animatie wordt afgespeeld, maar herhaalt zich steeds. Bij de idle en run animaties is dat prima, maar de disappear animatie moet juist maar één keer worden afgespeeld. We plaatsen de :python:`player.animate()` aanroep in een :python:`if` statement zodat deze alleen wordt uitgevoerd wanneer de speler niet dood is of wanneer de animatie nog niet het laatste frame heeft bereikt:

.. code-block:: python
    :linenos:
    :lineno-start: 110
    :emphasize-lines: 3-4

    def update():
        # PLAYER
        if not player.dead or player.image != player.images[-1]:
            player.animate()

Nu werkt de animatie zoals het hoort. De speler valt in de valkuil, gaat dood en verdwijnt met de disappear animatie.

.. figure:: images/player_die_in_trap.gif
    :align: center
    :width: 600px