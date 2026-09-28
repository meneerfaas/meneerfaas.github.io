Dictionaries
====================

In de huidige tekstversie van het Alchemy spel hebben we lijsten gebruikt voor de elementen, recepten en ontdekkingen. Daaraan kleven echter een aantal nadelen. De recepten en ontdekkingen verwijzen naar de indices van de elementen in :python:`elements`. Stel dat om de een of andere reden de volgorde van de elementen in :python:`elements` verandert (en dus de indices), dan klopt er niets meer van de recepten en de ontdekkingen. Een ander nadeel is dat de tweedimensionale lijst van recepten al snel erg groot wordt. In een spel met 100 elementen, heb je een receptenlijst van 100 ✕ 100 = 10.000 items nodig, waarvan het merendeel de waarde :python:`None` heeft. Dat is niet erg efficiënt. Daarom gaan we :python:`elements` en :python:`recipes` vervangen door *dictionaries*. Een dictionary is een soort lijst, maar in plaats van dat je de items opvraagt met een index (een getal), doe je dat met een sleutel (meestal een string). De sleutel kan van alles zijn, zolang het maar uniek is. Het voordeel daarvan is dat je de volgorde van de items in de dictionary niet meer hoeft te onthouden. Je kunt ze altijd opvragen met hun unieke sleutel. Dat maakt het ook makkelijker om nieuwe elementen toe te voegen of bestaande elementen te verwijderen. De volgorde doet er dan niet meer toe. Dictionaries zijn dus veel flexibeler dan lijsten.

Key-value pairs
---------------------

Een *dictionary* wordt meestal omschreven als een verzameling van *key-value pairs* (sleutel-waarde paren). In Python maak je een dictionary aan met accolades (:python:`{` en :python:`}`). De *key* en de *value* worden gescheiden door een dubbele punt (:python:`:`). De verschillende key-value paren worden gescheiden door een komma (:python:`,`). Een voorbeeld:

.. code-block:: python

   capitals = {
       'Nederland': 'Amsterdam',
       'België': 'Brussel',
       'Duitsland': 'Berlijn',
       'Frankrijk': 'Parijs'
   }

Het opvragen en wijzigen van items gaat op dezelfde manier als bij lijsten, maar in plaats van een index gebruik je de sleutel. Om de hoofdstad van Nederland op te vragen, doe je:

.. code-block:: python

   print(capitals['Nederland'])

Het toevoegen van een nieuw key-value paar gaat ook op deze manier:

.. code-block:: python

   capitals['Luxemburg'] = 'Luxemburg'
   print(capitals)

De uitvoer is:

.. code-block:: python

   {'Nederland': 'Amsterdam', 'België': 'Brussel', 'Duitsland': 'Berlijn', 'Frankrijk': 'Parijs', 'Luxemburg': 'Luxemburg'}

Voor keys in een dictionary worden meestal strings of integers gebruikt. De values kunnen van alles zijn: strings, integers, floats, lijsten, dictionaries, enzovoort.

Alchemy met dictionaries
---------------------------

We gaan de code in :file:`alchemytxt.py` van boven naar beneden herschrijven om de lijsten te vervangen door dictionaries, te beginnen met de volgende regels:

.. code-block:: python
   :caption: alchemytxt.py
   :linenos:

   ################
   # ALCHEMY GAME #
   # Text version #
   ################

   # DICTIONARIES AND LISTS

   elements = {
      'fire': 'vuur',
      'water': 'water',
      'wind': 'wind',
      'earth': 'aarde',
      'steam': 'stoom',
      'wave': 'golf',
      'plant': 'plant',
      'smoke': 'rook',
      'lava': 'lava',
      'dust': 'stof'
   }

   elements_inverse = {v: k for k, v in elements.items()}

   recipes = {}

   discoveries = []

De :python:`elements` lijst is een dictionary geworden waarvan de keys de Engelse namen van de elementen zijn en de values de Nederlandse vertalingen. |br|
In regel 21 zie je een dictionary comprehension, die de inverse (omgekeerde) maakt van :python:`elements`. Dat is nodig omdat de speler straks commando's gaat typen met de Nederlandse namen van de elementen. Voor de grafische versie van het spel zullen we die inverse straks niet meer nodig hebben, maar voor de tekstversie wel. |br|
Zoals je ziet is :python:`discoveries` gewoon een lijst gebleven, en geen dictionary geworden. Wellicht kun je zelf bedenken waarom?

