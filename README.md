# **Décryptage algorithme Yuka**
![](images/yuka.png?style=centerme)


## **Chapitre 1 : Introduction** 

1. **L’application Yuka :** 

Yuka est une application mobile pour iOS et Android, développée par Yuca SAS, qui scanne les aliments et les cosmétiques pour obtenir des informations détaillées sur les effets des produits sur la santé. Son objectif est d'aider les consommateurs à choisir des produits considérés comme bénéfiques pour leur santé et d'encourager les fabricants à améliorer les ingrédients de leurs produits. 

2. **Fonctionnement :** 

La lecture du code-barres d'un produit par téléphone permet à l'application d'accéder à des informations détaillées sur les ingrédients du produit et de renvoyer des commentaires dans des couleurs allant du vert au rouge et un score allant de 0 à 100. Lorsque son impact est jugé négatif, l'application peut vous recommander des produits similaires qui vous conviennent mieux. 

3. **Le score Yuka :** 

Ce que nous intéresse c’est la fonction d’attribution de ce score.  On veut décrypter la fonction de calcul de ce score en commençant par les informations disponibles sur le site de l’application qui dit que ce score est sous la forme suivante. 
<p align="center">
<b>Score Yuka= 0.6*U<sub>N</sub>(Nutri score) + 0.3* U<sub>A</sub>(additives) + 0.1* U<sub>B</sub>(Bio)</b>
</p>


On peut voir que les facteurs qui détermine la note d’un aliment allant de le plus impactant au moins sont le **Nutri Score**, les **additifs** et le critère **BIO**.  On va commencer détailler les entrées de cette formule et essayer de les comprendre chacun seul sans tenir en compte leurs corrélation. 

1. **Nutri-Score :** 

Le Nutri-Score a été développé pour faciliter la compréhension des informations nutritionnelles par les consommateurs  et  ainsi  de  les  aider  à  faire  des  choix  éclairés  d’où  la  lutte  contre  les  maladies cardiovasculaires, l'obésité et le diabète.[1]  Ce score se calcule à la base d’une évaluation de 100g ou 100mL d’un produit. Cette évaluation se divise sur deux comportements. Le premier c’est l’augmentation avec la quantité des nutriments favorable comme les fibres, les légumes, les fruits et les protéines. Le deuxième c’est la diminution avec les nutriments défavorables comme l’énergie, les acides gras saturés, les sucres et le sel. 

Ce score est une valeur comprise entre –15 et +40. Mais il se manifeste sous la forme d’un logo apposé en face avant des emballages qui informe sur la qualité nutritionnelle des produits sous une forme simplifiée. La représentation simplifiée ce décompose d’une lettre avec un couleur les lettres allant de A à E allant du meilleur au plus mauvais.   
<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/nutri_score.png?raw=true">   
</p>
<p align="center">
<b>Figure 1:Nutri Score</b>
</p>





- A correspondant à une valeur comprise entre –15 et –2 
- B correspondant à une valeur comprise entre –1 à +3 
- C correspondant à une valeur comprise entre +4 à +11 
- D correspondant à une valeur comprise entre +12 à +16  
- E correspondant à une valeur comprise entre +17 à +40 

2. **Additifs :** 

L’application extraite les additifs existants dans un aliment et puis il fait les évaluer en se basant sur une base qui est formé par des études indépendantes et des avis de l’EFSA, de l’ANSES et du CIRC. L’évaluation consiste à les classer sous 4 groupes : 

- Sans risque (pastille verte) 
- Risque limité (pastille jaune) 
- Risque modéré (pastille orange) 
- Risque élevé (pastille rouge) 

Les  informations  sur  les  risques  associés  à  chaque  additif,  ainsi  que  les  sources  scientifiques correspondantes sont affichés après la classification. 

<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/exemple.png?raw=true">   
</p><p align="center">
<b>Figure 2: Exemple Liste additifs et description
</b>
</p>

Ce critère compte de 30% du score Yuka. 

3. **BIO** 

Il s'agit d'une bonification accordée aux produits considérés comme biologiques. Les produits considérés comme biologiques portent un label biologique européen (Euro-feuille). 

<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/bio1.png?raw=true">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/bio2.png?raw=true">   
</p>

</p><p align="center">
<b>Figure 3: Exemple Bio   &emsp;  Figure 4:  Euro-feuille Label 
</b>
</p>

## **Chapitre 2 : Décryptage de l’algorithme de Yuka**  

Dans ce chapitre on va parler des travaux réalisés pour comprendre mieux l’algorithme de Yuka et le décrypter en partant de l’hypothèse que c’est un modèle additif. 

1. **Construction de la base :** 

Afin d’atteindre notre objectif on a besoin de préparer une base de données qui contient à la fois tous les éléments utilisés pour calculer le score Yuka et le score Yuka même. 
<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/base.png?raw=true">   
</p>
<p align="center">
<b>Figure 5: Composition de la base
</b>
</p>

- Composition du produit : 

On a pris les ingrédients à partir de « OpenFoodFacts ». OpenFoodFacts est une base de données sur les produits  alimentaires  faite  par  tout  le  monde,  pour  tout  le  monde. Cette  base  contient  presque  la composition détaillée de chaque aliment. 

- Score Yuka 

Pour les scores de Yuka, on est sorti au supermarché. On a scanné plus que 500 produits. En faisant la combinaison de la base OpenFoodFacts et les scores Yuka des produits scannés, on a eu une base complète pour commencer les analyses. 

<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/capture base.png?raw=true">   
</p>
<p align="center">
<b>Figure 6 : Capture Base
</b>
</p>

