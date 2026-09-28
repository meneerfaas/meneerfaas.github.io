Formatted strings (f-strings)
==============================

Zoals je in het hoofdstuk :doc:`strings <strings>` zag, kun je de waarde van een variabele aan een string vastplakken met de :python:`+` operator. En als de variabele geen string is, moet je typecasten naar een string met de functie :python:`str()` . Bijvoorbeeld:

.. _string_concat_example:
.. code-block:: python

    prijs = 11
    print('Het boek kost ' + str(prijs) + ' euro.')

Sinds versie 3.6 van Python, kan het verwerken van variabelen in strings echter een stuk eenvoudiger door zogenoemde *f-strings*  te gebruiken.

.. dropdown:: Wat leer je in dit hoofdstuk
    :open:
    :color: primary
    :icon: book

    * Hoe maak je een f-string.
    * Hoe verwerk je waarden van variabelen in een f-string.
    * Hoe verwerk je *expressies* in een f-string.
    * Hoe kun je extra *formatting* (opmaak) toepassen in een f-string.

Accolades
----------

Het enige wat je hoeft te doen om een f-string te maken, is de letter ``f`` voor het eerste aanhalingsteken zetten:

.. code-block:: python
    :class: no-copybutton
    
    tekst = f'Het boek kost 11 euro.'
    print(tekst)

Uiteraard kun je de f-string ook rechtstreeks aan de :python:`print()` aanroep meegeven, zonder de variabele :python:`tekst` te gebruiken:

.. code-block:: python
    :class: no-copybutton
    
    print(f'Het boek kost 11 euro.')

Waarden van variabelen verwerk je in een f-string door de naam van de variabele tussen accolades (:python:`{` en :python:`}`) te zetten:

.. code-block:: python
    :class: no-copybutton
    
    prijs = 11
    print(f'Het boek kost {prijs} euro.')

Deze code leest toch een stuk prettiger dan die in het :ref:`eerste voorbeeld <string_concat_example>`? Een f-string bespaart je aanhalingstekens en plus-tekens waardoor de code beter leesbaar is. En het mooiste is: je hoeft niet te typecasten!

Hoe meer variabelen er in het spel zijn, hoe duidelijker het voordeel van f-strings blijkt:

.. code-block:: python
    :class: no-copybutton

    stad = 'Harlingen'
    temperatuur = 15    
    windkracht = 6

    # Met een f-string:
    print(f'In {stad} is het {temperatuur} graden bij windkracht {windkracht}.')

    # Zonder f-string:
    print('In ' + stad + ' is het ' + str(temperatuur) + ' graden bij windkracht ' + str(windkracht) + '.')

Expressies in een f-string
---------------------------

Behalve de naam van een variabele kun je tussen de accolades in een f-string nog meer code zetten. Bijvoorbeeld *expressies* zoals :python:`9 + 6` of :python:`3 * 'ha'`.

.. dropdown:: Wat is een expressie?
    :color: info
    :icon: info

    In programmeertalen is een expressie een berekening met waarden, variabelen, operatoren en functies die een bepaalde waarde oplevert. We zeggen dan dat de expressie *evalueert* naar die waarde.

.. code-block:: python
    :class: no-copybutton

    prijs_per_stuk = 0.60
    aantal = 4
    print(f'U betaalt {aantal} * {prijs_per_stuk} = {aantal * prijs_per_stuk} euro.')

Het resultaat van deze code:

.. code-block::
    :class: no-copybutton

    U betaalt 4 * 0.6 = 2.4 euro.

Door in de f-string :python:`{aantal * prijs_per_stuk}` op te nemen, rekent Python de totale prijs uit en plaatst die in de string. Het is alleen wel jammer dat de bedragen :python:`0.6` en :python:`2.4` worden afgerond op één cijfer achter de komma, terwijl het gebruikelijk is om geldbedragen op twee cijfers achter de komma af te ronden. Geen nood, met wat extra *formatting* lossen we dat snel op.