Voor de recepten gaan we later een tekstbestand gebruiken, maar nu stoppen we ze alvast in een multi-line string. Ter herinnering: in Python maak je een multi-line string met drie aanhalingstekens.

.. code-block:: python
   :linenos:
   :lineno-start: 27

   recipes_txt = '''water+fire=steam
   water+wind=wave
   water+earth=plant
   fire+wind=smoke
   fire+earth=lava
   wind+earth=dust'''

Nu we lijsten door dictionaries hebben vervangen, moeten we ook de helper functies aanpassen. Dat is de volgende stap.

Helper functies
-----------------

De functie :python:`add_recipe()` gaan we vervangen door een functie :python:`build_recipes()`. Deze nieuwe functie heeft als taak de recepten in de string :python:`recipes_txt` op te slaan in de dictionary :python:`recipes`. Die dictionary zal er uiteindelijk zo uit gaan zien:

.. code-block:: python

   recipes = {
      'fire': {
         'water': 'steam',
         'wind': 'smoke'
      },
      'water': {
         'wind': 'wave'
      },
      'earth': {
         'water': 'plant',
         'fire': 'lava',
         'wind': 'dust'}
   }

Toen :python:`recipes` nog een tweedimensionale lijst was, konden we de symmetrie van bijvoorbeeld :python:`water+fire` en :python:`fire+water` eenvoudig verwerken door beide mogelijkheden in de lijst op te nemen. Nu gaan we het anders aanpakken: elke combinatie van twee elementen sorteren we eerst op *alfabetische volgorde*. Dus als in een recept :python:`water+fire` staat, maken we daar eerst :python:`fire+water` van, alvorens het recept in de dictionary op te slaan. Later zullen we hetzelfde doen met de commando's die de speler typt. Je ziet in bovenstaande dictionary dat we bijvoorbeeld het recept :python:`fire+earth` (zoals in regel 31 staat) in de dictionary kunnen terugvinden onder de key :python:`'earth'` en vervolgens de key :python:`'fire'`.  

Verwijder de functie :python:`add_recipe()` en voeg in plaats daarvan onderstaande code toe:

.. code-block:: python
   :linenos:
   :lineno-start: 34

   # HELPER FUNCTIONS

   def build_recipes():
      lines = recipes_txt.split('\n')
      for line in lines:
         left, right = line.split('=')
         ingredients = left.split('+')
         if (len(ingredients) != 2):
               raise Exception('Recipe error: number of ingredients must be exactly 2.')
         if ingredients[0] not in elements or ingredients[1] not in elements:
               raise Exception('Recipe error: unknown ingredients.')
         ingredients.sort()
         if ingredients[0] not in recipes: 
               recipes[ingredients[0]] = {ingredients[1]: right}
         else:
               recipes[ingredients[0]][ingredients[1]] = right

In regel 37 splitsen we :python:`recipes_txt` op in regels, door het newline karakter :python:`'\n'` als separator te kiezen. lines is dus een list die er zo uitziet:

.. code-block:: python

   ['water+fire=steam',
    'water+wind=wave', 
    'water+earth=plant', 
    'fire+wind=smoke', 
    'fire+earth=lava', 
    'wind+earth=dust']

Met de for loop in regel 38 bekijken we elke regel en doen daarmee het volgende: |br|
Regel 39: We splitsen de regel op het = teken. Het gedeelte links van het = teken slaan we op in :python:`left` en het gedeelte rechts in :python:`right`. |br|
Regel 40: Het linkerdeel splitsen we nogmaals op het + teken en het resultaat stoppen we in :python:`ingredients`. |br|
Regels 41-42: We checken of :python:`ingredients` twee elementen bevat. Zo niet, dan veroorzaken we een foutmelding. |br|
Regels 43-44: We checken of de twee ingrediënten bestaan in de :python:`elements` dictionary. Zo niet, dan veroorzaken we een foutmelding. |br|
Regel 45: We sorteren de twee ingrediënten op alfabetische volgorde met de listfunctie :python:`sort()`. |br|
Regels 46-49: We moeten checken of het eerste ingrediënt al een key is in de :python:`recipes` dictionary. Als dat niet het geval is, maken we hem aan en geven de value :python:`{ingredients[1]: right}` mee. Als de key wel al bestaat, voegen we het tweede ingrediënt toe aan de bestaande value.