|     |   additives_n |   nova_group |   energy_100g |   fat_100g |   saturated-fat_100g |   sugars_100g |   fiber_100g |   proteins_100g |   salt_100g |   sodium_100g |   nutrition-score-fr_100g |   Score Yuka |
|----:|--------------:|-------------:|--------------:|-----------:|---------------------:|--------------:|-------------:|----------------:|------------:|--------------:|--------------------------:|-------------:|
|   0 |             1 |            4 |         590   |       4.3  |                0.5   |          5.5  |         1.8  |            6.2  |     0.59    |      0.236    |                         0 |           84 |
|   1 |           nan |          nan |        1477   |       6.1  |                1.5   |         10    |         9.9  |            5.4  |     5.5     |      2.2      |                         1 |           72 |
|   2 |           nan |          nan |         176   |       0    |                0     |          9.7  |         0    |            0.5  |     0       |      0        |                         3 |           59 |
|   3 |             0 |            3 |        1022   |      14    |                1.9   |          0.5  |         1.4  |           12    |     0.83    |      0.332    |                         1 |          nan |
|   4 |             1 |            1 |         196   |       0.5  |                0     |         11    |         0.5  |            0.7  |     0.1     |      0.04     |                         5 |           50 |
|   5 |             3 |            4 |        1126   |      12    |                2.5   |         19    |         2    |            6.1  |     0.26    |      0.104    |                         5 |           33 |
|   6 |             1 |            3 |        1004   |      14    |                5.3   |          0.5  |       nan    |           29    |     6.3     |      2.52     |                        17 |            2 |
|   7 |             1 |            1 |         136   |       0.1  |                0     |          5.1  |         1.5  |            1    |     0.03    |      0.012    |                        -5 |          100 |
|   8 |             2 |            1 |         241   |       0.6  |                0.2   |         11    |         1.3  |            0.5  |     0.01    |      0.004    |                        -4 |          100 |
|   9 |           nan |          nan |        1255   |      21    |                2.6   |          1.1  |       nan    |           18    |     1.1     |      0.44     |                         4 |           88 |
|  10 |             0 |            3 |        1544   |      36    |                5.7   |          0.5  |        10    |            1.9  |     8       |      3.2      |                        14 |           57 |
|  11 |             2 |            4 |        1067   |      21    |                1.8   |          0.5  |         1.5  |           15    |     0.35    |      0.14     |                        -1 |           78 |
|  12 |             9 |            4 |        1293   |       7.3  |                1.4   |          2.5  |         3    |            8.2  |     1.3     |      0.52     |                         1 |           45 |
|  13 |           nan |          nan |        2163   |      29    |                3.1   |         42    |       nan    |           17    |     0.005   |      0.002    |                        18 |           39 |
|  14 |             1 |            1 |        2829   |      67    |               59     |          6.3  |        15    |            7.7  |     0.06    |      0.024    |                         5 |           35 |
|  15 |             0 |            3 |         484   |       1.6  |                0.4   |          0    |         0    |           25    |     1.2     |      0.48     |                         1 |           75 |
|  16 |             1 |            4 |         576   |      12    |                8.6   |          4.1  |       nan    |            3    |     0.1     |      0.04     |                         8 |           39 |
|  17 |             0 |            4 |        1054   |      16    |                2.6   |          2.8  |       nan    |            8.9  |     0.77    |      0.308    |                       nan |          nan |
|  18 |             1 |            4 |        2204   |      29    |               10     |         57    |         3.4  |            6.1  |     0.13    |      0.052    |                        22 |           32 |
|  19 |             4 |            4 |        1094   |      22    |                7.6   |          0.6  |         0    |           16    |     2.5     |      1        |                        20 |            0 |
|  20 |             3 |            4 |         621   |       6.7  |                2.5   |          2.2  |         0.9  |            6.9  |     0.88    |      0.352    |                         2 |           75 |
|  21 |             1 |            3 |          73   |       0    |                0     |          2.6  |         1    |            0.9  |     0.48    |      0.192    |                        -4 |           90 |
|  22 |             5 |            4 |         265   |       1    |                0.7   |         11    |       nan    |            2.4  |     0.11    |      0.044    |                         1 |          nan |
|  23 |             5 |            4 |         267   |       1.1  |                0.8   |         11    |       nan    |            2.4  |     0.11    |      0.044    |                         1 |          nan |
|  24 |             3 |            4 |         454   |       3    |                1.1   |          1.2  |       nan    |           19    |     1.9     |      0.76     |                         5 |           33 |
|  25 |             0 |            3 |        1005   |      11    |                2.5   |          2.7  |         1.3  |           10    |     0.86    |      0.344    |                         1 |           75 |
|  26 |             0 |            3 |        2614   |      50    |                6.3   |          6.9  |         8.6  |           24    |     0.84    |      0.336    |                        12 |           72 |
|  27 |             4 |            4 |         272   |       1    |                0.7   |         11    |         0    |            2.4  |     0.1     |      0.04     |                         1 |          nan |
|  28 |             0 |            3 |         267   |       1.1  |                0.7   |         11    |       nan    |            2.6  |     0.11    |      0.044    |                         1 |          nan |
|  29 |             2 |            4 |        1004   |      24    |               17     |          1.9  |       nan    |            4.9  |     1       |      0.4      |                        13 |           20 |
|  30 |             7 |            4 |         991   |       8.3  |                3.8   |         21    |         1.2  |            3.9  |     0.18    |      0.072    |                         6 |           30 |
|  31 |             8 |            4 |         876   |       7    |                2.5   |         20    |         1.9  |            3.9  |     0.14    |      0.056    |                         5 |           33 |
|  32 |             1 |            4 |        1149   |      23    |               15     |          0.5  |         0.9  |           16    |     1.6     |      0.64     |                        15 |           24 |
|  33 |             5 |            4 |         615   |       8.3  |                2.5   |          0.6  |       nan    |           16    |     2       |      0.8      |                        11 |           23 |
|  34 |             1 |            4 |         143   |       0.5  |                0     |          8.4  |         0    |            0.5  |     0.01    |      0.004    |                        11 |           40 |
|  35 |             5 |            4 |        1192   |      16    |               11     |         29    |         0.5  |            2.4  |     0.2     |      0.08     |                        19 |            9 |
|  36 |             3 |            4 |         460   |       3    |                1.1   |          1.5  |       nan    |           19    |     1.4     |      0.56     |                         3 |           39 |
|  37 |             3 |            4 |         460   |       3    |                1.1   |          1.5  |         0    |           19    |     1.4     |      0.56     |                         3 |           39 |
|  38 |             3 |            4 |         460   |       3    |                1.1   |          1.5  |       nan    |           19    |     1.4     |      0.56     |                         3 |           39 |
|  39 |             3 |            4 |         946   |      11    |                3.3   |          2.4  |         3.6  |           10    |     0.92    |      0.368    |                         1 |           49 |
|  40 |             5 |            4 |        1946   |      27    |                5.5   |         41    |         3.5  |            6.2  |     0.51    |      0.204    |                        18 |            2 |
|  41 |             0 |            2 |        3378   |      91    |               13     |          0    |         0    |            0    |     0       |      0        |                         6 |           75 |
|  42 |             0 |            3 |        1005   |      11    |                2.5   |          2.7  |         1.3  |           10    |     0.86    |      0.344    |                         1 |           75 |
|  43 |             3 |            4 |           0   |      26    |               10     |         38    |         2    |            4.8  |     0.22    |      0.088    |                        15 |           24 |
|  44 |             2 |            4 |         984   |       0    |                0     |         55    |         2.8  |            0    |     0.02    |      0.008    |                         9 |           39 |
|  45 |           nan |          nan |         569   |       6.2  |                1.7   |          0.7  |       nan    |           19    |     1.8     |      0.72     |                         4 |           76 |
|  46 |             1 |            4 |        2231   |      32    |               20     |         49    |         7.6  |            6    |     0.02    |      0.008    |                        21 |           40 |
|  47 |             0 |          nan |          42   |       3    |                2     |          2    |       nan    |            2    |     2       |      0.8      |                         3 |           54 |
|  48 |             1 |            4 |         921   |      14    |                1.7   |          1.5  |         5.5  |            7.1  |     0.72    |      0.288    |                        -3 |           90 |
|  49 |             2 |            4 |         761   |      13    |                0.9   |         12    |       nan    |            2.4  |     0.48    |      0.192    |                         5 |           90 |
|  50 |             0 |            4 |         300   |       3.4  |                2.1   |          4.5  |         0    |            4    |     0.11    |      0.044    |                         0 |           78 |
|  51 |             0 |            3 |        1443   |      32    |               13     |          0.7  |         1.1  |           13    |     1.6     |      0.64     |                        20 |           30 |
|  52 |             0 |          nan |         142   |       0    |                0     |          4.3  |       nan    |            3.4  |     0.1     |      0.04     |                        -2 |          100 |
|  53 |             1 |            4 |        2000   |      51    |                5.6   |          5.2  |       nan    |            0    |     1.9     |      0.76     |                        19 |            0 |
|  54 |             4 |            4 |        1100   |      22    |                7.8   |          0.6  |       nan    |           16    |     2.1     |      0.84     |                        19 |            0 |
|  55 |             2 |            4 |         455   |       2.5  |                0.9   |          0    |       nan    |           21    |     1.4     |      0.56     |                         2 |           42 |
|  56 |             4 |            4 |        1018   |      19    |                7.5   |          0.6  |       nan    |           17    |     1.8     |      0.72     |                        17 |            2 |
|  57 |             2 |            4 |         465   |       3    |                1     |          0.8  |       nan    |           20    |     1.39954 |      0.559816 |                         2 |           42 |
|  58 |             2 |            4 |         452   |       2.5  |                0.9   |          0    |       nan    |           21    |     1.4     |      0.56     |                         2 |           42 |
|  59 |             1 |            3 |        1578   |      20    |               14     |          2    |         1.3  |            5.9  |     0.99    |      0.396    |                        17 |           32 |
|  60 |           nan |          nan |         238   |       2.1  |                1.3   |          4.2  |         2.6  |            1    |     0.68    |      0.272    |                         2 |          100 |
|  61 |             0 |            3 |        1888   |      19    |                5     |          2.2  |        10    |           17    |     1.9     |      0.76     |                        12 |           48 |
|  62 |           nan |          nan |         356   |       0.4  |                0     |          0.6  |       nan    |            5.3  |     0.6     |      0.24     |                         0 |           75 |
|  63 |             5 |            4 |        1512   |      23    |               13     |         28    |         1.6  |            4.9  |     0.18    |      0.072    |                        19 |            1 |
|  64 |             4 |            4 |         611   |       5.9  |                1.5   |          2.5  |         1.2  |            5.6  |     0.6     |      0.24     |                         0 |           78 |
|  65 |             1 |            4 |         293   |       1.8  |                0.4   |          9.4  |       nan    |            3.3  |     0.1     |      0.04     |                         0 |            0 |
|  66 |             3 |            4 |         845   |       5.2  |                2.8   |          4.2  |         2.3  |           11    |     1.1     |      0.44     |                         1 |           45 |
|  67 |           nan |          nan |         741   |       6.4  |                0.6   |          0.6  |       nan    |           20    |     0.91    |      0.364    |                         1 |           84 |
|  68 |             1 |            4 |         154   |       1.5  |                1.1   |          2.3  |         1.3  |            0    |     0.57    |      0.228    |                         1 |           75 |
|  69 |             3 |            4 |         774   |      18    |                2.4   |          2.1  |         0.6  |            3.1  |     0.03    |      0.012    |                         3 |          nan |
|  70 |             4 |            4 |         787   |       0    |                0     |          3.8  |         0.9  |            0    |     0.03    |      0.012    |                         1 |           60 |
|  71 |           nan |          nan |        1134   |      24    |                8.9   |          1.1  |       nan    |           13    |     1.8     |      0.72     |                        18 |            1 |
|  72 |             6 |            4 |         930   |       8.2  |                4     |          4.5  |         3.1  |           11    |     1.30048 |      0.520192 |                         2 |           49 |
|  73 |             0 |            4 |        1008   |       8    |                4.9   |          2.6  |         2.4  |           11    |     1.6     |      0.64     |                        12 |           48 |
|  74 |           nan |          nan |        1121   |      12    |                5.6   |          3    |       nan    |           11    |     1.5     |      0.6      |                        14 |           23 |
|  75 |           nan |          nan |         795   |       4.8  |                2.9   |          5    |         1.8  |            9    |     1.2     |      0.48     |                         4 |           69 |
|  76 |             0 |            4 |        1114   |      11    |                5.6   |          3.2  |         1.3  |           12    |     1       |      0.4      |                        11 |          nan |
|  77 |             0 |            4 |         891   |       7.4  |                3     |          4.4  |         2.1  |            7.6  |     1.1     |      0.44     |                         2 |           72 |
|  78 |             1 |            4 |         238   |       2.1  |                1.5   |          4.8  |       nan    |            0.8  |     0.8     |      0.32     |                         5 |           57 |
|  79 |             4 |            4 |         745   |       9.8  |                6     |         16    |       nan    |            4.5  |     0.12    |      0.048    |                         8 |           54 |
|  80 |             0 |            3 |        1783   |      35    |               23     |          1    |         1.5  |           27    |     0.8     |      0.32     |                        12 |           39 |
|  81 |           nan |          nan |        1515   |      28    |               18     |          1    |       nan    |           26    |     1.5     |      0.6      |                        15 |           30 |
|  82 |             2 |            4 |        1110   |       3.8  |                0.4   |          4.8  |         5.5  |            8.4  |     1.1     |      0.44     |                        -2 |           75 |
|  83 |             1 |            4 |        1506   |      27    |               16     |         21    |         1.5  |            6.1  |     0.18    |      0.072    |                        17 |           32 |
|  84 |           nan |          nan |         916   |       7.1  |                3.8   |          2.3  |       nan    |           12    |     1.8     |      0.72     |                        12 |            9 |
|  85 |             0 |            3 |        1611   |       0.7  |                0     |          0.6  |         5.4  |            6.5  |     0.28    |      0.112    |                        -4 |          100 |
|  86 |             6 |            4 |         397   |       4.4  |                2.2   |          0.8  |         1.6  |            3.3  |     0.63    |      0.252    |                         2 |           48 |
|  87 |             5 |            4 |         275   |       3.1  |                0.7   |          0    |       nan    |            8.7  |     5       |      2        |                         5 |           33 |
|  88 |             1 |            4 |        1257   |      28    |               11     |          0.5  |       nan    |           14    |     1.3     |      0.52     |                        18 |           31 |
|  89 |             0 |            1 |         544   |       5    |                2.3   |          0    |         0    |           12    |     0       |      0        |                        -2 |           90 |
|  90 |             2 |            4 |         764   |       9.5  |                0.8   |          2.2  |         2    |            3.7  |     1.3     |      0.52     |                         3 |           69 |
|  91 |             2 |            4 |        1202   |      30    |               20     |          3.2  |       nan    |            2.2  |     0.1     |      0.04     |                        13 |            7 |
|  92 |             0 |            1 |        1046   |       5    |                5     |          0    |       nan    |           12    |     2.5     |      1        |                        17 |          nan |
|  93 |             3 |            4 |         506   |       3.7  |                1.1   |          0.8  |       nan    |           21    |     2.9     |      1.16     |                        12 |            8 |
|  94 |             2 |            4 |         477   |       2.8  |                1.1   |          0.7  |       nan    |           21    |     1.8     |      0.72     |                         4 |           36 |
|  95 |             3 |            4 |         454   |       3    |                1.1   |          1.2  |         0    |           19    |     1.9     |      0.76     |                         5 |           33 |
|  96 |             3 |            4 |         470   |       3.2  |                1     |          0.9  |       nan    |           20    |     1.9     |      0.76     |                         4 |           36 |
|  97 |             3 |            4 |         515   |       3.7  |                1.5   |          0.8  |       nan    |           22    |     2.2     |      0.88     |                        11 |            9 |
|  98 |             4 |            4 |        1084   |      23    |                7.8   |          1.3  |       nan    |           11    |     1.7     |      0.68     |                        17 |            2 |
|  99 |             5 |            4 |         925   |      12    |                2.3   |          2.1  |         2.3  |           11    |     1.3     |      0.52     |                         2 |           45 |
| 100 |             4 |            4 |         895   |      11    |                3     |          2.1  |         1.4  |           13    |     1.42    |      0.568    |                         4 |           45 |
| 101 |             4 |            4 |        1293   |      28    |                8.2   |          1.2  |       nan    |           14    |     2       |      0.8      |                        19 |            0 |
| 102 |             3 |            4 |        1123   |      24    |                8.9   |          1.1  |       nan    |           13    |     1.8     |      0.72     |                        18 |            1 |
| 103 |             3 |            4 |        1688   |      24    |               12     |          1.5  |         1.4  |            5.4  |     0.88    |      0.352    |                        17 |           32 |
| 104 |             0 |            3 |        1200   |      30    |               18     |          2.9  |       nan    |            2.4  |     0.07    |      0.028    |                        13 |           30 |
| 105 |             1 |            4 |        1200   |      30    |               20     |          3.3  |       nan    |            2    |     0.08    |      0.032    |                        13 |           22 |
| 106 |             3 |            4 |        1708   |      23    |               11     |          3    |         1.3  |            5.7  |     0.89    |      0.356    |                        17 |           31 |
| 107 |             0 |            4 |         300   |       3.4  |                2.1   |          4.5  |       nan    |            4    |     0.11    |      0.044    |                         0 |           78 |
| 108 |           nan |          nan |         180   |       1.1  |                0.7   |          4.2  |         0    |            4    |     0.13    |      0.052    |                        -2 |           90 |
| 109 |             0 |            3 |         295   |       3    |                1.9   |          3.3  |       nan    |            7.5  |     0.1     |      0.04     |                        -3 |           90 |
| 110 |             0 |            1 |         594   |       9.9  |                2.6   |          0.8  |         0    |           12.6  |     0.3     |      0.12     |                        -1 |           84 |
| 111 |             0 |            1 |        1410   |       1.1  |                0.3   |          0.9  |         5    |            9.1  |     0       |      0        |                        -6 |           90 |
| 112 |             3 |            4 |        1305   |       3.5  |                1     |          6.3  |         3.1  |           12    |    20       |      8        |                        11 |           32 |
| 113 |           nan |          nan |         192   |       1.6  |                1     |          4.8  |       nan    |            3.2  |     0.13    |      0.052    |                         0 |          nan |
| 114 |           nan |          nan |         431   |       2    |                0.6   |          0.7  |         0.5  |           20    |     1.8     |      0.72     |                         3 |          nan |
| 115 |             0 |            3 |        1552   |       1    |                0.3   |          6    |         3    |            8    |     1.5     |      0.6      |                         8 |           75 |
| 116 |             2 |            3 |          75   |       0    |                0     |          2    |         1.1  |            0.9  |     0.34    |      0.136    |                        -5 |           90 |
| 117 |             2 |            4 |        1485   |      27    |               11     |          0    |         0    |           26    |     4.9     |      1.96     |                        24 |            0 |
| 118 |           nan |          nan |        1937   |      17    |                2     |         21    |       nan    |            6.4  |     0.65    |      0.26     |                        12 |           36 |
| 119 |             2 |            4 |         456   |       2.1  |                0.7   |          0.8  |       nan    |           22    |     1.9     |      0.76     |                         4 |           39 |
| 120 |             1 |            3 |         906   |      11    |                3.9   |          0.1  |       nan    |           29    |     6.2     |      2.48     |                        15 |           44 |
| 121 |             1 |            4 |         481   |       4.2  |                0.9   |          2.7  |         2.3  |            8.2  |     0.57    |      0.228    |                        -4 |           90 |
| 122 |             3 |            4 |         420   |       2.7  |                1.7   |         11    |       nan    |            6.6  |     0.09    |      0.036    |                         0 |           48 |
| 123 |           nan |          nan |         130   |       1.4  |                0.3   |          0.3  |       nan    |            0.6  |     0.7     |      0.28     |                         3 |           69 |
| 124 |           nan |          nan |        1397   |      35    |               21     |          2.7  |       nan    |            1.9  |     0.09    |      0.036    |                        14 |          nan |
| 125 |           nan |          nan |         255   |       0    |                0     |         13    |       nan    |            0.5  |     0       |      0        |                        -3 |           90 |
| 126 |             0 |            1 |        1506   |       2    |                0.3   |          2    |         3    |           12    |     0       |      0        |                        -4 |           90 |
| 127 |             0 |            1 |        1460   |       6.5  |                1.1   |          1    |        10    |           12    |     0.03    |      0.012    |                        -5 |          100 |
| 128 |             0 |            4 |         546   |       4.9  |                1.1   |          3.1  |         1.4  |            4.8  |     0.55    |      0.22     |                         1 |           69 |
| 129 |             3 |            4 |         524   |       4.7  |                1.9   |          1.4  |         0    |           19    |     1.8     |      0.72     |                         4 |           36 |
| 130 |             6 |            4 |         908   |       7.9  |                1     |          3.7  |         3.7  |            8.1  |     1.2     |      0.48     |                        -1 |           69 |
| 131 |            14 |            4 |        1109   |      12    |                6.1   |          5.2  |         1.7  |           10    |     1.9     |      0.76     |                        17 |            3 |
| 132 |             2 |            4 |        1510   |      31    |               13     |          1.6  |       nan    |           26    |     4.3     |      1.72     |                        24 |            0 |
| 133 |             3 |            4 |         710   |       9.3  |                4.6   |          1.4  |         0.7  |            6.2  |     0.68    |      0.272    |                         6 |           30 |
| 134 |             2 |            4 |        1983   |      38    |               15     |          3.5  |         0    |           29    |     4.7     |      1.88     |                        25 |            0 |
| 135 |           nan |          nan |         757   |      10    |                1.7   |          0    |         0.5  |           22    |     3       |      1.2      |                        13 |           37 |
| 136 |             1 |            4 |         576   |       6.7  |                1     |          1.9  |         1.5  |            7.6  |     0.75    |      0.3      |                        -1 |           90 |
| 137 |             0 |            4 |         285   |       1.2  |                0.8   |         10    |       nan    |            2.7  |     0.09    |      0.036    |                         1 |           30 |
| 138 |             2 |            4 |           4   |       1    |                1     |          1    |       nan    |            1    |     0       |      0        |                         0 |          nan |
| 139 |             0 |            4 |         163   |       1    |                1     |          4.1  |         0    |            3.4  |     0.1     |      0.04     |                        -2 |           90 |
| 140 |             0 |            4 |         238   |       4.5  |                2     |          0.7  |         0    |            2.2  |     0.7     |      0.28     |                         3 |           69 |
| 141 |             2 |            4 |         163   |       0.5  |                0     |          6.1  |         2    |            1.3  |     0.25    |      0.1      |                        -5 |           90 |
| 142 |             1 |            4 |         820   |       0    |                0     |         47    |         1    |            0    |     0       |      0        |                        10 |           48 |
| 143 |           nan |          nan |         177   |       0    |                0     |          9.4  |       nan    |            0.7  |     0       |      0        |                         3 |           59 |
| 144 |             0 |            1 |         169   |       0.5  |                0.1   |          9.4  |       nan    |            0.7  |     0.01    |      0.004    |                        13 |           59 |
| 145 |             0 |          nan |         174   |       0.5  |                0     |          9.8  |         0.5  |            0.5  |     0.1     |      0.04     |                         3 |           59 |
| 146 |           nan |          nan |         393   |       4.5  |                0.3   |          4.9  |       nan    |            4.5  |     3.7     |      1.48     |                        12 |           57 |
| 147 |             4 |            4 |        2185   |      33    |               20     |         40    |         8.5  |            7.7  |     0.03    |      0.012    |                        19 |           13 |
| 148 |             3 |            4 |         110   |       0    |                0     |          6.1  |         0.5  |            0.5  |     0.01    |      0.004    |                         9 |           26 |
| 149 |             0 |            4 |         369   |       0    |                0.1   |          4.6  |         5.8  |            5.4  |     0.65    |      0.26     |                        -9 |           90 |
| 150 |             2 |            4 |         736   |       7.5  |                0.7   |          1.6  |         1.4  |            6.3  |     0.7     |      0.28     |                         1 |           75 |
| 151 |             4 |            4 |         473   |       2.9  |                1.1   |          2.2  |       nan    |           19    |     2.2     |      0.88     |                        11 |            9 |
| 152 |             4 |            4 |         698   |      10    |                4     |          0.6  |       nan    |           19    |     2.2     |      0.88     |                        14 |            5 |
| 153 |           nan |          nan |        1699   |      21    |               11     |         20    |       nan    |            5.1  |     0.47    |      0.188    |                        21 |            1 |
| 154 |             0 |            1 |         176   |       0.5  |                0.1   |          8.7  |         0    |            0.7  |     0       |      0        |                         2 |           64 |
| 155 |             0 |            1 |         197   |       0    |                0     |         11    |       nan    |            0.5  |     0       |      0        |                         5 |           50 |
| 156 |             1 |            4 |        2653   |      54    |               42     |         28    |         7.2  |            4.5  |     0.03    |      0.012    |                        18 |           40 |
| 157 |             1 |            4 |        2343   |      36    |                5.8   |         51    |         2.9  |            5.5  |     0.1     |      0.04     |                        18 |           26 |
| 158 |             2 |            4 |         745   |       9    |                5.5   |         17    |         1.4  |            4.6  |     0.1     |      0.04     |                         7 |           57 |
| 159 |             1 |            4 |         154   |       1.5  |                1.1   |          2.3  |         1.3  |            0.5  |     0.57    |      0.228    |                         1 |           75 |
| 160 |             3 |            4 |        1000   |      25    |                1.8   |          1    |       nan    |            0.3  |     2       |      0.8      |                        11 |           33 |
| 161 |             5 |            4 |        1544   |      14    |                1.3   |         12    |         1.3  |            7.7  |     1       |      0.4      |                        10 |           18 |
| 162 |             7 |            4 |        1841   |      41    |                3.3   |         12    |         0.9  |            1.3  |     1.6     |      0.64     |                        17 |           18 |
| 163 |             0 |            3 |        1498   |      32    |               22     |          0    |         0    |           17    |     1.3     |      0.52     |                        14 |           35 |
| 164 |             2 |            4 |         946   |      19    |                7.4   |          0.9  |         0.5  |           13    |     2.1     |      0.84     |                        18 |            1 |
| 165 |             1 |            4 |        1272   |      27    |               11     |          0.22 |       nan    |           16    |     2       |      0.8      |                        21 |            0 |
| 166 |             3 |            4 |        1004   |      19    |                7     |          0.7  |       nan    |           17    |     2.4     |      0.96     |                        18 |            0 |
| 167 |           nan |          nan |         649   |       8    |                3.5   |          0.8  |         0    |           20    |     2       |      0.8      |                        12 |            8 |
| 168 |             1 |            4 |        1147   |      23    |                8.7   |          0.5  |       nan    |           16    |     2.8     |      1.12     |                        21 |            0 |
| 169 |             8 |            4 |         723   |       7.5  |                4.2   |          1    |         1.7  |           12    |     1.1     |      0.44     |                         4 |           39 |
| 170 |             1 |            4 |         649   |       8    |                3.5   |          0.8  |         0    |           20    |     2       |      0.8      |                        12 |            8 |
| 171 |             5 |            4 |         435   |       8    |                0.6   |          5.5  |         2    |            1    |     0.8     |      0.32     |                         3 |           60 |
| 172 |             1 |            4 |        1343   |      24    |               17     |          0    |       nan    |           25    |     2       |      0.8      |                        17 |           26 |
| 173 |             0 |            3 |        1636   |      30    |               18     |          0    |         1.1  |           29    |     0.5     |      0.2      |                        10 |           58 |
| 174 |             3 |            4 |        1501   |      33    |               12     |          1    |         1    |           13    |     2       |      0.8      |                        21 |            0 |
| 175 |             2 |            4 |         460   |       3    |                1.2   |          0.8  |       nan    |           20    |     2       |      0.8      |                         5 |           33 |
| 176 |             4 |            4 |        1669   |      40    |               15     |          2    |       nan    |            9    |     1.6     |      0.64     |                        21 |            0 |
| 177 |             3 |            3 |         904   |      10    |                3.9   |          1    |       nan    |           30    |     5.9     |      2.36     |                        15 |            4 |
| 178 |             9 |            4 |         558   |       8.8  |                1     |          0.9  |         1.4  |            4    |     1.1     |      0.44     |                         2 |           48 |
| 179 |             2 |            4 |         405   |       9.3  |                0.7   |          1.3  |         1    |            1.2  |     0.8     |      0.32     |                         1 |           84 |
| 180 |             3 |            4 |         350   |       6.8  |                1.1   |          1.9  |         2.5  |            1    |     0.92    |      0.368    |                         2 |           72 |
| 181 |             4 |            4 |         700   |      10    |                4     |          0.7  |       nan    |           19    |     2.2     |      0.88     |                        14 |            1 |
| 182 |             3 |            4 |         454   |       3    |                1.1   |          1.2  |       nan    |           19    |     1.9     |      0.76     |                         5 |           33 |
| 183 |             0 |            3 |        1071   |      20    |               14     |          0    |         0    |           19    |     1.4     |      0.56     |                        14 |           35 |
| 184 |             3 |            4 |         779   |      18    |               13     |          3.7  |         0    |            2.7  |     0.09    |      0.036    |                        12 |            8 |
| 185 |             0 |            3 |        1576   |      29    |               20     |          1    |         1.2  |           28    |     0.6     |      0.24     |                        10 |           38 |
| 186 |             0 |            3 |        1506   |      28    |               20     |          0.01 |         0    |           27    |     0.9     |      0.36     |                        12 |           38 |
| 187 |             0 |            3 |        1207   |      30    |               20     |          2.9  |         0.5  |            2.3  |     0.09    |      0.036    |                        13 |           37 |
| 188 |             6 |            4 |         536   |       5.4  |                3.6   |         15    |       nan    |            2.8  |     0.2     |      0.08     |                         6 |           33 |
| 189 |             7 |            4 |         393   |       2.7  |                1.8   |         14    |       nan    |            3.3  |     0.1     |      0.04     |                         3 |           63 |
| 190 |             1 |            1 |          61   |       0.3  |                0     |          0    |         2    |            1    |     0.12446 |      0.049784 |                        -7 |           90 |
| 191 |             2 |            4 |         341   |       1.7  |                0.3   |         11    |         1.4  |            3.1  |     0.08    |      0.032    |                         1 |           75 |
| 192 |             3 |            4 |        1477   |      11    |                7.5   |         12    |         1.5  |            7.2  |     0.77    |      0.308    |                        15 |           14 |
| 193 |             0 |            4 |        2569   |      54    |               34     |          9.7  |        13    |           12    |     0.03    |      0.012    |                        14 |           42 |
| 194 |             1 |            4 |        2139   |      27    |                2.1   |          4.4  |         3.9  |            4.2  |     1.2     |      0.48     |                         9 |           36 |
| 195 |           nan |          nan |         378   |       3.5  |                2.3   |         11    |       nan    |            3.2  |     0.1     |      0.04     |                         4 |           69 |
| 196 |             2 |            4 |        1000   |       0    |                0     |         59    |         1.4  |            0    |     0       |      0        |                        10 |           48 |
| 197 |             2 |            4 |        1150   |       5.3  |                0.6   |          4.6  |         5.2  |           10    |     1.3     |      0.52     |                        -1 |           54 |
| 198 |             4 |            4 |        1067   |      21    |                8.3   |          1.1  |       nan    |           15    |     2.2     |      0.88     |                        20 |           10 |
| 199 |           nan |          nan |         556   |       5.8  |                2.4   |          2.7  |       nan    |            4.8  |     0.6     |      0.24     |                         3 |           72 |
| 200 |           nan |          nan |         971   |       0    |                0     |         55    |       nan    |            0    |     0       |      0        |                         7 |           54 |
| 201 |           nan |          nan |         494   |       1.3  |                0.1   |          3.5  |       nan    |            3    |     0.53    |      0.212    |                        -3 |           90 |
| 202 |           nan |          nan |        1536   |       5    |                1.3   |          2.6  |       nan    |           15    |     0.12    |      0.048    |                         0 |           90 |
| 203 |             4 |            4 |        1156   |       4.9  |                0.8   |          5.8  |         5.4  |            9.7  |     1.1     |      0.44     |                        -2 |           60 |
| 204 |             3 |            4 |        1005   |       3.4  |                0.7   |          7    |         5.5  |            8.1  |     1       |      0.4      |                        -3 |           60 |
| 205 |             5 |            4 |        2142   |      23    |               14     |         36    |         4.3  |            6.9  |     0.75    |      0.3      |                        22 |            0 |
| 206 |             3 |            4 |        1469   |      26    |                9     |          1.6  |         1.2  |           27    |     4.4     |      1.76     |                        21 |            0 |
| 207 |             0 |            1 |         193   |       1.6  |                1     |          4.8  |       nan    |            3.2  |     0.09    |      0.036    |                         0 |           94 |
| 208 |             2 |            3 |         594   |      14    |                2     |          0    |         5.1  |            1    |     2.7     |      1.08     |                         2 |           57 |
| 209 |             1 |            3 |         519   |      13    |                2     |          0    |       nan    |            0.8  |     3.5     |      1.4      |                        12 |           57 |
| 210 |             1 |            3 |         253   |       0    |                0     |         12    |         1.9  |            0    |     0.02    |      0.008    |                        -4 |           84 |
| 211 |             2 |            4 |        1181   |       3.4  |                0.7   |          5.9  |         1.9  |            8    |     1.2     |      0.48     |                         4 |           60 |
| 212 |             0 |          nan |         335   |       0.7  |                0.16  |          1    |       nan    |            1.86 |     0       |      0        |                        -6 |           84 |
| 213 |           nan |          nan |         347   |       4    |                3     |         11    |       nan    |            6.3  |     0.96    |      0.384    |                         6 |          100 |
| 214 |           nan |          nan |         364   |       0    |                0     |          0    |       nan    |            6.6  |     0.69    |      0.276    |                         0 |           75 |
| 215 |             0 |            2 |        3378   |      91    |               13     |          0    |       nan    |            0    |     0       |      0        |                         6 |           82 |
| 216 |           nan |          nan |          67   |       0.5  |                0.2   |          0.5  |         2    |            1.2  |     1.5     |      0.6      |                         4 |           63 |
| 217 |             2 |            4 |        1138   |      10.7  |                1.4   |         32.4  |        11.9  |            5.6  |     1.02    |      0.408    |                        10 |           48 |
| 218 |             0 |            4 |         211   |       0.5  |                0.1   |          3.7  |         4.1  |            2.8  |     0.5     |      0.2      |                        -5 |           90 |
| 219 |             1 |            4 |         713   |       9.5  |                1     |          3.6  |         5.2  |           14    |     0.86    |      0.344    |                        -5 |          100 |
| 220 |             1 |            4 |        1653   |      17.9  |               12.5   |         22.3  |       nan    |            8.1  |     0.76    |      0.304    |                        21 |           30 |
| 221 |             7 |            4 |         502   |       3.1  |                0.3   |          2.4  |         0    |            6.8  |     1.5     |      0.6      |                         3 |           39 |
| 222 |             0 |          nan |         221   |       2.4  |                1.7   |          3.7  |       nan    |            3.5  |     0.1     |      0.04     |                        -1 |           69 |
| 223 |           nan |          nan |         331   |       0.5  |                0.1   |         15    |       nan    |            4.2  |     0.16    |      0.064    |                         1 |           84 |
| 224 |             4 |            4 |        1389   |       8    |                0.9   |          1.3  |       nan    |            9.3  |     1.3     |      0.52     |                         4 |           48 |
| 225 |           nan |          nan |        1536   |       6.8  |                6.3   |         67    |       nan    |            2    |     0.31    |      0.124    |                        21 |           34 |
| 226 |           nan |          nan |        2054   |      24    |               11     |         33    |       nan    |            5.5  |     1.3     |      0.52     |                        28 |            0 |
| 227 |           nan |          nan |        1423   |      35    |                4.9   |          3.6  |       nan    |            0.6  |     0.95    |      0.38     |                        12 |           32 |
| 228 |           nan |          nan |         682   |      16    |                1.6   |          0.6  |       nan    |            3.8  |     0       |      0        |                         1 |           84 |
| 229 |           nan |          nan |         883   |      10    |                3.6   |          0.5  |       nan    |           30    |     3.2     |      1.28     |                        15 |            4 |
| 230 |           nan |          nan |        1849   |      12    |                1.2   |         21    |       nan    |            7    |     1       |      0.4      |                        14 |           36 |
| 231 |           nan |          nan |        1594   |       6.4  |                3.8   |         24    |         8.8  |            8.6  |     1.1     |      0.44     |                        11 |           51 |
| 232 |           nan |          nan |         389   |       1    |                0     |          2.4  |       nan    |            0.8  |     0.25    |      0.1      |                         2 |           90 |
| 233 |           nan |          nan |        1527   |      13    |                7.9   |         31    |       nan    |            6.7  |     0.47    |      0.188    |                        19 |           12 |
| 234 |           nan |          nan |         192   |       1.6  |                1.1   |          4.7  |       nan    |            3.1  |     0.13    |      0.052    |                         1 |           75 |
| 235 |             3 |            4 |         578   |       5.1  |                4.3   |         18    |         1.5  |            3.7  |     0.2     |      0.08     |                         5 |          nan |
| 236 |             3 |            4 |         355   |       1.6  |                0.3   |         10    |         1    |            2.9  |     0.15    |      0.06     |                         1 |           45 |
| 237 |             2 |            1 |         225   |       0    |                0     |         11    |         1.7  |            0    |     0.04    |      0.016    |                        -4 |           90 |
| 238 |             1 |            4 |         190   |       2.3  |                0.3   |          0.5  |         0.8  |            3.7  |     0.08    |      0.032    |                        -2 |           90 |
| 239 |             0 |            1 |        1192   |       0    |                0     |         58    |         6.9  |            2.2  |     0.1     |      0.04     |                         2 |           82 |
| 240 |             2 |            4 |         473   |       2.1  |                0.6   |         15    |         1.7  |            3.5  |     0.1     |      0.04     |                         1 |          nan |
| 241 |             0 |            4 |         862   |      11    |                6.6   |          2.7  |         4    |            5.1  |     0.9     |      0.36     |                         7 |           48 |
| 242 |             2 |            4 |         339   |       1.8  |                0.2   |         12    |       nan    |            3    |     0.05    |      0.02     |                         2 |           42 |
| 243 |           nan |          nan |        2167   |      27    |               18     |         25    |       nan    |            6    |     0.6     |      0.24     |                        23 |            0 |
| 244 |             0 |          nan |        1318   |      27    |               18     |          0.5  |       nan    |           17    |     1.8     |      0.72     |                        15 |           30 |
| 245 |             0 |            3 |        1234   |      23    |               16     |          0    |       nan    |           21    |     1.2     |      0.48     |                        13 |           31 |
| 246 |           nan |          nan |        1213   |      23    |               16     |          0    |         0    |           20    |     1.7     |      0.68     |                        15 |           35 |
| 247 |             0 |            3 |         493   |       2.4  |                0.4   |          0.5  |         7    |            6.5  |     0.5     |      0.2      |                        -6 |           90 |
| 248 |             0 |            3 |        3090   |      83    |               58     |          0.7  |         0    |            0.6  |     0.02    |      0.008    |                        19 |          nan |
| 249 |             0 |            3 |        3058   |      82    |               60     |          0.6  |         0    |            0.8  |     0.05    |      0.02     |                        19 |          nan |
| 250 |           nan |          nan |        3113   |      82    |               53     |          0.6  |       nan    |            0.7  |     0.04    |      0.016    |                        19 |           35 |
| 251 |           nan |          nan |         205   |       0    |                0     |          3.3  |         1.9  |            0.7  |     0.03    |      0.012    |                        -6 |           60 |
| 252 |           nan |          nan |         377   |       2.8  |                0.6   |         13    |       nan    |            0.8  |     0.09    |      0.036    |                         3 |           24 |
| 253 |           nan |          nan |         113   |       0    |                0     |          4.8  |       nan    |            0    |     0.68    |      0.272    |                         4 |           69 |
| 254 |           nan |          nan |        1481   |      27    |               15     |          0    |       nan    |           28    |     1.3     |      0.52     |                        19 |           29 |
| 255 |           nan |          nan |        2648   |      70    |                6     |          0.5  |       nan    |            0.9  |     1.3     |      0.52     |                        17 |           34 |
| 256 |           nan |          nan |        1552   |       6.7  |                1.4   |          0.9  |         9.1  |           12    |     0.01    |      0.004    |                        -5 |           90 |
| 257 |             0 |            3 |        1282   |      25    |               18     |          0    |         0    |           21    |     1.5     |      0.6      |                        14 |           30 |
| 258 |             0 |            1 |         883   |      15    |                6.2   |          0    |         0    |           15    |     0.25    |      0.1      |                         4 |           66 |
| 259 |           nan |          nan |        1741   |       8.8  |                2.4   |         12    |         1.2  |           11    |     1.2     |      0.48     |                        13 |           51 |
| 260 |             0 |            4 |         263   |       0.6  |                0.2   |          3.7  |         4.2  |            3.4  |     0.57    |      0.228    |                        -9 |           90 |
| 261 |             9 |            4 |          41   |       0    |                0     |          0.7  |         0.5  |            0    |     0.1     |      0.04     |                         3 |           29 |
| 262 |             2 |            3 |        1254   |       0.8  |                0.2   |          2.4  |         2.2  |           10    |     0.89    |      0.356    |                        -1 |           75 |
| 263 |             5 |            4 |         938   |      11    |                1.7   |          5.9  |         2.6  |           11    |     1.1     |      0.44     |                         1 |           72 |
| 264 |             0 |            3 |        1603   |       1.4  |                0.6   |         16    |         5.2  |            7.9  |     0.8     |      0.32     |                         1 |           85 |
| 265 |             0 |            3 |        1540   |      32    |               24     |          0    |         0    |           19    |     3.2     |      1.28     |                        19 |           30 |
| 266 |           nan |          nan |         130   |       5.68 |                0.024 |          1.57 |       nan    |            1.35 |     0       |      0        |                        -5 |           90 |
| 267 |             5 |            4 |         494   |       7.2  |                0.7   |          0.7  |         1.4  |            3.7  |     0.95    |      0.38     |                         2 |           42 |
| 268 |             0 |            1 |           0   |       0    |                0     |          0    |         0    |            0    |     0       |      0        |                         0 |           90 |
| 269 |             4 |            4 |         732   |       7    |                4.4   |         22    |         1.5  |            5.1  |     0.14    |      0.056    |                         6 |           48 |
| 270 |             0 |          nan |         176   |       0    |                0     |          8.7  |         0    |            0.7  |     0       |      0        |                        12 |           64 |
| 271 |           nan |          nan |        1690   |       6.3  |                0.8   |          6.8  |       nan    |           12    |     1.1     |      0.44     |                         5 |           78 |
| 272 |             0 |          nan |        1458   |       2    |                0.5   |          3.5  |         3    |           12    |     0.03    |      0.012    |                        -4 |           90 |
| 273 |             3 |            4 |        2086   |      26    |                2.2   |          5    |         3.8  |            4.3  |     1.2     |      0.48     |                        10 |           21 |
| 274 |             6 |            4 |        1393   |       0    |                0     |         60    |       nan    |            4.6  |     0.03    |      0.012    |                        14 |            8 |
| 275 |             0 |            4 |         714   |       6.3  |                0.6   |          3.8  |         3    |            3.9  |     0.93    |      0.372    |                         1 |           72 |
| 276 |             0 |            3 |         703   |       1.6  |                0.3   |          0.5  |         3.8  |            5.8  |     1.3     |      0.52     |                         0 |           88 |
| 277 |             0 |            1 |         164   |       0    |                0     |          8.7  |       nan    |            0.6  |     0       |      0        |                         2 |           64 |
| 278 |             5 |            4 |         787   |      11    |                3.2   |          2.6  |         1.1  |            9    |     0.9     |      0.36     |                         2 |           54 |
| 279 |             0 |            3 |          66   |       0.5  |                0.5   |          0.6  |         2.9  |            1.4  |     0.5     |      0.2      |                        -6 |           84 |
| 280 |             0 |          nan |        1530   |       6.6  |                1.2   |          1.1  |        11    |           12    |     0.01    |      0.004    |                        -5 |           90 |
| 281 |             7 |            4 |        1419   |       1.1  |                0.5   |         55    |         0.6  |            4.2  |     0.61    |      0.244    |                        16 |            3 |
| 282 |             0 |            3 |        1129   |      22    |               16     |          0    |       nan    |           17    |     2       |      0.8      |                        16 |           33 |
| 283 |             1 |            4 |        2301   |      34    |               18     |         53    |         2.1  |            6.1  |     0.15    |      0.06     |                        24 |           24 |
| 284 |             5 |            4 |        2123   |      22    |               14     |         40    |         2.6  |            6.8  |     0.75    |      0.3      |                        25 |            0 |
| 285 |           nan |          nan |         962   |       0    |                0     |         57    |       nan    |            1    |     0.1     |      0.04     |                         7 |           39 |
| 286 |           nan |          nan |         305   |       0.08 |                0.05  |         12    |       nan    |            3.6  |     0.1     |      0.04     |                         0 |           57 |
| 287 |           nan |          nan |        3378   |      91    |               13     |          0    |       nan    |            0    |     0       |      0        |                         6 |           75 |
| 288 |           nan |          nan |        2435   |      44    |               27     |         26    |       nan    |            9.5  |     0       |      0        |                        22 |           40 |
| 289 |           nan |          nan |        1226   |       3    |                0.5   |         11    |       nan    |           30    |     0.02    |      0.008    |                         0 |           90 |
| 290 |           nan |          nan |        1707   |      38    |               15     |          0.5  |       nan    |           15    |     1.1     |      0.44     |                        19 |           40 |
| 291 |             1 |            3 |         231   |       2.4  |                1.3   |          1.2  |         2.3  |            2.4  |     0.5     |      0.2      |                         0 |           94 |
| 292 |             1 |            1 |         101   |       0    |                0     |          2.8  |         1.5  |            0.8  |     0.07    |      0.028    |                        -6 |           90 |
| 293 |             5 |            4 |         987   |      25    |                2     |          0.6  |       nan    |            0.7  |     1.8     |      0.72     |                        10 |           48 |
| 294 |           nan |          nan |        1544   |      39    |                2.9   |          3.3  |       nan    |            0.7  |     1.8     |      0.72     |                        13 |           16 |
| 295 |           nan |          nan |        3301   |       8.9  |                5.9   |         23    |       nan    |            2.4  |     0.13    |      0.052    |                        19 |           24 |
| 296 |             1 |            4 |        1188   |      19    |               13     |          0.6  |       nan    |           26    |     1       |      0.4      |                        12 |          nan |
| 297 |             0 |            3 |        1573   |      33    |               23     |          0    |         1.5  |           18    |     1.3     |      0.52     |                        13 |           32 |
| 298 |           nan |          nan |         548   |       0    |                0     |         27    |       nan    |            0    |     0.06    |      0.024    |                         6 |           66 |
| 299 |           nan |          nan |         393   |       2    |                0     |          0    |       nan    |           21    |     0.62    |      0.248    |                        -2 |          nan |
| 300 |             3 |            4 |         833   |       4.1  |                1.6   |          0.7  |       nan    |           39    |     4.8     |      1.92     |                        13 |            7 |
| 301 |             0 |            1 |          91.6 |       0    |                0     |          0.96 |         1.32 |            0.92 |     0.012   |      0.0048   |                        -1 |           90 |
| 302 |           nan |          nan |        1983   |      38    |               15     |          3.5  |       nan    |           29    |     4.8     |      1.92     |                        25 |            0 |
| 303 |             0 |          nan |        1054   |      20    |                2     |          0    |       nan    |           18    |     0       |      0        |                        -1 |          nan |
| 304 |             1 |            4 |         426   |       6.8  |                0.8   |          7.2  |         2    |            1.3  |     0.78    |      0.312    |                        -2 |          100 |
| 305 |             8 |            4 |        1012   |      13    |                2.7   |          1.4  |         2.1  |           11    |     1.2     |      0.48     |                         3 |           39 |
| 306 |             5 |            4 |         823   |       8    |                1.5   |          0.5  |         0.6  |           18    |     1       |      0.4      |                         2 |           42 |
| 307 |             0 |            1 |          75   |       0.1  |                0.1   |          1.5  |         1.3  |            0.9  |     0.0254  |      0.01016  |                        -6 |           84 |
| 308 |           nan |          nan |        1477   |       1.1  |                0.2   |          1.5  |       nan    |           11    |     0       |      0        |                        -1 |          100 |
| 309 |             0 |            3 |        1209   |      23    |               17     |          0    |         0    |           20    |     1.3     |      0.52     |                        13 |           47 |
| 310 |             0 |            3 |        1506   |      28    |               20     |          0    |       nan    |           27    |     0.9     |      0.36     |                        12 |           38 |
| 311 |             5 |            4 |        1385   |       0.9  |                0.3   |         26    |         6.2  |            5.8  |     6.6     |      2.64     |                         2 |           75 |
| 312 |             0 |            4 |        2142   |      25    |               17     |         30    |         1.6  |            6    |     0.9     |      0.36     |                        24 |           30 |
| 313 |             0 |            1 |        1544   |       3.5  |                1     |          1.6  |         4.7  |           14    |     0.12    |      0.048    |                        -5 |           90 |
| 314 |           nan |          nan |        2121   |      29    |               18     |         49    |       nan    |            5.7  |     0       |      0        |                        26 |           30 |
| 315 |           nan |          nan |        1008   |       0    |                0     |         35    |       nan    |            2    |     0       |      0        |                         4 |           84 |
| 316 |           nan |          nan |         657   |       8.5  |                3.1   |          2.1  |       nan    |           18    |     2.1     |      0.84     |                        13 |            7 |
| 317 |           nan |          nan |        1439   |      35    |                4.6   |          3.8  |       nan    |            1.3  |     1.2     |      0.48     |                        13 |           17 |
| 318 |             8 |            4 |         654   |       2.8  |                1.7   |         25    |         0.7  |            2.2  |     0.17    |      0.068    |                         6 |          nan |
| 319 |             0 |          nan |        1900   |      17    |                2.2   |          1.8  |         3.3  |            2.6  |     1.88    |      0.752    |                        12 |           39 |
| 320 |             4 |            4 |         594   |       5.8  |                2.7   |          1.5  |         0.5  |            4.4  |     1       |      0.4      |                         5 |           63 |
| 321 |             0 |            3 |         335   |       2.6  |                0.3   |          2.5  |         2.5  |            2.3  |     0.88    |      0.352    |                        -1 |          100 |
| 322 |           nan |          nan |        2372   |      38    |               18     |         49    |         2.9  |            6.7  |     0.15    |      0.06     |                        24 |           40 |
| 323 |             0 |            4 |        1665   |       4.5  |                2.6   |         45    |         6.7  |            5.9  |     0.03    |      0.012    |                        10 |           58 |
| 324 |           nan |          nan |        2255   |      35    |               18     |         37    |         5.6  |            9.2  |     0.07    |      0.028    |                        19 |           40 |
| 325 |             1 |            3 |         368   |       6.6  |                0.9   |          4    |         1.4  |            1.4  |     0.98    |      0.392    |                        -1 |           94 |
| 326 |             0 |            4 |         142   |       1.9  |                0.4   |          2.3  |         1.7  |            0.5  |     0.55    |      0.22     |                         1 |           88 |
| 327 |             0 |            3 |        2026   |      24    |               15     |         24    |         6    |            6.1  |     0.0254  |      0.01016  |                        16 |           43 |
| 328 |           nan |          nan |         109   |       0    |                0     |          4.1  |       nan    |            0.6  |     0.34    |      0.136    |                        -2 |           84 |
| 329 |             0 |          nan |         322   |       0.6  |                0.2   |          0.5  |         3.7  |            5.9  |     0.61    |      0.244    |                        -4 |          100 |
| 330 |             0 |            1 |        1515   |       4.8  |                0.9   |          1.2  |         5.8  |           17    |     0       |      0        |                        -6 |          100 |
| 331 |             0 |            3 |        1586   |      29    |               19     |          0.5  |       nan    |           29    |     0.85    |      0.34     |                        12 |           49 |
| 332 |             5 |            4 |        1860   |      16    |                7.4   |         32    |         2    |            5.4  |     0.14    |      0.056    |                        17 |           12 |
| 333 |             0 |            3 |         117   |       0.5  |                0.1   |          0.8  |         2.2  |            1.4  |     0.5     |      0.2      |                        -5 |           94 |
| 334 |             6 |            4 |         521   |       5.8  |                2.3   |          0.7  |         4    |            8    |     0.94    |      0.376    |                        -1 |           49 |
| 335 |             1 |            4 |         360   |       2.1  |                1     |          1.9  |         1.8  |            4.2  |     1       |      0.4      |                         2 |           75 |
| 336 |             0 |            3 |         106   |       0.5  |                0.1   |          0.5  |         3    |            1.6  |     0.64    |      0.256    |                        -6 |           90 |
| 337 |             6 |            4 |         450   |       5    |                1.8   |          1.1  |         2.3  |            6.9  |     1.1     |      0.44     |                         0 |           49 |
| 338 |             2 |            4 |        1038   |       0    |                0     |         60    |         1    |            0    |     0       |      0        |                        11 |           39 |
| 339 |             4 |            4 |        1000   |      25    |                1.8   |          1    |         0    |            0.6  |     2       |      0.8      |                        11 |           33 |
| 340 |           nan |          nan |         707   |       2.9  |                1.2   |         30    |         0    |            5.3  |     0.2     |      0.08     |                         6 |           30 |
| 341 |             1 |            4 |         139   |       1.5  |                0.3   |          2.2  |         1.1  |            0.9  |     0.62    |      0.248    |                         1 |           75 |
| 342 |             1 |            4 |         114   |       0.6  |                0.4   |          1.8  |         0.9  |            0.8  |     0.58    |      0.232    |                         2 |           75 |
| 343 |             3 |            4 |        1854   |      21    |                2.1   |         24    |         1.2  |            5.8  |     0.81    |      0.324    |                        14 |            7 |
| 344 |           nan |          nan |         192   |       1.6  |                1     |          4.8  |       nan    |            3.2  |     0.13    |      0.052    |                         0 |           78 |
| 345 |           nan |          nan |         268   |       3.6  |                2.2   |          4.8  |       nan    |            3.2  |     0.11    |      0.044    |                         2 |           30 |
| 346 |             0 |            1 |         192   |       1.6  |                1     |          4.8  |       nan    |            3.2  |     0.13    |      0.052    |                         0 |           78 |
| 347 |             0 |            1 |         250   |       0    |                0     |         12    |         0.6  |            0    |     0       |      0        |                        -3 |           72 |
| 348 |             0 |            1 |         205   |       0    |                0     |         10    |         1    |            0    |     0       |      0        |                        -4 |           90 |
| 349 |             9 |            4 |         902   |      12    |                2.5   |          4.5  |       nan    |           11    |     1.3     |      0.52     |                         4 |           36 |
| 350 |             0 |            2 |        3464   |      92    |               10     |          0    |         0    |            0    |     0       |      0        |                        11 |           60 |
| 351 |           nan |          nan |        1138   |      22    |                3     |          0    |       nan    |           18    |     1       |      0.4      |                         4 |           66 |
| 352 |           nan |          nan |           0   |       0    |                0     |          5.2  |       nan    |            0    |     0       |      0        |                         4 |           33 |
| 353 |           nan |          nan |        1577   |       0.8  |                0.1   |          6.5  |       nan    |            6.8  |     1.9     |      0.76     |                        13 |           54 |
| 354 |             2 |            4 |          96   |       0    |                0     |          5.2  |       nan    |            0    |     0       |      0        |                         8 |          nan |
| 355 |             4 |            4 |        1343   |      30    |                2.4   |          8.1  |       nan    |            0.7  |     2.1     |      0.84     |                        16 |           12 |
| 356 |             1 |            4 |        1971   |      20    |                7.1   |         19    |         6.1  |            9.7  |     0.4     |      0.16     |                        12 |           17 |
| 357 |           nan |          nan |        1883   |       9.6  |                3.4   |         33    |       nan    |            5.1  |     0.03    |      0.012    |                        15 |          nan |
| 358 |             1 |            4 |        1937   |      18    |               10     |         65    |         3.6  |            6.3  |     0.15    |      0.06     |                        21 |           30 |
| 359 |             2 |            4 |         468   |       3    |                1.5   |          1    |       nan    |           20    |     1.8     |      0.72     |                         4 |           46 |
| 360 |             2 |            4 |        1481   |      27    |               11     |          0.7  |       nan    |           28    |     4.7     |      1.88     |                        24 |            0 |
| 361 |             0 |            4 |        1615   |       2    |                1     |         30    |       nan    |            6.5  |     0.5     |      0.2      |                        12 |           54 |
| 362 |             4 |            4 |         481   |       3.2  |                1.5   |          1.5  |       nan    |           19    |     2       |      0.8      |                         5 |           33 |
| 363 |           nan |          nan |        1011   |      19    |               13     |          0.7  |       nan    |           18    |     0.5     |      0.2      |                        10 |           58 |
| 364 |             1 |            3 |         132   |       0    |                0     |          1.6  |         2.2  |            2.5  |     0.6     |      0.24     |                        -6 |           90 |
| 365 |           nan |          nan |         728   |       7    |                4.4   |         22    |       nan    |            5.1  |     0.14    |      0.056    |                         7 |           33 |
| 366 |           nan |          nan |         728   |       5.5  |                3.7   |         23    |       nan    |            1.7  |     0.13    |      0.052    |                         9 |           36 |
| 367 |           nan |          nan |        2033   |      26    |                3.2   |         38    |       nan    |           12    |     0.16    |      0.064    |                        17 |           39 |
| 368 |           nan |          nan |         556   |       8.3  |                2.5   |          0    |       nan    |           14    |     1       |      0.4      |                         2 |           66 |
| 369 |           nan |          nan |        1548   |       5.3  |                0.7   |         23    |       nan    |            6.6  |     0.6     |      0.24     |                        11 |           54 |
| 370 |             0 |            1 |         159   |       0    |                0     |          9.5  |       nan    |            0    |     0       |      0        |                         3 |           59 |
| 371 |             0 |            3 |        2682   |      53    |               10     |          5.9  |       nan    |           20    |     1.2     |      0.48     |                        23 |           31 |
| 372 |             1 |            4 |         467   |       4.5  |                3.3   |          4.3  |         0.8  |            6    |     0.73    |      0.292    |                         4 |           54 |
| 373 |           nan |          nan |         180   |       0    |                0     |          9.6  |       nan    |            0.6  |     0       |      0        |                         3 |          nan |
| 374 |             0 |            4 |        2021   |      21    |               10     |         18    |       nan    |            6    |     1.6     |      0.64     |                        25 |           39 |
| 375 |           nan |          nan |         305   |       1.4  |                0.2   |          5.1  |       nan    |            2.7  |     0.45    |      0.18     |                         1 |           90 |
| 376 |             0 |          nan |         192   |       0    |                0     |          4    |       nan    |            7.5  |     0.1     |      0.04     |                        -4 |           90 |
| 377 |           nan |          nan |        1887   |      23    |                2.8   |         30    |       nan    |            4.2  |     1       |      0.4      |                        17 |            4 |
| 378 |           nan |          nan |        2130   |      25    |               14     |         35    |       nan    |            6    |     0.5     |      0.2      |                        25 |           24 |
| 379 |           nan |          nan |         435   |       5.4  |                0.8   |          1.7  |       nan    |            5.3  |     0.3     |      0.12     |                        -1 |           90 |
| 380 |             0 |            3 |        1854   |      24    |                2.6   |         38    |         7.9  |           14    |     0.02    |      0.008    |                         0 |           78 |
| 381 |           nan |          nan |         347   |       1.8  |                1.1   |         12    |       nan    |            3    |     0.1     |      0.04     |                         3 |           15 |
| 382 |           nan |          nan |         197   |       0.5  |                0.1   |          1.4  |       nan    |            2    |     0.1     |      0.04     |                        -6 |           90 |
| 383 |             0 |          nan |         339   |       0.1  |                0.02  |          0.2  |       nan    |            1.9  |     0       |      0        |                        -5 |           78 |
| 384 |           nan |          nan |        1255   |      13    |                4.6   |          1.9  |       nan    |           11    |     1.1     |      0.44     |                        11 |           51 |
| 385 |           nan |          nan |        1519   |       4.6  |                1.2   |          3    |       nan    |           15    |     0.1     |      0.04     |                         0 |           90 |
| 386 |           nan |          nan |         368   |       4.2  |                3     |          1    |       nan    |            1.5  |     0.6     |      0.24     |                         5 |           90 |
| 387 |           nan |          nan |         933   |      15    |                4.5   |          1.7  |       nan    |           12    |     0.74    |      0.296    |                         4 |           69 |
| 388 |           nan |          nan |        2008   |      20    |               11     |         29    |       nan    |            8.8  |     0.6     |      0.24     |                        23 |           41 |
| 389 |           nan |          nan |        2598   |      50    |                6.7   |          6    |       nan    |           24    |     1.6     |      0.64     |                        21 |           33 |
| 390 |           nan |          nan |        3059   |      81    |               55     |          0    |       nan    |            1.1  |     1.8     |      0.72     |                        26 |           30 |
| 391 |             1 |            3 |         121   |       0    |                0     |          2.3  |         2.6  |            2.5  |     0.59    |      0.236    |                        -1 |           90 |
| 392 |             0 |            4 |         368   |       0.9  |                0.2   |          3.7  |         6.1  |            5.8  |     0.54    |      0.216    |                       -10 |           90 |
| 393 |           nan |          nan |         247   |       0    |                0     |         11    |       nan    |            0    |     0       |      0        |                        -3 |           90 |
| 394 |           nan |          nan |        2590   |      50    |                7.1   |          5.1  |       nan    |           27    |     0.8     |      0.32     |                        18 |           75 |
| 395 |             4 |            4 |         979   |       0    |                0     |          0.5  |       nan    |            0    |     0       |      0        |                         2 |           60 |
| 396 |           nan |          nan |        1515   |       2    |                0.4   |          3.5  |       nan    |            8.2  |     0.6     |      0.24     |                         1 |           90 |
| 397 |           nan |          nan |         176   |       1.8  |                0.9   |          3.2  |       nan    |            1.2  |     0.59    |      0.236    |                         2 |           64 |
| 398 |           nan |          nan |         105   |       0.5  |                0.1   |          0    |       nan    |            1.8  |     0.04    |      0.016    |                        -1 |           90 |
| 399 |           nan |          nan |         506   |       1    |                0.1   |          0    |       nan    |           28    |     0.7     |      0.28     |                        -1 |           84 |
| 400 |           nan |          nan |        1900   |      15    |                9.6   |         23    |       nan    |            8.8  |     0.72    |      0.288    |                        22 |            2 |
| 401 |             0 |            3 |         711   |       6    |                1     |          0    |       nan    |           29    |     0.69    |      0.276    |                         0 |           78 |
| 402 |           nan |          nan |         318   |       1.3  |                1     |          0    |       nan    |           16    |     0.41    |      0.164    |                        -4 |           75 |
| 403 |             0 |            4 |        1824   |      21.2  |               12.2   |          9.8  |       nan    |            9.8  |     0.93    |      0.372    |                        21 |           24 |
| 404 |           nan |          nan |        1791   |      12    |                7.6   |         29    |       nan    |            5.1  |     0.57    |      0.228    |                        20 |            1 |
| 405 |             5 |            4 |         569   |       5.8  |                3.8   |         15    |       nan    |            2.9  |     0.18    |      0.072    |                         6 |           55 |
| 406 |             0 |            1 |        1544   |       3.5  |                1     |          1.6  |         4.7  |           14    |     0.12    |      0.048    |                        -5 |           90 |
| 407 |           nan |          nan |        1377   |      27    |               18     |          0.5  |       nan    |           21    |     1.7     |      0.68     |                        16 |           24 |
| 408 |           nan |          nan |        1519   |       4.6  |                1.2   |          3    |       nan    |           15    |     0.1     |      0.04     |                         0 |           90 |
| 409 |           nan |          nan |         649   |       9.5  |                2.7   |          0.1  |       nan    |           17    |     1.6     |      0.64     |                         5 |           63 |
| 410 |           nan |          nan |         285   |       1.2  |                0.8   |         10    |       nan    |            0    |     0.09    |      0.036    |                         2 |           30 |
| 411 |           nan |          nan |        1519   |       4.6  |                1.2   |          3    |       nan    |           15    |     0.1     |      0.04     |                         0 |           90 |
| 412 |           nan |          nan |          96   |       0.5  |                0.1   |          0.5  |       nan    |            2.8  |     0.05    |      0.02     |                        -6 |           90 |
| 413 |           nan |          nan |         967   |      17    |                3.1   |          3.1  |       nan    |           14    |     1.2     |      0.48     |                         5 |           36 |
| 414 |           nan |          nan |         678   |      10    |                3.1   |          0.5  |       nan    |           18    |     1.6     |      0.64     |                        12 |            8 |
| 415 |           nan |          nan |         686   |       9.4  |                2.3   |          1    |       nan    |           18    |     1       |      0.4      |                         3 |           72 |
| 416 |           nan |          nan |        2368   |      59    |                9.3   |          0    |       nan    |            4.8  |     0.73    |      0.292    |                        19 |           30 |
| 417 |           nan |          nan |        1636   |       3.9  |                0.5   |          6.8  |       nan    |           12    |     0.95    |      0.38     |                         4 |           84 |
| 418 |           nan |          nan |        1519   |       4.6  |                1.2   |          3    |       nan    |           15    |     0.1     |      0.04     |                         0 |           90 |
| 419 |           nan |          nan |        1803   |      21    |                2.1   |         30    |       nan    |            4.7  |     1       |      0.4      |                        17 |          nan |
| 420 |           nan |          nan |         201   |       2.6  |                0.9   |          1    |       nan    |            2.2  |     0.7     |      0.28     |                        -3 |           90 |
| 421 |           nan |          nan |         297   |       0    |                0     |         13    |       nan    |            0    |     0       |      0        |                        -3 |           90 |
| 422 |             2 |            3 |         322   |      18    |               18     |         17    |       nan    |            0    |     0       |      0        |                         8 |           90 |
| 423 |           nan |          nan |         100   |       0.5  |                0.1   |          0.5  |       nan    |            2.3  |     0.9     |      0.36     |                         2 |           75 |
| 424 |           nan |          nan |         971   |       0    |                0     |         38    |       nan    |            2.4  |     0       |      0        |                         4 |           84 |
| 425 |           nan |          nan |        1519   |       4.6  |                1.2   |          3    |       nan    |           15    |     0.1     |      0.04     |                         0 |           90 |
| 426 |             0 |            1 |        1544   |       3.5  |                1     |          1.6  |         4.7  |           14    |     0.12    |      0.048    |                        -5 |           90 |
| 427 |             2 |            3 |        2018   |      50    |               22     |          0.9  |         0.5  |            6    |     1.1     |      0.44     |                        20 |            0 |
| 428 |             5 |            4 |         523   |       5.8  |                2.3   |          0.7  |         3.5  |            8    |     0.94    |      0.376    |                         0 |           49 |
| 429 |             3 |            4 |         100   |       0    |                0     |          5.6  |         0    |            0    |     0.05    |      0.02     |                         8 |           28 |
| 430 |             0 |            3 |        1615   |      30    |               19     |          0    |         0    |           29    |     1.1     |      0.44     |                        13 |           37 |
| 431 |             1 |            4 |        1269   |       0    |                0     |         56    |         7.3  |            2.4  |     0.02    |      0.008    |                         2 |           57 |
| 432 |           nan |          nan |          96   |       0    |                0     |          2.1  |       nan    |            1.4  |     0.02    |      0.008    |                        -5 |           90 |
| 433 |           nan |          nan |         979   |      17    |                4     |          0    |       nan    |           21    |     0.3     |      0.12     |                         1 |           75 |
| 434 |             4 |            4 |         667   |      10    |                5.3   |          1    |         1.5  |            7.3  |     1       |      0.4      |                         5 |           39 |
| 435 |             0 |          nan |        2787   |      63    |                8.4   |          4.5  |        10    |           15    |     0.01    |      0.004    |                         1 |           75 |
| 436 |           nan |          nan |         181   |       0    |                0     |         10    |       nan    |            0    |     0       |      0        |                         4 |           60 |
| 437 |             0 |            3 |        1490   |      32    |               23     |          0.5  |       nan    |           16    |     1.4     |      0.56     |                        15 |           30 |
| 438 |             0 |          nan |        2606   |      51    |                7.5   |          4.6  |         8.8  |           27    |     0.06    |      0.024    |                        10 |           78 |
| 439 |           nan |          nan |        1057   |       4    |                0.5   |          6    |         6.5  |            8.4  |     1.1     |      0.44     |                        -2 |           60 |
| 440 |           nan |          nan |         866   |       9    |                3.1   |          0.5  |       nan    |           31    |     6.6     |      2.64     |                        15 |            4 |
| 441 |             2 |            4 |         486   |       3.8  |                1.4   |          0.9  |         0    |           19.1  |     1.9558  |      0.78232  |                         5 |           43 |
| 442 |             0 |            1 |          71   |       0.2  |                0     |          0.8  |         1.7  |            1.2  |     0.1     |      0.04     |                        -6 |          nan |
| 443 |             0 |          nan |         784   |       5.1  |                0.6   |          2.7  |         3.2  |            9    |     1.2     |      0.48     |                        -1 |           60 |
| 444 |             0 |            1 |        1463   |       1    |                0.2   |          0.2  |         1    |            7.4  |     0       |      0        |                        -1 |           84 |
| 445 |             0 |            1 |        1477   |       1.1  |                0.2   |          0.3  |         1.5  |            8.1  |     0       |      0        |                        -2 |          100 |
| 446 |             0 |          nan |        1490   |       2    |                0.3   |          3    |         5    |           11    |     0       |      0        |                        -6 |          100 |
| 447 |             0 |            3 |        1485   |       2.9  |                0.5   |          1.3  |         9.9  |           11    |     2.3     |      0.92     |                        -1 |           61 |
| 448 |             0 |            3 |        1904   |      15    |                2.2   |          2.1  |       nan    |           11    |     1.7     |      0.68     |                        14 |           51 |
| 449 |             0 |            4 |        2013   |      21    |               14     |          2.6  |         4.1  |           11    |     2.1     |      0.84     |                        21 |          nan |
| 450 |           nan |          nan |        2050   |      23    |               16     |          2.7  |         3.4  |           13    |     1.8     |      0.72     |                        20 |           30 |
| 451 |             4 |            4 |        2285   |      34    |               14     |         14    |         1.8  |            9.1  |     1.6     |      0.64     |                        25 |           24 |
| 452 |             3 |            4 |        2168   |      29    |                5.5   |          6.5  |         2.7  |           14    |     1.5     |      0.6      |                        16 |            4 |
| 453 |             0 |            3 |        2564   |      50    |                7.1   |          5.1  |         7.9  |           27    |     0.8     |      0.32     |                        13 |           69 |
| 454 |             0 |            3 |        2588   |      51    |                7.2   |          5.2  |         8    |           27    |     0.04    |      0.016    |                         0 |           78 |
| 455 |             0 |            3 |        2566   |      48    |                8.8   |          6.6  |         5.8  |           20    |     0.8     |      0.32     |                         4 |           66 |
| 456 |             0 |            3 |        1975   |      20    |                2     |          1.5  |         4    |            6.6  |     0.9     |      0.36     |                         1 |           78 |
| 457 |             1 |            4 |        1127   |      25.1  |               10.8   |          0.3  |       nan    |           10.9  |     1.54    |      0.616    |                        19 |            0 |
| 458 |             6 |            4 |        2105   |      26    |                2     |          5.3  |         4    |            4.8  |     1.5     |      0.6      |                        10 |           24 |
| 459 |             0 |            3 |         592   |       1.4  |                0.7   |          0.9  |         3.1  |            6.2  |     0.8     |      0.32     |                        -2 |           90 |
| 460 |             0 |            1 |         nan   |     nan    |              nan     |        nan    |       nan    |          nan    |     0.00112 |      0.00045  |                         0 |          100 |
| 461 |             7 |            4 |        1031   |      10    |                5.7   |          2.7  |         2.6  |           10    |     1.7     |      0.68     |                        13 |            8 |
| 462 |             0 |            3 |        3058   |      82    |               60     |          0.6  |         0    |            0.8  |     0.05    |      0.02     |                        19 |           35 |
| 463 |           nan |          nan |         289   |       0.9  |                0.2   |          0.1  |       nan    |            0.6  |     9       |      3.6      |                        10 |          nan |
| 464 |             1 |            3 |        1035   |      15    |                5.9   |          0.5  |         0    |           29    |     4.5     |      1.8      |                        18 |            2 |
| 465 |             0 |            3 |        1100   |      16    |                6.3   |          0.5  |       nan    |           30    |     5.8     |      2.32     |                        19 |           30 |
| 466 |           nan |          nan |        1812   |      24    |                2.3   |         28    |       nan    |            4.6  |     0.63    |      0.252    |                        15 |            5 |
| 467 |           nan |          nan |         506   |       5.5  |                1.8   |          2.3  |       nan    |            4.2  |     0.84    |      0.336    |                         3 |           75 |
| 468 |           nan |          nan |         749   |       5.4  |                1.9   |          3.8  |         2.6  |            7.5  |     1       |      0.4      |                         1 |          nan |
| 469 |           nan |          nan |        1105   |      11    |                6.4   |          0.8  |         2.2  |           11    |     1       |      0.4      |                        11 |           88 |
| 470 |           nan |          nan |        1527   |       1.5  |                1     |         15    |       nan    |            3.1  |     0.13    |      0.052    |                         6 |           63 |
| 471 |             2 |            4 |         779   |      11    |                3.2   |          0.8  |         0.6  |           20    |     1.6     |      0.64     |                        12 |           33 |
| 472 |             1 |            4 |         106   |       1.7  |                0.2   |          0.5  |         0.5  |            0.7  |     0.1     |      0.04     |                         0 |           65 |
| 473 |             1 |            4 |         384   |       2.9  |                1.8   |         12    |         0    |            3.3  |     0.12    |      0.048    |                         2 |           82 |
| 474 |            12 |            4 |         804   |       6.5  |                0.6   |          2.2  |         2.7  |            8.5  |     1.016   |      0.4064   |                        -1 |           49 |
| 475 |             1 |            3 |        1719   |      10    |                1     |          6.1  |         7.1  |           10    |     1.1     |      0.44     |                         0 |           60 |
| 476 |             1 |            4 |         182   |       2.1  |                0.3   |          1.8  |         0.9  |            0.6  |     0.64    |      0.256    |                         1 |           78 |
| 477 |             4 |            4 |        1425   |       0.5  |                0     |         63    |       nan    |            5.4  |     0.05    |      0.02     |                        14 |           35 |
| 478 |             1 |            4 |        2226   |      28    |               16     |         35    |         1.1  |            5.4  |     0.4     |      0.16     |                        23 |           15 |
| 479 |             0 |            3 |        2343   |      37    |                3.4   |          0.7  |         5.5  |            6    |     1.3     |      0.52     |                         9 |           57 |
| 480 |             0 |            4 |        2123   |      24    |                3.1   |          2.5  |         3.5  |           15    |     1.5     |      0.6      |                        12 |           39 |
| 481 |             0 |            4 |        2029   |      23    |                2.1   |          0.5  |         4    |            3.5  |     2.4     |      0.96     |                        14 |           23 |
| 482 |             6 |            4 |         452   |       5    |                1.8   |          1.1  |         2.3  |            6.9  |     1.1     |      0.44     |                         0 |           49 |
| 483 |             0 |            3 |        2263   |      34    |                3.1   |          0.5  |         5.5  |            5.5  |     1.1     |      0.44     |                         8 |           54 |
| 484 |             1 |            3 |        1662   |       5    |                0.7   |          6.5  |         4    |           11    |     1.1     |      0.44     |                         0 |           84 |
| 485 |             0 |            1 |        1502   |       0    |                0     |          0    |         1.5  |            7.4  |     0       |      0        |                        -1 |           78 |
| 486 |             0 |            1 |         192   |       1.6  |                1     |          4.8  |         0    |            3.2  |     0.1     |      0.04     |                         0 |           78 |
| 487 |           nan |          nan |         195   |       1.6  |                1     |          4.8  |       nan    |            3.2  |     0.1     |      0.04     |                         0 |           78 |
| 488 |             0 |            3 |         360   |       0.5  |                0.1   |          0.5  |         3.8  |            6.3  |     0.58    |      0.232    |                        -4 |           90 |
| 489 |             0 |            3 |        2226   |      34    |                2.9   |          0.6  |         4.6  |            6    |     0.1     |      0.04     |                         1 |           78 |
| 490 |           nan |          nan |         167   |       0.1  |                0.05  |          3.6  |       nan    |            0.3  |     0.03    |      0.012    |                        -5 |           49 |
| 491 |             0 |            4 |         770   |      12    |                4.6   |          0.8  |       nan    |           18    |     1.8     |      0.72     |                        13 |           63 |
| 492 |             4 |            4 |        2105   |      51    |                3.4   |          0    |       nan    |            8.6  |     1.6     |      0.64     |                        16 |            3 |
| 493 |             0 |            2 |        3404   |      92    |               15     |          0    |       nan    |            0    |     0       |      0        |                        12 |           57 |
| 494 |           nan |          nan |          68   |       0    |                0.2   |          0    |       nan    |            1.2  |     1.5     |      0.6      |                         5 |           69 |
| 495 |             0 |            1 |         134   |       0.5  |                0.1   |          3.4  |         3    |            1.7  |     0.05    |      0.02     |                        -9 |          100 |
| 496 |             0 |            3 |         109   |       0.5  |                0.1   |          2.3  |         2.1  |            1.5  |     0.53    |      0.212    |                        -5 |          100 |
| 497 |           nan |          nan |         699   |       7.1  |                1.9   |          0.7  |       nan    |           25    |     1.4     |      0.56     |                         4 |          nan |
| 498 |             1 |            3 |         272   |       0    |                0     |         15    |         1.2  |            0    |     0.01    |      0.004    |                        -3 |           75 |
| 499 |             0 |            4 |         262   |       3.4  |                2.1   |          4.1  |         0    |            3.9  |     0.13    |      0.052    |                         0 |           88 |
| 500 |           nan |          nan |         573   |       7.6  |                0.8   |          3.3  |       nan    |           12    |     1.5     |      0.6      |                         2 |          nan |
| 501 |             2 |            4 |        2092   |      27    |               18     |          1.8  |         3    |           10    |     2.9     |      1.16     |                        23 |           24 |
| 502 |             4 |            4 |         435   |       2.2  |                0.7   |          0.8  |         0    |           20    |     2.5     |      1        |                        11 |            9 |
| 503 |             0 |            1 |        1510   |       6.9  |                0.6   |          5.1  |         8.3  |           12    |     0.03    |      0.012    |                        -5 |           90 |
| 504 |           nan |          nan |        1590   |       4.7  |                0.9   |          1.1  |       nan    |           12    |     2.2     |      0.88     |                        13 |           20 |
| 505 |             1 |            4 |         686   |      15    |                9.8   |          3.5  |         0.5  |            2.8  |     0.11    |      0.044    |                        11 |           23 |
| 506 |             1 |            1 |         100   |       0.1  |                0     |          2.9  |         1.2  |            1.4  |     0.01    |      0.004    |                        -6 |           90 |
| 507 |             6 |            4 |         946   |      12    |                2.2   |          5.3  |         2.7  |            6.8  |     1.5     |      0.6      |                         9 |           24 |
| 508 |             0 |            3 |        1619   |      31    |               20     |          0    |       nan    |           27    |     0.75    |      0.3      |                        12 |           38 |
| 509 |             0 |            1 |         384   |       1    |                0.1   |         13    |         2.3  |            4.8  |     0.07    |      0.028    |                        -6 |           90 |
| 510 |             0 |            3 |         703   |       8.8  |                1.3   |          0    |       nan    |           22    |     3.9     |      1.56     |                        13 |           37 |
| 511 |             8 |            4 |         835   |      12    |                5.7   |          1.2  |         1.4  |            9.4  |     1.1     |      0.44     |                        10 |           18 |
| 512 |             0 |            4 |         331   |       3    |                2.1   |         10    |       nan    |            2.9  |     0.08    |      0.032    |                         3 |           69 |
| 513 |             4 |            4 |         615   |       6.8  |                4.4   |         15    |       nan    |            3.3  |     0.1     |      0.04     |                         6 |           43 |
| 514 |             3 |            4 |        1498   |      29    |               12     |          1.2  |         0    |           23    |     4.1     |      1.64     |                        24 |            0 |
| 515 |             0 |            3 |        1202   |      30    |               20     |          3    |         0    |            2.4  |     0.05    |      0.02     |                        13 |           47 |
| 516 |             1 |            4 |         496   |       4.5  |                0.5   |          2.9  |         0.5  |            7    |     1.8     |      0.72     |                         4 |           66 |
| 517 |             3 |            4 |        1172   |      23    |                8.4   |          1.2  |         0    |           17    |     1.7     |      0.68     |                        18 |            1 |
| 518 |             0 |            3 |        1026   |      19    |               13     |          1    |       nan    |           18    |     0.6     |      0.24     |                        10 |           48 |
| 519 |             0 |            3 |         729   |      12    |                2.9   |          0    |         1    |           17    |     4       |      1.6      |                        13 |           37 |
| 520 |             6 |            4 |         981   |       6.8  |                3.4   |          2.2  |         1.8  |           11    |     1.4     |      0.56     |                        10 |           24 |
| 521 |             3 |            4 |         560   |       3.3  |                2     |         18    |         1    |            4.3  |     0.15    |      0.06     |                         2 |           66 |
| 522 |            12 |            4 |        1199   |      15    |                7.1   |          2.3  |         1.9  |           12    |     1.7     |      0.68     |                        16 |            4 |
| 523 |             2 |            4 |        1247   |       5.7  |                0.8   |          5.4  |         3.1  |            9.5  |     1.2     |      0.48     |                         1 |           48 |
| 524 |             5 |            4 |         272   |       0.5  |                0.2   |          0.1  |         0    |           14.4  |     1.8     |      0.72     |                         2 |           57 |
| 525 |             0 |            3 |          72   |       0    |                0     |          0    |         1.5  |            1.9  |     0.5     |      0.2      |                        -5 |           90 |
| 526 |             7 |            4 |         907   |       6.2  |                3     |          1.9  |         2.1  |            9.8  |     1.3     |      0.52     |                         2 |           45 |
| 527 |             1 |            4 |         126   |       0    |                0     |          7.4  |         0    |            0    |     0       |      0        |                        10 |           30 |
| 528 |             0 |            1 |         146   |       0    |                0     |          8    |         0    |            0    |     0       |      0        |                         1 |           69 |
| 529 |             0 |            3 |        2272   |      34    |                3.1   |          0.5  |         5.5  |            5.5  |     1.1     |      0.44     |                         8 |           54 |
| 530 |             1 |            1 |        2634   |      55    |                4.3   |          4    |        10    |           26    |     0.02    |      0.008    |                         6 |           90 |
| 531 |             0 |            3 |         906   |      10    |                3.5   |          0.5  |         0.5  |           31    |     6       |      2.4      |                        15 |           39 |
| 532 |             1 |            4 |        1431   |       5.3  |                0.7   |         17    |        16    |            9.7  |     0.15    |      0.06     |                        -3 |          100 |
| 533 |           nan |          nan |        1485   |       0.8  |                0.2   |          0    |         1.5  |            7.3  |     0.02    |      0.008    |                        -1 |           90 | 
 
 

