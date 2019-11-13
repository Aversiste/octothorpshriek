# Shell hands on

Liste des défis:

- [cardreader](cardreader/cardreader.md) ;
- [dot-matrix](dot-matrix/dot-matrix.md) ;
- [antenna](antenna/antenna.md) ;
- [decoder](decoder/decoder.md).

But: implémenter les quatres défis et être la première équipe à trouver le code secret.

Les scripts sont à déposer dans le répertoire `result`, avoir des droits d'exécution et le bon nom.

## Victoire

Le fichier `key.card` contient la clef utilisée pour chiffrer le message secret transmis par radio.
Le fichier `radio.txt` comprend un dump du protocole radio utilisé pour transmettre le message secret.

Avec l'ensemble des outils il est possible de récupérer le message d'origine de la façon suivante:

    $ cyphertext=...
    $ ./decoder -m "$cyphertext" "$(./cardreader -f key.card)" | ./dot-matrix

## Bonus

Ne pas utiliser bash.
