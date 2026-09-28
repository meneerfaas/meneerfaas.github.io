Pygame Zero
===========

Pygame mode
----------------
Voor het maken van een game in Mu editor, is het nodig dat je Mu in de Pygame Zero stand zet. Dat gaat als volgt:

1. Klik op de :guilabel:`Mode` knop.
   
   .. image:: images/mu_mode_button.png

2. Selecteer Pygame Zero en klik op Ok.

   .. image:: images/mu_mode.png

Als je dit goed hebt gedaan, staat nu rechtsonder in de statusbalk van Mu editor *Pygame Zero*.

.. figure:: images/mu_pygame_zero_mode.png

Je ziet tevens dat de knoppenbalk een beetje is veranderd. Bijvoorbeeld in plaats van :guilabel:`Run` vind je nu een :guilabel:`Play` knop.

.. _mappen-structuur-maken:

Mappenstructuur
---------------

Bij het programmeren van een game gebruik je vaak veel bestanden: afbeeldingen, geluiden, lettertypes etcetera. Om te voorkomen dat je na enige tijd door de bomen het bos niet meer ziet, is het belangrijk dat je werkt met een goede mappenstructuur. Maak in je :file:`Documenten/Python Projecten` map een submap :file:`Games`.

.. uml::
    :align: center
    :html_format: svg

    @startuml
        @startfiles
        /Documenten/Python Projecten/Experimenten/
        /Documenten/Python Projecten/Games/
        /Documenten/Python Projecten/Oefeningen/
        @endfiles
    @enduml

.. dropdown:: Elke game een eigen map
    :open:
    :color: warning
    :icon: alert

    In de volgende lessen ga je verschillende games maken. Voor elke game schrijf je natuurlijk een Python codebestand maar daarnaast gebruik je ook afbeeldingen, geluiden en andere bestanden. Het is belangrijk dat je de bestanden die bij één game horen netjes bij elkaar bewaart. Maak daarom altijd voor elke game een eigen map.

    Je kunt die map maken in de Verkenner, maar hieronder zul je zien dat het ook via het *Safe file* venster van Mu editor kan.

Codebestand
-----------------

Maak in Mu editor een nieuwe bestand aan met de :guilabel:`New` knop. Voordat je er code in gaat typen, sla je het bestand op in een nieuwe map. Doe dat op de volgende manier:

1. Klik op :guilabel:`Save`. Navigeer naar je nieuwe :file:`Games` map en klik daarna op de knop :guilabel:`Nieuwe map` (Engels: :guilabel:`New folder`).

   .. image:: images/save_file_01.png

2. Noem de nieuwe map :file:`alien`.

   .. image:: images/save_file_02.png

3. Dubbelklik op de map :file:`alien` om hem te openen. Sla je bestand vervolgens op onder de naam :file:`alien.py`.

   .. image:: images/save_file_03.png

Als je alle stappen goed hebt uitgevoerd, bevindt je bestand :file:`alien.py` zich in de map :file:`Documenten/Python Projecten/Games/alien`.

.. uml::
    :align: center
    :html_format: svg

    @startuml
        @startfiles
        /Documenten/Python Projecten/Games/alien/alien.py
        @endfiles
    @enduml

Vensterafmetingen
------------------

En nu is het tijd voor de eerste code. Typ het volgende in je bestand. Niet kopiëren en plakken, want het is belangrijk dat je deze code (letterlijk) in de vingers krijgt voor later. Let op het verschil tussen hoofdletters en kleine letters.

.. code-block:: python
   :class: no-copybutton
   :linenos:
   :caption: alien.py
   :name: alien_v01

   # Vensterafmetingen
   WIDTH = 600
   HEIGHT = 400

Klik op :guilabel:`Play` om deze code te runnen. Er verschijnt een venster:

.. image:: images/game_window.png

Waarschijnlijk begrijp je al wat de woorden :python:`WIDTH` en :python:`HEIGHT` betekenen. Zo niet, verander dan iets aan de getallen en run de code opnieuw om het effect ervan te zien.

Misschien lijkt het alsof je veel werk hebt moeten verzetten om alleen maar een zwart venster op het scherm te toveren, maar van de mappenstructuur die je in dit deel hebt gemaakt, ga je nog veel plezier hebben. En in het volgende deel wordt de invulling van het venster uiteraard een stuk interessanter.