De :python:`get_recipe()` functie moeten we aanpassen om met de :python:`recipes` dictionary te kunnen werken. Dat doen we als volgt:

.. code-block:: python
   :linenos:
   :lineno-start: 51

   def get_recipe(ingredient1, ingredient2):
      ingredients = sorted([ingredient1, ingredient2])
      if ingredients[0] in recipes:
         if ingredients[1] in recipes[ingredients[0]]:
            return recipes[ingredients[0]][ingredients[1]]
      return None

In de nieuwe :python:`get_recipe()` functie sorteren we eerst de twee ingrediënten op alfabetische volgorde. Vervolgens checken we of het eerste ingrediënt als key in :python:`recipes` voorkomt en als dat het geval is of het tweede ingrediënt als key voorkomt in de value bij de eerste key. Als dat zo is, bestaat het recept en retourneren we het resultaat. In het andere geval retourneren we :python:`None`.

Omdat we nu niet meer met indices te maken hebben, worden de functies :python:`add_discovery()` en :python:`is_discovered()` een stuk eenvoudiger:

.. code-block:: python
   :linenos:
   :lineno-start: 58

   def add_discovery(element):
      if element not in discoveries:
         discoveries.append(element)
      
   def is_discovered(element):
      return element in discoveries

Main program
------------------

In het hoofdprogramma roepen we eerst :python:`build_recipes()` aan en vervolgens voegen we de vier discoveries toe, zoals in de vorige versie ook het geval was:

.. code-block:: python
   :linenos:
   :lineno-start: 65

   # MAIN PROGRAM

   build_recipes()
   add_discovery('water')
   add_discovery('fire')
   add_discovery('wind')
   add_discovery('earth')

De game loop is door het gebruik van dictionaries iets ingewikkelder geworden, omdat de gebruiker de Nederlandse namen van de elementen typt, die we moeten opzoeken in :python:`elements_inverse`. Maar voor het overige lijkt hij sterk op de vorige versie. Merk op dat we alle teksten nu ook het Nederlands afdrukken.

.. code-block:: python
   :linenos:
   :lineno-start: 73

   print('Mix twee ingrediënten met een + teken, bijv. water+vuur')
   print('d om ontdekkingen te zien, x om te sluiten')
   while True:
      command = input('> ')
      if command == 'x':
         print('SPEL BEËINDIGD')
         break
      elif command == 'd':
         print('-' * 20)
         print('ONTDEKKINGEN:')
         for d in discoveries: print(elements[d])
         print('-' * 20)
      else:
         ingredients = command.split('+')
         if len(ingredients) != 2:
               print('Dit commando ken ik niet.')
               continue
         ing_val_0 = ingredients[0]
         ing_val_1 = ingredients[1]
         if ing_val_0 not in elements_inverse or ing_val_1 not in elements_inverse:
               print('Dit commando ken ik niet.')
               continue
         ing_key_0 = elements_inverse[ing_val_0]
         ing_key_1 = elements_inverse[ing_val_1]
         if not (is_discovered(ing_key_0) and is_discovered(ing_key_1)):
               print('Dit commando ken ik niet.')
               continue
         r = get_recipe(ing_key_0, ing_key_1)
         if r == None:
               print(f'Helaas, geen recept beschikbaar voor {ing_val_0} en {ing_val_1}.')
         elif r in discoveries:
               print(f'Je had {elements[r]} al ontdekt.')
         else:
               print(f'Je hebt {elements[r]} ontdekt!')
               add_discovery(r)

