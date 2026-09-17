While loops
=======================

De volgende code tekent een vierkant met Python turtle:

.. code-block:: python
    :linenos:
    :caption: turtle_square.py

    import turtle

    tony = turtle.Turtle()

    tony.fd(100)
    tony.lt(90)
    tony.fd(100)
    tony.lt(90)
    tony.fd(100)
    tony.lt(90)
    tony.fd(100)
    tony.lt(90)

De regels 5 t/m 11 bevatten veel herhaling. Het zou beter zijn als we tegen Python konden zeggen: 'voer de regels 5 en 6 vier keer achter elkaar uit.' Dat scheelt typwerk en het maakt de code overzichtelijker.

.. dropdown:: Wat leer je in dit hoofdstuk
    :open:
    :color: primary
    :icon: book

    * Wat zijn vergelijkingsoperatoren en hoe werken ze.
    * Hoe laat je code meermaals uitvoeren met een while loop (Nederlands: while lus).
    * Hoe kun je in Mu editor code stap voor stap laten uitvoeren.
    * Hoe geef je in Python met indentation (Nederlands: inspringing) aan tot welk blok een coderegel behoort.
    * Waarom gebruiken we voor inspringen de :kbd:`Tab` toets. 

Vergelijkingsoperatoren
------------------------
Voordat we de while loop introduceren, moet je weten wat een *vergelijkingsoperator* is. Het begrip *operator* ben je eerder tegengekomen in het hoofdstuk :doc:`/ch01_arithmetic/ch01_01_arithmetic` bij de behandeling van de *rekenkundige operatoren*: de symbolen die je gebruikt voor berekeningen zoals :python:`+`, :python:`*` en :python:`//`. Vergelijkingsoperatoren zijn symbolen die je gebruikt om twee waarden met elkaar te vergelijken. We hebben er zes:

.. card:: Vergelijkingsoperatoren
    
    .. list-table::
        :header-rows: 1
        :align: center

        * - Operator
          - Naam
          - Voorbeeld
          - Uitkomst
        * - :python:`==`
          - Gelijk aan
          - :python:`1 == 2`
          - :python:`False`
        * - :python:`!=`
          - Niet gelijk aan
          - :python:`1 != 2`
          - :python:`True`
        * - :python:`>`
          - Groter dan
          - :python:`3 > 3`
          - :python:`False`
        * - :python:`<`
          - Kleiner dan
          - :python:`4 < 5`
          - :python:`True`
        * - :python:`>=`
          - Groter of gelijk aan
          - :python:`3 >= 3`
          - :python:`True`
        * - :python:`<=`
          - Kleiner of gelijk aan
          - :python:`4 <= 5`
          - :python:`True`

Je kunt de werking van de vergelijkingsoperatoren testen in de CLI:

.. figure:: images/comparison_operators.png

Let op het verschil tussen de assignment operator :python:`=` en de vergelijkingsoperator :python:`==`. Deze twee moet je niet met elkaar verwarren.  

Code herhalen
--------------

Maak in Mu editor een nieuw bestand, kopieer en plak de onderstaande code erin en sla het op als :file:`turtle_while.py`.

.. code-block:: python
    :linenos:
    :caption: turtle_while.py
    :name: turtle_while_v01

    import turtle

    tony = turtle.Turtle()

    zijde = 0
    while zijde < 4:
        tony.fd(100)
        tony.lt(90)
        zijde = zijde + 1

Run de code om het resultaat te bekijken. Snap je hoe deze code werkt? Het Python keyword :python:`while` in regel 6 kun je vertalen als *terwijl* of *zolang*. En de vergelijkingsoperator :python:`<`  symbool betekent *kleiner dan*. Je kunt deze regel dus lezen als '*Zolang de waarde van* :python:`zijde` *kleiner is dan 4:*'. Let ook op de dubbele punt :python:`:` aan het einde van deze regel. |br|
Schematisch weergegeven doet de code het volgende:

.. uml::
    :align: center 

    @startuml
    <style>
        activityDiagram {
            FontName Monospaced
            FontSize 15
        }
        document {
            BackgroundColor transparent
        }
        </style>
    start
    :zijde = 0;
    while (zijde < 4) is (True)
        :tony.fd(100);
        :tony.lt(90);
        :zijde = zijde + 1;
    endwhile (False)
    stop
    @enduml

Eerst krijgt de variabele :python:`zijde` de waarde :python:`0`. Vervolgens checkt Python of :python:`zijde` kleiner is dan :python:`4`. Als dat zo is, wordt :python:`tony` vooruit gestuurd en gedraaid. Daarna wordt de waarde van :python:`zijde` met :python:`1` opgehoogd. We springen terug naar regel 6 en Python checkt weer of :python:`zijde` kleiner is dan :python:`4`.

