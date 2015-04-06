### Prefer C++-style casts

Utiliser les new-style-cast (c++ casts):
 * `const_cast`: supprime le const
 * `dynamic_cast`: cast de base à derived en vérifiant que le type est bon
 * `static_cast`: fait à peu près tous les casts possibles (à utiliser le plus souvent)
 * `reinterpret_cast`: pour du code low level (par exemple, void* to int)

Ces casts sont plus lisibles et plus safe que le C-style cast.
