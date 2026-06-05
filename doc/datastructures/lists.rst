.. role:: python(code)
   :language: python

.. |br| raw:: html

   <br/>

Werken met lijsten
===================

Je bent inmiddels bekend met enkele basale datatypes in Python: integers, floats, strings en booleans.

.. code-block:: python

   getal_01 = 42     # Een integer
   getal_02 = 3.14   # Een float
   tekst = 'Hallo'   # Een string
   succes = True     # Een boolean

Voor het programmeren van games heb je zelden genoeg aan deze vier datatypes. Bij veel games heb je namelijk te maken met verzamelingen van waarden. Bijvoorbeeld, een verzameling vijanden, een lijst van items in een inventory of een reeks levels.

Python biedt voor het opslaan van verzamelingen verschillende *container* datatypes, zoals *sets*, *lists*, *tuples* en *dictionaries*. In deze paragraaf gaan we aan de slag met het datatype *list*.

Blokhaken
-----------------
In Python gebruik je vierkante haakjes om een list aan te maken. De waarden in de list worden gescheiden door komma's. Hier is een voorbeeld:

.. code-block:: python

   boodschappen = ['melk', 'brood', 'eieren']   # Een list van strings

Je kunt ook een list maken van andere datatypes, zoals integers, floats, booleans of alles door elkaar:

.. code-block:: python

   machten_van_2 = [1, 2, 4, 8, 16]   # Een list van integers
   mix = [1, 'twee', 3.0, True]       # Een list van verschillende datatypes

En je kunt zelfs een list maken van andere lists:

.. code-block:: python

   nested_list = [[1, 2], [3, 4]]     # Een list van lists
   
Indices
---------

Een list is niet zomaar een verzameling, het is een *geordende* verzameling. Dat wil zeggen: de volgorde van de waarden in de list is belangrijk. Elke waarde in de list heeft een zogenoemde *index*. De index is een getal dat aangeeft op welke positie de waarde in de list staat. Let op: we tellen hierbij niet vanaf 1 maar vanaf 0! De eerste waarde in de list heeft dus index :python:`0`, de tweede waarde heeft index :python:`1`, enzovoort.

Neem bijvoorbeeld de volgende list:

.. code-block:: python

   fruit = ['appel', 'banaan', 'citroen']

Hieronder zie je de indices van de waarden in de list:

.. list-table::
   :header-rows: 0
   :stub-columns: 1

   * - Index
     - :python:`0` 
     - :python:`1`
     - :python:`2` 
   * - Waarde
     - :python:`'appel'`
     - :python:`'banaan'`
     - :python:`'citroen'`

Met behulp van de index kun je een waarde uit een list opvragen:

.. code-block:: python

   >>> fruit = ['appel', 'banaan', 'citroen']
   >>> fruit[1]
   'banaan'

Of een waarde in een list veranderen:

.. code-block:: python

   >>> fruit = ['appel', 'banaan', 'citroen']
   >>> fruit[1] = 'bosbes'
   >>> fruit
   ['appel', 'bosbes', 'citroen']


Lengte van een list: len()
---------------------------

Het komt vaak voor dat je wilt weten hoeveel waarden er in een list staan. Dat kan met de functie :python:`len()`. Deze functie geeft het aantal waarden in de list terug.

.. code-block:: python

   >>> fruit = ['appel', 'banaan', 'citroen']
   >>> len(fruit)
   3

De functie :python:`len()` stelt je tevens in staat om het laatste item in een list op te vragen:

.. code-block:: python

   >>> fruit = ['appel', 'banaan', 'citroen']
   >>> fruit[len(fruit) - 1]
   'citroen'

Merk op dat je hier :python:`len(fruit) - 1` als index moet gebruiken, omdat we bij 0 beginnen met tellen, waardoor de index van het laatste item in de list gelijk is aan de lengte van de list min 1.

Python biedt echter een snellere manier om het laatste item in een list op te vragen. Je kunt namelijk ook een negatieve index gebruiken. De index :python:`-1` verwijst naar het laatste item in de list, :python:`-2` naar het op één na laatste item, enzovoort.

   >>> fruit = ['appel', 'banaan', 'citroen']
   >>> fruit[-1]
   'citroen'
   >>> fruit[-2]
   'banaan'

Items toevoegen en verwijderen
--------------------------------------------------

Om een nieuwe waarde aan bestaande list toe te voegen, kun je de functie :python:`append()` gebruiken. Deze functie is een *methode* van de list en daarom roep je hem aan met een punt achter de list naam:

.. code-block:: python
   :emphasize-lines: 2

   >>> groenten = ['andijvie', 'broccoli', 'courgette']
   >>> groenten.append('doperwt')
   >>> groenten
   ['andijvie', 'broccoli', 'courgette', 'doperwt']
   
Een waarde uit een list verwijderen kan met de methode :python:`remove()`. Deze functie verwijdert de eerste waarde die overeenkomt met de opgegeven waarde. Als je bijvoorbeeld de waarde :python:`'broccoli'` uit de list wilt verwijderen, doe je dat als volgt:

.. code-block:: python
   :emphasize-lines: 2

   >>> groenten = ['andijvie', 'broccoli', 'courgette']
   >>> groenten.remove('broccoli')
   >>> groenten
   ['andijvie', 'courgette']

