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
 $ N^m/\sqrt\omega > 2^{\omega/4}(\det L)^{1/\omega}$. On considère le
réseau engendré par les $g_{u,v}$ avec $0\leq u\leq d-1$ et $0\leq v
\leq m$ et $\omega=d(m+1)$ est la dimension du réseau. On a
$d°g_{u,v}\leq u + dv \leq d-1+dm=\omega-1$.

D'après le lemme, si $x_0$ est une racine modulo $N^m$ d'une combinaison
linéaire des $g_{u,v}(B)$, avec $|x_0|< B$ de norme inférieure à $
2^{\omega/4}(\det L)^{1/\omega}$. 

On peut montrer qu'on a l'inégalité voulue pour un choix de
$m=O(1/d\min(1/\varepsilon,ln(N))$.

*Application :* Attaque de Hastad généralisée.

On a des $C_i = (f_i(M))^e \mod N_i$. Le destinataire numéro $i$ donne
sa clé publique $(e_i, N_i)$ et le polynôme $f_i$.

*Attaque :* $\bar N = N_1\times\dots\times N_k$ ; $g_i = (f_i)^e-C_i$ et
$d°g_i\leq d$ et $k\geq d$ et $\forall i,\; g_i(M) = 0 \mod N$. On peut
supposer les $g_i$ unitaires. En effet, si un $g_i$ ne l'est pas, on le
normalise en divisant par son coefficient dominant. Si jamais le
coefficient dominant *n'est pas inversible*, alors on a trouvé un
facteur de $N$. On peut supposer que pour tout $i$, on a $d°g_i = d$,
quitte à multiplier au besoin par un monôme. On pose $g(x) =
\sum_{i=1}^k T_ig_i(x)$ avec $T_i=0\mod N_j$ ($j\neq i$) et $T_i = 1
\mod N_i $. $T_i$ existe et est unique d'après le théorème chinois.

On a $ g(M) = 0 $, et $ M < N_i $ pour $ 1 \leq i \leq k $. Donc $ M^k < \bar N \Longrightarrow M < \bar N^{1/k} \leq \bar N^{1/d} \Longrightarrow $ on trouve $M$ en temps polynomial (via Coppersmith).

*Attaque (Coppersmith)* sur la méthode "short pad"

$ C = (M || r)^e \mod N $ où $r$ a été choisi au hasard et 
$ r < \lfloor n/e \rfloor $ où $n$ est le nombre de bits de $r$. À partir de deux $C_i$ définit comme ça, on peut retrouver $M$ en temps polynomial.

$C = M^e \mod N$, pareil pour $C_i$.
$ x\mapsto g_1(x,y) = x^e - C_1 $
$ x\mapsto g_2(x,y) = (x + y)^e - C_2 $

On veut voir si on a des racines communes entre les $g_i$, pour cela on
regarde le résultant de ces deux polynômes $Res_x(g_1,g_2) = h(y)$.
$\Delta = r_2-r_1$ annule $h$ et $|\Delta| < N^{1/e^2}$. 

#### d) Attaque de Bleichenbacker

**Standard PKCS** (RSA Data Security) : Public Key Cryptographic
Standard

PKCS#1 v1.5

$ C = (\mu(M))^e\mod N $ où
$ \mu(M) = \varnothing\varnothing\varnothing 2
||r||\varnothing\varnothing || M $ et où $ r $ est aléatoire et fait au
moins 8 octects.

Alice --> Bob

$ C^d \mod N = \varnothing\varnothing\varnothing 2 \[ \neq 0 \]
\varnothing\varnothing M $.

*Exemple simplifié :* clé publique $ e, N $

On suppose qu'on a un *oracle* $ \mathcal A $ qui nous donne la parité du clair
correspondant au chiffré qu'on envoie.

Supposons que l'attaquant connaisse le chiffré $ C_0 $ d'un message $
M_0 $. Comment peut-il retrouver $ M_0 $ ? 

$ \mathcal A(2^e C_0 \mod N) = 0$ si $ 0\leq M_0 \leq N/2 $, $ =1 $
sinon. Ça nous permet d'avoir un intervalle dans lequel se trouve $ M_0
$. On continue avec $ 4^e, 8^e $ etc ... Par dichotomie, on retrouve
exactement $ M_0 $.

### Attaques physiques

Dans une carte bleu (*i.e.* carte à microprocesseurs) il y a : des poules des canards des oies. CPU
(unité centrale), ROM (mémoire sans ré-écritude),
RAM (mémoire volatile), EEPROM (mémoires où on met les clés).

Dans tous les livres de crypto, on considère la carte bleu comme un
boite noire. Mais Paul Kocher, dans les années 90, s'est dit que ces
calculs n'étaient pas des choses abstraites, et qu'on pouvait peut-être
trouver d'autres informations en mesurant des données physiques :

* consommation électrique
* champ électro-magnétique 
* température

et inversement on peut se demander si on peut modifier le fonctionnement
d'une carte en modifiant des paramètres physiques, comme avec :

* changement d'alimentation
* laser
* chauffer
* champs électromagnétique (courrants de Foucault)
* coups de cutter

**1. SPA Simple Power Analysis** (Kocher, 1998)

Quand on faisait des *square and multiply*, en fonction de la parité du
bit $i$ de la clé, la consommation électrique n'était pas la même. Donc
avec un oscilloscope, on pouvait retrouver tous les bits de la clé.
Gênant.

Du coup on fait un *square and multiply always*, on fait les mêmes
calculs à chaque fois, comme ça il est impossible de différencier les
bits de la clé.

**13 Janvier**

**2. Timing attacks** (Kocher, 1996)

En mesurant le temps que prends le codage, on peut récupérer le nombre
de 0 et le nombre de 1. Car le temps pris par la machine dépend du
nombre de 1, c'est-à-dire du poids de Hamming. C'est le cas car les
opérations de multiplications coûtent moins cher (en temps) que le
square. 

Le temps d'une multiplication $ a \times b \mod N $ peut dépendre de $
a $ et $ b $.

*Exemple :* multiplication de Montgomery. Le temps peut-être $ T_1 $ ou
$ T_2 $. On essaie de retrouver le graphe des multiplications/square qui
ont lieu. 

On va choisir plein de messages $ M_1, \dots, M_n $ et on considère
$A = \\{i |$ temps $(M_i^2\times M_i)=T_1\\}$ et $B$ le même ensemble
avec $T_2$. On chronomètre le temps total $\tau_i$ du calcul $ M_i^d\mod N $

On a $\cfrac{1}{A}\sum_{i\in A} \tau_i = \tau_A, \tau_i=T_1 +$ random et
pareil avec $B$. Mais cela n'est vrai que si à la première séparation 
dans le graphe des appels, on a que le 1022ème bit est un 1. Sinon on ne
pourra pas distinguer de différence significative dans les temps de
codage. (On considère que le temps que prennent les autres opérations
est aléatoire et d'une moyenne constante.)

**3. DPA Differential Power Analysis** (Paul Kocher, 1998)

On choisit $n$ message aléatoires $ M_1,\cdots, M_n $ et $\left\\{ i,
  1\leq i\leq n | \text{bit de poids faible}(M_i^2\times M_i) = 0
\right\\}$ et pareil pour $B$ avec 1. On fait la même chose qu'avec le
temps mais avec le courant.

Quelles sont les solutions ?

On va randomiser le calcul, on va calculer $S = m^d \mod N =
\frac{(\lambda M)^d}{\lambda^d}$. Donc on doit calculer deux RSA et
faire une division, c'est plus couteux mais personne ne peut prédire les
calculs qui vont être faits.

Si on veut rendre la consommation constante on peut faire une opération
puis une autre qui est l'inverse, mais cela crée un champs
électromagnétique qui peut lui aussi être exploitée. Pas de bol.

**4. DEMO Differential Electro Magnetic Attack**

C'est autrement plus compliqué parce qu'on a quelque chose de dimension
3 !

Toutes ces attaques sont appelées des *attaques par canaux auxiliaires* 
et sont des attaques *passives*.

**5. DFA (Differential Fault Analysis)**

C'est une attaque *active* où l'on va provoquer des erreurs de calcul.
Pour y parvenir, on va modifier l'environnement de la carte, on peut par
exemple utiliser :

* pics d'alimentaton électrique
* chauffer/refroidir
* champs électromagnétique
* rayon laser

En 1997, Boneh, De Millo, Lipton, ont fait la première attaque par faute
sur le RSA. Pour faire cette attaque, ils ont supposé que le RSA était
implémenté en utilisant les restes chinois. C'est-à-dire en calculant
$S_p = M^{d_p} \mod p$ et $S_q = M^{d_q} \mod q$ où $d_p = d\mod (p-1)$
et où $d_q = d \mod (q-1)$, on récupère ensuite $S$ avec l'isomorphisme
du théorème des restes chinois. Comme un RSA est en fait un square and
multiply d'un nombre de $k$ bit, ce qui coûte en gros $O(k^3)$, donc en
réduisant les tailles de moité, on gagne un facteur 8, et comme on doit
le faire deux fois on gagne un facteur 4 *in fine* (on considère que le
théorème chinois ne coûte quasiment rien). 

*Attaque n°1 :* l'attaquant connaît $S = M^d \mod N$ et $S'$ avec
erreur de calcul. On considère qu'on a $S = S_p \mod p$, $S = S_q \mod
q$ et $S' = S'_p\neq S_p\mod p$ et $S'_q = S_q \mod q$. Ainsi on peut
récupérer un facteur via pcgd$(S-S', N) = q$.

*Attaque n°2 :* l'attaquant ne connait que $S', N, M, e$, alors on a
encore pgcd$(S'^e - M, N) = q$. Contre-mesure :

* à la fin du calcul, vérifier que $S^e = M\mod N$
* si on ne connaît pas $e$, on prend $\lambda$ aléatoire et premier, et on fait une
implémentation différente toujours basée sur les restes chinois, on
calcul $M_{\lambda p} = M \mod \lambda p$, puis $S_{\lambda p} =
M_{\lambda p}^{d_{\lambda p}} \mod \lambda p$ (où $d_{\lambda p} =d\mod
(p-1)(\lambda-1)$ et enfin $S_p =
S_{\lambda p} \mod p$. On fait la même chose pour $q$ et on retrouve
$S$. Au milieu on teste si $S_{\lambda p}\mod \lambda = S_{\lambda
q}\mod\lambda$.

*Remarque : safe errors*

Dans un *square and multiply always*, si l'attaquant observe qu'une
faute n'a pas eu de conséquences, il peut en déduire qu'une opération
était inutile et donc qu'on a un 0 à l'endroit où on a provoqué la
faute. C'est donc un peu un casse-tête chinois (comme le théorème) car
on avait justement créé cette version du square and multiply pour
résoudre un autre problème...

**7. Bug attack (Shamir)**

On suppose qu'on a une multiplication buggée $M(a,b) = a\times b$ si
$(a,b)\neq(a_0,b_0)$ et $M(a,b)\neq a\times b$ si $(a,b) = (a_0,b_0)$.
On suppose qu'on utilise RSA avec les restes chinois (comme le
casse-tête). On a $N = p\times q$ et on prend $M$ et $p < M < q$.
Ensuite on ne change que les derniers bits de $M$, on met ceux de $a_0$
et $b_0$. On récupère $p$ en calculant le pgcd de $S'^e-M$ et $N$. 

## Courbes elliptiques

### Rappels sur les courbes elliptiques

* degré 1 : droites
* degré 2 : coniques 
  * paraboles : $y=ax^2$
  * hyperboles : $\frac{x^2}{a^2}-\frac{y^2}{b^2}=1$
  * ellipses : $\frac{x^2}{a^2}+\frac{y^2}{b^2}=1$
* cubiques : dont les courbes elliptiques

On part d'une équation $ E: y^2 + a_1xy + a_3y = x^3 + a_2x^2 +a_4x +a_6
$. En caractéristique différente de $2$ et $3$, on peut simplifier en
l'équation $ y^2 = x^3 + ax + b $.

En caractérique 2, on a

* *cas super-singulier* $y^2+a_3y = x^3 +ax +b$
* *cas non super-singulier* $y^2 +a_1xy = x^3 +ax +b$

et en caractéristique 3 on a $ y^2 = x^3 + ax^2 + b $.

On définit une loi de groupe sur la courbe d'équation $ y^2 = x^3 + ax +
b $ de manière géométrique. Pour construire $ P + Q $, on trace la
droite $\Delta$ passant par ces deux points, on pose $R$ le troisième
point d'intersection entre $\Delta$ et la courbe $\mathcal C $, et on
pose enfin $ P + Q $ le symétrique de $R$ par rapport à l'axe des
abcisses.

Il y a une série de problèmes :

* que vaut $ P + P $ ?
* y a-t-il toujours un troisième point d'intersection ?
* que se passe-t-il si la droite $\Delta$ est verticale ?

Pour résoudre le problème $ P + P $, on considère la tangente à la
courbe $\mathcal C$ en $P$. Mais y a-t-il toujours une tangente ? Pour
vérifier cela, on regarde les dérivées partielles, on résout le système
et trouve que l'existence d'un point où les deux dérivées partielles
s'annulent implique $4a^2+27b^2=0$. On va donc prendre des courbes *qui
ne vérifient pas* cette équation.

Pour le cas où $\Delta$ est vertical, on va introduire un point à
l'infini $\mathcal O$ vérifiant

* $ P + \mathcal O = P $
* $ \mathcal O + P = P $
* $ \mathcal O + \mathcal O = \mathcal O $

**Théorème :** $(E,+)$ est un groupe commutatif.

L'associativité est le seul point difficile.

Pour essayer de le résoudre, on va passer au point de vue calculatoire
(comme René Descartes). Si on a $ P = (x_P, y_P) $ et $ Q = (x_Q, y_Q)
$, et $ R = P+Q = (x_R, y_R) $. On a $\Delta : y = ux + v$ où $ u
=\cfrac{y_Q - y_P}{x_Q - x_P} $ (c'est la pente) et $v=y_P - ux_P$ (on
dit juste que $P$ est sur la droite).

Puis on résout le système $y = ux + v$, $y^2 = x^3 +ax+b$ et on a $x_R =
u^2 - x_P - x_Q$, $y_R = -(ux_R+v)$ (dans les cas ordinaires, sinon il y
a d'autres calculs) et on peut vérifier l'associativité par calcul.

On peut remplacer $\mathbb{K}$ par n'importe quel corps, car tous les
calculs faits ne dépendent pas du corps.

On va prendre dans ce qui suit $\mathbb{K}=\mathbb{F}_p$ où $p$ premier
$>3$ et $E=\left\\{  (x,y)\in(\mathbb{F}_p)^2 \;|\; y^2=x^3+ax+b\right\\}$.

On peut aussi utiliser les coordonnées projectives pour éviter de faire
des divisions et gagner en performance, comme dans MECM.

### Diffie-Hellman

Protocole de Diffie-Hellman : 1976 ``New directions in cryptography''

*Remarque :* dériver une clé $K$ (128 bits) à partir d'un entier de 1024
bits n'est pas forcément trivial. Dans les faits, on va utiliser une
fonction de hachage.

*Sécurité de Diffie-Hellman contre un attaquant ``passif'' :*

**Problème DH calculatoire :** à partir de $p, g, g^a \mod p$ et
$g^b\mod p$,trouver $g^{ab}\mod p$

**Problème de log discret :** à partir de $p, g, g^a\mod p$, trouver
$a$.

Savoir résoudre DL implique savoir résoudre DH. Mais on ne sait pas si
les deux problèmes sont équivalents.

*Sécurité de Diffie-Hellman contre un attaquant ``actif'' :*

Attaque ``man in the middle''. Cette attaque est possible parce qu'on ne
peut pas vérifier qui envoit les messages. Pour contrer cette attaque,
on peut signer les messages avec des algorithmes de signatures
quelconques. Pour s'échanger la clé publique de signature d'Alice, il
faut *certifier* que c'est bien la bonne clé. Pour faire ça, il faut
qu'une *autorité* signe un message du type `Alice||PK_A`

Ce genre d'architecture s'appelle PKI (Public Key Infrastructure)

### Diffie-Hellman sur les courbes elliptiques

#### Log discret

La complexité du log discret dans un groupe
$G=(\mathbb{Z}/p\mathbb{Z})^*$ est en $\mathcal L(1/3)$.

**Algorithme baby step giant step** : données $g$, $g^a$, sur un groupe
$G$ quelconque. Problème : trouver $a$. 

Méthode naïve (recherche exhaustive) en $O(|G|)$.

    Baby step Giant step

$ a = dq +r $ (division euclienne) ; $ d = \lfloor\sqrt{|G|}\rfloor $ ;
$0\leq r < d$ ; $ 0\leq
q=\frac{a-r}{d}\leq\frac{a}{d}\leq\frac{|G|-1}{\sqrt{|G|}-1}$

Ensuite on remarque que $g^a=y\Leftrightarrow g^{dq}=yg^{-r}$. On
construit deux tables pour les $g^{dq}$ et pour les $yg^{-r}$ (ces
tables sont de l'ordre de $\sqrt{|G|}$), puis on trie les tables et on
regarde les collisions. Ça donne un algorithme en
$O(\sqrt{|G|}\ln\sqrt{|G|})$.

*Remarque :* dans le cas de $G = (\mathbb{Z}/p\mathbb{Z})^*$ on a aussi
la technique du calcul d'indice (la meilleure connue) qui est en
$\mathcal L(1/3)$.

Mais sur une courbe elliptique la meilleure technique connue est en
$O(\sqrt{|G|}$.

**Cardinal de ** $E$

**Propriété 1 :** Dans $(\mathbb{Z}/p\mathbb{Z})^*$, il y a $(p-1)/2$
carrés.

**Propriété 2 :** Soit $v\in(\mathbb{Z}/p\mathbb{Z})^*$, $v$ est un
carré ssi $v^{\cfrac{p-1}{2}} = 1\mod p$.

*Preuve :* tous les carrés sont solutions du polynôme $X^{(p-1)/2}$ et
il n'y a pas plus de carrés. 

**Propriété 3 :** On suppose $p=3\mod4$, si $v$ est un carré alors
$v=u^2\mod p$ avec $u=v^{(p+1)/4}$.

**Théorème de Hasse** (1940) : $p+1-2\sqrt{p}\leq |E| \leq
p+1+2\sqrt{p}$.

*Remarque :* Il existe des algorithmes efficaces pour compter le nombre
de points d'une courbe elliptique (Schoof, Elkies, Atkin).

| **DH** $k=1024$ | **ECDH** $k=160$ |
| $g^a\mod p\;O(k^3)$ | $aP\;O(k^3)$ |
| $(g^b)^a\mod p\;O(k^3)$ | $a(bP)\;O(k^3)$ |

## Signature sur courbe elliptique

**El Gamal**

On prend $g$ un générateur de $(\mathbb{Z}/p\mathbb{Z})^*$. La clé
secrète est $x$ et la clé publique est $y=g^x\mod p$

**Signature d'un message M**

* Choisir $k$ aléatoire
* Calculer $r=g^k\mod p$
* Calculer $s=\frac{h(M)-xr}{k}\mod(p-1)$

La signature est $(r,s)$. Où $h$ est une fonction de hachage de
$\left\\{ 0,1 \right\\}^*$ dans $\mathbb{Z}/(p-1)\mathbb{Z}$.

**Vérification :** on calcul $y^rr^s=g^{h(M)}\mod p$.

*Remarque :* Si jamais $k$ était pris constant, on pourrait retrouver $x$ en
signant seulement deux messages (deux inconnues et deux équations).

**DSA :** (DSA standard du NIST). C'est un peu un El Gamal où on se
place sur un sous-groupe de $(\mathbb{Z}/p\mathbb{Z})^*$ d'ordre $q$. On
prend le générateur $g^{\frac{p-1}{q}}$

**ECSDA :** $E$ courbe elliptique, $P$ un point de la courbe. On
considère $\langle P\rangle$ (où ce groupe a $2^{160}$)

Clé secrète : $x$

Clé publique : $Q=xP$

**Signature de M**

* $k$ aléatoire
* $R=kP$
* $r$ l'abcisse de $R$
* $s=\cfrac{h(M)+xr}{k}\mod(p-1)$

La signature est $(r,s)$. on vérifie que l'abcisse de
$(\frac{h(M)}{s}P+\frac{r}{s}Q)$ est $r$.

# Chasse aux traitres (Traitor tracing)

## Diffusion de contenu vidéo

Schéma DVB (Digital Video Brodcast). Un émetteur diffuse un message $M$,
mais il doit être codé en un certain $E_{CW}(M)$ où CW (control word)
change toutes les dix secondes. Les mots de contrôle sont transmis de
manière codée également, on transmet $F_{K_c}(CW)$ où $K_c$ est une clé
symétrique qui change tous les mois. Et on transmet $K_c$ via une mise à
jour personnelle pour chaque utilisateur.

**Attaque n°1 :** rediffuser M. Solutions : TV intégrant le décodage,
watermarking

**Attaque n°2 :** rediffuser CW (typiquement 128 bits toutes les dix
secondes). En temps réel ou en différé. Solution : ne pas faire passer
CW en clair, donc mettre $F^{-1}$ et $E^{-1}$ dans le même composant.
Mais ça coûte cher.

**Attaque n°3 :** Diffuser $K_c$ (obtenu via une attaque physique)

**Attaque n°4 :** Diffuser $KU_i$ (obtenu via attaque physique)

L'algorithme $E$ est appelé *scrambling algorithm* : DVBCSA (Common
Scrambling Algorithm).

**Méthode de Chor Fiat Naor :**

**Méthode simple :** tout le monde a la même clé.

**Méthode lourde :** avoir une clé par personne. Mais ça fait vraiment
trop de données à transmettre.

L'idée de Chor Fiat Naor, c'est d'écrire $x=x_1\oplus
x_2\oplus\cdots\oplus x_l$ où les $x_i$ sont pris au hasard (sauf le
dernier) et de donner $2l$ clés $K_1^0, K_1^1, K_2^0, K_2^1, \cdots,
K_l^0, K_l^1$.

Chaque utilisateur possède une clé de la forme
$(K_1^{a_{i1}},\ldots,K_l^{a_{il}})$ où les $a_{ij}\in\left\\{ 0,1
\right\\}$.

On diffuse $E_{K_1^0}(x_1), E_{K_1^1}(x_1), \cdots, E_{K_l^0}(x_l),
E_{K_l^1}(x_l)$. L'utilisateur ne pourra pas tout décoder mais il pourra
décoder suffisemment pour reconstruire $x$.

## Codes correcteurs d'erreurs

*Définition :* un code linéaire binaire (ou code) de longueur $n$ est
une partie $C$ de $\mathbb{F}_2^n$ stable par addition (sev). Un vecteur
de $C$ est appelé un mot de $C$. Le support de $x=(x_1, \cdots, x_n)$
est l'ensemble des indices tels que $x_i\neq 0$. Le poids de $x$ est le
cardinal du support. La distance de Hamming entre $x$ et $y$ est le
poids du vecteur $x-y$. Distance minimale d'un code $C$. Paramètre d'un
code $[n,k,d]$ où $n$ est la longueur, $k$ la dimension, $d$ la distance
minimale.

*Décodage :* trouver le mot ayant la distance de Hamming la plus petite
avec le mot qu'on a reçu.

**Théorème :** Si le code $C$ a une petite distance minimale $d$ et si
le nb d'erreur est inférieur $(d-1)/2$ alors on peut décoder.

Reformulation du problème des décodeurs pirates. Chaque décodeur est un
mot de $C$, $C$ est l'ensemble des décodeurs légaux. 

*Définition :* on dit que $e=(e_1, \cdots, e_n)$ est un descendant de la
paire $\left\\{ x,y \right\\}$ ssi pour tout $1\leq i \leq
n,\;e_i\in\left\\{x_i, y_i  \right\\}$.

*Définition :* on dit que $C$ est une partie indentifiante (ou code
identifiant) si pour tout descendant $e$ d'une paire d'éléments de $C$
l'ensemble $P_e$ des paires $P$ dont $e$ est un descendant est
d'intersection non vide. $\bigcap_{P\in P_e}P\neq\emptyset$.

**Proposition :** Soit $C$ un code de $\mathbb{F}_q$ de longueur $n$ et
de distance minimale $d$. Si $d>3n/4$ alors $C$ est une partie
identifiante de $\mathbb{F}_q^n$.

*Preuve :* par l'absurde. Supposons que $C$ n'est pas une partie
identifiante de $\mathbb{F}_q$, il existe $e$ tel que $P_e$ est non vide
et l'intersection de toutes les paires est vide. *Premier cas.* Il
existe $\left\\{x,y  \right\\}$ et $\left\\{ x',y' \right\\}$
d'intersection vide. Parmi $x_i-x_i'$, $x_i-y_i'$, $y_i-x_i'$,
$y_i-y_i'$, au moins des nul, donc on a $4d\leq
d(x,x)+d(x,y)+d(y,x')+d(y,y')\leq3n$, ce qui implique que $d\leq 3n/4$,
une contradiction. *Deuxième cas.* On a un système en triangle $x,y,z$
où il y a toujours un élément en commun mais pas le même à chaque fois.
On fait la même chose qu'avant avec $2d\leq d(x,y)+d(y,z)+d(x,z)$. 

*Codes de Reed-Solomon.* On prend $n$ élements distincts $\alpha_1,
\cdots, \alpha_n$ de $\mathbb{F}_q$. On prend le code $C=\left\\{
  x\;|\;\exists Q, Q(\alpha_i)=x_i \right\\}$.

**Propriété :** la distance minimale $d$ du code $C$ vérifie $d\geq
n-(k-1)$.

# Chiffrement homomorphe

On a $c_i=E(m_i)$, on voudrait que $c_1\circ c_2=E(m_1\circ m_2)$.

Graig Gentry (IBM) 2009

On code en faisant $c=m+2e+\lambda p$ (avec $e$ et $\lambda$ random) On décode en faisant $\mod
p\mod2$. C'est homomorphe mais attention à l'aléat !

*Bootstrapping (Gentry, 2009)*

On a $f_c(p):=D_p(C)=(c\mod p)\mod 2=m$. Il vient $f(E(p))=E(m)=c'$.
C'est un nouveau chiffré dont le niveau de bruit est plus bas.