We gebruiken de variabele :python:`zijde` in dit voorbeeld als een *teller* variabele. De waarde ervan geeft aan hoeveel zijden er tot op dat moment zijn getekend. Doordat we de waarde van :python:`zijde` na elke tekenactie ophogen, zal de check in regel 6 op een zeker moment :python:`False` zijn. De uitvoering van de loop stopt, en in dit geval is het programma dan ook afgelopen.  


Debugging mode
----------------

Mu editor biedt de mogelijkheid om de uitvoering van een programma stap voor stap te volgen. Je kunt op die manier goed zien hoe de while loop werkt.

Klik op regel 6 in :file:`turtle_while.py` met de muiscursor in het grijze stukje rechts naast het regelnummer 6 om een zogenoemd **breakpoint** te plaatsen.

.. figure:: images/debug_breakpoint.png
    :align: left
    :figwidth: 100%

Klik daarna op :guilabel:`Debug` bovenin de knoppenbalk.

.. figure:: images/debug_debug_button.png
    :align: left
    :figwidth: 100%

Nu start Mu editor de uitvoering van de code, maar pauzeert op regel 6, waar je zojuist het breakpoint plaatste. Gebruik de knop :guilabel:`Step over` om de code vanaf het breakpoint telkens een stapje verder uit te voeren. Houd daarbij in de gaten wat er in de turtle tekening gebeurt, maar ook wat de waarde van de variabele :python:`zijde` is. Die waarde wordt in Mu editor aan de rechterkant getoond. 

.. figure:: images/debug_step_over.png
    :align: left
    :figwidth: 100%

Begrijp je nu hoe de while loop werkt? Klik op :guilabel:`Stop` om het debuggen te stoppen. 

In de volgende opdrachten ga je je eigen while loops schrijven. Je zult merken dat je met loops mooie patronen kunt tekenen.

Indentation
------------

Kijk nog eens goed naar de regels 7, 8 en 9 van :file:`turtle_while.py`. Deze regels zijn ingesprongen. Dat is belangrijk! Daardoor weet Python dat die drie regels binnen de 'while lus' vallen. In Python bepaalt de inspringing (Engels: indentation) van een coderegel tot welk blok die regel behoort. Kopieer de onderstaande code naar Mu editor (in een bestand :file:`hello_while.py`) om te zien hoe dat werkt:

.. code-block:: python
    :linenos:
    :caption: hello_while.py
    :name: hello_while

    i = 0
    while i < 3:
        print('Deze zin wordt drie keer geprint.')
        print('En deze zin valt ook binnen de while lus.')
        i = i + 1
    print('Maar deze zin wordt slechts één keer geprint.')

De regels 3, 4 en 5 van :file:`hello_while.py` zijn ingesprongen: ze worden voorafgegaan door 4 spaties. Daardoor weet Python dat die regels één blokje vormen binnen de while lus.
Regel 6 is niet ingesprongen en hoort daardoor niet bij het blokje dat wordt herhaald.

Bestudeer nu de volgende code eens, nadat je hem in Mu editor hebt uitgevoerd. Je ziet hier een while lus bínnen een andere while lus.

.. code-block:: python
    :linenos:
    :caption: turtle_while.py
    :name: turtle_while_v02

    import turtle

    tony = turtle.Turtle()

    vierkant = 0
    while vierkant < 3:
        zijde = 0
        while zijde < 4:
            tony.fd(100)        # Deze regels
            tony.lt(90)         # vallen binnen
            zijde = zijde + 1   # de 2e while lus
        tony.pu()
        tony.lt(120)
        tony.fd(100)
        tony.pd()
        vierkant = vierkant + 1

De regels 7 t/m 16 in deze code vallen binnen de while lus die begint op regel 6. Maar in dat blok begint op regel 8 een tweede while lus, die de regels 9 t/m 11 herhaalt. Let op de inspringing van regel 12: die valt niet meer onder de tweede while lus.

Wanneer je in Mu editor op :kbd:`Enter` drukt nadat je regel 6 hebt getypt, springt de volgende regel automatisch in. Wil je handmatig een regel laten inspringen, dan kun je daarvoor de :kbd:`Tab` toets gebruiken (links naast de :kbd:`Q`).

.. dropdown:: Spaties of tabs?
    :open:
    :color: warning
    :icon: alert

    Gebruik voor het inspringen van coderegels nooit de spatiebalk! Ten eerste is het heel onhandig om telkens spaties te moeten tellen, en ten tweede krijg je sneller indentation errors.

    .. figure:: images/keyboard_tab_vs_space.png

    Met de :kbd:`Tab` toets kun je in één keer een grotere inspringing maken. Ook is het mogelijk in Mu editor een aantal regels code te selecteren en vervolgens op :kbd:`Tab` te drukken om die regels tegelijkertijd te laten inspringen; probeer het maar eens. Om ze weer te laten terugspringen gebruik je :kbd:`Shift` + :kbd:`Tab`.

