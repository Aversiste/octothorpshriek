# Dot-matrix

Cette partie du défi consiste à créer un simulateur d'écran de type *dot-matrix* permettant d'afficher une ligne de treize caractères.

## Synopsis

    dot-matrix [-c char] [-f font-file] [-m message]

L'option -c permet de choisir le caractère utilisé pour simuler l'allumage d'une LED sur l'écran *dot-matrix*.
Si cette option n'est pas fournie alors le caractère « # » sera utilisé.
Dans tous les cas le caractère « . » est utilisé pour simuler une LED éteinte.

L'option -f permet de fournir un fichier décrivant une police d'écriture utilisée pour afficher le message.
Si cette option n'est pas fournie alors le programme cherchera le fichier `default.font` dans le répertoire courant.
Le format de ce fichier est décrit dans la section § Police d'écriture.

Si l'option -m n'est pas fournie alors le programme écoutera sur l'entrée standard pour obtenir son message à afficher.

Toute autre option est une erreur et doit provoquer l'affichage d'une aide sous la forme suivante:

    usage: dot-matrix [-c char] [-f font-file] [-m message]

Exemple:

    $ echo '}' | ./dot-matrix -g
    dot-matrix: unkown option -- g
    usage: dot-matrix [-c char] [-f font] [-m message]

## Police d'écriture

Les fichiers de police d'écriture contiennent des lignes décrivant chaque caractères du code ASCII 32 au code 126.
Ces lignes peuvent être séparée par un nombre arbitraire de lignes vides ou de lignes démarrant par un commentaire (représenté par un « # »).
Il est également possible que des commentaires se trouvent en fin de ligne, après les descriptions de code.
Tout ce qui se trouve à droite d'un commentaire est à ignorer.

Les lignes de code, au nombre de quatre vingt seize exactement, décrivent chacune un caractère de la table ASCII compris entre 32 et 126, soit du caractère espace jusqu'au caractère tilde (« ~ ») plus une ligne finale indiquant un caractère vide et servant de délimiteur de fin.

Chaque ligne de code comprend six nombres séparés par des virgules et un nombre arbitraire d'espaces.
Chacun de ces nombres est un entier positif codé sur huit bits.

Exemple:

La ligne suivante décrit le caractère « A »:

    62, 9, 9, 9, 9, 62 # A

Converti en binaire:

    00111110, 00001001, 00001001, 00001001, 00001001, 00111110

Puis disposé sous forme de grille 8x6 avec les bits de poids faible vers le haut:

    011110
    100001
    100001
    111111
    100001
    100001
    000000
    000000

Rendu:

    .####.
    #....#
    #....#
    ######
    #....#
    #....#
    ......
    ......

La ligne suivante décrit le caractère « } »:

    68, 124, 16, 0, 0, 0

Converti en binaire:

    01000100, 01111100, 00010000, 00000000, 00000000, 00000000

Puis disposé sous forme de grille 8x6:

    000000
    000000
    110000
    010000
    011000
    010000
    110000
    000000

Rendu:

    ......
    ......
    ##....
    .#....
    .##...
    .#....
    ##....
    ......
