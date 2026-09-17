Functiebeginselen
=======================

Je hebt inmiddels kennisgemaakt met de functies :python:`print()`, :python:`input()`, :python:`type()`, :python:`int()`, :python:`float()` en :python:`str()`. Dit zestal vormt slechts het topje van de spreekwoordelijke ijsberg. Python telt bijna zeventig standaard ingebouwde functies, en daarnaast nog duizenden (misschien zelfs miljoenen) andere functies die je kunt gebruiken. Functies zijn, net als variabelen, belangrijke bouwstenen voor een computerprogramma. Hoog tijd dus om er wat dieper op in te gaan.      

.. dropdown:: Wat leer je in dit hoofdstuk
    :open:
    :color: primary
    :icon: book

    * Wat bedoelen we met een functie als *black box*.
    * Wat bedoelen we met de *parameters* van een functie.
    * Wat bedoelen we met de *returnwaarde* van een functie.
    * Wat doen de functies :python:`min()`, :python:`max()`, :python:`round()` en :python:`len()`.
    * Wat bedoelen we met een *module*.
    * Hoe importeer je een module in je programma.
    * Hoe roep je een functie aan uit een module.
    * Wat doen de functies :python:`math.sqrt()` en :python:`random.randint()`.     

Onderdelen van een functie
----------------------------

Wanneer je in Python :python:`print('Hello, World!')` aanroept, wordt op de achtergrond een stuk code uitgevoerd waardoor de tekst ``Hello, World!`` op het scherm verschijnt. Die code is geprogrammeerd door de makers van Python. Een functie is dan ook eigenlijk een blok code dat je kunt laten uitvoeren wanneer je het nodig hebt. Om dat voor elkaar te krijgen moet je drie dingen weten:

* De naam van de functie.
* De eventuele parameters die de functie nodig heeft (de input).
* De eventuele waarde die de functie teruggeeft (de output).

Je zou een functie enigszins kunnen vergelijken met een recept in een kookboek. Wanneer je een appeltaart wilt bakken, gebruik je het recept ``Appeltaart``. De ingrediënten voor de taart zijn de input en de output is een overheerlijke versgebakken appeltaart.

.. card::

    .. list-table::
        :header-rows: 1
        :align: center
        :stub-columns: 1

        * - 
          - Functie
          - Recept
        * - Naam
          - :python:`str()` 
          - ``Appeltaart``
        * - Input
          - een waarde, bijvoorbeeld :python:`42` 
          - ingrediënten
        * - Output
          - de :python:`string` versie van de waarde: :python:`'42'` 
          - een taart

Meestal verbeelden we een functie als een *black box* of een machientje waar je input in kunt stoppen en waar output uit komt:

.. figure:: images/function_black_box_transparent.png
    :width: 500

Naam
^^^^^^

Net als variabelen hebben functies een naam. En net als bij variabelen mag die naam slechts bestaan uit letters, cijfers en underscores, en mag hij niet met een cijfer beginnen. Zoals het handig is om voor variabelen namen te gebruiken die iets zeggen over de inhoud van de variabele, hebben functies een naam die aangeeft wat de functie doet.

Parameters
^^^^^^^^^^^^

Aan de meeste functies kun je input meegeven in de vorm van zogenoemde *parameters*. Dat zijn waarden die je aan de functie meegeeft om te verwerken. De parameterwaarden zet je tussen de haakjes bij de functieaanroep. Bijvoorbeeld de functie :python:`int()` roep je aan met één parameter, namelijk de waarde waarvan de functie probeert een integer versie te maken. 

.. code-block:: python

  >>> int('21')
  21

De functie :python:`max()`, die het grootste getal in een reeks teruggeeft, kun je aanroepen met twee parameters...

.. code-block:: python

  >>> max(3, 5)
  5

...maar ook met meer parameters:

.. code-block:: python

  >>> max(3, 5, 2, 8, 1, 6)
  8