In regels 90-91 maken we de variabelen :python:`ing_val_0` en :python:`ing_val_1` die de values bevatten die in :python:`elements` zouden moeten voorkomen. Deze values zijn de keys in :python:`elements_inverse`. In regels 95-96 vullen we :python:`ing_key_0` en :python:`ing_key_1` met de keys die bij de values horen. De code is weinig elegant, maar als we straks de grafische versie gaan maken, is deze omslachtigheid niet meer nodig; voor nu accepteren we het.

Run de code om te zien of het spel naar behoren werkt. Probeer ook gerust zelf wat dingen uit. Wellicht kun je ervoor zorgen dat de foutmelding in regel 42 of 44 wordt geactiveerd?

.. dropdown:: Opdracht 01
   :open:
   :color: secondary
   :icon: pencil

   In dit deel hebben we ervoor gezorgd dat de recepten uit de string :python:`recipes_txt` worden gelezen en in de dictionary :python:`recipes` worden opgeslagen. Dat gebeurt in de helper functie :python:`build_recipes()`. In deze opdracht ga je :python:`build_recipes()` nog iets geavanceerder maken. 

   Wijzig de string :python:`recipes_txt` als volgt:

   .. code-block:: python
      :linenos:
      :lineno-start: 27

      recipes_txt = '''water
      fire
      wind
      earth
      -
      water+fire=steam
      water+wind=wave
      water+earth=plant
      fire+wind=smoke
      fire+earth=lava
      wind+earth=dust'''

   De string bestaat nu uit twee delen: de eerste vier regels bevatten de elementen die bij aanvang van het spel beschikbaar zijn voor de speler, daarna volgt een scheidingsteken :python:`-`, en het tweede deel bevat de recepten.

   Breid :python:`build_recipes()` uit met de volgende functionaliteit:

   * De string :python:`recipes_txt` wordt opgesplitst in de twee delen :python:`first_part` en :python:`second_part`. Maak hiervoor gebruik van het scheidingsteken :python:`-`, maar je zult merken dat je hier nog iets aan moet toevoegen om de splitsing goed te krijgen.
   * De string :python:`first_part`, die de vier basiselementen bevat, wordt opgesplitst in regels, en het resultaat opgeslagen in de variabele :python:`primes`.
   * De string :python:`second_part`, die de recepten bevat, wordt opgesplitst in regels, en het resultaat opgeslagen in de variabele :python:`combinations`.
   * Met een for loop wordt voor elke :python:`prime` in :python:`primes` eerst gecheckt of die als key voorkomt in :python:`elements`. Als dat niet zo is, wordt een foutmelding gegenereerd. In het andere geval wordt de :python:`prime` toegevoegd aan :python:`discoveries` met behulp van de functie :python:`add_discovery()`.
   * De rest van de code in :python:`build_recipes()` blijft hetzelfde, behalve dat de string :python:`lines` wordt vervangen door :python:`combinations`.

   Als je dit hebt gedaan, kun je de vier aanroepen van :python:`add_discovery()` in het hoofdprogramma verwijderen. De vier elementen worden nu automatisch toegevoegd aan :python:`discoveries` in de helper functie :python:`build_recipes()`. Run het programma en kijk of alles nog steeds werkt.

   
   .. dropdown:: Oplossing
      :color: secondary
      :icon: check-circle

      .. code-block:: python
         :linenos:
         :lineno-start: 41

         def build_recipes():
            first_part, second_part = recipes_txt.split('\n-\n')
            primes = first_part.split('\n')
            combinations = second_part.split('\n')
            for prime in primes:
               if prime not in elements:
                     raise Exception('Recipe error: unknown prime element.')
               add_discovery(prime)
            for combination in combinations:
               left, right = combination.split('=')
               ingredients = left.split('+')
               if (len(ingredients) != 2):
                     raise Exception('Recipe error: number of ingredients must be exactly 2.')
               if ingredients[0] not in elements or ingredients[1] not in elements:
                     raise Exception('Recipe error: unknown ingredients.')
               ingredients.sort()
               if ingredients[0] not in recipes: 
                     recipes[ingredients[0]] = {ingredients[1]: right}
               else:
                     recipes[ingredients[0]][ingredients[1]] = right