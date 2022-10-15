# Analyse de séries temporelles avec un CNN 1D pour la maintenance prédictive


Ce travail vise à implémenter et à évaluer une méthode d'analyse de
série temporelle multivariée basée sur un réseau de neurones convolutif 1D, pour la détection d'évènements anormaux dans des puits de pétrole.


## Jeu de données

https://github.com/ricardovvargas/3w_dataset


## Motivations

L'analyse de série temporelle est devenu un enjeu majeur dans de
nombreux domaines tels que la la finance, la santé ou encore la
cyber-sécurité [1,2]. Au cours des dernières années, il y a
eu un intérêt croissant pour l'utilisation des réseaux de neurones
convolutifs pour accomplir cette tâche. L'objectif à travers leur
utilisation est d'extraire automatiquement les caractéristiques
profondes des données, en opposition aux méthodes basées sur les
caractéristiques elles-même, où la performance dépend fortement des
caractéristiques fabriquées manuellement [1]. Grâce à l'introduction
de champs réceptifs locaux, les réseaux de neurones convolutifs sont
particulièrement robustes aux variations [1] et fournissent une
excellente extraction des caractéristiques spatiales [2]. De
plus, le partage des poids rend leur coût en calcul avantageux comparé aux autres méthodes telles que celles basées sur des distances comme la classification par plus proches voisins [1]. Ces avantages justifient leur intérêt pour une application aux séries temporelles.

## La méthode

### Transformation des données

L'objectif est de générer des séquences labélisées exploitables avec un réseau de neurones convolutif 1D.

Une série temporelle multivariée est une séquence de plusieurs
variables, typiquement mesurées successivement à intervalle régulier
dans le temps. Ainsi, la tâche de classification se traduit en
l'attribution d'une classe à chaque séquence du jeu de données. Or dans
notre cas, le jeu de données consiste en une unique séquence avec une
classe associée à chaque pas de temps. Le problème consiste donc à
transformer cette dernière en sous-séquences de longueur fixe $T$ et à
attribuer une classe à chacune d'entre elles. Pour ce faire, nous
faisons évoluer une fenêtre glissante de taille $T$ sur les données,
avec un certain pas $P$, afin de générer les sous-séquences. Une
sous-séquence est alors considérée comme anormale si elle contient au
moins une valeur anormale. Plus précisément, elle sera classée en
fonction des 3 classes présentes dans le jeu de données : *Anormale* si
elle contient au moins une telle valeur, sinon *Transitoire* si elle
contient au moins une telle valeur, sinon *Normale*.

Suite à cette transformation, les instances du jeu de données sont des
séries multivariées labélisées de même longueur. La section suivante
décrit comment elles peuvent être classifiées à l'aide d'un réseau de
neurones convolutif 1D.

### Réseau de neurones convolutif 1D

Comment un CNN 1D peut-il être utilisé pour classifier les séries temporelles multivariées créées dans la première partie ?

Rappelons brièvement qu'un réseau de neurones convolutif se décompose en
un extracteur de caractéristiques suivi d'un classifieur. Le premier est
généralement composé de blocs consécutifs incluant chacun une couche de
convolution et une couche de pooling. Les entrées et sorties de chaque
couche sont appelées *feature maps*. De plus, une couche d'activation
est intégrée à chaque couche de convolution, sous la forme d'opérations
non-linéaires sur les *feature maps*. À la sortie des extracteurs de
caractéristiques, les *feature maps* résultantes sont mises à plat grâce
à une couche de *flattening*, pour former l'entrée d'un perceptron
multicouche qui réalise la classification à partir des caractéristiques
apprises précédemment.

Un réseau de neurones convolutif 1D peut alors être utilisé pour
classifier des séries temporelles multivariées [1], en traitant
leurs variables de manière analogue à un réseau de neurones convolutif
2-D traîtant les canaux -- typiquement *RGB* -- d'une image. Ainsi, les
séries créées dans la première partie peuvent être fournies comme telles
à un réseau de neurones convolutif 1D. L'entrée du réseau est dans ce
cas à deux dimensions : la première est la taille d'une série, et la
seconde est le nombre de variables de la série, à l'instar du nombre de
canaux d'une image.


## Références

[1] Bendong Zhao et al. « Convolutional neural networks for time series clas-
sification ». In : _Journal of Systems Engineering and Electronics_ 28 (2017),
p. 162-169.

[2] Tae-Young Kim et Sung-Bae Cho. « Web traffic anomaly detection using
C-LSTM neural networks ». In : _Expert Syst. Appl._ 106 (2018), p. 66-76.

