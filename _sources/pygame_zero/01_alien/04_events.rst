Events
===========

De meeste computerprogramma's, en games in het bijzonder, reageren op handelingen van de gebruiker, bijvoorbeeld wanneer de gebruiker de muis beweegt of een toets op het toetsenbord indrukt. Een gebeurtenis waarop een computerprogramma kan reageren noemen we een *event*. Voorbeelden van events zijn:

* de gebruiker klikt met de linker muisknop;
* de gebruiker hovert (zweeft) met de muis ergens overheen;
* de gebruiker drukt op de spatiebalk.

Je kunt je programma op events laten reageren door speciale functies te definiëren, zogenoemde *event handler* functies. De namen van dit soort functies beginnen altijd met het Engelse woord 'on'. Voorbeelden van de in Pygame Zero beschikbare event handler functies zijn:

* :python:`on_mouse_down()`
* :python:`on_mouse_move()`
* :python:`on_key_down()`

We starten in dit hoofdstuk met de code die onze alien van links naar rechts door het venster laat bewegen:

.. code-block:: python
   :linenos:
   :caption: alien.py
   :name: alien_v23

   # Vensterafmetingen
   WIDTH = 600
   HEIGHT = 400

   # Roze alien Actor
   alien = Actor('alien_pink')
   alien.midleft = (0, HEIGHT / 2)
   alien.speed = 10

   # De draw() functie van de game
   def draw():
      screen.clear()
      alien.draw()

   # De update() functie van de game
   def update():
      alien.x += alien.speed
      if alien.left >= WIDTH:
         alien.right = 0

Muisklikken
-----------

Om het programma op een muisklik te laten reageren, voeg je de :python:`on_mouse_down()` event handler toe:

.. code-block:: python
   :class: no-copybutton
   :linenos:
   :emphasize-lines: 21-23
   :caption: alien.py
   :name: alien_v24

   # Vensterafmetingen
   WIDTH = 600
   HEIGHT = 400

   # Roze alien Actor
   alien = Actor('alien_pink')
   alien.midleft = (0, HEIGHT / 2)
   alien.speed = 10

   # De draw() functie van de game
   def draw():
      screen.clear()
      alien.draw()

   # De update() functie van de game
   def update():
      alien.x += alien.speed
      if alien.left >= WIDTH:
         alien.right = 0

   # Mouse down event handler
   def on_mouse_down():
      print("Muisklik!")

Run deze code. Klik enkele keren met de muis in het game venster en zie dat onderin het Mu editor venster bij elke klik de tekst *Muisklik!* wordt afgedrukt.

.. figure:: images/events_01.png

We kunnen in de :python:`on_mouse_down()` functie een variabele met de naam :python:`pos` gebruiken om de positie van de muis te achterhalen. Pas de functie als volgt aan en bekijk het resultaat:

.. code-block:: python
   :class: no-copybutton
   :linenos:
   :lineno-start: 21
   :emphasize-lines: 2-3
   :caption: alien.py
   :name: alien_v25

   # Mouse down event handler
   def on_mouse_down(pos):
      print("Muisklik op positie", pos)

Ook is het mogelijk te detecteren met welke muisknop is geklikt. Probeer het volgende maar eens:

.. code-block:: python
   :class: no-copybutton
   :linenos:
   :lineno-start: 21
   :emphasize-lines: 2-3
   :caption: alien.py
   :name: alien_v26

   # Mouse down event handler
   def on_mouse_down(button, pos):
      print("Muisklik met", button, "op positie", pos)

Met de positie van de muisklik is het mogelijk om te checken of de gebruiker óp of náást onze roze alien heeft geklikt. Daarvoor hebben we *collision detection* nodig.

.. _collision-detection:

Collision detection
-------------------

