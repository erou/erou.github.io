---
layout: default
permalink: /cours/
---

## Cours de monsieur GOUBIN

Cette page a pour but d'énerver
[Simon](https://fr.linkedin.com/in/simon-masson-74837889). Et aussi pour voir si c'est
faisable en pratique de prendre son cours sur le vif et sur le site
internet.

## Pivot de Gauss

On peut améliorer le pivot de Gauss en améliorant les algoritmes de
multiplication matricielle. On peut utiliser [l'algorithme de
Strassen](https://fr.wikipedia.org/wiki/Algorithme_de_Strassen) ou
[l'algorithme de
Coppersmith](https://en.wikipedia.org/wiki/Coppersmith%E2%80%93Winograd_algorithm).

## Logarithme discret

### Complexité

|**LOG** | Instance :| $p,g,y,t$, et où $g$ générateur de $\mathbb{F}_p^*$.|
||Question :| $ y = g^x \mod p $ a-t-il une solution $x\leq t$ ?|

Supposons qu'on dispose d'un algo $\mathfrak A$ pour ce pb de décision.
On fait par dichotomie comme pour la factorisation.

### Méthode du calcul d'indice

*But :* calculer le logarithme de $y$ en base $g$ modulo $p$ *i.e.$
trouver $x$ tel que $g^x = y \mod p$.

On pose $B = \{-1,p_1,\dots,p_h\}$ et on a comme objectif de trouver les
logarithmes discrets de $p_j$ pour $1\leq j \leq h$. 
