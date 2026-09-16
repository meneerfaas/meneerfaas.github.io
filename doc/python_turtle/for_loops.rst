For loops
=======================

Wanneer je van tevoren al weet hoe vaak je een blok code wilt herhalen, is een while loop enigszins omslachtig. Je hebt er namelijk drie regels code voor nodig:

* één om de teller variabele te maken met een assignment statement;
* één waarin het keyword :python:`while` staat, gevolgd door de voorwaarde(n);
* één waarin de waarde van de teller variabele wordt bijgewerkt.

.. code-block:: python

    n = 0
    while n < 10:
        ...
        ...
        n = n + 1

Je kunt dit gemakkelijker doen met een *for loop*. Dat wil niet zeggen dat je een while loop nooit nodig hebt. Er zijn situaties waarin een for loop niet werkt en een while loop wel.

.. dropdown:: Wat leer je in dit hoofdstuk
    :open:
    :color: primary
    :icon: book

    * Wat doet de functie :python:`range()`.
    * Hoe maak je een for loop met de functie :python:`range()`.  

De functie :python:`range()`
-----------------------------

Met de functie :python:`range()` kun je een serie opeenvolgende getallen genereren. Om die serie zichtbaar te maken in de CLI, hebben we ook de functie :python:`list()` nodig:

.. code-block:: python

    >>> list(range(5))
    [0, 1, 2, 3, 4]
    >>> list(range(9))
    [0, 1, 2, 3, 4, 5, 6, 7, 8]

Je ziet dat :python:`range(n)` de getallen :python:`0` tot en met :python:`n-1` genereert. Meestal is dit voor ons doel (een for loop) voldoende, maar het is mogelijk om aan :python:`range()` drie parameterwaarden mee te geven:

.. py:function:: range(start, stop, step)

    Retourneert een reeks getallen, die standaard bij 0 begint, telkens met 1 ophoogt en stopt juist voor het meegegeven getal.

    :Parameters:
        * Het startgetal van de serie. Niet verplicht; standaardwaarde is 0.
        * Het stopggetal van de serie. Verplicht.
        * De stapgrootte. Niet verplicht; standaardwaarde is 1.
    :Returnwaarde:
        Een serie getallen. Om de serie te tonen in de CLI heb je de :python:`list()` functie nodig. 
    :Voorbeelden:
        .. code-block:: python
            :class: no-copybutton

            >>> list(range(3))          # alleen stop meegegeven.
            [0, 1, 2]

            >>> list(range(2, 6))       # start en stop meegegeven.
            [2, 3, 4, 5]

            >>> list(range(4, 11, 2))   # start, stop en step meegegeven.
            [4, 6, 8, 10]

For en range
-------------

De functie :python:`range` wordt vaak gebruikt in for loops. Dat ziet er zo uit:

.. code-block:: python
    :linenos:
    :caption: for_loop.py

    for i in range(5):
        print(i)

Je kunt regel 1 lezen als: *voor elke waarde van* :python:`i` *in de reeks 0 t/m 4, doe het volgende*. De variabele :python:`i` krijgt dus achtereenvolgens de waarden :python:`0`, :python:`1`, :python:`2`, :python:`3` en :python:`4`. In de output van het programma zie je dit terug:

.. code-block::

    0
    1
    2
    3
    4

Uiteraard is niet verplicht om de waarde van :python:`i` binnen de for loop te gebruiken:

.. code-block:: python
    :linenos:
    :caption: for_loop.py

    for i in range(5):
        print('Hello, World!')

Het resultaat:

.. code-block::

    Hello, World!
    Hello, World!
    Hello, World!
    Hello, World!
    Hello, World!

Wanneer je een for loop gebruikt voor het tekenen van een vierkant met Python turtle, heb je minder coderegels nodig dan wanneer je dat met een while loop doet:

