.. role:: python(code)
   :language: python

.. |br| raw:: html

   <br/>

Player character
===================

...

Sprites
-----------------

Voor de player Actor gaan we verschillende sprites gebruiken. We willen het karakter namelijk animeren door verschillende sprites achter elkaar te tonen.

.. grid:: 2

   .. image:: images/player_idle.gif
      :alt: Idle sprites
      :align: center
      :width: 32px

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

Voor het animeren van de player gaan we gebruik maken van de ``pgzhelper`` module, gemaakt door `A Posteriori <https://www.aposteriori.com.sg/pygame-zero-helper/>`_. Deze module biedt handige functies voor het werken met sprites en animaties in Pygame Zero.

Download het bestand ``pgzhelper.py`` en plaats het in de hoofdmap van je project (dus in dezelfde map als je ``level_devil.py`` bestand).

.. card:: Pygame Zero Helper
    :width: 25%
    :link: https://www.aposteriori.com.sg/pygame-zero-helper/

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