Het Engelse woord collision betekent botsing. Collision detection is een belangrijke techniek bij het programmeren van games, die we gebruiken om vast te stellen of twee objecten elkaar raken of zelfs overlappen. Wanneer je bijvoorbeeld een schietspel programmeert is het handig om te signaleren wanneer een granaat sprite de vijand sprite raakt, want waarschijnlijk moet er dan iets gebeuren (de vijand ontploft, er wordt een punt bij de score opgeteld, etcetera).

.. figure:: images/collision_01.png

Actors in Pygame Zero beschikken over verschillende functies voor collision detection. Met bijvoorbeeld de functie :python:`colliderect()` kun je checken of de twee rechthoeken die twee sprites innemen elkaar overlappen. 

.. figure:: images/collision_02.png

Voor onze muisklik event handler hebben we een andere functie nodig, namelijk een die checkt of een punt zich binnen de rechthoek van een sprite bevindt. Want wij willen weten of het punt van de muisklik zich binnen het gebied van de alien sprite bevindt.

.. figure:: images/collision_03.png

De functie die wij nodig hebben is :python:`collidepoint()`. Deze gebruiken we in het volgende :python:`if` statement:

.. code-block:: python
   :class: no-copybutton
   :linenos:
   :lineno-start: 21
   :emphasize-lines: 3-6
   :caption: alien.py
   :name: alien_v27

   # Mouse down event handler
   def on_mouse_down(button, pos):
      if alien.collidepoint(pos):
         print("Au!")
      else:
         print("Mis!")

De regels 23 tot en met 26 kun je vertalen als: "Als de muispositie :python:`pos` zich binnen de rechthoek van :python:`alien` bevindt, druk dan :python:`"Au!"` af en druk anders :python:`"Mis!"` af." Test de werking van deze code. Gaat de alien te snel om hem te kunnen raken, verlaag dan de snelheid in regel 8 van je code.

Met het printen van :python:`"Au!"` en :python:`"Mis!"` kun je snel testen of de collision detection goed werkt, maar het is natuurlijk leuker als een muisklik gevolgen heeft voor de alien. Je zou hem bijvoorbeeld met elke klik sneller kunnen laten bewegen:

.. code-block:: python
   :class: no-copybutton
   :linenos:
   :lineno-start: 21
   :emphasize-lines: 4
   :caption: alien.py
   :name: alien_v28

   # Mouse down event handler
   def on_mouse_down(button, pos):
      if alien.collidepoint(pos):
         alien.speed += 1

Zet voordat je deze code uitvoert de startsnelheid in regel 8 op 1:

.. code-block:: python
   :class: no-copybutton
   :linenos:
   :lineno-start: 8
   :caption: alien.py
   :name: alien_v29

   alien.speed = 1

.. dropdown:: Extra: snelheid in het venster tonen
   :color: info
   :icon: info

   Misschien vind je het leuk om de snelheid van de alien in het venster te zien. Dit kun je doen door aan je :python:`draw()` functie de volgende regel toe te voegen:

   .. code-block:: python
      :class: no-copybutton
      :linenos:
      :lineno-start: 10
      :emphasize-lines: 4
      :caption: alien.py
      :name: alien_v30

      # De draw() functie van de game
      def draw():
         screen.clear()
         screen.draw.text(f"Snelheid: {alien.speed}.", (10, 10), color="orange")
         alien.draw()

   In regel 13 gebruiken we de functie :python:`screen.draw.text(text, pos, color)` om een tekst op een bepaalde positie in een bepaalde kleur op het scherm te tonen. Het :python:`text` argument ziet er een beetje ingewikkeld uit:
   
      :python:`f"Snelheid: {alien.speed}."`
   
   De letter f geeft aan dat de tekst een *formatted string*, kortweg f-string, is. Met zo'n f-string kun je op een mooie manier de waarden van variabelen verwerken in een tekst door ze tussen accolades :python:`{...}` te zetten.

   In de paragraaf over muisklikken hierboven deden we bijvoorbeeld dit:

   .. code-block:: python
      :class: no-copybutton
      :name: alien_v31

      print("Muisklik met", button, "op positie", pos)

   Maar je zou dit met een f-string als volgt kunnen doen:
   
   .. code-block:: python
      :class: no-copybutton
      :name: alien_v32

      print(f"Muisklik met {button} op positie {pos}.")

   Behalve de snelheid, zou je ook de positie van de alien op het scherm kunnen tonen, bijvoorbeeld op deze manier:

   .. code-block:: python
      :class: no-copybutton
      :linenos:
      :lineno-start: 13
      :caption: alien.py
      :name: alien_v33

      screen.draw.text(f"Snelheid: {alien.speed}. Positie: {alien.center}.", (10, 10), color="orange")
      
   En om deze twee zinnen op twee regels af te drukken, kun je het newline karakter :python:`\\n` gebruiken:

   .. code-block:: python
      :class: no-copybutton
      :linenos:
      :lineno-start: 13
      :caption: alien.py
      :name: alien_v34

      screen.draw.text(f"Snelheid: {alien.speed}.\nPositie: {alien.center}.", (10, 10), color="orange")


