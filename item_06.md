### Distinguish between prefix and postfix forms of increment and decrement operators

Overload de la pré-(incrémentation|décrémentation):

```cpp
class SomeClass {

public:
  // retourne une référence sur l'objet
  // ne prend pas de paramètre
  SomeClass &operator++(void) {
    // applique directement l'incrémentation sur l'objet courant
    ++some_value;
    
    // retourne une référence sur l'objet courant
    return *this;
  }

};
```

Overload de la post-(incrémentation|décrémentation)

```cpp
class SomeClass {

public:
  // retourne une copie constant
  // prend en paramètre un int (qui a toujours pour valeur 0)
  const SomeClass operator++(int) {
    // stocke une copie de l'objet inchangé
    SomeClass old = *this;
    
    // incrémente l'objet courant en faisant appel à la pré-incrémenation
    ++(*this);
    
    // retourne la copie inchangée
    return old;
  }

};
```

Comme on peut le voir dans les exemples, afin de pouvoir différencier l'overload de la pré incrémentation de celui de la post incrémentation:
* la pré-incrémentation ne prends pas de paramètre
* la post-incrémentation prends un int en paramètre dont la valeur vaut toujours 0

Par ailleurs, la pré incrémentation modifie directement l'objet et en retourne une référence.

La post-incrémentation, au contraire, retourne une copie inchangée qui sera utilisée pour les opérations à faire avant l'incrémentation.

La post-incrémentation retourne un objet *constant* pour éviter certains utilisations telles que `obj++++`.

Afin d'avoir le moins de redondance de code, la post-incrémenation fait appel à la pré-incrémentation.

En raison de la lourdeur des opérations impliquées par une post-incrémentation (création d'une copie inchangée), il faut utiliser la pré-incrémentation dès que possible.
