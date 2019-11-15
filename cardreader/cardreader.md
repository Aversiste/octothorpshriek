# Cardreader

Cette partie du défi consiste à créer un lecteur de carte perforée virtuel.

Le programme doit lire des cartes perforées et en afficher la valeur sur la sortie standard.

## Synopsis

    cardreader [-f card-file]

L'option -f permet de fournir un fichier contenant les cartes perforées.
Si cette option n'est pas fournie alors _cardreader_ lira sur l'entrée standard.

Toute autre option est une erreur et doit provoquer l'affichage d'une aide sous la forme suivante:

    usage: cardreader [-f card-file]

Exemple:

    $ ./cardreader -k
    cardreader: unkown option -- k
    usage: cardreader [-f card-file]

## Cartes perforées

Les cartes perforées de notre machine sont de longs rubans de papier percés de rangées de trous circulaires.
Ces derniers, aux nombres maximum de sept, permettent d'encoder des valeurs sur 7-bits.
Une huitième colonne, laissée vide et inusitée, est là pour une évolution future de l'appareil.
Un trou suplémentaire, plus petit, est situé entre le troisième et quatrième trou en partant de la droite: il s'agit du guide d'entrainement du mécanisme de lecture.
Le bit le plus faible est par ailleurs situé tout à droite.

Chaque rangée est donc positionnée ainsi:

    | 7654.321|
     ^    ^
     |    +---- Trou de guidage
     +--------- Colonne vide

Exemples:

    
    |    .  o| # le nombre un
    |    . o | # le nombre deux
    |    . oo| # le nombre trois
    | oo .  o| # le nombre 97 ou la lettre « a » dans la table ASCII

Chaque bande commence et termine par une série de tirets.

Exemple:

    ----------
    |  oo. o |
    |  oo.o o|
    |  oo. o |
    ----------

## Fichiers de test

Vous disposez de cinq fichiers de test :

- card-1
- card-2
- card-3
- card-4
- card-5

Ils contiennent respectivement le contenu suivant:

- J'aime les frites
- lol
- 43889288
- DMARC permits Identifier Alignment, based on the result of a DKIM authentication, to be strict or relaxed.  (Note that these are not related to DKIM's simple and relaxed canonicalization modes.)
- OIyAOIyAIAOIy@AAOIyAAAAAIA
