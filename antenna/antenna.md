# Antenna

Cette partie du défi consiste à créer une antenne virtuelle qui va analyser un flux radio contenant quatre communications distinctes.
Il faudra valider les paquets radios composant ces communications afin de déterminer laquelle afficher sur la sortie standard.

## Synopsis

    antenna [-f radio-file]

L'option -f permet de fournir un fichier de signal radio.
Si cette option n'est pas fournie alors *antenna* lira sur l'entrée standard.

Toute autre option est une erreur et doit provoquer l'affichage d'une aide sous la forme suivante:

    usage: antenne [-f radio-file]

Exemple:

    $ ./antenna -l
    antenna: unkown option -- l
    usage: antenna [-f radio-file]

## Signal radio

Le signal radio se présente sous la forme d'un fichier représentant une transmission radio complète comprenant quatre canaux de communication.
Ces canaux représentent quatre communications séparées.

Une seule de ces communications est valide et devra être imprimée sur la sortie standard.

Chaque ligne du fichier représente un paquet radio, respectant le format suivant:

    numéro_de_canal niveau numéro_de_paquet sommes_de_contrôle paquet

* Le numéro de canal est constitué par un nombre de 1 à 4 préfixé par un point « . ».
* Le niveau est un nombre signé allant de -31 à 31 indiquant le volume en décibel de la communication.
* Le numéro de paquet permet d'ordonner la retransmission du flux car ils peuvent arriver dans un ordre différent de celui de la transmission initiale.
* La somme de contrôle permet de valider l'intégrité d'un paquet.
* Le paquet lui même est une série de vingt caractères de la table ASCII comprenant les nombres, les lettres et la ponctuation, soit les caractères allant de l'indice 32 (l'espace) à 126 (le tilde).

Exemple:

    .2 0 1 1452 Hello sailor.       
    .1 0 1 1676 J'aime  les  frites.
    .3 0 1 1929 Lorem ipsum dolor si
    .3 0 4 1865  do eiusmod tempor i
    .3 0 3 1905 adipiscing elit, sed
    .3 0 5 1920 ncididunt ut labore 
    .3 0 6 1897 et dolore magna aliq
    .3 0 2 1878 t amet, consectetur 
    .3 0 7 804 ua.                 
    .4 0 1 1049 3,141592653589793238
    .1 -15 2 1946 ;wRZ^Vpp]VdppWcZeVd~

### La somme de contrôle

Ce protocole radio rudimentaire permet une vérification tout aussi rudimentaire de l'intégrité des données.
Pour la calculer il faut d'abord convertir chaque caractère en son numéro dans la table ASCII puis de les additionner ensemble.

Exemple:

    .2 0 1 1452 Hello sailor.       

Ici les vingt caractères formant "Hello sailor.       " sont convertis en nombre et additionnés: 72 + 101 + 108 + 108 + 111 + 32 + 115 + 97 + 105 + 108 + 111 + 114 + 46 + 32 + 32 + 32 + 32 + 32 + 32 + 32 = 1452.

Si la somme de contrôle est invalide alors le flux radio est corrompu et ce paquet doit être rejeté.
Si un paquet est rejeté alors le canal entier est considéré comme corrompu.

### Le niveau

*Cette partie est totalement optionnelle.*

Le niveau de chaque paquet indique la puissance du signal d'émission.
Dans des conditions optimales la puissance sera de zéro, le paquet radio a donc été transmis normalement et n'a pas besoin de traitement.
Malheureusement il arrive que des perturbations atmosphériques ou la présence d'obstacle sur le chemin du signal altèrent la transmission en la diminuant ou en l'amplifiant.
Dans ces cas là le niveau sera représenté par un nombre négatif ou positif et il sera nécessaire de normaliser le paquet reçu en augmentant ou diminuant le signal, jusqu'à retrouver la puissance normale de zéro.

Si le niveau est négatif, alors on l'augmente et au contraire s'il est positif on le diminue.

Pour cela il faut convertir chaque caractère du paquet en son numéro dans la table ASCII, lui additionner ou soustraire le niveau puis le reconvertir dans la table ASCII.
Les bornes de ce calcul se font sur le caractère espace et le caractère tilde, soit les nombres 32 et 126.
Si la somme obtenue est inférieur à 32 ou supérieur à 126 il est nécessaire de simuler l'*underflow* et l'*overflow*.

Exemple:

    .1 -15 2 1946 ;wRZ^Vpp]VdppWcZeVd~

Ici la puissance du signal est de -15, il est donc nécessaire d'ajuster les caractères en cherchant pour chacun d'eux l'élément 15 entrées plus loin dans la table ASCII.

Le caractère point virgule « ; » correspond au numéro 59.
En lui additionnant quinze nous obtenons la lettre « J ».

Le caractère « w » est un peu plus complexe à amplifier car il correspond au numéro 119.
Additionné de 15 il devient 134, ce qui est supérieur à la borne des caractères autorisés dans les paquets radio, à savoir 126.
Pour trouver le bon caractère il faut compter à partir de 32 la différence entre la somme trouvée et la borne supérieur.

Soit: 32 + 134 - 126 - 1 = 39.

Le caractère obtenu est l'apostrophe « ' ».

## Fichiers de test

Vous disposez des fichiers de test suivant :

- radio-1
- radio-2
- radio-3

En les interprétants _antenna_ doit afficher les lignes suivantes, respectivement:

- ABCDE
- Ne toques mi, je poins
- Ornythorynque ! Boit-sans-soif ! Bachi-bouzouk ! Anthropophage ! Jocrisse !
