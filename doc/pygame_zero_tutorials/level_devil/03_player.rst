.. role:: python(code)
   :language: python

.. |br| raw:: html

   <br/>

Player character
===================

In dit deel introduceren we het hoofdpersoontje van onze game. Het is een klein figuurtje dat door de speler wordt bestuurd en dat heelhuids door het level moet zien te komen zonder in een valkuil te vallen. 

Sprites
-----------------

Voor onze :python:`player` Actor gaan we verschillende sprites gebruiken. We willen het karakter namelijk animeren door een aantal sprites achter elkaar te tonen.

.. grid:: 2

    .. grid-item::

       .. image:: images/player_idle.gif
          :alt: Idle sprites
          :align: center
          :width: 32px

    .. grid-item::

        .. image:: images/player_run.gif
            :alt: Run sprites
            :align: center
            :width: 32px

Download de *zip folder* (een gecomprimeerde map) met alle sprites en pak de inhoud uit in de :file:`images` map van je project. 

.. card:: Zip folder
    :width: 25%

    .. image:: images/zip_folder.png
        :alt: Sprites
        :width: 100px

    :download:`download <assets/level_devil_sprites.zip>`

Voor het animeren van de player gebruiken we de ``pgzhelper`` module, gemaakt door `A Posteriori <https://www.aposteriori.com.sg/pygame-zero-helper/>`_. Deze module biedt handige functies voor het werken met sprites en animaties in Pygame Zero. Download het bestand :file:`pgzhelper.py` en plaats het in de hoofdmap van je project (dus in dezelfde map als je :file:`level_devil.py` bestand).

.. card:: Pygame Zero Helper
    :width: 25%

    .. image:: images/python_logo.png
        :alt: Pygame Zero Helper
        :width: 100px

    :download:`download <assets/pgzhelper.py>`

Als het goed is, beschik je nu over de volgende mappenstructuur in je project, waarbij de mappen in :file:`images` gevuld zijn met de juiste sprites:

.. uml::
    :align: center
    :html_format: svg

    @startuml
        @startfiles
        /games/level devil/level_devil.py
        /games/level devil/pgzhelper.py
        /games/level devil/images/effects/lightcast/
        /games/level devil/images/items/
        /games/level devil/images/player/appear/
        /games/level devil/images/player/disappear/
        /games/level devil/images/player/idle/
        /games/level devil/images/player/jump/
        /games/level devil/images/player/run/
        @endfiles
    @enduml

.. dropdown:: Bronvermelding
    :color: info
    :icon: cross-reference
    :open:

    De sprites zijn afkomstig van `itch.io <https://itch.io/>`_ uit de sprite packs `Pixel Adventure <https://pixelfrog-assets.itch.io/pixel-adventure-1>`_ en `2D Pixel Art Game - Spell/Magic FX <https://ppeldo.itch.io/2d-pixel-art-game-spellmagic-fx>`_. De pgzhelper module is zoals gezegd gemaakt door `A Posteriori <https://www.aposteriori.com.sg/pygame-zero-helper/>`_.

Idle
------------------

Wanneer de speler stil staat, willen we de *idle* animatie tonen. Deze bestaat uit 11 frames. In de :file:`images/player/idle/` map vind je de sprites voor deze animatie. De sprites zijn genummerd van 00 tot en met 10.

Begin met het importeren van de pgzhelper module, helemaal bovenaan je codebestand:

.. code-block:: python
    :linenos:
    :emphasize-lines: 1

    from pgzhelper import *
    
    # WINDOW SETTINGS
    WIDTH = 600
    HEIGHT = 400
    TITLE = 'Level Devil'

Dit is een andere manier van importeren dan we tot nu toe gewend zijn. Wellicht had je de code :python:`import pgzhelper` verwacht. Voor dit project is :python:`from pgzhelper import *` echter gemakkelijker, omdat we zo direct gebruik kunnen maken van de functies die deze module biedt, zonder dat we telkens :python:`pgzhelper.` ervoor hoeven te zetten.

.. dropdown:: Verschillende manieren van importeren
    :color: info
    :icon: info

    Er zijn verschillende manieren om een module te importeren in Python. De keuze voor een bepaalde manier hangt af van de situatie en persoonlijke voorkeur. Op internet kun je hier meer informatie over vinden.

    Meestal heeft de vorm :python:`import <modulenaam>` de voorkeur, omdat het duidelijk maakt waar de functies vandaan komen, waardoor je minder kans op errors hebt. In dit geval kiezen we echter voor :python:`from <modulenaam> import *`, omdat de pgzhelper module op deze manier gemakkelijker te gebruiken is in Pygame Zero code.

Lijst van sprites
^^^^^^^^^^^^^^^^^^^^^^^

Voor de animatie is het nodig dat we een lijst maken van de sprites die bij de animatie horen. We maken een lijst genaamd :python:`player_img_idle` en vullen deze met de namen van de 11 sprites voor de idle animatie. Je zou dit als volgt kunnen doen:

.. code-block:: python
    :linenos:
    :lineno-start: 11
    :emphasize-lines: 5-15

    room_rect = Rect((0, 0), (0, 0))
    trap_rects = []
    active_traps = set()

    player_img_idle = ['player/idle/player_idle_00',
                       'player/idle/player_idle_01',
                       'player/idle/player_idle_02',
                       'player/idle/player_idle_03',
                       'player/idle/player_idle_04',
                       'player/idle/player_idle_05',
                       'player/idle/player_idle_06',
                       'player/idle/player_idle_07',
                       'player/idle/player_idle_08',
                       'player/idle/player_idle_09',
                       'player/idle/player_idle_10']