Wanneer je de index weet van de waarde die je wilt verwijderen, kun je de functie :python:`pop()` gebruiken. Deze methode verwijdert de waarde op de opgegeven positie uit de list:

.. code-block:: python
   :emphasize-lines: 2

   >>> groenten = ['andijvie', 'broccoli', 'courgette']
   >>> groenten.pop(0)
   >>> groenten
   ['broccoli', 'courgette']

De functie :python:`pop()` geeft de verwijderde waarde ook terug. Dit kan handig zijn als je de verwijderde waarde nog even wilt gebruiken in je programma. Hier is een voorbeeld:

Alle items in een list langslopen
--------------------------------------------------

Het langslopen van alle items in een list noemen we *itereren*. Dit kan op verschillende manieren. De meest gebruikelijke manier is met een *for* loop. Hier is een voorbeeld:

.. code-block:: python
   :linenos:

   kleuren = ['rood', 'groen', 'blauw']
   for kleur in kleuren:
      print(kleur)

De uitvoer van dit programma is:

.. code-block:: python

   rood
   groen
   blauw            

.. _Enumerate:

Soms wil je bij het itereren ook de index weten van het item dat je aan het verwerken bent. Dit kan met de functie :python:`enumerate()`:

.. code-block:: python
   :linenos:

   kleuren = ['rood', 'groen', 'blauw']
   for index, kleur in enumerate(kleuren):
      print(index, kleur)

De uitvoer van dit programma is:

.. code-block:: python

   0 rood
   1 groen
   2 blauw

List comprehensions
--------------------------------------------------

Stel dat je een lijst wilt maken met de kwadraten van de getallen 1 tot en met 10. Dit kan met een for loop:

.. code-block:: python
   :linenos:

   kwadraten = []
   for n in range(1, 11):
      kwadraten.append(n * n)
   print(kwadraten)

In regel 1 maken we een lege list aan met de naam :python:`kwadraten`. In regel 2 gebruiken we een for loop om door de getallen 1 tot en met 10 te itereren. In regel 3 voegen we het kwadraat van elk getal toe aan de list :python:`kwadraten`. Tot slot printen we de list naar het scherm. De uitvoer van dit programma is:

.. code-block:: python

   [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

Met een zogenoemde *list comprehension* kan dit echter sneller en eleganter:

.. code-block:: python
   :linenos:

   kwadraten = [n * n for n in range(1, 11)]
   print(kwadraten)

Met een list comprehension kun je in één regel een nieuwe list maken op basis van een bestaande list. De syntax is als volgt:

:tt:`nieuwe_list = [<uitdrukking> for <item> in <bestaande_list>]`

Misschien denk je nu, hoezo *bestaande list*? We hebben toch helemaal geen bestaande list gebruikt voor de :python:`kwadraten` list? Klopt, maar we kunnen ook een range gebruiken als bestaande list. De volgende twee regels zijn dus gelijkwaardig:

.. code-block:: python

   kwadraten = [n * n for n in [0, 1, 2, 3]]
   kwadraten = [n * n for n in range(4)]

List comprehensions zijn heel handig om snel lijsten te maken. Bijvoorbeeld, wanneer je een lijst met 100 nullen wilt maken:

.. code-block:: python

   nullen = [0 for n in range(100)]

Tweedimensionale lists
--------------------------------------------------

Zoals gezegd kan een list andere lists bevatten:

.. code-block:: python

   >>> my_list = [['a', 'b', 'c'], ['d', 'e']]
   >>> my_list[0]
   ['a', 'b', 'c']
   >>> my_list[1]
   ['d', 'e']

Hoe zou je de waarde :python:`'b'` uit de list :python:`my_list` kunnen ophalen? Het is het tweede item in de eerste list. Dus je moet eerst de eerste list opvragen en dan het tweede item uit die list:

.. code-block:: python

   >>> my_list[0][1]
   'b'

Met een list comprehension kun je ook een lijst maken van lijsten. Dit noemen we een *tweedimensionale list*. Hier is een voorbeeld:

.. code-block:: python
   :linenos:

   tabel = [[n for n in range(1, 4)] for m in range(1, 7)]
   for row in tabel: print(row)

Regel 1 ziet er ingewikkeld uit hè? Laten we hem even ontleden. We maken een list met de naam :python:`tabel`. Deze list bevat 6 lists, omdat we in de buitenste list comprehension :python:`for m in range(1, 7)` gebruiken. De binnenste list comprehension maakt met :python:`n for n in range(1, 4)` een lijst met de getallen 1 tot en met 3. Dit gebeurt 6 keer, omdat we in de buitenste list comprehension 6 keer itereren. De for loop in regel 2 print elke rij van de tabel naar het scherm. |br|
De uitvoer van dit programma is:

.. code-block:: python

   [1, 2, 3]
   [1, 2, 3]
   [1, 2, 3]
   [1, 2, 3]
   [1, 2, 3]
   [1, 2, 3]

Als je alles wat hierboven staat hebt begrepen, ben je klaar voor de platformerprogrammeertechniek *tilemaps*. Zijn er dingen die je nog niet helemaal snapt? Lees de uitleg dan nog eens door en experimenteer zelf met lists in Mu Editor.