.. dropdown:: Opdracht 01
   :color: secondary
   :icon: pencil

   a. Wijzig je programma zodat de alien van richting verandert zodra erop wordt geklikt. Ging de alien naar rechts, dan moet hij dus naar links en vice versa.

   .. dropdown:: Hint
      :color: secondary
      :icon: light-bulb

      Je hoeft slechts één regel code te veranderen.

      .. code-block:: python
         :class: no-copybutton
         :linenos:
         :lineno-start: 21
         :emphasize-lines: 4
         :caption: alien.py
         :name: alien_v35

         # Mouse down event handler
         def on_mouse_down(button, pos):
            if alien.collidepoint(pos):
               alien.speed = ...

   .. dropdown:: Oplossing
      :color: secondary
      :icon: check-circle

      .. code-block:: python
         :class: no-copybutton
         :linenos:
         :lineno-start: 21
         :emphasize-lines: 4
         :caption: alien.py
         :name: alien_v36

         # Mouse down event handler
         def on_mouse_down(button, pos):
            if alien.collidepoint(pos):
               alien.speed = -alien.speed

   b. Zorg ervoor dat de alien weer aan de andere kant van het venster verschijnt nadat hij buiten beeld verdwijnt.

   .. dropdown:: Hint
      :color: secondary
      :icon: light-bulb

      Hiervoor moet je het :python:`if` statement in de :python:`update()` functie uitbreiden met een :python:`elif`. 

   .. dropdown:: Oplossing
      :color: secondary
      :icon: check-circle

      .. code-block:: python
         :class: no-copybutton
         :linenos:
         :lineno-start: 15
         :emphasize-lines: 6-7
         :caption: alien.py
         :name: alien_v37

         # De update() functie van de game
         def update():
            alien.x += alien.speed
            if alien.left >= WIDTH:
               alien.right = 0
            elif alien.right <= 0:
               alien.left = WIDTH     

.. dropdown:: Opdracht 02
   :color: secondary
   :icon: pencil

   Vervang je code door de onderstaande (kopiëren en plakken) en run de code om te zien wat er gebeurt.

   .. code-block:: python
      :linenos:
      :caption: alien.py
      :name: alien_v38

      # Vensterafmetingen
      WIDTH = 600
      HEIGHT = 400

      # Roze alien Actor
      alien = Actor('alien_pink')
      alien.center = (WIDTH / 2, HEIGHT / 2)
      alien.speed = 1

      # De draw() functie van de game
      def draw():
         screen.clear()
         alien.draw()

      # De update() functie van de game
      def update():
         alien.y += alien.speed
         if alien.bottom > HEIGHT:
            pass
      
      # Mouse down event handler
      def on_mouse_down(button, pos):
         if alien.collidepoint(pos):
            pass

   De alien beweegt naar beneden en verdwijnt uit het venster. Vervang het keyword :python:`pass` (wat in Python betekent 'doe niets') in de regels 19 en 24 door code die ervoor zorgt dat:

   * de alien stil blijft staan zodra hij de onderkant van het venster raakt;
   * de alien 50 pixels omhoog gaat zodra je er met de muis op klikt (en vervolgens weer naar beneden valt).

   .. dropdown:: Oplossing
      :color: secondary
      :icon: check-circle

      .. code-block:: python
         :linenos:
         :lineno-start: 15
         :emphasize-lines: 5, 10
         :caption: alien.py

         # De update() functie van de game
         def update():
            alien.y += alien.speed
            if alien.bottom > HEIGHT:
               alien.bottom = HEIGHT
         
         # Mouse down event handler
         def on_mouse_down(button, pos):
            if alien.collidepoint(pos):
               alien.y -= 50 