Verschillende voorwaarden
--------------------------

In de voorgaande voorbeelden gebruikten we in de voorwaarde van de while loop telkens een teller variabele, oftewel een integer. Maar je kunt in de voorwaarde van een while loop ook andere datatypes gebruiken, zoals een string:

.. code-block:: python
    :linenos:
    :caption: annoying_while.py

    name = ''
    while name != 'je naam':
        name = input('Typ je naam: ')

    print('Dankjewel!')

Probeer dit programma maar eens uit. Het is afkomstig uit het boek `Automate the Boring Stuff <https://automatetheboringstuff.com/2e/chapter2/>`_ van Al Sweigart. Daarin heet dit programma *An Annoying while Loop*. Kun je bedenken waarom?

Hieronder zi je het stroomdiagram dat hoort bij de code. Er wordt nu geen integer variabele gebruikt voor de while loop, maar een string :python:`naam`. Zolang de waarde van :python:`naam` ongelijk is aan :python:`'je naam'` blijft de loop zich herhalen.   

.. uml::
    :align: center 

    @startuml
    <style>
        activityDiagram {
            FontName Monospaced
            FontSize 15
        }
        document {
            BackgroundColor transparent
        }
        </style>
    start
    :naam = '';
    while (naam != 'je naam') is (True)
        :name = input('Typ je naam: ');
    endwhile (False)
    :print('Dankjewel!');
    stop
    @enduml

Je kunt in de voorwaarde van de while loop variabelen ook helemaal achterwege laten, zoals in het volgende voorbeeld:

.. code-block:: python
    :linenos:
    :caption: endless_while.py

    while True:
        print('Dit stopt niet meer!')

Met :python:`while True:` creëer je een oneindige loop. Er zijn wel mogelijkheden om daar weer uit te ontsnappen, maar die bewaren we voor een ander moment. Uiteraard wil je liever niet dat je programma in een loop blijft hangen waar de gebruiker nooit meer uitkomt. Let dus altijd goed op wanneer je een while loop programmeert.

Opdrachten
-----------

.. dropdown:: Opdracht 01
    :open:
    :color: secondary
    :icon: pencil

    Maak een nieuw bestand in Mu editor en sla het op als :file:`driehoeken.py`. Kopieer de onderstaande code van :file:`turtle_while.py` naar je nieuwe bestand. 
    
    .. code-block:: python
        :linenos:
        :caption: turtle_while.py

        # While loops - opdracht 01

        import turtle

        tony = turtle.Turtle()

        zijde = 0
        while zijde < 4:
            tony.fd(100)
            tony.lt(90)
            zijde = zijde + 1
    
    Pas de code zodanig aan dat in plaats van een vierkant een driehoek met zijden van 100 pixels wordt getekend, uiteraard met gebruikmaking van een while loop. Na het tekenen van een zijde moet de turtle telkens 120 graden draaien.

    .. dropdown:: Oplossing
        :color: secondary
        :icon: check-circle

        .. code-block:: python
            :linenos:
            :caption: driehoeken.py
            :name: turtle_while_opdr01

            # While loops - opdracht 01
            
            import turtle

            tony = turtle.Turtle()

            zijde = 0
            while zijde < 3:
                tony.fd(100)
                tony.lt(120)
                zijde = zijde + 1

.. dropdown:: Opdracht 02
    :open:
    :color: secondary
    :icon: pencil

    Breid de code in :file:`driehoeken.py` van opdracht 01 uit, opdat met behulp van een **while loop binnen een andere while loop** vier driehoeken op een rij worden getekend zoals hieronder afgebeeld.

    .. image:: images/triangles_in_a_row.png
        
    .. dropdown:: Hint
        :color: secondary
        :icon: light-bulb

        Gebruik voor je programma de volgende structuur:

        .. code-block:: python
            :name: turtle_while_opdr02_hint

            # While loops - opdracht 02

            import turtle

            tony = turtle.Turtle()

            driehoek = 0
            while driehoek < 4:
                zijde = 0
                while zijde < 3:
                    ...
                    ...
                    ...
                ...
                ...

    .. dropdown:: Oplossing
        :color: secondary
        :icon: check-circle

        .. code-block:: python
            :linenos:
            :caption: driehoeken.py
            :name: turtle_while_opdr02

            # While loops - opdracht 02

            import turtle

            tony = turtle.Turtle()

            driehoek = 0
            while driehoek < 4:
                zijde = 0
                while zijde < 3:
                    tony.fd(100)
                    tony.lt(120)
                    zijde = zijde + 1
                tony.fd(100)
                driehoek = driehoek + 1