Over het algemeen kan een functie de waarden van de parameters niet wijzigen. Wanneer je bijvoorbeeld aan de functie :python:`int()` een stringvariabele meegeeft, blijft dat een stringvariabele:

.. code-block:: python

  >>> a = '21'
  >>> int(a)
  21
  >>> a
  '21'

De aanroep :python:`int(a)` in het voorbeeld hierboven heeft de integer :python:`21` teruggegeven, maar de variabele :python:`a` bevat nog steeds de stringwaarde :python:`'21'`. Als je wilt dat :python:`a` de integerwaarde :python:`21` krijgt, zou je het resultaat van :python:`int(a)` weer moeten opslaan in :python:`a` met een assignment statement:

.. code-block:: python
  :emphasize-lines: 2

  >>> a = '21'
  >>> a = int(a)
  >>> a
  21

Returnwaarde
^^^^^^^^^^^^^^^^

De output van een functie wordt vaak de *returnwaarde* of *retourwaarde* genoemd. Bijvoorbeeld de functie :python:`int()` retourneert een integer versie van de inputwaarde. Zoals je in het laatste voorbeeld hierboven zag, kun je die returnwaarde weer in een variabele stoppen met een assignment statement. Maar je kunt de waarde ook op een andere manier gebruiken, bijvoorbeeld in een berekening of in een :python:`print()` aanroep.

.. code-block:: python
  
  >>> a = '21'
  >>> b = int(a) // 7
  >>> print(b * int(4.75))
  12

Niet alle functies retourneren een waarde. Bijvoorbeeld :python:`print()` drukt tekst af op het scherm, maar geeft geen waarde terug. Dat kun je als volgt checken:

.. code-block:: python
  
  >>> a = print('Hello, World!')
  Hello, World!
  >>> print(a)
  None

In dit voorbeeld wordt de returnwaarde van :python:`print('Hello, World!')` opgeslagen in :python:`a`. Wanneer we vervolgens :python:`print(a)` aanroepen om de waarde van :python:`a` te tonen, wordt :python:`None` afgedrukt. Dit is een van de Python keywords die je eerder tegenkwam in het hoofdstuk :ref:`Variabelen <python-keywords>`. Het geeft aan dat :python:`a` geen waarde heeft. 

Ingebouwde functies
---------------------

Zoals gezegd kent Python een kleine zeventig ingebouwde functies. De werking van :python:`print()`, :python:`input()`, :python:`type()`, :python:`int()`, :python:`float()` en :python:`str()` ken je inmiddels, maar hieronder volgen er nog enkele die je vaak van pas kunnen komen.


.. py:function:: min()

    Retourneert de kleinste waarde uit een reeks waarden. Als de waarden strings zijn, wordt gekeken naar de alfabetische volgorde.

    :Parameters:
        Één of meerdere waarden.
    :Returnwaarde:
        De kleinste waarde.
    :Voorbeelden:
        .. code-block:: python
            :class: no-copybutton

            >>> min(4, 8, 3, 11)
            3

            >>> min('kers', 'druif', 'bosbes')
            'bosbes'

.. py:function:: max()

    Retourneert de grootste waarde uit een reeks waarden. Als de waarden strings zijn, wordt gekeken naar de alfabetische volgorde.

    :Parameters:
        Één of meerdere waarden.
    :Returnwaarde:
        De grootste waarde.
    :Voorbeelden:
        .. code-block:: python
            :class: no-copybutton

            >>> max(4, 8, 3, 11)
            11

            >>> max('kers', 'druif', 'bosbes')
            'kers'

.. py:function:: round()

    Rondt een getal af op een gegeven aantal cijfers achter de komma.

    :Parameters:
        Een getal en (eventueel) het aantal decimalen waarop moet worden afgerond. Als je geen aantal decimalen meegeeft, wordt afgerond op gehelen.
    :Returnwaarde:
        De afgeronde versie van het getal.
    :Voorbeelden:
        .. code-block:: python
            :class: no-copybutton

            >>> round(8.75)
            9

            >>> round(3.14159, 2)
            3.14

