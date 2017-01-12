---
title: Cryptographie
layout: default
permalink: /crypto/
visible: false
---

Voici mes notes de cours de cryptographie, elles contiennent probablement des erreurs.

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
2. $h$ est à collision faibles difficiles (*second preimage resistant*) : étant donné $x\in\\{0,1\\}^* $ et $y=h(x)$, il est calculatoirement
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

Comment fait-on pour trouver deux valeurs identiques dans un tableau ?
On trie en $n\ln(n)$, puis les deux valeurs identiques sont côte à côte
donc cela devient facile.

**Quelques fonctions de hachage :**

Nom | $\ell$ | Anniversaires | Meilleure attaque connue
MD4 (1990, Rivest) | $128$ | $2^{64}$ | 1995, Dobberton --> collision
MD5 (1991, Rivest) | $128$ | $2^{64}$ | 2005, Wang --> collision en $2^{20}$
SHA (1993, NIST) | $160$ | $2^{80}$ | 2004, Joux --> collision en $2^{51}$
SHA-1 (1994, NIST) | $160$ | $2^{80}$ |
SHA-256 (NIST, 2002) | $256$ | $2^{128}$ | 2005-2006, Wang --> collision en $2^{69}$
SHA-384 (NIST, 2002) | $384$ | $2^{192}$ | 
SHA-512 (NIST, 2002) | $512$ | $2^{256}$ | 
SHA-3 | $224$ | $2^{112}$ |

Les algorithmes SHA-256, SHA-384, SHA-512 sont de la même famille,
appelée famille SHA-2. KECCAK = SHA-3 est un algorithme qui a gagné un
concours organisé par le NIST en 2008. Des versions en $\ell =
256,384,512$ (et avec des attaques via le paradoxe des anniversaires
correspondantes) de SHA-3 existent.

On signe donc en faisant $S=h(M)^d\mod n$. $h(M)$ fait par exemple 256
bits et donc tous les messages, quelles que soient leurs tailles,
peuvent être signé par RSA. $h$ n'est pas multiplicative, en général
$h(M\times M')\neq h(M)h(M')$, ainsi toutes les attaques dont on avait
parlé qui se basaient sur la multiplicativité de RSA ne fonctionnent
plus.

### 2. Chiffrement

On a envie de dire que le chiffré est simplement $C=M^e\mod n$.

#### a. Attaque sur les petits messages

Si $M<\sqrt[e] n$ alors $C=M^e\mod n=M^e$. Donc $M=\sqrt[e] C$.

#### b. Attaque de Hastad

On prend $e=3$, on suppose qu'Alice envoit le même message à plusieurs
déstinataires. On note $y_i=x^3\mod n_i$ et $n_i=p_iq_i$ avec $0\leq
x<n_i$. On a alors $0\leq x^3<n_1n_2n_3$. Mais comme on connaît $x^3$
modulo $n_1n_2n_3$, on peut retrouver $x$ en faisant une simple racine
cubique. Ensuite d'après les restes chinois on peut retrouver $x$.

De manière générale on peut retrouver $x$ si le message est envoyé à $e$
personnes différents.

#### c. Déterminisme

Un autre défaut encore plus intrinsèque est que le protocol est
déterministe, cela veut dire qu'un même message aura toujours le même
chiffré, et donc on peut avoir des problèmes si on envoie souvent des
messages identiques. On introduit donc de l'aléat dans les protocols.

Dans les systèmes comme AES, on fait du CBC *Cypher Block Chaining* : on
XOR le fin du block $i$ avec l'entrée du block $i+1$ et on XOR également
la toute première entrée avec une IV *initial value* aléatoire. Si
jamais le message ne fait pas un nombre de bit multiple de 128 (par
exemple) on peut ajouter que des zéros jusqu'à ce que le nombre de bit
soit bon. Et si le message complet est déjà bon, on ajoute un nouveau
bloc avec un 1 et que des zéros. Comme ça quand on déchiffre on peut
bien retomber sur le message. De manière générale il faut faire
attention à ce que le déchiffrement se passe correctement.

#### d. Théorème de [Coppersmith](https://en.wikipedia.org/wiki/Don_Coppersmith)

**Théorème (Coppersmith, 1997)**

Soit $N$ un entier, $\varepsilon>0$, $f\in\mathbb Z[X]$ un polynôme de degré $d$ et
$B=N^{1/d-\varepsilon}$. Alors, étant donné $f$ et $N$, on peut trouver
en temps polynômial tous les entiers $x_0$ tels que $|x_0|<B$ et
$f(x_0)=0\mod N$.

