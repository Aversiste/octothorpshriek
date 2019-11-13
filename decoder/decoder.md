# Decoder

Cette partie du défi consiste à implémenter l'algorithme de décodage du message secret.

Le programme doit prendre en paramètre une clef de déchiffrement et lire un message à déchiffrer.

## Synopsis

    decoder [-m message] key

L'option -m permet de fournir le message à déchiffrer.
Si cette option n'est pas fournie alors _decoder_ lira sur l'entrée standard.

Toute autre option est une erreur et doit provoquer l'affichage d'une aide sous la forme suivante:

    usage: decoder [-m message] key

Exemple:

    $ ./decoder -k test
    decoder: unkown option -- k
    usage: decoder [-m message] key

## Algorithme

Le chiffrement du message secret a été réalisé grâce à un masque jetable, aussi nommé chiffre de Vernam.
La clef et le message secret font la même longueur et ont été récupéré par le biais de l'antenne et du lecteur de carte perforée.
Il n'y a plus qu'à décoder le tout !

L'algorithme est simple: à partir du message chiffré « sBB?ELJXZ » et de la clef « 123456789 » on pratique une soustracation des valeurs ASCII de chaque caractère un à un, à laquelle on applique ensuite une opération de vérification d'_overflow_ et d'_underflow_ sur les valeurs 126 et 32.

Exemple:

    sBB?ELJXZ:   115  66  66  63  69  76  74 88 90
    123456789:    49  50  51  52  53  54  55 56 57
    soustraction: 66  16  15  11  16  22  19 32 33
    overflow:     66 111 110 106 111 117 114 32 33
    message:       B   o   n   j   o   u   r     !
