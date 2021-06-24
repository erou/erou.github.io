---
layout: default
title: Avis de soutenance
permalink: /soutenance/
---

<center><span style='font-size:15pt;font-weight:bold;'><br/>Avis de Soutenance</span><br><br>  <span style='font-size:15pt; '>Monsieur Édouard ROUSSEAU</span><br><br>  <span style="font-size:14pt">Mathématiques fondamentales</span>
                <br><br>Soutiendra  publiquement  ses travaux de thèse intitulés<br><br><i>Arithmétique efficace des extensions de corps finis</i>
                <br/><br/>dirigés par Luca DE FEO, Hugues  RANDRIAMBOLOLONA
             et  Eric SCHOST<br /><br />Soutenance prévue le <b><i>  lundi 12 juillet 2021 </i></b>à 15h00<br />Lieu : &nbsp; Université de Versailles, 45 avenue des États-Unis, 78035 Versailles  <br/>Salle : Amphi E, bâtiment Descartes <br><br/><b>Composition du jury</b><br>
</center>

| M. Daniel AUGOT            | INRIA Saclay et LIX, Institut Polytechnique de Paris             | Examinateur           |
| M. Stéphane BALLET         | Institut de mathématiques de Marseille, Aix-Marseille université | Rapporteur            |
| M. Luca DE FEO             | IBM Research Zürich et Université de Versailles                  | Directeur de thèse    |
| M. Claus FIEKER            | University of Kaiserslautern                                     | Rapporteur            |
| M. David MADORE            | Télécom Paris                                                    | Invité                |
| Mme Julia PIELTANT         | CNAM                                                             | Examinatrice          |
| M. Hugues RANDRIAMBOLOLONA | ANSSI et Télécom Paris                                           | Invité                |
| M. Eric SCHOST             | University of Waterloo                                           | Co-directeur de thèse |

**Mots-clés :** calcul formel, corps finis, extensions

**Résumé :** Les corps finis sont omniprésents en cryptographie et en théorie
des codes, deux domaines de première importance dans les communications
modernes. Ainsi, il est crucial de représenter les corps finis et d’y faire des
calculs de la façon la plus efficace possible. Dans cette thèse, nous
travaillons sur l’arithmétique des extensions de corps finis, de deux manières
différentes et indépendantes.

Dans la première partie, nous étudions
l’arithmétique d’une unique extension de corps fini $$\mathbb F_{p^k}$$. Une
manière d'estimer la complexité d'un algorithme dans une extension est de
compter le nombre de multiplications effectuées dans le corps de base $$\mathbb
F_p$$. Ce modèle est connu sous le nom de complexité bilinéaire. Dans cette
thèse, nous généralisons les résultats connus pour la complexité bilinéaire à un
nouveau type de complexité, qualifiée d'hypersymétrique. Nous fournissons un
algorithme pour calculer la complexité hypersymétrique de la multiplication dans
l'extension $$\mathbb F_{p^k}$$, ainsi qu'une implémentation et son analyse. Nous
généralisons également aux cas hypersymétrique et multilinéaire des résultats
asymptotiques connus dans le cas de la complexité bilinéaire classique.

Dans la seconde partie, notre but est de construire une structure de donnée
efficace pour représenter plusieurs extensions simultanément, ainsi que les
plongements entre elles. Ces plongements sont rarement uniques, et il faut donc
s'assurer que les choix effectués soient compatibles, c'est-à-dire que dès lors
que trois corps finis sont en jeu, le plongement calculé est indépendant du
chemin parcouru. Nous donnons une implémentation de l’algorithme de
Bosma-Canon-Steel, qui permet d’obtenir des plongements compatibles, et qui était
uniquement disponible dans MAGMA, mais est désormais disponible dans le système de calcul
formel Nemo. Nous proposons également une nouvelle
construction nommée réseau standard de corps finis compatiblement plongés,
inspirée de l'algorithme de Bosma-Canon-Steel et des polynômes de Conway, qui
sont à la base d'une autre méthode populaire pour obtenir la compatibilité.
Contrairement à l'utilisation des polynômes de Conway, notre
méthode permet d’utiliser des corps finis définis de manière arbitraire, tout en restant
efficace. Nous analysons en détails la complexité de nos algorithmes, et donnons
une implémentation montrant que notre construction peut être utilisée en
pratique.

