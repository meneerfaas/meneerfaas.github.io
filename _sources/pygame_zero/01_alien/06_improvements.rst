Extra uitbreidingen
====================

Zwaartekracht
-------------

De alien beweegt nu met een constante snelheid naar beneden. In de echte wereld vallen objecten niet op die manier. Om de beweging realistischer te maken, kun je zwaartekracht aan je spel toevoegen.

1. Maak een variabele :python:`GRAVITY` aan. Deze variabele verandert gedurende het spel niet van waarde. Zo'n onveranderlijke variabele noemen we een *constante*. In Python is het de gewoonte om constanten met hoofdletters te schrijven (net zoals :python:`WIDTH` en :python:`HEIGHT` voor de vensterafmetingen).

   .. code-block:: python
      :linenos:
      :lineno-start: 19
      :caption: alien.py

      # Constants
      GRAVITY = 0.05

2. Wijzig regels 8 en 9 als volgt:

   .. code-block:: python
      :linenos:
      :lineno-start: 8
      :caption: alien.py

      alien.speed = 1
      alien.jump_speed = -5

   Hiermee wordt de aanvangssnelheid van de alien op 1 gezet. En in plaats van hem bij een jump 150 pixels omhoog te verplaatsen, geven we hem een negatieve snelheid (dus omhoog) van -5.

3. Voeg aan de :python:`update()` functie een regel toe waarmee de snelheid toeneemt met de zwaartekracht:

   .. code-block:: python
      :linenos:
      :lineno-start: 35
      :emphasize-lines: 5
      :caption: alien.py

      # De update() functie van de game
      def update():
         global game_over
         alien.y += alien.speed
         alien.speed += GRAVITY
         if alien.bottom > HEIGHT:
            alien.bottom = HEIGHT
            game_over = True
            clock.unschedule(increment_score)

4. Wijzig de jump actie in :python:`on_mouse_down()`:

   .. code-block:: python
      :linenos:
      :lineno-start: 45
      :emphasize-lines: 6
      :caption: alien.py

      # Mouse down event handler
      def on_mouse_down(button, pos):
         if game_over:
            return
         if alien.collidepoint(pos):
            alien.speed = alien.jump_speed

Dat is alles. Bekijk het resultaat van deze uitbreiding. Dat ziet er al meteen een stuk realistischer uit toch?



Beweging in twee richtingen
----------------------------

Momenteel beweegt de alien alleen verticaal. Het spel wordt interessanter als hij ook horizontaal beweegt. Om dat voor elkaar te krijgen, moeten we de snelheid opsplitsen in twee componenten: een horizontale snelheid en een verticale snelheid.

1. Vervang :python:`alien.speed = 3` in regel 8 door de volgende twee regels:

   .. code-block:: python
      :linenos:
      :lineno-start: 5
      :emphasize-lines: 4-5
      :caption: alien.py

      # Roze alien Actor
      alien = Actor('alien_pink')
      alien.midbottom = (WIDTH / 2, 0)
      alien.vx = 2
      alien.vy = 1
      alien.jump_speed = -4

   In de natuurkunde wordt voor snelheid vaak de letter *v* gebruikt. Daarom noemen we hier de horizontale snelheid :python:`vx` (de snelheid in x-richting) en de verticale snelheid :python:`vy` (de snelheid in y-richting).

2. Vervang overal in je code :python:`alien.speed` door :python:`alien.vy`. Je kunt dit handmatig doen, of in één keer door in Mu editor op :kbd:`Ctrl` + :kbd:`F` te drukken, waarmee je het Find / Replace venster opent:

   .. figure:: images/find_replace.png

3. Voeg de horizontale beweging toe aan de :python:`update()` functie en zorg er voor dat de alien weer verschijnt wanneer hij uit beeld is verdwenen. 

   .. code-block:: python
      :linenos:
      :lineno-start: 36
      :emphasize-lines: 4, 7-8
      :caption: alien.py

      # De update() functie van de game
      def update():
         global game_over
         alien.x += alien.vx 
         alien.y += alien.vy
         alien.vy += GRAVITY
         if alien.left > WIDTH:
            alien.right = 0
         if alien.bottom > HEIGHT:
            alien.bottom = HEIGHT
            game_over = True
            clock.unschedule(increment_score)

   Wanneer je deze code test, zul je merken dat de alien blijft bewegen na een 'game over'. Dit is snel opgelost:

   .. code-block:: python
      :linenos:
      :lineno-start: 36
      :emphasize-lines: 4-5
      :caption: alien.py

      # De update() functie van de game
      def update():
         global game_over
         if game_over:
            return
         alien.x += alien.vx 
         alien.y += alien.vy
         alien.vy += GRAVITY
         if alien.left > WIDTH:
            alien.right = 0
         if alien.bottom > HEIGHT:
            alien.bottom = HEIGHT
            game_over = True
            clock.unschedule(increment_score)

   Nu springt de alien vrolijk van links naar rechts door het venster.