*Remarque :* Dans le cas $F(X)=X^e-y$, cela redonne l'attaque du
paragraphe a.

*Remarque :* C'est facile si $N$ est premier. On peut résoudre les
équations polynômiale dans $\mathbb Z/p\mathbb Z$ : on veut résoudre
quelque chose de la forme $f(x)=0\mod p$ mais on sait aussi que $x^p=x$,
les racines sont donc les racines de pgcd($f(x),x^p-x$). Ensuite il faut
savoir trouver le pgcd simplement (il faut mieux que l'algorithme d'Euclide).

*Démonstration de Coppersmith :*

Notations : $h(x) = \sum a_i x^i \in \mathbb Z[x],
|| h ||^2 = \sum |a_i|$

**Lemme :** Soit $h(x)\in\mathbb Z[x]$ de degré $d$, $B>0$ entier.
Supposons $||h(B\cdot)||<\frac{N}{\sqrt{d+1}}$. Si $|x_0|<B$ satisfait
$h(x_0)=0\mod N$ alors $h(x_0)=0$ sur les entiers.

*Preuve du lemme :* 

$|h(x_0)|=|\sum_{i=0}^d a_i x_0^i|\leq \sum_{i=0}^d |a_iB^i|$ par
inégalité triangulaire. Puis via l'inégalité de Cauchy-Schwarz on a
$|h(x_0)|\leq \sqrt{d+1}||h(B\cdot)||<N$

On a $h(x_0)=0\mod N$ et $h(x_0)<N$ donc $h(x_0)=0$.

Reprennons la preuve du théorème. On a donc $f(x_0)=0\mod N$ et
$|x_0|<B=N^{1/d-\varepsilon}$.

*Idée :* trouver d'autres polynômes que $f$ 

* ayant également $x_0$ comme solution $\mod N$
* satisfaisant la confition de norme du lemme

*1ère idée :* $f, xf, x^2f, \dots$ puis combinaisons linéaires.

*2ème idée :* si $f(x_0)=0\mod N$, alors $f(x_0)^k=0\mod N^k$.

$g_{u,v}(x)=N^{m-v}x^uf(x)^v$ pour un $m$ fixé. avec $u\geq0$ et $0\leq
v \leq m$ Si $x_0$ est racine de $f\mod N$ alors $x_0$ est racine de
$g_{u,v}\mod N^m$. Reste à trouver une combinaison linéaire h des
$g_{u,v}$ telle que $||h(B\cdot)||<\frac{N}{\sqrt{d°h+1}}$ où $d°h$ est le
degré de $h$.

**Cours du 12 Janvier**

## Réseaux

*Définition :* Un réseau $L$ engendré par $(u_1, \dots, u_\omega)$ est
l'ensemble des combinaisons linéaires à coefficients entiers de
$u_1,\dots,u_\omega$.

*Définition :* Le déterminant de $L=\det(L)$ est le déterminant de la
matrice $\omega\times\omega$ dont les lignes sont les vecteurs
$u_1,\dots,u_\omega$.

**Théorème** (Hermite) : Tout réseau $L$ de dimension $\omega$ contient
un vecteur $v\in L\setminus\\{0\\}$ tel que $||v||\leq\gamma_\omega(\det
L)^{1/\omega}$.
C'est un résultat d'existence, mais on aimerait avoir une façon
algorithmique pour construire le vecteur $v$ dont parle le théorème. En
fait Hermite dans sa preuve, avait déjà un algorithme. Mais cet
algorithme n'était pas très performant. 

L'algotihme qu'on utilise est l'algorithme LLL (1982) de Lenstra,
Lenstra, et Lovasz. Il permet de trouver $v\in L\setminus\\{0\\}$ tel
que $||v||\leq 2^{\omega/4}(\det L)^{1/\omega}$ en temps polynomial.

*Revenons à la démonstration du théorème de Coppersmith.* On veut
 $ N^m/\sqrt\omega > 2^{\omega/4}(\det L)^{1/\omega)$. On considère le
réseau engendré par les $g_{u,v}$ avec $0\leq u\leq d-1$ et $0\leq v
\leq m$ et $\omega=d(m+1)$ est la dimension du réseau. On a
$d°g_{u,v}\leq u + dv \leq d-1+dm=\omega-1$.