.. py:function:: len()

    Retourneert het aantal karakters in een string.

    :Parameters:
        Een stringwaarde. Later zul je zien dat :python:`len()` ook met andere datatypes overweg kan, maar vooralsnog gebruiken we deze functie alleen voor strings. 
    :Returnwaarde:
        Het aantal karakters in de string.
    :Voorbeelden:
        .. code-block:: python
            :class: no-copybutton

            >>> len('Python')
            6

            >>> len('')
            0

            >>> len('A B')              # Spaties zijn ook karakters
            3

            >>> len('Hello,\nWorld!')   # \n is ook een karakter
            13

Modules
---------

Naast de standaard in Python ingebouwde functies zijn er nog vele andere functies die je in je programma's kunt gebruiken. Deze functies bevinden zich in zogenoemde *modules*. Een module is een Python codebestand met een verzameling functies die passen bij een bepaald onderwerp. Zo bevat de ``math`` module allerlei wiskundige functies en de ``random`` module allerlei functies voor het genereren van willekeurige getallen.

Om de functies uit een module te kunnen gebruiken, moet je twee dingen doen:

* Boven in je programma moet je de module *importeren*. Dat doe je met de code :python:`import <modulenaam>`.
* De functieaanroep moet je vooraf laten gaan door de naam van de module en een punt.

Laten we bijvoorbeeld de ``math`` module eens importeren en de :python:`math.sqrt()` functie aanroepen, waarmee je de wortel uit een getal kunt trekken:

.. code-block:: python
    
    import math

    print(math.sqrt(9))

De output van deze code is:

.. code-block:: python

    3.0

En dit klopt, want :math:`\sqrt{9}=3`.

.. dropdown:: Wat is worteltrekken?
    :color: info
    :icon: info

    Om te begrijpen wat worteltrekken is, moet je eerst weten wat we bedoelen met *kwadrateren*. Kwadrateren betekent 'tot de macht 2 verheffen', oftewel 'vermenigvuldigen met zichzelf':

    .. card:: Kwadrateren

        :math:`1^{2} = 1 \times 1 = 1` |br|
        :math:`2^{2} = 2 \times 2 = 4` |br|
        :math:`3^{2} = 3 \times 3 = 9` |br|
        :math:`4^{2} = 4 \times 4 = 16` |br|
        :math:`5^{2} = 5 \times 5 = 25` |br|
        ...enzovoort.

    Worteltrekken is het omgekeerde van kwadrateren. We gebruiken hiervoor  het :math:`\sqrt{\text{ }}` symbool:

    .. card:: Worteltrekken

        :math:`\sqrt{1} = 1 \text{ want } 1^{2} = 1` |br|
        :math:`\sqrt{4} = 2 \text{ want } 2^{2} = 4` |br|
        :math:`\sqrt{9} = 3 \text{ want } 3^{2} = 9` |br|
        :math:`\sqrt{16} = 4 \text{ want } 4^{2} = 16` |br|
        :math:`\sqrt{25} = 5 \text{ want } 5^{2} = 25` |br|
        ...enzovoort.

    Wellicht vraag je je na het zien van bovenstaand rijtje af of bijvoorbeeld :math:`\sqrt{2}` en :math:`\sqrt{3}` ook bestaan. Dat kun je uiteraard zelf uitproberen in Python met de code :python:`print(math.sqrt(2))` en :python:`print(math.sqrt(3))`. Je ziet dan dat deze wortels inderdaad bestaan, maar geen mooie ronde getallen zijn:

    .. card:: 

        :math:`\sqrt{2} \approx 1.4142135623730951 \text{  want  } 1.4142135623730951^{2} \approx 2` |br|
        :math:`\sqrt{3} \approx 1.7320508075688772 \text{  want  } 1.7320508075688772^{2} \approx 3` |br|

    Het symbool :math:`\approx` betekent 'is ongeveer gelijk aan'.

    De wortels in de voorgaande voorbeelden worden ook wel *vierkantswortels* genoemd. In het Engels is een vierkantswortel een *square root*. Nu begrijp je ook waarom de functie :python:`math.sqrt()` heet. 

