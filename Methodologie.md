# Méthodologie du tri et de l'exploration de données
*PAr 114 - Yuexuan Kong & Thomas Jusseaume*

Hypothèses de travail
--

1. Les comparaisons à des placebos ou au soin médical usuel (*standard of care*) ne nous intéresseront pas pour la suite de notre étude des données.
2. Chaque bras que nous considérons dans notre étude provient d'un RCT (Randomized Controlled Trial).
3. Nous allons nous intéresser à des **bras** et non des **traitements** différents : deux bras n'apparaîtront pas sous la même entrée, quand bien même l'un des deux est composé en partie par le(s) traitement(s) de l'autre bras. (exemple : Remdesivir + baricitinib & Remdesivir ne seront pas sous la même entrée et cela s'applique même si le baricitinib une importance très faible par rapport au Remdesivir).
4. *A contrario*, deux bras seront rassemblés lors du tri de données s'ils sont composés des mêmes traitements **même** s'ils n'ont pas la même dose de chaque traitement (approche qualitative).

Tri des données
---
Comme expliqué précedemment, le tri des données va se faire exclusivement par **bras de traitement**, en filtrant notamment toutes les comparaisons à un placebo ou au soin médical usuel (*standard of care*). Cela s'explique par le fait que nous souhaitons mettre en évidence les **gaps de recherche d'efficacité**, soit les comparaisons traitement à traitement afin de déterminer lequel des bras du RCT est le plus efficace dans la lutte contre le COVID-19. Cela est fait avec ces lignes de code présentes dans le Jupyter Notebook de ce GitHub, plus précisément à la troisième ligne avec les conditions apposées à la création de la *DataFrame data* de *Pandas* :

> df = pd.read_excel("Database.xlsx")
> index = ["Treatment name","Treatment type1","n randomized in this arm","Total sample size"]
> data = pd.DataFrame(df,columns = index)[(df["Treatment type1"] !='*') & (df["n randomized in this arm"] != "*")]



Choix de la heatmap
---
Nous avons d'abord choisi une *heatmap* pour notre visualisation car c'était la mise en forme qui, a notre sens, mettait le mieux en évidence les **points chauds**, c'est-à-dire les bras ayant été les plus comparés. Cela permettait avec un simple code couleur de montrer facilement les gaps de recherche qui seraient *a contrario* des endroits noirs de la heatmap. Nous avons donc pour cela utilisé [Seaborn](https://seaborn.pydata.org/), module dont nous connaissions déjà l'existence au préalable et qui permettait la création facile de *heatmaps*.

Plusieurs problèmes se sont cependant posés une fois la première *heatmap* réalisée comme vous allez pouvoir le voir dans la partie suivante.

Abandon de la heatmap
---
En effet, nous avons pu voir qu'il y avait beaucoup trop de données pour que la visualisation soit lisible et claire. Nous nous sommes retrouvés avec un carré presque entièrement noir et quelques points de couleur difficilement discernables : nous avons notamment dû modifier le contraste et la saturation d'une capture de la heatmap pour pouvoir réellement discerner les points de couleur.
De plus, le nombre de données étant trop grand, nous ne pouvions afficher pour chaque ligne et colonne le nom du traitement ou du bras considéré. Cela empêchait donc toute lisibilité. 

Nous avions plusieurs solutions pour cela : pouvoir modifier à volonté le jeu de données affiché dans la heatmap afin de le restreindre à une taille plus claire ou changer de type de visualisation. Nous avons opté pour un compromis entre les deux car nous pensons que la diminution de l'échantillon sera de toute façon nécessaire afin de pouvoir analyser correctement la visualisation de notre jeu de données.

Liste des traitements les plus comparés
---
En trouvant les 10 maximums valeurs dans la matrice patients_nb où chaque caisse correspond au nombre de comparaisons faits entre deux traitements (un traitement correspondant à la ligne et un autre correspondant au colonne), on a réussi à récuperer ces 9 traitements les plus comparés. Par contre, il y a deux couples de traitemnts qui ont été comparés le même nombre de fois. Avec le code actuel, il ne récupère que le premier index dans la valeur retournée par la fonction argwhere (c'est-à-dire un seul couple de traitements). Il faudra qu'on modifie le code pour qu'il puisse donner une liste d'un nombre quelconque des traitements les plus comparés.

Voici la liste des 9 traitements les plus comparés :
[['hydroxychloroquine', 'antimalaria'],
 ['lopinavir+ritonavir', 'antiviral+antiretrovirals'],
 ['chloroquine', 'antimalaria'],
 ['placebo', 'control'],
 ['standard of care', 'control'],
 ['favipiravir', 'antiviral+broad spectrum'],
 ['tocilizumab', 'monoclonal antibodies'],
 ['umifenovir', 'antiviral+broad spectrum'],
 ['mesenchymal stem cells', 'atmp']]
