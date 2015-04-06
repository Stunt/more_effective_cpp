### Never treat arrays polymorphically

En effet, cela peut entrainer des effets de bords non voulus, bien que le programme compile.

Typiquement, imaginons que nous ayons deux classes:

```cpp
class Base { /* ... */ };
class Derived : public Base { /* ... */ };
```

On suppose ici que `sizeof(Derived) > sizeof(Base)`.

Imaginons que nous ayons également une fonction dump qui a pour but de dump un objet de type Base:

```cpp
void dump(Base array[], size_t size) {
  size_t i;
  
  for (i = 0; i < size; i++)
    std::cout << array[i];
};
```

Il est alors possible d'utiliser cette fonction de la manière suivante:

```cpp
int main(void) {
  Base array[50];
  
  dump(array, 50);
  
  return 0;
}
```

Jusque là, tout fonctionnera très bien. Cependant, il n'en sera pas de même si nous utilisons un array de manière polymorphique.

```cpp
int main(void) {
  // on crée un array de Derived
  Derived array[50];
  
  // On passe l'array à la fonction dump qui prends en param un array de Base
  dump(array, 50);
  
  return 0;
}
```

Le programme compilera, pour autant le comportement est undefined.
En effet, `sizeof(Derived) > sizeof(Base)`: cela va causer un problème lors du parcours du tableau dans ce second cas, car pour chaque élément, on se déplacera de `i * sizeof(Base)` plutôt que de `i * sizeof(Derived)`.
