{% include image.html %}
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

{% include image.html url="images/nutri_score.png" description="Nutri Score" %}


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

*Figure 2: Exemple Liste additifs et description ![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.005.png)*

Ce critère compte de 30% du score Yuka. 

3. **BIO** 

Il s'agit d'une bonification accordée aux produits considérés comme biologiques. Les produits considérés comme biologiques portent un label biologique européen (Euro-feuille). 

![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.006.png)![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.007.png)

*Figure 4: Exemple Bio  Figure 3 Euro-feuille Label* 

## **Chapitre 2 : Décryptage de l’algorithme de** 

**Yuka**  

Dans ce chapitre on va parler des travaux réalisés pour comprendre mieux l’algorithme de Yuka et le décrypter en partant de l’hypothèse que c’est un modèle additif. 

1. **Construction de la base :** 

Afin d’atteindre notre objectif on a besoin de préparer une base de données qui contient à la fois tous les éléments utilisés pour calculer le score Yuka et le score Yuka même. 

Base

Composition  Score Yuka du produit![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.008.png) du produit

*Figure 5: Composition de la base* 

- Composition du produit : 

On a pris les ingrédients à partir de « OpenFoodFacts ». OpenFoodFacts est une base de données sur les produits  alimentaires  faite  par  tout  le  monde,  pour  tout  le  monde. Cette  base  contient  presque  la composition détaillée de chaque aliment. 

- Score Yuka 

Pour les scores de Yuka, on est sorti au supermarché. On a scanné plus que 500 produits. En faisant la combinaison de la base OpenFoodFacts et les scores Yuka des produits scannés, on a eu une base complète pour commencer les analyses. 

![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.009.png)

*Figure 6 : Capture Base* 

