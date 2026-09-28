.. role:: python(code)
   :language: python

Hello World
=============

Het is tijd om je eerste programma te maken in Mu editor. Het woord programma is enigszins overdreven, want het bestaat slechts uit twee regels code. Typ de onderstaande code voor het programma hello_word.py in Mu editor. Uiteraard hoef je de regelnummers 1 en 2 niet te typen.

.. code-block:: python
    :class: no-copybutton
    :linenos:
    :caption: hello_world.py
    :name: hello_world

    # Dit is mijn eerste programma
    print('Hello, World!')

.. dropdown:: Hoe typ je aanhalingstekens?
    :color: info
    :icon: info

    Een aanhalingsteken typ je door eerst op de toets met het aanhalingsteken :kbd:`'` te drukken, gevolgd door :kbd:`Spatie`. Het aanhalingsteken verschijnt pas nadat je op de spatiebalk heb gedrukt.

    .. image:: images/typing_quotes_small.png

    Je kunt ook dubbele aanhalingstekens gebruiken (voor Python maakt dat weinig uit). Daarvoor moet je :kbd:`Shift` ingedrukt houden terwijl je op :kbd:`'` drukt. En daarna weer :kbd:`Spatie`.

Klik in de knoppenbalk op :guilabel:`Save` om het bestand op te slaan. Navigeer naar je :file:`Documenten/Python Projecten` map en sla daarin het bestand op onder de naam :file:`hello_world`.

.. uml::
    :align: center
    :html_format: svg

    @startuml
        @startfiles
        /Documenten/Python Projecten/hello_world.py
        @endfiles
    @enduml

Klik in de knoppenbalk op :guilabel:`Run` om je programma uit te voeren. Als je alles goed hebt gedaan, wordt de tekst :code:`Hello, World!` getoond.

.. image:: images/mu_hello_world.png

Klik in de knoppenbalk op de knop :guilabel:`Stop` om de uitvoering van het programma te stoppen.

.. dropdown:: Wist je dat?
    :open:
    :color: info
    :icon: info

    Een programma dat :code:`Hello, World!` op het scherm toont, is traditioneel het eerste dat elke programmeur maakt wanneer zij/hij een nieuwe programmeertaal leert. Het is eenvoudig, maar toch heb je nu al een aantal dingen geleerd:

    * Hoe je in Mu editor code typt, opslaat en uitvoert.
    * Dat je in Python met een hekje :python:`#` commentaar kunt aangeven. Commentaar wordt door Python genegeerd bij het uitvoeren van de code.
    * Dat je in Python een tekst op het scherm kunt tonen met de functie :python:`print()` en dat de tekst tussen aanhalingstekens moet staan.
