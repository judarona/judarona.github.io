.. raw:: html

    <style> .blue {color:blue} </style>

.. role:: blue

HOW TO DEFINE A STATIONNARY TEST-CASE ?
==========================================

L'objectif ici est de mettre en place un cas-test **stationnaire**. Par stationnaire, on entend que seul le resultat stabilise
(donc en fin de transitoire) est digne d'interet et d'analyse. Dans ce type de cas tests, **seuls les resultats stationnaires
finaux sont justes**. Les resultats obtenus pendant la periode transitoire permettant d'obtenir l'etat stationnaire final ne sont, quant a eux,
pas forcement justes et ne doivent pas etre analyses.

Prenons l'exemple de la modelisation d'une cavite carree avec une paroi superieure defilante (vitesse imposee sur la paroi
superieure en monophasique. La mise en mouvement de la paroi superieure va creer un tourbillon principal au centre de la
cavite avec des recirculations au niveau des 2 angles inferieurs.

Les etapes pour definir ce genre de calcul sont les suivantes :
 * **Le maillage** : pour bien prendre en compte la vitesse imposee de la paroi superieure, il est necessaire de definir un maillage suffisamment fin a ce niveau. Il est egalement necessaire de raffiner le maillage sur les autres bords du domaine afin que les forces de frottement soient bien calculees. On definira donc une maillage raffine sur les bords du domaine et un maillage plus grossier en son centre.
 * **La physique resolue** : le probleme resolu ici est un probleme laminaire qui sera definit avec le mot cle :blue:`Pb_thermohydraulique`.
 * **Le schema de convection** : dans ce cas de figure le choix du schema est important. Il s'agit de trouver un compromis entre un schema trop grossier et un schema trop fin. Si on utilise un schema grossier du er ordre, les petits tourbillons atendus dans les angles inferieurs de la cavite seront ecrases par la viscosite numerique. En revanche, si le schema est trop precis (comme un schema de type centre), des oscillations parasites risquent d'apparaitre. Dans le cas d'une discretisation VDF, on priviligera un schema de type **quick** et en VEF un schema **muscl**.
 * **Le schema de diffusion** : dans un cas stationnaire laminaire, il n'est pas necessaire de definir un schema de diffusion.
 * **Le schema en temps** : pour obtenir les resultats d'un cas stationnaire, le solveur a utiliser sera un solveur implicite. L'objectif de ce type de calcul est d'obtenir un resultat juste une fois le transitoire stabilise et non pas de comprendre et d'etudier la phenomenologie du transitoire. Durant la periode transitoire, les resultats obtenus ne seront pas forcement "justes" et ne doivent pas etre interpretes et analyses. Seuls les resultats finaux sont garantis et dignes d'interet. Nous recommandons d'utiliser un schema Euler implicite avec un schema **GMRES** pour la vitesse et un schema **GCP** (gradients conjugues PETSC) pour la pression.
 