.. dropdown:: Opdracht 03
    :open:
    :color: secondary
    :icon: pencil

    Kopieer de code van :file:`turtle_while.py` naar een nieuw bestand dat je opslaat als :file:`bloem.py`. Breid de code zodanig uit dat met behulp van een **while loop binnen een andere while loop** 20 vierkanten worden getekend, waarbij elk vierkant 18 graden gedraaid is ten opzicht van het vorige. Dit moet de volgende figuur opleveren:
    
    .. image:: images/star_of_squares.png

    .. dropdown:: Hint
        :color: secondary
        :icon: light-bulb

        Je programma bestaat uit twee while loops, waarvan de binnenste het tekenen van één vierkant verzorgt. Na het tekenen van een vierkant moet de turtle 18 graden linksom draaien.

        .. code-block:: python
            :name: turtle_while_opdr03_hint

            ...
            while ...:
                # Deze while loop zorgt voor 20 herhalingen.
                ...
                while ...:
                    # Deze while loop zorgt voor één vierkant.
                    ...
                    ...
                tony.lt(18)  # Draai tony 18 graden linksom
                ...

.. dropdown:: Opdracht 04
    :open:
    :color: secondary
    :icon: pencil

    Maak een nieuw bestand in Mu editor, kopieer onderstaande de code erin en sla het op onder de naam :file:`turtle_dots.py`. 

    .. code-block:: python
        :linenos:
        :emphasize-lines: 8, 10, 12-18
        :caption: turtle_dots.py
        :name: turtle_dots

        # While loops - opdracht 04

        import turtle

        tony = turtle.Turtle()
        tony.hideturtle()
        tony.speed(0)

        rij = 0
        while ...:
            kolom = 0
            while ...:
                tony.dot(20, 'red')
                ...
                ...
                ...
                ...
            ...
            ...
            ...
    
    Vervang de puntjes in de gemarkeerde regels door code die ervoor zorgt dat een rooster van 10 bij 10 rode puntjes wordt getekend.

    .. image:: images/red_dots.png

.. dropdown:: Opdracht 05
    :open:
    :color: secondary
    :icon: pencil

    Schrijf een programma dat de onderstaande figuur tekent. Je mag zelf de kleur en de pendikte bepalen, maar voor het daadwerkelijke tekenen mag je **maximaal 5 regels code** gebruiken.

    .. image:: images/five_pointed_star.png
        :align: center

    .. dropdown:: Hint
        :color: secondary
        :icon: light-bulb

        Vind je het lastig om de draaiingshoek te bepalen? Bedenk dan dat de turtle in totaal 2 keer volledig ronddraait (dus de totale draaiingshoek is 2 * 360° = 720°) en dat die volledige draai over 5 stappen wordt verdeeld.

.. dropdown:: Opdracht 06
    :open:
    :color: secondary
    :icon: pencil

    We willen de turtle een regelmatige veelhoek laten tekenen. Dat is een veelhoek waarvan alle zijden even lang zijn en de hoeken even groot.
    
    .. figure:: images/regular_polygons.png

    Om het programma flexibel te maken, gaan we een variabele :python:`aantal_hoeken` gebruiken, die het aantal hoeken van de veelhoek aangeeft. Als bijvoorbeeld :python:`aantal_hoeken = 3` dan moet een driehoek worden getekend en als :python:`aantal_hoeken = 8` een achthoek.
    
    Kopieer onderstaande code in een nieuw bestand, dat je opslaat als :file:`turtle_veelhoeken.py`.

    .. code-block:: python
        :linenos:
        :caption: turtle_veelhoeken.py

        # While loops - opdracht 06

        import turtle

        tony = turtle.Turtle()

        aantal_hoeken = 5
        draaiingshoek = ...

        hoek = ...
        while ...:
            tony.fd(100)
            tony.lt(draaiingshoek)
            hoek = hoek + 1  

    Op regels 8, 10 en 11 ontbreekt code. Vul zelf in wat op de puntjes moet staan, opdat het programma een regelmatige vijfhoek tekent. De waarde van :python:`draaiingshoek` is afhankelijk van :python:`aantal_hoeken`, dus op de puntjes in regel 8 moet een berekening met :python:`aantal_hoeken` komen.   

    Test je programma door in regel 7 voor :python:`aantal_hoeken` verschillende waarden te kiezen. Als je bijvoorbeeld :python:`aantal_hoeken = 7` kiest, zou een regelmatige zevenhoek moeten worden getekend.

    Gelukt? Breid dan je programma uit met een :python:`input()` aanroep waarmee aan de gebruiker wordt gevraagd hoeveel hoeken de veelhoek moet hebben en sla het antwoord van de gebruiker op in :python:`aantal_hoeken`.

    .. figure:: images/polygon_01.png

    .. figure:: images/polygon_02.png