Extra formatting
------------------

Door achter de variabele of expressie in een f-string een dubbele punt :python:`:` te plaatsen, kun je extra opmaakregels instellen. Bijvoorbeeld om een getal af te ronden op 2 cijfers achter de komma typ je :python:`:.2f`.

.. code-block:: python
    :class: no-copybutton

    prijs_per_stuk = 0.60
    aantal = 4
    print(f'U betaalt {aantal} * {prijs_per_stuk:.2f} = {aantal * prijs_per_stuk:.2f} euro.')

Nu worden de bedragen getoond zoals het hoort:

.. code-block::
    :class: no-copybutton

    U betaalt 4 * 0.60 = 2.40 euro.

Het is ook mogelijk om het aantal karakters dat een waarde moet innemen te specificeren. Dat doe je door een integer achter de :python:`:` te typen. Dit is bijvoorbeeld handig om tekst netjes onder elkaar te positioneren zoals in een tabel:

.. code-block:: python
    :class: no-copybutton

    voornaam1 = 'Graham'
    voornaam2 = 'John'
    voornaam3 = 'Terry'
    achternaam1 = 'Chapman'
    achternaam2 = 'Cleese'
    achternaam3 = 'Gilliam'
    print(f'{voornaam1:10}{achternaam1}')
    print(f'{voornaam2:10}{achternaam2}')
    print(f'{voornaam3:10}{achternaam3}')

Deze code levert de volgende output:

.. code-block::
    :class: no-copybutton

    Graham    Chapman   
    John      Cleese    
    Terry     Gilliam

De naam ``Graham`` telt zes karakters (letters), maar doordat in de f-string :python:`{voornaam1:10}` staat, wordt de naam met spaties aangevuld tot 10 karakters. Door dit met alle voornamen te doen, komen de achternamen mooi recht onder elkaar te staan.

Er zijn nog veel meer opmaakmogelijkheden met f-strings. Als je nieuwsgierig bent, kun je `hier <https://www.w3schools.com/python/python_string_formatting.asp>`_ een lijst vinden.

Opdrachten
-----------

.. dropdown:: Opdracht 01
    :open:
    :color: secondary
    :icon: pencil

    Maak in Mu editor een nieuw codebestand en sla het op onder de naam :file:`f_strings.py`. Kopieer de onderstaande code naar het bestand:

    .. code-block::
        :linenos:
        :caption: f_strings.py

        # F-strings - opdracht 01

        vak1 = 'Oude Talen en Verhalen'
        cijfer1 = 9
        vak2 = 'Informatica'
        cijfer2 = 8

        tekst = 'Voor ' + vak1 + ' had Mariska een ' + str(cijfer1) + ' en voor ' + vak2 + ' een ' + str(cijfer2) + '.'
        print(tekst)

    Run het programma om de output te bekijken. Wijzig vervolgens regel 8 van de code zodat de variabele :python:`tekst` één f-string wordt. Uiteraard mag de output van het programma hierdoor niet veranderen.
    

.. dropdown:: Opdracht 02
    :open:
    :color: secondary
    :icon: pencil

    Maak in Mu editor een nieuw codebestand en sla het op als :file:`f_strings_2.py`. Kopieer de onderstaande code naar het bestand:

    .. code-block::
        :linenos:
        :caption: f_strings_2.py

        # F-strings - opdracht 02

        a = 12
        b = 25
        c = 8

        print(f' ')

    Plaats in regel 7 code tussen de aanhalingstekens zodat het programma exact de volgende output geeft:

    ``Het gemiddelde van de getallen 12, 25 en 8 is 15.``

    Je mag in regel 7 de variabelen :python:`a`, :python:`b` en :python:`c` gebruiken, maar niet de getallen :python:`12`, :python:`25`, :python:`8` en :python:`15`. Je mag ook geen extra aanhalingstekens toevoegen.    