.. _keyboard_events:

Keyboard events
-------------------

In veel spellen kan de speler de game besturen met het toetsenbord. In Pygame Zero kun je op toetsaanslagen reageren met de :python:`on_key_down()` en :python:`on_key_up()` event handlers. De eerste reageert op het indrukken van een toets, de tweede op het loslaten van een toets.

Springen met de spatiebalk
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Voor de volgende voorbeelden hebben we een nieuwe sprite nodig. Download de :download:`alien jump sprite <assets/images/alien_pink_jump.png>` naar de :file:`images` map van je game.

.. image:: assets/images/alien_pink_jump.png

Vervang de code in :file:`alien.py` door de volgende code:

.. code-block:: python
   :linenos:
   :caption: alien.py

   # Vensterafmetingen
   WIDTH = 600
   HEIGHT = 400

   # Roze alien Actor
   alien = Actor('alien_pink')
   alien.x = WIDTH / 2
   alien.bottom = HEIGHT

   # De draw() functie van de game
   def draw():
      screen.clear()
      alien.draw()

   # De update() functie van de game
   def update():
      pass

De code voor de muisklikken is verdwenen, in regels 7 en 8 zorgen we ervoor dat de alien midden onderin het venster staat en de :python:`update()` functie doet niets. Het keyword :python:`pass` in regel 17 is nodig omdat Python geen lege functies toestaat.  

.. figure:: images/alien_midbottom.png

We gaan nu de sprite van de alien Actor veranderen als de spatiebalk wordt ingedrukt. Voeg de onderstaande event handlers toe. Je kunt de code uiteraard kopiëren en plakken, maar het is beter om zelf te typen. Zo leer je de code beter kennen.

.. code-block:: python
   :class: no-copybutton
   :linenos:
   :emphasize-lines: 10-13, 15-18
   :caption: alien.py

   # Vensterafmetingen
   WIDTH = 600
   HEIGHT = 400

   # Roze alien Actor
   alien = Actor('alien_pink')
   alien.x = WIDTH / 2
   alien.bottom = HEIGHT

   # Key down event handler
   def on_key_down(key):
      if key == keys.SPACE:
         alien.image = 'alien_pink_jump'
         
   # Key up event handler
   def on_key_up(key):
      if key == keys.SPACE:
         alien.image = 'alien_pink'

   # De draw() functie van de game
   def draw():
      screen.clear()
      alien.draw()

   # De update() functie van de game
   def update():
      pass

Als je nu in de game op de spatiebalk drukt, zie je dat de alien sprite verandert in een springende alien. Laat je de spatiebalk los, dan verandert de sprite weer in de gewone alien.

In de regels 12 en 17 wordt de voorwaarde :python:`key == keys.SPACE` getest om te checken of de spatiebalk wordt ingedrukt. De variabele :python:`key` bevat de waarde van de toets die is ingedrukt. Op `deze pagina <https://pygame-zero.readthedocs.io/en/stable/hooks.html#keys>`_ vind je een lijst met alle toetsen die je kunt gebruiken in Pygame Zero.

Wil je de alien echt laten springen wanneer de speler de spatiebalk indrukt, voeg dan de volgende code toe:

.. code-block:: python
   :class: no-copybutton
   :linenos:
   :emphasize-lines: 5-6, 12, 18, 32-37
   :caption: alien.py

   # Vensterafmetingen
   WIDTH = 600
   HEIGHT = 400

   # Zwaartekracht
   GRAVITY = 0.5

   # Roze alien Actor
   alien = Actor('alien_pink')
   alien.x = WIDTH / 2
   alien.bottom = HEIGHT
   alien.speed = 0

   # Key down event handler
   def on_key_down(key):
      if key == keys.SPACE:
         alien.image = 'alien_pink_jump'
         alien.speed = -10
         
   # Key up event handler
   def on_key_up(key):
      if key == keys.SPACE:
         alien.image = 'alien_pink'

   # De draw() functie van de game
   def draw():
      screen.clear()
      alien.draw()

   # De update() functie van de game
   def update():
      alien.y += alien.speed
      if alien.bottom > HEIGHT:
         alien.bottom = HEIGHT
         alien.speed = 0
      else:
         alien.speed += GRAVITY

In regel 6 hebben we een constante :python:`GRAVITY` toegevoegd die de zwaartekracht simuleert. In regel 12 geven we de alien een startsnelheid van 0 (want hij staat nog stil). In de :python:`on_key_down()` event handler geven we de alien een snelheid van -10, waardoor hij omhoog zal bewegen. In de :python:`update()` functie veranderen we de positie van de alien met de snelheid. Wanneer de alien voorbij de onderkant van het venster dreigt te gaan, positioneren we hem weer precies op de onderkant van het venster en zetten we de snelheid op 0. Als de alien nog niet de onderkant van het venster heeft bereikt, verhogen we de snelheid met de zwaartekracht.

Bewegen met de pijltjestoetsen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^	

Om de alien met de pijltjestoetsen te laten bewegen, moeten we een andere techniek gebruiken. Gebruiken van de :python:`on_key_down()` event handler is niet geschikt, zoals je zult merken als je de onderstaande code uitvoert:

.. code-block:: python
   :linenos:
   :emphasize-lines: 11-16
   :caption: alien.py

   # Vensterafmetingen
   WIDTH = 600
   HEIGHT = 400

   # Roze alien Actor
   alien = Actor('alien_pink')
   alien.x = WIDTH / 2
   alien.y = HEIGHT / 2
   alien.speed = 4

   # Key down event handler
   def on_key_down(key):
      if key == keys.LEFT:
         alien.x -= alien.speed
      elif key == keys.RIGHT:
         alien.x += alien.speed
         
   # De draw() functie van de game
   def draw():
      screen.clear()
      alien.draw()

   # De update() functie van de game
   def update():
      pass

Begrijp je het probleem met deze code? De :python:`on_key_down()` event handler wordt slechts één keer aangeroepen wanneer je een toets indrukt. We zouden graag willen dat de alien blijft bewegen wanneer je de toets ingedrukt *houdt*, maar dat lukt niet op deze manier. Door extra variabelen toe te voegen en ook de :python:`on_key_up()` event handler te gebruiken, kunnen we dit probleem weliswaar oplossen, maar het is veel eenvoudiger om de :python:`update()` functie te gebruiken:

.. code-block:: python
   :linenos:
   :emphasize-lines: 18-21
   :caption: alien.py

   # Vensterafmetingen
   WIDTH = 600
   HEIGHT = 400

   # Roze alien Actor
   alien = Actor('alien_pink')
   alien.x = WIDTH / 2
   alien.y = HEIGHT / 2
   alien.speed = 4
   
   # De draw() functie van de game
   def draw():
      screen.clear()
      alien.draw()

   # De update() functie van de game
   def update():
      if keyboard[keys.LEFT]:
         alien.x -= alien.speed
      elif keyboard[keys.RIGHT]:
         alien.x += alien.speed