Met name bij het programmeren van games heb je vaak random getallen nodig. De ``random`` module biedt hiervoor bijvoorbeeld de functie :python:`random.randint()`.

.. py:function:: random.randint(a, b)

    Retourneert een willekeurig geheel getal (een random integer) tussen de grenzen :python:`a` en :python:`b`.  

    :Parameters:
        Twee integers :python:`a` en :python:`b`.  
    :Returnwaarde:
        Een willekeurige integer die minstens :python:`a` is en hoogstens :python:`b`.  
    :Voorbeelden:
        .. code-block:: python
            :class: no-copybutton

            >>> import random
            >>> random.randint(1, 3)
            3
            >>> random.randint(1, 99)
            42

Wanneer je in een game de speler bijvoorbeeld met een dobbelsteen wilt laten gooien, kun je :python:`random.randint(1, 6)` gebruiken.

Opdrachten
-----------

.. dropdown:: Opdracht 01
    :open:
    :color: secondary
    :icon: pencil

    Maak in Mu editor een nieuw codebestand en sla het op onder de naam :file:`woordlengte.py`. Schrijf hierin een programma dat de gebruiker om een woord vraagt en vervolgens toont hoeveel letters dat woord bevat.

    .. figure:: images/woordlengte.png
        :scale: 80%

.. dropdown:: Opdracht 02
    :open:
    :color: secondary
    :icon: pencil

    Maak in Mu editor een nieuw codebestand en sla het op onder de naam :file:`langstewoord.py`. Kopieer de onderstaande code naar het bestand:

    .. code-block::
        :linenos:
        :caption: langstewoord.py

        # Functiebeginselen - opdracht 02

        woord1 = input('Typ het eerste woord: ')
        woord2 = input('Typ het tweede woord: ')

    Voeg code toe zodat het programma toont hoeveel letters het langste woord bevat.

    .. figure:: images/langstewoord.png
        :scale: 80%

.. dropdown:: Opdracht 03
    :open:
    :color: secondary
    :icon: pencil

    Maak in Mu editor een nieuw codebestand en sla het op onder de naam :file:`driegetallen.py`. Schrijf hierin een programma dat de gebruiker om drie (gehele) getallen vraagt en vervolgens toont:

    * wat het kleinste getal is;
    * wat het grootste getal is;
    * wat het gemiddelde van de drie getallen is, afgerond op 2 decimalen.

    .. figure:: images/driegetallen.png
        :scale: 80%

    Denk eraan dat de functie :python:`input()` altijd een string retourneert. Je hebt hier dus type casting nodig om getalswaarden te verkrijgen waarmee Python kan rekenen. 

.. dropdown:: Opdracht 04
    :open:
    :color: secondary
    :icon: pencil

    Een vierkant is een rechthoek met vier gelijke zijden. De oppervlakte van een vierkant bereken je met de formule

    .. math:: 
        
        Oppervlakte = zijde \times zijde

    Wanneer de oppervlakte van een vierkant bekend is, kun je terugrekenen hoe lang de zijden moeten zijn. Dat doe je met worteltrekken.

    .. figure:: images/vierkant.png
        :scale: 75%

    Maak in Mu editor een nieuw codebestand en sla het op onder de naam :file:`vierkant.py`. Schrijf hierin een programma dat de gebruiker om de oppervlakte van een vierkant vraagt en vervolgens de lengte van de zijden toont, afgerond op 1 decimaal:

    .. figure:: images/vierkant_02.png

