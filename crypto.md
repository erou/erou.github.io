---
title: Cryptographie
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

### Méthode "hash and sign"

Avant de signer le message, on va le passer dans une fonction de
hachage, cela rendra le protocole plus sûr.

$M$ --> $h$ --> RSA --> $S$.

*Définition :* une fonction $h:\\{0,1\\}^*\rightarrow\\{0,1\\}^l$ est appelée **fonction de hachage** si et seulement si elle vérifie les trois propriétés suivantes :

1. $h$ est à sens unique (*one way*) : $\forall y \in \\{0,1\\}^l$, il
   est calculatoirement difficile de trouver un antécédent
   $x\in\\{0,1\\}^*$ tel que $h(x)=y$ .
2. $h$ est à collision faibles difficiles (*second preimage resistant*)
   : étant donné $x\in\\{0,1\\}^* $ et $y=h(x)$, il est calculatoirement
difficile de trouver $x'\neq x$ tel que $h(x')=y$. 
3. $h$ est à collisions fortes difficiles (*collision resistant*) : il
   est calculatoirement difficile de trouver $x,x'\in\\{0,1\\}^*$ avec
   $x\neq x'$ et $h(x)=h(x')$.

La fonction $h$ n'est pas injective mais la propriété 3 dit qu'il est en
pratique difficile de trouver un exemple de non-injectivité. On a
$3\Rightarrow 2 \Rightarrow 1$.

**Algo 1 :** Tirer $x$ aléatoirement jusqu'à ce que $h(x)=y$. Proba de
succès $\approx 1/2^l$. Complexité en moyenne $O(2^l)$.

**Algo 2 :** Tirer des $x'\neq x$ aléatoires jusqu'à ce que $h(x')=y$.
Complexité en moyenne $O(2^l)$.

**Algo 3 :** Générer des messages $x_1,\dots,x_n$ (tous distincts) et
les hachés $y_i=h(x_i)$ jusqu'à trouver $i\neq j$ tel que $y_i=y_j$.

Appelons $P$ la probabilité qu'une collision apparaisse. On va plutôt
calculer $1-P$. Notons $p_i$ la probabilité que le $i$-ième message
provoque une collision. On a $p_1=1$, $p_2=1-1/2^l$, $p_n=1-(n-1)/2^l$.
On a ainsi $1-P=p_1\times\dots\times p_n$. On a $\ln(1-u)\sim -u$, et in
fine $\ln(1-P)\approx -1/2^l-2/2^l-\dots-(n-1)/2^l)$. D'où
$\ln(1-P)\approx -n(n-1)/2^{l+1}$ et $1-P\approx \exp(-n(n-1)/2^{l+1})$.

On en déduit $P = 1-\exp(-n(n-1)/2^{l+1}) $. En inversant la formule, on
a également $n\approx 2^{l/2}\sqrt{2\ln(\frac{1}{1-P})}$. Il y a un
violent changement de situation aux alentours de $2^{l/2}$, c'est ce
qu'on appelle le *paradoxe des anniversaires*. 
