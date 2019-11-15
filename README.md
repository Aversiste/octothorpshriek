# OctothorpShriek

OctothorpShriek est un jeu de programmation en shell qui sera sans doute un jour disponible sur www.octothorpshriek.org.
En attendant il est toujours possible d'y jouer via Github et de me soumettre manuellement le résultat.

Pour participer nul besoin d'ordinateur de compétition ou de connaissance poussée en programmation: juste d'un environnement UNIX classique et d'un shell, respectant la norme POSIX de préférence.

Le but du jeu est d'implémenter quatres défis simulant du matériel ancien, en shell, afin d'obtenir le code secret de victoire.
Les instructions et des jeux de données de tests sont présentes dans des sous-répertoires dédiés:

- [cardreader](cardreader/cardreader.md) ;
- [dot-matrix](dot-matrix/dot-matrix.md) ;
- [antenna](antenna/antenna.md) ;
- [decoder](decoder/decoder.md).

## Scénario

Du matériel d'espionnage complètement obsolète provenant d'un stock de la guerre froide a fini par être déclassifié et mis à la disposition du public.

Étrangement personne n'a cherché à cracker le code secret transmis par radio à l'aide de la clef récupérée sur les cartes perforées.

## Buts

Chaque défi implémenté en respectant les interfaces demandées rapporte 10 points et la victoire est atteinte lorsque le code secret peut être décodé de la façon suivante:

    $ ./antenna -f data/radio.txt
    $ cyphertext=...
    $ ./decoder -m "$cyphertext" "$(./cardreader -f data/key.card)" | ./dot-matrix -f data/default.font

Le fichier `key.card` contient la clef utilisée pour chiffrer le message secret transmis par radio.
Le fichier `radio.txt` comprend un dump du protocole radio utilisé pour transmettre le message secret.

## Points bonus

Dix points bonus sont attribués pour les efforts suplémentaires:

- ne pas utiliser de bashism ou kshism (pas de tableau, pas d'extension non POSIX).
- portabilité vers les autres UNIX que Linux.

## Conseils

Ne pas se jeter sur StackOverflow, les [pages de man](http://man.openbsd.com/) sont plus utiles et mieux écrites.

## FAQ

### Comment est-ce que l'on peut lire un fichier ligne par ligne ?

    $ while read line; do ... ; done < "$file"

### Comment est-ce qu'on découpe une ligne en fragments ?

    $ man [cut](http://man.openbsd.com/cut)

### Comment est-ce que l'on peut convertir d'une base vers une autre ?

    $ man [bc](http://man.openbsd.com/bc)

### Comment est-ce que l'on peut supprimer des caractères inutiles ?

    $ man [tr](http://man.openbsd.com/tr)	# Plus simple
    $ man [sed](http://man.openbsd.com/sed)	# Plus difficile
