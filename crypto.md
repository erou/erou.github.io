---
layout: default
permalink:/crypto
---

Voici mes notes de cours de cryptographie, il contient probablement des
erreurs.

## Utilisation du RSA

### Signatures

Il y a un problème de sécurité, si on a deux signatures $S_1=M_1^d\mod
n$ et $S_2=M_2^d$ on peut fabriquer des signatures pour $M'=M_1M_2\mod
n$, et pour tous les produits formés de cette façon. Même avec un seul
message, on peut signer tous les produits dudit message.

C'est même plus grave, on peut fabriquer des couples valides $(S^e,S)$
pour n'importe quel $S$. Le message n'aura pas beaucoup de sens mais on
considère quand même que l'algorithme de signature n'est pas bon.
*Quelle est la bonne définition de la sécutité d'une signature ?*

### Sécurité d'un algorithme de signature

**Objectif de l'attaquant :**

1. "Forger" une signature, c'est-à-dire construire un couple $(M,S)$ où
   $S$ est une signature valide de $M$.
	* forge existentielle : on y parvient pour au moins un message
	  non choisi
	* forge sélective : l'attaquant peut choisir un ou plusieurs
	  messages et obtenir leurs signatures
	* forge universelle : quel que soit $M$, l'attaquant est capable
	  de trouver une signature valide $S$ de ce message $M$
2. Trouver la clé secrète. 

**Informations dont dispose l'attaquant :**

* clé publique "no message attack"
* attaque à *messages connus* : l'attaquant connaît des couples
  $(M_i,S_i)$
* attaque à *messages choisis* : l'attaquant peut choisir des messages
  $M_i$, et obtenir leurs signatures $S_i$

*Définition :* On dit qu'un algorithme de signature est **sûr** si et
seulement si pour un attaquant qui choisit $M_1,\dots,M_n$ et obtient
leurs signatures $S_1,\dots,S_n$, il doit être *calculatoirement
difficile* de construire $(M,S)$ avec $M\notin \\{M_1,\dots,M_n\\}$ et
$S$ une signature valide de $M$.

La méthode $S=M^d\mod n$ n'est pas sûre.
