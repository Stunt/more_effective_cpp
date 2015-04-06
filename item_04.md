### Avoid gratuitous default constructor

Avoir un constructeur par défaut ne prenant pas de paramètres peut s'avérer utile dans certains cas:
* Initialisation de tableau: un simple `SomeClass array[50]` ne suffira plus puisqu'il faut fournir un argument au constructeur de chaque object initalisé.
* Init dans des containeurs templatés: Pour le même genre de raisons qu'au dessus.
* Init de la base class dans une derived class: il faut alors que chaque derived class init la base class dans son init list.

Cependant, bien que la tentation de créer un constructeur par défaut soit alors grande, cela reste déconseillé.

En effet, il faut alors setter des valeurs spécifiques marquant la non-initialisation (par exemple `NULL`).
Cela implique alors d'inclure des tests de vérification dans un certains nombres de fonctions membres, mais également à l'extérieur (dans le cas où la fonction membre fail à cause d'une valeur non initialisée):
* Le programme est alors moins performant
* Le binaire généré est plus lourd