La base préparée est disponible sur ce lien : <b>[ https://www.kaggle.com/dhiagharsallaoui/yuka-base1 ](https://www.kaggle.com/dhiagharsallaoui/yuka-base1)
</b>

2. **Analyse corrélation :** 

Dès que la base est prête on a commencé l’exploitation. Tout d’abord on a essayé de voir la corrélation entre tous les variables et le score Yuka. Le Heatmap ci-dessous montre les degrés de corrélation. 

<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/heatmap.png?raw=true">   
</p>
<p align="center">
<b>Figure 7 Heatmap des corrélations
</b>
</p>



Ce Heatmap confirme les informations données par Yuka. On peut voir que le **Nutri score** est le plus corrélé avec le score Yuka avec -0.81. Cette corrélation est négative ce qui est logique car le nutri score diminue avec l’augmentation de la qualité de nutriment. Et après on l’**additives\_n** qui représente le nombre d’additive existant dans l’aliment qui est corrélé négativement aussi et enfin le **nova\_group** qui communique une information sur les transformations utilisées pour avoir le produit final. 

Vu que le Nutri score est le plus impactant dans le score Yuka on va concentrer à la suite de l’étudier en étudiant la fonction utilité **U<sub>N</sub>** attribué à ce dernier dans la formule  


<p align="center">
<b>Score Yuka= 0.6*U<sub>N</sub>(Nutri score) + 0.3* U<sub>A</sub>(additives) + 0.1* U<sub>B</sub>(Bio)</b>
</p>

3. **Fonction utilité Nutri-score :** 

Pour analyser mieux l’effet de Nutri score on a éliminé les effets des additifs. Par la préparation d’une sous base qui contient seulement les aliments qui sont sans additives. La contribution vient de ce critère est le maximum qui est égale à 30. 

Et dans ce cas pour calculer la fonction Utilité de Nutri-score on a utilisé la formule suivante : 
<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/formule2.PNG?raw=true">   
</p>
    
Avec **I<sub>BIO</sub>** représente l’indice Bio qui est égale à 1 si l’aliment porte la label biologique et 0 sinon. 

<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/exemple Un.png?raw=true">   
</p>
<p align="center">
<b>Figure 8: Exemple de calcul de U<sub>N</sub>
</b>
</p>

 

Après ça on a divisé ce tableau en deux en séparant les aliments avec indice Bio égale à 1 et ceux avec indice Bio égale à 0. On a fait ça pour voir le comportement de chaque famille indépendamment d’où on limite les effets de critère Bio. 
<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/exemple Un BIO.png?raw=true">   
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/exemple Un non BIO.png?raw=true">
</p>
<p align="center">
<b>Figure 9: Exemple de U<sub>N</sub> des aliments Bio    &emsp; &emsp; &emsp;   Figure 10: Exemple de U<sub>N</sub> des aliments NON Bio
</b>
</p>




Après préparer les tableaux ce dessus on a essayé de tracer les fonctions utilitées UN en fonction des Nutri- score. Les graphes représentent ces fonctions sont ce dessous nous aide à interpréter plus d’informations sur leurs variations.  

<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/fonction%20Un.png?raw=true">   
</p>
<p align="center">
<b>Figure 11: fonction Utilité U<sub>N</sub> en fonction de Nutri-score      
</b>
</p>

<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/fonction%20Un%20BIO.png?raw=true">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/fonction%20Un%20non%20BIO.png?raw=true">
</p>
<p align="center">
<b>Figure 12: U<sub>N</sub> en fonction de Nutri-score pour les aliments BIO  &emsp; &emsp; &emsp;  Figure 13: U<sub>N</sub> en fonction de Nutri-score pour les aliments NON BIO
</b>
</p>




En regardant les graphes ci-dessus, on peut voir qu'il y a des utilités différentes pour la même valeur de Nutri-score. D’où la fonction utilité ne respecte pas la définition d’une fonction. Et par suite elle ne peut pas être une fonction.  

Ces résultats nous donnent l’intuition d’investiguer si ce score Yuka est croissant avec le Nutri score comme il est déclaré ou non. Et pour faire ça, j’ai parcouru ma base pour trouver s’il y a un contre-exemple de monotonie. Et par le contre-exemple, je veux dire deux aliments qui ont le même critère BIO avec le même nombre d’additifs qui est zéro et ayant des Nutri score différents ne vérifiant pas la monotonie. C’est-à-dire l’aliment, avec le plus grand Nutri score, a le plus grand score Yuka. 

<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/contre%20exemple.png?raw=true">
</p>
<p align="center">
<b>Figure 14: Contre-exemple de la monotonie du Score Yuka
</b>
</p> 

Comme il montre le contre-exemple ci-dessus, le score Yuka n’est pas monotone. Aussi ce Score ne peut pas être un modèle additif d’où cette hypothèse est rejetée.  

D’où j’ai décidé de faire un recours sur des modèles non additifs et je commencer à faire des modèles de Machine Learning. 

4. **Application Machine Learning :** 

Après l’élimination de l’hypothèse que score Yuka est sous cette forme additive : 

<p align="center">
<b>Score Yuka= 0.6*U<sub>N</sub>(Nutri score) + 0.3* U<sub>A</sub>(additives) + 0.1* U<sub>B</sub>(Bio)</b>
</p>

On a pensé à faire des essais avec les modèles de Machine Learning.  

1. **Pré-processing :** 

Afin d’avoir des bons résultats, on a commencé à préparer les entrés de modèles. Pour atteindre notre objective on a utilisé une technique qui s’appelle « One-Hot Encoding ».  Cette technique nous permet de transformer  une  variable  catégorique  en  plusieurs  variables  binaires.  On  l’a  utilisé  pour  la  variable  **« additifs »** en la transformant à 101 variables binaires.
Le résultat se trouve dans la figur e ce dessous.
<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/one-hot%20encoding.png?raw=true">
</p>
<p align="center">
<b>Figure 15: "One-Hot Encoding" appliqué sur additifs
</b>
</p> 
 

 Maintenant les entrés sont prêts et on peut commencer l’apprentissages. 
 
2. **Les Modèles :** 

Pour l’apprentissages on a décidé d’utiliser des modèles de différents comportements et architecture. Commençant par la régression linéaire pour confirmer le rejet de l’hypothèse qu’il est additif. Et après des modèles d’architecture Arbre et enfin un Modèle de Deep-learning qui est le réseau de neurones. En résumant les modèles utilisé sont comme suit : régression linéaire, arbre de décision, Random Forest et Réseau de neurones. 

Afin d’avoir le plus d’information, on a décidé de faire deux modèles de chaque type l’un en utilisant les additifs et l’autre en éliminant ce critère par prendre que les aliments avec zéro additif. 

3. **Résultats avec des additifs :** 

En entrainant nos modèles sur la base complète on a eu des résultats convenants. Ces résultats sont présentés sur le graphe ci-dessous *figure 17* où on a le « Erreur absolue moyenne » pour chaque modèle utilisé. 

<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/comp1.png?raw=true">
</p>
<p align="center">
<b>Figure 16: Comparaison de l'erreur absolue moyenne pour chaque modèle
</b>
</p> 
 

Comme il montre la figure le pire c’est le modèle de régression linéaire ce qui est bien attendu après qu’on a  montré  que  le  modèle  est  non  additif.  En  contrepartie  le  Random  Forest  qui  est  un  modèle d’architecture arbre est le meilleur. Ces résultats sont bien attendus et se convient avec notre logique. 

4. **Résultats sans des additifs**  

En investiguant plus, on a entrainé les modèles sur une base des aliments avec zéro additif. Le résultat de ces modèles est représenté dans la figure ci-dessous.  

<p align="center">
  <img src="https://github.com/dhia-gharsallaoui/Decryption-of-Yuka-algorithm/blob/main/images/comp2.png?raw=true">
</p>
<p align="center">
<b>Figure 17: Comparaison de l'erreur absolue moyenne pour chaque modèle sans additif
</b>
</p> 
 

En éliminant le critère des additifs la régression performe mieux mais n’a pas encore le meilleur. Dans tous les cas ces performance sont inutiles car on a éliminé un grand part des informations par supprimer les additives. 

5. **Conclusion :** 

Le score de Yuka est un moyen développé pour aider les consommateurs à choisir les bons aliments pour leurs  santés.  Mais  le  code  source  de  ce  score  n’est  pas  partagé  seule  l’attribution  de  chacun  est communique jusqu’à maintenant. En faisant notre étude on a constaté que score est n’est pas additif. Ça ne pose pas un grand problème mais le fait que n’est pas monotone avec le Nutri-score pose plusieurs questions. Sachant que le Nutri-score est développé par des organismes publics en France et utilisé avec l’accord de gouvernement. D’où c’est un peu difficile d’accepter un tel score lorsqu’il ne convient pas avec le Nutri-score.  Par suite il faut soit clarifier ce point par les responsables ou réviser l’algorithme de calcul et ajuster ce point. 



**Tout le travail réalisé est disponible sur ce Lien :[ https://www.kaggle.com/dhiagharsallaoui/final-yuka ](https://www.kaggle.com/dhiagharsallaoui/final-yuka)**
