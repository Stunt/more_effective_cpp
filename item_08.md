### Understand the different meanings of new and delete

Il faut bien faire la différence entre `new operator` et `operator new`.

`new operator` s'occupe de deux choses:
* allouer de la mémoire pour l'objet
* appeler le constructeur de l'objet

Pour allouer la mémoire de l'objet, il fait appel à `operator new` qui peut être overloadé afni de gérer l'allocation de mémoire différemment.

Il en est de même pour delete: il faut distinguer `delete operator` et `operator delete`.

`delete operator` s'occupe de deux choses:
* appeler le destructeur de l'objet
* désallouer la mémoire précédemment allouée

Pour désallouer la mémoire de l'objet, il fait appel à `operator delete` qui lui aussi peut être overloadé pour gére rla désalocation différemment.

```cpp
int main(void) {
  void *ptr = SomeClass::operator new(sizeof(SomeClass)); // call operator new: n'alloue que de la mémoire, sans call le constructeur
  SomeClass *obj = new (ptr) SomeClass(); // call new operator en placement new: call le constructeur et call le placement operator new (qui n'alloue rien car ptr est déjà alloué)
  obj->~SomeClass(); // call le destructeur manuellement.
  SomeClass::operator delete(obj); // call operator delete qui désalloue la mémoire
  
  SomeClass obj2 = new SomeClass(); // call new operator qui call operator new et le constructeur
  delete obj2;  // call delete operator qui call le destructeur et operator delete
}
```

Il existe également les opérateurs équivalent pour les arrays: `new[]` et `delete[]`.

Le principe est exactement le même, hormis que `new operator` va faire appel à plusieurs constructeur et `operator new` doit gérer l'allocation d'un tableau (et pareillement pour la désallocation avec delete).