.. grid:: 2

    .. grid-item:: 
        :columns: 6

        .. code-block:: python
            :linenos:
            :caption: turtle_while.py

            import turtle

            tony = turtle.Turtle()

            zijde = 0
            while zijde < 4:
                tony.fd(100)
                tony.lt(90)
                zijde = zijde + 1

    .. grid-item:: 
        :columns: 6

        .. code-block:: python
            :linenos:
            :caption: turtle_for.py

            import turtle

            tony = turtle.Turtle()

            for zijde in range(4):
                tony.fd(100)
                tony.lt(90)

Opdrachten
-----------

.. dropdown:: Opdracht 01
    :open:
    :color: secondary
    :icon: pencil

    Hieronder zie je code waarmee de turtle een driehoek tekent. Als je de opdrachten over :doc:`while loops </ch07_loops/ch_07_01_while_loops>` hebt gemaakt, beschik je al over deze code in het bestand :file:`driehoeken.py`. Zo niet, kopieer dan onderstaande code naar een nieuw bestand :file:`driehoeken.py`. 

    .. code-block:: python
        :linenos:
        :caption: driehoeken.py
        :name: turtle_while_opdr01

        # For loops - opdracht 01
        
        import turtle

        tony = turtle.Turtle()

        zijde = 0
        while zijde < 3:
            tony.fd(100)
            tony.lt(120)
            zijde = zijde + 1

    Vervang de while loop in deze code door een for loop.


.. dropdown:: Opdracht 02
    :open:
    :color: secondary
    :icon: pencil

    Maak een nieuw bestand in Mu editor, kopieer onderstaande de code erin en sla het op onder de naam :file:`turtle_dots_for.py`.

    .. code-block:: python
        :linenos:

        # For loops - opdracht 02

        import turtle

        tony = turtle.Turtle()
        tony.speed(0)
        tony.up()

        for rij in range(5):
            for kolom in range(5):
                tony.dot(20, 'red')
                tony.fd(30)
            # Stuur tony naar het begin van de volgende rij:
            tony.bk(5*30)
            tony.lt(90)
            tony.fd(30)
            tony.rt(90)
            
        tony.home()
    
    Dit programma tekent een rooster van 5 bij 5 rode dots. De opdracht :python:`tony.home()` in regel 17 zorgt ervoor dat de turtle weer teruggaat naar het beginpunt. De output van het programma ziet er zo uit:

    .. figure:: images/red_dots_02.png

    Voeg aan het programma na regel 19 code toe waarmee een rooster van 4 bij 4 blauwe dots met een grootte van 30 pixels wordt getekend, die precies tussen de rode dots in komen:

    .. figure:: images/red_dots_03.png

    Pak dit handig aan. Je zou bijvoorbeeld de regels 9 t/m 17 kunnen kopiëren en aanpassen.

.. dropdown:: Opdracht 03
    :open:
    :color: secondary
    :icon: pencil

    Maak een nieuw bestand, kopieer onderstaande code erin en sla het op als :file:`turtle_spiral.py`. 

    .. code-block:: python
        :linenos:
        :caption: turtle_spiral.py
        :name: turtle_spiral_v01

        # For loops - opdracht 03

        import turtle

        tony = turtle.Turtle()
        tony.speed(0)

        for i in range(300):
            tony.fd(i * 2)
            tony.lt(30)

    Deze code tekent eerst een lijnstukje van 0 pixels, vervolgens een lijnstukje van 2 pixels, dan 4 pixels, dan 6 pixels, enzovoort tot het laatste lijnstukje van 598 pixels (299 * 2). En tussendoor draait de turtle telkens 30 graden. Kijk maar eens wat dat oplevert, door het programma te runnen.

    Experimenteer met de code in :file:`turtle_spiral.py` door telkens één getal te veranderen en te bekijken hoe de figuur verandert. Je zou :python:`tony` bijvoorbeeld telkens 91° kunnen laten draaien in plaats van 30°. En wat gebeurt er als je op regel 10 :python:`tony.lt(30)` vervangt door :python:`tony.lt(i)` of :python:`tony.lt(i * 3)`? Probeer maar uit!
