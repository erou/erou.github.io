---
title: Cryptographie
layout: default
permalink: /teaching/crypto-l3/
visible: false
---

Je donne des Travaux Dirigés (TD) de cryptographie à l'université de Versailles en licence 3
d'informatique. Le cours est assuré par [Christina Boura](http://christina-boura.info/en). Je mettrai les feuilles d'exercices sur cette page au fur et à
mesure de l'avancement de l'année. Si vous avez une question, vous pouvez me contacter par
[e-mail](mailto:{{site.email}}).

1. [Chiffrements historiques](td1.pdf)
2. [ENIGMA et chiffrement à flot](td2.pdf)
3. [One-Time-Pad et LFSR](td3.pdf)
4. [Chiffrement par bloc et schémas de Feistel](td4.pdf)
5. [DES et modes opératoires](td5.pdf)
6. [Arithmétique modulaire](td6.pdf)
7. [RSA](td7.pdf)
8. [Tests de primalité et cryptographie à clé publique](td8.pdf)

# Une conjecture issue du TD sur les LFSR

Un étudiant a formulé la conjecture suivante

**Conjecture**

*Un LFSR ayant des coefficents de rétroaction tous nuls sauf les deux premiers
est de période maximale.*

et m'a demandé si ce résultat était vrai. En effet, on rencontre des LFSR de
longueur 3 et 4 dans le TD qui possèdent ces propriétés. Comme j'avais assez peu
confiance en ce résultat j'ai utilisé [Sage](http://www.sagemath.org/) pour
enquêter. Il s'avère que c'est faux dès la longueur 5. Je n'ai pas eu à coder
grand chose car il y avait déjà une fonction pour générer des LFSR dans Sage.