La base préparée est disponible sur ce lien : [ https://www.kaggle.com/dhiagharsallaoui/yuka-base1 ](https://www.kaggle.com/dhiagharsallaoui/yuka-base1)

2. **Analyse corrélation :** 

Dès que la base est prête on a commencé l’exploitation. Tout d’abord on a essayé de voir la corrélation entre tous les variables et le score Yuka. Le Heatmap ci-dessous montre les degrés de corrélation. 

![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.010.png)

*Figure 7 Heatmap des corrélations* 

Ce Heatmap confirme les informations données par Yuka. On peut voir que le **Nutri score** est le plus corrélé avec le score Yuka avec -0.81. Cette corrélation est négative ce qui est logique car le nutri score diminue avec l’augmentation de la qualité de nutriment. Et après on l’**additives\_n** qui représente le nombre d’additive existant dans l’aliment qui est corrélé négativement aussi et enfin le **nova\_group** qui communique une information sur les transformations utilisées pour avoir le produit final. 

Vu que le Nutri score est le plus impactant dans le score Yuka on va concentrer à la suite de l’étudier en étudiant la fonction utilité **U** attribué à ce dernier dans la formule  

**N**

**Score Yuka= 0.6\*U (Nutri score) + 0.3\* U (additives) + 0.1\* U (Bio)   (1)** 

**N A B**

3. **Fonction utilité Nutri-score :** 

Pour analyser mieux l’effet de Nutri score on a éliminé les effets des additifs. Par la préparation d’une sous base qui contient seulement les aliments qui sont sans additives. La contribution vient de ce critère est le maximum qui est égale à 30. 

Et dans ce cas pour calculer la fonction Utilité de Nutri-score on a utilisé la formule suivante : 

**UN (X) = Score Yuka(X) – (30 + 10**\* **IBIO) (2)** 

Avec **IBIO** représente l’indice Bio qui est égale à 1 si l’aliment porte la label biologique et 0 sinon. 

![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.011.png)

*Figure 8: Exemple de calcul de UN* 

Après ça on a divisé ce tableau en deux en séparant les aliments avec indice Bio égale à 1 et ceux avec indice Bio égale à 0. On a fait ça pour voir le comportement de chaque famille indépendamment d’où on limite les effets de critère Bio. 

*Figure 9: Exemple de UN des aliments Bio ![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.012.png)*

![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.013.png)

*Figure 10: Exemple de UN des aliments NON Bio* 

Après préparer les tableaux ce dessus on à essayer de tracer les fonctions utilité UN en fonction des Nutri- score. Les graphes représentent ces fonctions sont ce dessous nous aide à interpréter plus d’informations sur leurs variations.  

![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.014.png)

*Figure 13: fonction Utilité UN en fonction de Nutri-score* 

*Figure 11: UN en fonction de Nutri-score pour les aliments BIO  Figure 12: UN en fonction de Nutri-score pour les aliments NON BIO ![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.015.png)![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.016.png)*

En regardant les graphes ci-dessus, on peut voir qu'il y a des utilités différentes pour la même valeur de Nutri-score. D’où la fonction utilité ne respecte pas la définition d’une fonction. Et par suite elle ne peut pas être une fonction.  

Ces résultats nous donnent l’intuition d’investiguer si ce score Yuka est croissant avec le Nutri score comme il est déclaré ou non. Et pour faire ça, j’ai parcouru ma base pour trouver s’il y a un contre-exemple de monotonie. Et par le contre-exemple, je veux dire deux aliments qui ont le même critère BIO avec le même nombre d’additifs qui est zéro et ayant des Nutri score différents ne vérifiant pas la monotonie. C’est-à-dire l’aliment, avec le plus grand Nutri score, a le plus grand score Yuka. 

![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.017.png)

*Figure 14: Contre-exemple de la monotonie du Score Yuka* 

Comme il montre le contre-exemple ci-dessus, le score Yuka n’est pas monotone. Aussi ce Score ne peut pas être un modèle additif d’où cette hypothèse est rejetée.  

D’où j’ai décidé de faire un recours sur des modèles non additifs et je commencer à faire des modèles de Machine Learning. 

4. **Application Machine Learning :** 

Après l’élimination de l’hypothèse que score Yuka est sous cette forme additive : 

**Score Yuka= 0.6\*U (Nutri score) + 0.3\* U (additives) + 0.1\* U (Bio)   (1)** 

**N A B**

On a pensé à faire des essais avec les modèles de Machine Learning.  

1. **Pré-processing :** 

Afin d’avoir des bons résultats, on a commencé à préparer les entrés de modèles. Pour atteindre notre objective on a utilisé une technique qui s’appelle « One-Hot Encoding ».  Cette technique nous permet de transformer  une  variable  catégorique  en  plusieurs  variables  binaires.  On  l’a  utilisé  pour  la  variable 

![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.018.png)

*Figure 15: "One-Hot Encoding" appliqué sur additifs* 

- **additifs »** en la transformant à 101 variables binaires. Le résultat se trouve dans la figure ce dessous. Maintenant les entrés sont prêts et on peut commencer l’apprentissages. 
2. **Les Modèles :** 

Pour l’apprentissages on a décidé d’utiliser des modèles de différents comportements et architecture. Commençant par la régression linéaire pour confirmer le rejet de l’hypothèse qu’il est additif. Et après des modèles d’architecture Arbre et enfin un Modèle de Deep-learning qui est le réseau de neurones. En résumant les modèles utilisé sont comme suit : régression linéaire, arbre de décision, Random Forest et Réseau de neurones. 

Afin d’avoir le plus d’information, on a décidé de faire deux modèles de chaque type l’un en utilisant les additifs et l’autre en éliminant ce critère par prendre que les aliments avec zéro additif. 

3. **Résultats avec des additifs :** 

En entrainant nos modèles sur la base complète on a eu des résultats convenants. Ces résultats sont présentés sur le graphe ci-dessous *figure 17* où on a le « Erreur absolue moyenne » pour chaque modèle utilisé. 

![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.019.png)

*Figure 16: Comparaison de l'erreur absolue moyenne pour chaque modèle* 

Comme il montre la figure le pire c’est le modèle de régression linéaire ce qui est bien attendu après qu’on a  montré  que  le  modèle  est  non  additif.  En  contrepartie  le  Random  Forest  qui  est  un  modèle d’architecture arbre est le meilleur. Ces résultats sont bien attendus et se convient avec notre logique. 

4. **Résultats sans des additifs**  

En investiguant plus, on a entrainé les modèles sur une base des aliments avec zéro additif. Le résultat de ces modèles est représenté dans la figure ci-dessous.  

![](Aspose.Words.1ad758f9-9bb6-4a64-8118-0b72fabf0688.020.png)

*Figure 17: Comparaison de l'erreur absolue moyenne pour chaque modèle sans additif* 

En éliminant le critère des additifs la régression performe mieux mais n’a pas encore le meilleur. Dans tous les cas ces performance sont inutiles car on a éliminé un grand part des informations par supprimer les additives. 

5. **Conclusion :** 

Le score de Yuka est un moyen développé pour aider les consommateurs à choisir les bons aliments pour leurs  santés.  Mais  le  code  source  de  ce  score  n’est  pas  partagé  seule  l’attribution  de  chacun  est communique jusqu’à maintenant. En faisant notre étude on a constaté que score est n’est pas additif. Ça ne pose pas un grand problème mais le fait que n’est pas monotone avec le Nutri-score pose plusieurs questions. Sachant que le Nutri-score est développé par des organismes publics en France et utilisé avec l’accord de gouvernement. D’où c’est un peu difficile d’accepter un tel score lorsqu’il ne convient pas avec le Nutri-score.  Par suite il faut soit clarifier ce point par les responsables ou réviser l’algorithme de calcul et ajuster ce point. 

Tout le travail réalisé est disponible sur ce Lien :[ https://www.kaggle.com/dhiagharsallaoui/final-yuka ](https://www.kaggle.com/dhiagharsallaoui/final-yuka)