Meerdere aliens
-----------------

Met één alien is het spel tamelijk eenvoudig. Je kunt de uitdaging vergroten door aliens toe te voegen.

1. Download de volgende sprites naar je :file:`alien/images` map:

   * :download:`alien_green <assets/images/alien_green.png>`
   * :download:`alien_blue <assets/images/alien_blue.png>`

   .. grid:: 2

      .. grid-item:: 
         :columns: auto

         .. image:: assets/images/alien_green.png

      .. grid-item:: 
         :columns: auto

         .. image:: assets/images/alien_blue.png

2. Hernoem de variabele :python:`alien` in je gehele code naar :python:`alien_pink`. Dit kan weer met :kbd:`Ctrl` + :kbd:`F`:

   .. figure:: images/find_replace_02.png

   Maar pas op, met deze 'Replace all' actie vervang je eigenlijk teveel!

   .. figure:: images/find_replace_03.png

   Omdat in de naam van de sprite :python:`alien_pink` ook het woord alien voorkomt, leidt onze vervangingsactie tot :python:`alien_pink_pink`. Gelukkig markeert Mu editor alle vervangingen grijs, zodat je eventuele foute vervangingen kunt terugdraaien. In ons geval betreft het slechts de vervangingen in regels 5 en 6. Pas deze met de hand aan.

   .. code-block:: python
      :linenos:
      :lineno-start: 5
      :caption: alien.py

      # Roze alien Actor
      alien_pink = Actor('alien_pink')

3. Nu kun je een extra :python:`Actor` toevoegen. Kies zelf de beginpositie en de snelheden.

   .. code-block:: python
      :linenos:
      :lineno-start: 14
      :caption: alien.py

      # Groene alien Actor
      alien_green = Actor('alien_green')
      alien_green.midbottom = (WIDTH, -50)
      alien_green.vx = -1
      alien_green.vy = 0.5
      alien_green.jump_speed = -4

4. Maak de nieuwe alien zichtbaar in de :python:`draw()` functie.

   .. code-block:: python
      :linenos:
      :lineno-start: 37
      :emphasize-lines: 5
      :caption: alien.py

      # De draw() functie van de game
      def draw():
         screen.blit('background', (0, 0))
         alien_pink.draw()
         alien_green.draw()
         screen.draw.text(f"Score: {score}", (10, 10), color = "yellow", fontsize = 40)
         if game_over:
            game_over_message.draw()

5. Breid de :python:`update()` functie uit om de alien in beweging te krijgen.

   .. code-block:: python
      :linenos:
      :lineno-start: 46
      :emphasize-lines: 9-11,14-15,16,18
      :caption: alien.py

      # De update() functie van de game
      def update():
         global game_over
         if game_over:
            return
         alien_pink.x += alien_pink.vx 
         alien_pink.y += alien_pink.vy
         alien_pink.vy += GRAVITY
         alien_green.x += alien_green.vx 
         alien_green.y += alien_green.vy
         alien_green.vy += GRAVITY
         if alien_pink.left > WIDTH:
            alien_pink.right = 0
         if alien_green.right < 0:
            alien_green.left = WIDTH
         if alien_pink.bottom > HEIGHT or alien_green.bottom > HEIGHT:
            alien_pink.bottom = HEIGHT
            alien_green.bottom = HEIGHT
            game_over = True
            clock.unschedule(increment_score)

6. Voeg aan de :python:`on_mouse_down()` event handler de verwerking van een muisklik op de nieuwe alien toe.

   .. code-block:: python
      :linenos:
      :lineno-start: 67
      :emphasize-lines: 7-8
      :caption: alien.py

      # Mouse down event handler
      def on_mouse_down(button, pos):
         if game_over:
            return
         if alien_pink.collidepoint(pos):
            alien_pink.vy = alien_pink.jump_speed
         elif alien_green.collidepoint(pos):
            alien_green.vy = alien_green.jump_speed

Tenslotte
----------

Het doel van deze les was je kennis te laten maken met het programmeren van een game in Pygame Zero. Je hebt ongetwijfeld gemerkt dat daar veel bij komt kijken. Toch hebben we slechts een glimp gezien van wat allemaal mogelijk is. Zo kun je bijvoorbeeld Actors animeren om ze levendiger te maken. En wat dacht je van geluidseffecten wanneer je op een alien klikt of wanneer het 'game over' is. De mogelijkheden zijn eindeloos.

Wil je meer weten over programmeren in Pygame Zero, kijk dan op de `officiële website van Pygame Zero <https://pygame-zero.readthedocs.io/>`_. Je vindt daar onze roze alien ook (waarvan je trouwens nog meer versies kunt downloaden op `Kenney <https://kenney.nl/assets>`_), alsmede verschillende tutorials voor het maken van games.