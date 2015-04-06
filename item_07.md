### Never overload &&, || or ,

En C et en C++, les expressions booléennes constituées de && et de || suivent certains règles:
* les expressions sont forcément évaluées de gauche à droite
* En fonction du résultat d'une des expressions, l'évaluation des expressions qui suivent peut s'arrêter.

Typiquement, dans le cas de `if (str != NULL && strlen(str) > 10)`, `strlen(str) > 10` ne sera pas évalué si `str` vaut `NULL`.

En overloadant les opérateurs && et ||, cela casse le processus.
En effet, en overloadant ces opérateurs, l'expression `if (expression1 && expression2)` sera remplacée soit par `if (expression1.operator&&(expression2))` ou par `if (operator&&(expression1, expression2))` en fonction de si l'overload est global ou est une fonction membre.

Or, les paramètres d'une fonction pouvant être évalués dans n'importe quel ordre avant l'appel de la fonction, les 2 règles précédemment énoncées ne sont plus valables.

Il en est de même pour l'opérateur ,.
