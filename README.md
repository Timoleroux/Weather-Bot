# Weather Bot

---


## Introduction
Bonjour ! <br>
Nous sommes une **équipe de trois lycéens** ayant un **projet d'école** à sujet libre.
Nous avons donc choisis de créer un **bot twitter** qui **publie la météo** d'une ville spécifique.
Le bot peut également **changer sa photo de profil selon la météo**.

Voici le compte du bot : [@HDedeux](https://twitter.com/HDedeux)

## C'est quoi le but ?

Weather Bot est un **bot codé entièrement en python** qui permet de **poster des tweets** annonçant la **météo** de n'importe quelle ville. On peut également programmer un **emploi du temps** où chaque jour, un tweet se postera aux heures prédéfinies.

---

## Prérequis pour lancer le script

**Attention :** *le script doit être exécuté avec la **version 3.10** de [**Python**](https://www.python.org/downloads/) au minimum.*</span>

Pour traiter les données de Twitter, c'est le module [**Tweepy**](https://www.tweepy.org/) qui s'en charge. Pour y avoir accès, il faut installer le module en exécutant la commande :
    
    pip install tweepy

Il est également nécessaire de posséder les tokens de Tweepy. Pour cela, il faut se rendre sur l'[espace développeur](https://developer.twitter.com/en/portal/petition/essential/basic-info) de twitter et créer un projet pour récupérer les tokens nécessaires et les ajouter dans le fichier [ressources.py](https://github.com/Timoleroux/Weather-Bot/blob/main/ressources.py).

Pour traiter les données météorologiques, c'est le module [**Pyowm**](https://pypi.org/project/pyowm/) qui s'en charge. Pour y avoir accès, il faut installer le module en exécutant  la commande :

    pip install pyowm

Il est également nécessaire de posséder le token de Pyowm. Pour cela, il faut se créer un compte sur [OWM](https://home.openweathermap.org/users/sign_up) puis, une fois connecté, se rendre [ici](https://home.openweathermap.org/api_keys) où vous trouverez le token nécessaire que vous pourrez ajouter au fichier [ressources.py](https://github.com/Timoleroux/Weather-Bot/blob/main/ressources.py). </br>
**Attention :** nous ne connaissons pas l'emplacement des stations météo grâce auxquelles cette API va chercher les informations. Par conséquent, il est probable qu'il y ait des légères différences avec les données fournies par des sites de météo officiels.</br>
Si les différences de données sont trop importantes, il est possible que la ville que vous avez entrée ait été confondue avec un autre endroit portant le même nom *(Ex : la ville de Brest existe en France et en Biélorussie)*.

---
## Documentation des fonctions

### ressources.py/AuthTweepy

    AuthTweepy()

Cette fonction permet d'**authentifier la connexion à Tweepy** et donc d'utiliser cette API. Voir la [documentation](https://docs.tweepy.org/en/stable/).

### ressources.py/AuthPyowm

    AuthPyowm()

Cette fonction permet d'**authentifier la connexion à Pyowm** et donc d'utiliser cette API. Voir la [documentation](https://pyowm.readthedocs.io/en/latest/).

### ressources.py/currentTime

    currentTime()
    Input : '%d/%m/%Y %H:%M:%S'
    Output : 05/11/2022 19:00:00

Cette fonction prend en entrée une string qui définit le format de l'heure qui sera retournée en sortie de la forme choisie.
| Commande |  Français  |  Anglais  |
|:--------:|:----------:|:---------:|
|   `%Y`   |    Année   |   Year    |
|   `%m`   |    Mois    |   Month   |
|   `%d`   |    Jour    |   Day     |
|   `%H`   |    Heure   |   Hour    |
|   `%M`   |    Minute  |   Minute  |
|   `%S`   |    Seconde |   Second  |

### app.py/allWeatherInfos

    allWeatherInfos()
    Input : 'Paris'
    Output : ['Paris', '05/11/2022', '19h00', 'nuageux', 10, 9, 16.67]

Cette fonction prend en entrée un **nom de ville** sous forme de **string**.
Grâce à Pyowm, elle retourne une liste contenant les informations suivantes :
1. La **ville** qui à été entrée *(string)*
2. La **date du jour** sous la forme : JJ/MM/AAAA *(string)*
3. L'**heure du jour** sous la forme : 23h59 *(string)*
4. La **météo** *(string)*
5. La **température** de la ville *(int)*
6. Le **ressenti** de cette température *(int)*
7. La **vitesse du vent** dans cette ville *(float)*

### app.py/updateProfilPicture

    updateProfilPicture()
    Input : 'nuageux'
    Output : True

Cette fonction prend en entrée le 4<sup>ème</sup> élément de la liste précédente (la météo). </br>
Si l'entrée fait partie des cas cités dans la fonction, alors elle change la photo de profil du bot et retourne `True`. </br>
Si l'entrée ne fait pas partie des cas cités dans la fonction, alors elle affiche dans la console « Aucune photo de profil correspond à {météo} pour le moment. » et retourne `False`.

### app.py/publishTweet

    publishTweet()
    Input : ['Paris', '05/11/2022', '19h00', 'nuageux', 10, 9, 16.67]
    Output : Voici la météo pour Paris, le 05/11/2022 à 19h00:
                ☀️ Temps : nuageux
                🌡️ Température moyenne : 10°C (ressenti 9°C)
                💨 Vitesse du vent : 16.67 km/h

Cette fonction prend en entrée la liste de données que retourne la fonction [`allWeatherInfos()`](#mainpyallweatherinfos). </br>
Elle publie un tweet grâce à la liste qui lui a été donnée et si l'opération réussit, elle retourne `True`.

### app.py/writeInLog

    writeInLog()
    Input : 'Ceci est un message enregistré dans LOG.txt'
    Output : 'Ceci est un message enregistré dans LOG.txt'

Cette fonction prend en entrée une string qui sera enregistré dans le fichier LOG.txt.
Si le fichier LOG.txt n'éxiste pas, la fonction le crée.
Elle retourne le même message d'entrée.

### app.py/manualRun

    manualRun()
    Input : None
    Output : True

Cette fonction ne prend rien en entrée. </br>
Elle demande à l'utilisateur de choisir une ville puis publie le Tweet correspondant et change la photo de profil selon la météo grâce à la fonctions [`updateProfilPicture()`](#mainpyupdateprofilpicture). </br>
Elle affiches le statut tu Tweet (en cours de publication, publié) dans la console et l'enregistre dans le ficher LOG.txt</br>
Si l'opération réussit elle retourne `True`.

### app.py/autoRun

    autoRun()
    Input : ['08h00', '12h00', '20h30']
    Output : None

Cette fonction prend en entrée une liste contenant des horaires sous la forme de **string**. </br>
Elle demande à l'utilisateur de choisir une ville.
Grâce aux fonctions précédentes, pour chacun des horaires de la liste, elle publie ensuite le tweet contenant la météo correspondant à la ville choisie et elle affiche la date de publication du tweet dans la console d'exécution. </br>
Elle affiches le statut tu Tweet (en cours de publication, publié) dans la console et l'enregistre dans le ficher LOG.txt</br>
Elle ne retourne rien.

---
## Documentation des erreurs communes

La plupart du temps, l'erreur suivante est causée par les tokens écrits dans [ressources.py](https://github.com/Timoleroux/Weather-Bot/blob/main/ressources.py) qui sont incorrects.

    tweepy.errors.Forbidden: 403 Forbidden
    187 - Status is a duplicate.

Si vous obtenez l'erreur suivante, c'est que le Tweet que vous essayez de publier existe déjà.

    tweepy.errors.Unauthorized: 401 Unauthorized
    32 - Could not authenticate you.

La plupart du temps, l'erreur suivante est causée car le nom de la ville n'est pas valide.

    pyowm.commons.exceptions.NotFoundError: Unable to find the resource

Si vous obtenez l'erreur suivante, c'est parce que vous avez exécuté ce script avec une version inférieure à la la **version 3.10** de [**Python**](https://www.python.org/downloads/).

    File "consoleApp.py", line 21
    match weather:
          ^
    SyntaxError: invalid syntax
