### Be wary of user-defined conversion functions

Il existe deux manières de faire de la conversion implicite pour un user-defined type:
* via l'operator de convertion implicite
* via un constructeur a un seul paramètre

Pour l'opérateur de conversion implicite:

```cpp
class SomeClass {

public:
  operator char(void) const { return 'a'; }

};

int main(void) {
  SomeClass obj;
  
  char c = obj; // conversion implicite de SomeClass en char
}
```

Pour le constructeur a un seul paramètre:

```cpp
class SomeClass {

public:
  SomeClass(int) {} // un seul paramètre
  SomeClass(int, int = 0) {} // également éligible pour une conversion implicite

}

void fct(const SomeClass &obj) {}

int main(void) {
  fct(10); // conversion implicite de 10 en SomeClass
}
```

Cependant, la conversion implicite est à éviter dans la majorité des cas car elle peut induire un comportement non prévu.

Typiquement, dans le cas de l'operateur de conversion implicite, elle peut créer un appel à la mauvaise fonction:

```cpp
class Rational {

public:
  operator double(void) const { /* return some double */ }
};

int main(void) {
  Rational r;
  
  std::cout << r;
}
```

Ici, plutôt que de lever une erreur à la compilation expliquant qu'il n'existe pas d'overload de l'operateur `<<` pour un object de type Rational, l'objet Rational va être implicitement converti en double.
Ainsi, si l'on voulait afficher un Rational sous la forme `1/2`, on se retrouvera malgré tout avec un affichage de `0.5`.

Il est ainsi conseillé de créé des fonctions membres faisant la conversion explicitement (par exemple, une fonction `to_double`, au même titre que la fonction `c_str` pour `std::string`).

De la même manière, un constructeur permettant la conversion implicite peut induire des erreurs:

```cpp
template <typename T>
class Array {

public:
  // constructeur permettant une conversion implicite de int vers Array
  Array(int size) {}

};

// un overload de l'operator == pour des Array
bool operator==(const Array &lhs, const Array &rhs) {}

int main(void) {
  // on crée un array de 10 cases
  Array<int> array(10);
  
  int i;
  // on parcours l'array
  for (i = 0;  i < 10; i++)
    // erreur de frappe: on écrit array au lieu de array[i]
    if (array == array[i])
      do_something();
      
  return 0;
}
```

Ici, on a fait une faute d'inatention dans le code en écrivant `array` au lieu de `array[i]`.
On pourrait penser que cela créera une erreur de compilation. Cependant, à cause d'une conversion implicite, ce ne sera pas le cas.
En effet, `array[i]` étant un int, il y aura une conversion implicite pour créer un Array temporaire et ainsi pouvoir comparer deux Array entre eux.

Il est alors conseillé d'utilisé le keywork `explicit` dès que possible pour éviter ce type de conversion implicite.

```cpp
class Array {

public:
  // constructeur déclaré en explicit: problème réglé.
  explicit Array(int size) {}
  
};
```