De :python:`update()` functie wordt 60 keer per seconde uitgevoerd. Met deze code wordt dus 60 keer per seconde gecontroleerd of de linker- of rechterpijltoets is ingedrukt. Als dat het geval is, wordt de positie van de alien aangepast.

In de `documentatie van Pygame Zero <https://pygame-zero.readthedocs.io/en/stable/builtins.html#the-keyboard>`_ kun je lezen dat je in plaats van :python:`if keyboard[keys.LEFT]:` ook :python:`if keyboard.left:` kunt gebruiken. Beide zijn goed, maar :python:`keyboard[keys.LEFT]` sluit iets beter aan bij de manier waarop we de spatiebalk hebben afgehandeld. Daar gebruikten we immers :python:`keys.SPACE`.

.. dropdown:: Opdracht 03
   :color: secondary
   :icon: pencil

   Gebruik de onderstaande code als uitgangspunt; kopieer deze naar je eigen :file:`alien.py` bestand. Breid de code uit zodat de alien ook naar boven en beneden kan bewegen met de pijltjestoetsen. Mocht je de keycodes van de pijltjestoetsen niet kunnen gokken, kijk dan `hier <https://pygame-zero.readthedocs.io/en/stable/hooks.html#keys.UP>`_.

   .. code-block:: python
      :linenos:
      :caption: alien.py

      # Vensterafmetingen
      WIDTH = 600
      HEIGHT = 400

      # Roze alien Actor
      alien = Actor('alien_pink')
      alien.x = WIDTH / 2
      alien.y = HEIGHT / 2
      alien.speed = 4

      # De draw() functie van de game
      def draw():
         screen.clear()
         alien.draw()

      # De update() functie van de game
      def update():
         if keyboard[keys.LEFT]:
            alien.x -= alien.speed
         elif keyboard[keys.RIGHT]:
            alien.x += alien.speed

.. dropdown:: Opdracht 04
   :color: secondary
   :icon: pencil

   Onderzoek het verschil tussen de volgende twee versies van de :python:`update()` functie:

   .. grid:: 2

      .. grid-item:: 
         :columns: 6

         .. code-block:: python

            def update():
               if keyboard[keys.LEFT]:
                  alien.x -= alien.speed
               elif keyboard[keys.RIGHT]:
                  alien.x += alien.speed
               elif keyboard[keys.UP]:
                  alien.y -= alien.speed
               elif keyboard[keys.DOWN]:
                  alien.y += alien.speed 
               
      .. grid-item:: 
         :columns: 6

         .. code-block:: python

            def update():
               if keyboard[keys.LEFT]:
                  alien.x -= alien.speed
               elif keyboard[keys.RIGHT]:
                  alien.x += alien.speed
               if keyboard[keys.UP]:
                  alien.y -= alien.speed
               elif keyboard[keys.DOWN]:
                  alien.y += alien.speed

   .. dropdown:: Hint
      :color: secondary
      :icon: light-bulb

      Om het verschil tussen de twee versies te zien, moet je in de game twee toetsen tegelijk ingedrukt houden.
                   
.. dropdown:: Opdracht 05
   :color: secondary
   :icon: pencil

   Gebruik de onderstaande code als uitgangspunt; kopieer deze naar je eigen :file:`alien.py` bestand. Pas de code zodanig aan dat de alien niet buiten het venster kan bewegen.

   .. code-block:: python
      :linenos:
      :caption: alien.py

      # Vensterafmetingen
      WIDTH = 600
      HEIGHT = 400

      # Roze alien Actor
      alien = Actor('alien_pink')
      alien.x = WIDTH / 2
      alien.bottom = HEIGHT
      alien.speed = 4

      # De draw() functie van de game
      def draw():
         screen.clear()
         alien.draw()

      # De update() functie van de game
      def update():
         if keyboard[keys.LEFT]:
            alien.x -= alien.speed
         elif keyboard[keys.RIGHT]:
            alien.x += alien.speed                   