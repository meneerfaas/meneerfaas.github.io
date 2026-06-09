.. role:: python(code)
   :language: python

.. |br| raw:: html

   <br/>

Voorbereidingen
=================

Starter code
-----------------

Maak voor deze game een nieuwe map aan met de naam :file:`car driver`. Start in Mu editor een nieuw codebestand en sla het op in de nieuwe map onder de naam :file:`car_driver.py`. 

.. uml::
    :align: center
    :html_format: svg

    @startuml
        @startfiles
        /games/car driver/car_driver.py
        @endfiles
    @enduml

Plaats de onderstaande code in het bestand.

.. code-block:: python
    :linenos:
    :caption: car_driver.py

    # WINDOW SETTINGS
    WIDTH = 600
    HEIGHT = 400
    TITLE = 'Car Driver'

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

Deze code is min of meer de standaardwijze waarop je een Pygame Zero game begint. Je definieert de instellingen van het venster en zorgt voor een :python:`draw()` functie en een :python:`update()` functie. In dit geval vult de :python:`draw()` functie het venster met een donkere groene kleur en doet de :python:`update()` functie nog niets. Het keyword :python:`pass` is een soort opvulling die je nodig hebt omdat Python geen lege functies toestaat.

De grote commentaarblokken in regels 6-8 en 12-14 zijn bedoeld om de code overzichtelijk te houden. Ze geven duidelijk aan waar de :python:`draw()` functie begint en waar de :python:`update()` functie begint. Dat is vooral handig als je later meer code aan deze functies toevoegt.

.. dropdown:: Kopiëren of zelf typen?
    :color: warning
    :icon: alert
    :open:

    Uiteraard kun je de code in deze tutorials kopiëren en plakken in Mu editor, maar het is een beter idee om de code zelf te typen. Op die manier leer je de code goed kennen en begrijp je het beter.

    Als je echt wilt leren programmeren, dan typ je zelf en stel je jezelf tijdens het typen bij elke coderegel de vraag: 'Begrijp ik wat deze code doet?' Dat is natuurlijk meer werk, maar het is de moeite waard. 

Assets
--------------------
Download de onderstaande sprites en plaats ze in de map :file:`car driver/images`.

.. card-carousel:: 4

    .. card:: Car 0

        .. image:: assets/car_0.png
           :alt: Car 0
           :height: 43px

        :download:`download <assets/car_0.png>`

    .. card:: Car 1

        .. image:: assets/car_1.png
           :alt: Car 1
           :height: 43px

        :download:`download <assets/car_1.png>`

    .. card:: Car 2

        .. image:: assets/car_2.png
           :alt: Car 2
           :height: 43px

        :download:`download <assets/car_2.png>`

    .. card:: Car 3

        .. image:: assets/car_3.png
           :alt: Car 3
           :height: 43px

        :download:`download <assets/car_3.png>`

    .. card:: Car 4

        .. image:: assets/car_4.png
           :alt: Car 4
           :height: 43px

        :download:`download <assets/car_4.png>`

    .. card:: Green light

        .. image:: assets/emergency_light_green.png
           :alt: Green light
           :height: 43px

        :download:`download <assets/emergency_light_green.png>`

    .. card:: Red light

        .. image:: assets/emergency_light_red.png
           :alt: Red light
           :height: 43px

        :download:`download <assets/emergency_light_red.png>`

In eerste instantie zullen we alleen de :file:`car_0` sprite gebruiken, maar voor de animatie van het rijdende autootje hebben we later alle vijf car sprites nodig. De emergency light sprites spelen pas een rol wanneer we de collision detection gaan implementeren.

Voor die animatie gaan we de ``pgzhelper`` module gebruiken, gemaakt door `A Posteriori <https://www.aposteriori.com.sg/pygame-zero-helper/>`_. Deze module biedt handige functies voor het werken met sprites en animaties in Pygame Zero. Download het bestand :file:`pgzhelper.py` en plaats het in de hoofdmap van je project (dus in dezelfde map als je :file:`level_devil.py` bestand).

.. card:: Pygame Zero Helper
    :width: 25%

    .. image:: images/python_logo.png
        :alt: Pygame Zero Helper
        :width: 100px

    :download:`download <assets/pgzhelper.py>`

Als het goed is, beschik je nu over de volgende mappen en bestanden in je project:

.. uml::
    :align: center
    :html_format: svg

    @startuml
        @startfiles
        /games/car driver/car_driver.py
        /games/car driver/pgzhelper.py
        /games/car driver/images/car_0.png
        /games/car driver/images/car_1.png
        /games/car driver/images/car_2.png
        /games/car driver/images/car_3.png
        /games/car driver/images/car_4.png
        /games/car driver/images/emergency_light_green.png
        /games/car driver/images/emergency_light_red.png
        @endfiles
    @enduml

.. dropdown:: Bronvermelding
    :color: info
    :icon: cross-reference
    :open:

    De car sprites zijn afkomstig uit de Metal Slug sprite database van `Retro Game Zone <https://retrogamezone.co.uk/metalslug/truck.htm>`_. De pgzhelper module is zoals gezegd gemaakt door `A Posteriori <https://www.aposteriori.com.sg/pygame-zero-helper/>`_.