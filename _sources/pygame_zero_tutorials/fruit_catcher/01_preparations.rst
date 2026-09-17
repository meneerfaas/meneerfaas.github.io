Voorbereiding
=============

Mappen aanmaken
---------------
Maak voor dit project in je :file:`games` map een nieuwe map aan met de naam :file:`fruitcatcher`. Maak in die nieuwe map alvast de mappen :file:`images` en :file:`fonts` aan. Hieronder zie je de gewenste structuur.

.. uml::
    :align: center
    :html_format: svg

    @startuml
        @startfiles
        /Documenten/Python Projecten/Games/fruitcatcher/images/
        /Documenten/Python Projecten/Games/fruitcatcher/fonts/
        @endfiles
    @enduml

Assets downloaden
-----------------
Game developers noemen de bestanden die ze in hun game gebruiken, zoals afbeeldingen, geluiden en lettertypes, vaak *assets*. Download de volgende vier afbeeldingen en zorg dat ze terechtkomen in de :file:`fruitcatcher\\images` map die je zojuist maakte.

.. grid:: 2
   :gutter: 3

   .. grid-item-card::

      .. image:: assets/images/basket.png
         :height: 44

      :download:`basket.png <assets/images/basket.png>`

   .. grid-item-card:: 
      
      .. image:: assets/images/apple.png
         :height: 44

      :download:`apple.png <assets/images/apple.png>`

   .. grid-item-card:: 
      
      .. image:: assets/images/heart.png
         :height: 44

      :download:`heart.png <assets/images/heart.png>`

   .. grid-item-card:: 
      
      .. image:: assets/images/game_over.png
         :height: 44

      :download:`game_over.png <assets/images/game_over.png>`

Download vervolgens het onderstaande lettertype en plaats het in de map :file:`fruitcatcher\\fonts`.

.. grid:: 2
   :gutter: 3

   .. grid-item-card::

      .. image:: images/boogaloo_font.png
         :height: 44

      :download:`boogaloo.ttf <assets/fonts/boogaloo.ttf>`

Je beschikt nu over alle assets voor het basisspel.

.. dropdown:: Tip
    :color: info
    :icon: light-bulb
    :open:

    Wil je een game maken en ben je op zoek naar leuke sprites? Typ in je zoekmachine dan `free game assets <https://duckduckgo.com/?va=g&t=hh&q=free+game+assets&ia=web>`_.

    .. image:: images/free_game_assets.png

Codebestand maken
-----------------

Maak in Mu editor een nieuw bestand en sla het op in je :file:`fruitcatcher` map onder de naam :file:`fruitcatcher.py`.

Check nog eens of je alle bestanden op de juiste plek hebt staan. De structuur zou de volgende moeten zijn:

.. uml::
    :align: center
    :html_format: svg

    @startuml
        @startfiles
        /Documenten/Python Projecten/Games/fruitcatcher/fonts/boogaloo.ttf
        /Documenten/Python Projecten/Games/fruitcatcher/images/apple.png
        /Documenten/Python Projecten/Games/fruitcatcher/images/basket.png
        /Documenten/Python Projecten/Games/fruitcatcher/images/game_over.png
        /Documenten/Python Projecten/Games/fruitcatcher/images/heart.png
        /Documenten/Python Projecten/Games/fruitcatcher/fruitcatcher.py
        @endfiles
    @enduml

Klopt het allemaal? Dan kan het programmeren beginnen!