Maar dit is nogal omslachtig. Gelukkig kunnen we dit ook veel korter schrijven met behulp van een *list comprehension* (zie ook de uitleg :ref:`hier <List_comprehensions>`):

.. code-block:: python
    :linenos:
    :lineno-start: 11
    :emphasize-lines: 5

    room_rect = Rect((0, 0), (0, 0))
    trap_rects = []
    active_traps = set()

    player_img_idle = [f'player/idle/player_idle_{i:02d}' for i in range(11)]

Met deze ene regel code, maak je een lijst bestaande uit f-strings die eindigen op de nummers 00 tot en met 10. De :python:`{i:02d}` syntax zorgt ervoor dat de getallen altijd uit twee cijfers bestaan, dus 0 wordt 00, 1 wordt 01, enzovoort. Hierdoor komen de namen van de sprites precies overeen met de strings in de lijst.

Je zou eventueel even een print statement kunnen toevoegen om te checken of de lijst correct is gevuld:

.. code-block:: python
    :linenos:
    :lineno-start: 15
    :emphasize-lines: 2

    player_img_idle = [f'player/idle/player_idle_{i:02d}' for i in range(11)]
    print(player_img_idle)

Je ziet dan in de shell dat de lijst de juiste namen bevat:

.. code-block:: none
    :class: wrap

    ['player/idle/player_idle_00', 'player/idle/player_idle_01', 'player/idle/player_idle_02', 'player/idle/player_idle_03', 'player/idle/player_idle_04', 'player/idle/player_idle_05', 'player/idle/player_idle_06', 'player/idle/player_idle_07', 'player/idle/player_idle_08', 'player/idle/player_idle_09', 'player/idle/player_idle_10']

Actor animeren
^^^^^^^^^^^^^^^^^^^^^^^

Nu we een lijst hebben met de namen van de sprites, kunnen we een Actor maken die deze sprites gebruikt voor de animatie. We maken een Actor genaamd :python:`player`:

.. code-block:: python
    :linenos:
    :lineno-start: 15
    :emphasize-lines: 3-5

    player_img_idle = [f'player/idle/player_idle_{i:02d}' for i in range(11)]

    player = Actor(player_img_idle[0], anchor = ('center', 'bottom'))
    player.images = player_img_idle
    player.fps = 20

In regel 17 geven we aan de Actor twee argumenten mee. Het eerste argument is de naam van de sprite die als eerste getoond moet worden. In dit geval is dat de sprite met index 0 in de lijst, dus :python:`player_img_idle[0]`. Het tweede argument geeft aan waar het ankerpunt van de Actor zich bevindt. In dit geval is dat het midden van de breedte en de onderkant van de hoogte van de sprite. Met het instellen van een ankerpunt zorg je ervoor dat de :python:`.pos`, :python:`.x` en :python:`.y` eigenschappen van de Actor verwijzen naar dat ankerpunt. Normaal gesproken verwijzen deze eigenschappen naar het midden van de sprite, maar we hebben dat dus gewijzigd naar het *midbottom* punt van de sprite. Meer informatie over ankerpunten vind je in de `Pygame Zero documentatie <https://pygame-zero.readthedocs.io/en/stable/builtins.html#anchor-point>`_.

Voor de animatie van de Actor moeten we de lijst met sprites toewijzen aan de :python:`.images` eigenschap van de Actor (regel 18) en de frames per seconde instellen met de :python:`.fps` eigenschap (regel 19). Dit zijn eigenschappen die door de pgzhelper module zijn toegevoegd aan de :python:`Actor` class.

De startpositie van de speler stellen we in in de :python:`setup_level()` functie:

.. code-block:: python
    :linenos:
    :lineno-start: 21
    :emphasize-lines: 10

    def setup_level(level):
        if level == 1:
            room_rect.width = int(3/5 * WIDTH)
            room_rect.height = int(1/3 * HEIGHT)
            room_rect.center = (WIDTH / 2, HEIGHT / 2)
            trap_rect = Rect((room_rect.left + 100, room_rect.bottom), (50, 0))
            trap_rects.append(trap_rect)
            trap_rect = Rect((room_rect.left + 200, room_rect.bottom), (50, 0))
            trap_rects.append(trap_rect)
            player.pos = (room_rect.left + 20, room_rect.bottom)

Omdat :python:`player.pos` verwijst naar het midbottom punt van de sprite (dankzij het ankerpunt dat we hebben ingesteld), wordt de speler precies op de grond geplaatst, 20 pixels vanaf de linkerkant van de kamer.

Om de speler zichtbaar te maken, moeten we :python:`player.draw()` aanroepen in de :python:`draw()` functie:

.. code-block:: python
    :linenos:
    :lineno-start: 38
    :emphasize-lines: 6

    def draw():
        screen.fill(COLOR_BACKGROUND)
        screen.draw.filled_rect(room_rect, COLOR_ROOM)
        for trap_rect in trap_rects:
            screen.draw.filled_rect(trap_rect, COLOR_ROOM)
        player.draw()

Tenslotte moeten we de animatie van de speler uitvoeren door :python:`player.animate()` aan te roepen in de :python:`update()` functie:

.. code-block:: python
    :linenos:
    :lineno-start: 45
    :emphasize-lines: 2

    def update():
        player.animate()
        for index, trap_rect in enumerate(trap_rects):
            if index in active_traps:
                if trap_rect.height < HEIGHT - room_rect.bottom:
                    trap_rect.height += 2

En voilà, de speler is nu zichtbaar en geanimeerd in het level!

.. image:: images/player_idle_in_room.gif
    :alt: Level Devil
    :align: center
    :width: 600px