<!-- **Résumé :** Les corps finis sont omniprésents en cryptographie et en théorie
des codes, deux domaines de première importance dans les communications
modernes. Ainsi, il est crucial de représenter les corps finis et d’y faire des
calculs de la façon la plus efficace possible. Dans cette thèse, nous
travaillons sur l’arithmétique des extensions de corps finis, de deux manières
différentes et indépendantes.

Dans la première partie, nous étudions
l’arithmétique d’une unique extension de corps fini $$\mathbb F_{p^k}$$. Lorsqu’on souhaite
estimer la complexité d’un algorithme dans une extension, on compte souvent les
opérations arithmétiques qui sont effectuées dans le corps de base $$\mathbb F_p$$. Dans un
tel modèle, toutes les opérations ont le même coût. Ce modèle est connu sous le
nom de complexité algébrique. Néanmoins, on sait que la multiplication est une
opération plus coûteuse que l’addition. Pour cette raison, des modèles
alternatifs ont été étudiés, comme par exemple celui de la complexité
bilinéaire, dans lequel on fait l’hypothèse que les additions n’ont aucun coût,
et on compte donc uniquement les multiplications. Pour avoir une multiplication
efficace dans l’extension $$\mathbb F_{p^k}$$, des recherches ont été menées pour obtenir
des formules dans lesquelles le nombre de multiplications dans le corps de base
$$\mathbb F_p$$ est minimal. Le nombre optimal de multiplications nécessaires est, par
définition, la complexité bilinéaire de la multiplication dans l’extension
$$\mathbb F_{p^k}$$. Trouver la valeur exacte de la complexité bilinéaire d’une extension
est difficile, mais il existe des algorithmes pour chercher des formules
optimales en petite dimension. Asymptotiquement, d’autres algorithmes trouvent
des formules qui ne sont pas nécessairement optimales, mais qui donnent une
borne supérieur linéaire en le degré de l’extension pour la complexité
bilinéaire. Nous généralisons ces résultats à un nouveau type de complexité,
qualifiée d’hypersymétrique, qui est liée à des formules possédant plus de
symétries. Nous fournissons un algorithme pour trouver des formules
hypersymétrique, ainsi qu’une implémentation et son analyse. Nous prouvons
également que la complexité hypersymétrique est elle aussi linéaire.

Dans la
seconde partie, nous étudions plusieurs extensions simultanément. Dans la
plupart des systèmes de calcul formel, il est possible de travailler avec des
corps finis, mais deux extensions arbitraires sont souvent considérées comme des
objets indépendants, et les liens entre ces extensions ne sont pas
nécessairement accessibles à l’utilisateur ou l’utilisatrice. Notre but dans
cette partie est de construire une structure de donnée efficace pour représenter
plusieurs extensions, ainsi que les plongements entre elles. Nous voulons aussi
que ces plongements soient compatibles, c’est-à-dire que si $$a$$, $$b$$, $$c$$ sont trois
entiers
(avec $$a \mid b \mid c$$), la composition entre les plongements de $$\mathbb F_{p^a}$$ vers $$\mathbb F_{p^b}$$
et $$\mathbb F_{p^b}$$ vers $$\mathbb F_{p^c}$$ doit être égale au plongement de $$\mathbb F_{p^a}$$ dans $$\mathbb F_{p^c}$$.
Nous appelons cette structure de donnée un réseau de corps finis compatiblement
plongés. Nous donnons une implémentation de l’algorithme de Bosma-Canon-Steel,
qui permet d’avoir un réseau compatible, et qui était uniquement disponible dans
MAGMA. Cet algorithme est désormais disponible dans le système de calcul
formel Nemo. Une autre méthode populaire pour obtenir des réseaux compatibles
vient des polynômes de Conway. C’est une manière efficace d’obtenir des
plongements, mais les extensions doivent alors être définies en utilisant ces
polynômes précalculés si l’on veut garantir la compatibilité. Inspiré par ces
polynômes et l’algorithme de Bosma-Canon-Steel, nous proposons une nouvelle
construction nommée réseau standard de corps finis compatiblement plongés. Cette
dernière nous permet d’utiliser des corps finis définis de manière arbitraire, tout en restant
efficace. Nous analysons en détail la complexité de nos algorithmes, et donnons
une implémentation montrant que notre construction peut être utilisée en
pratique.
-->
