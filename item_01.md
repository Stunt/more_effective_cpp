### Distinguish between pointers and references

| Catégorie | Signe | Utilisation | Initialisation  à la déclaration | Reset | Nullable |
|-----------|-------|-------------|----------------------------------|-------|----------|
| Pointeurs | * | -> | Pas obligatoire | Possible | Oui |
| Références | & | . | Obligatoire | Impossible | Non |

Il faut utiliser les références dès que possible:
* plus simple d'utilisation
* syntaxe plus élégante
* moins de vérification à faire
* ....

Les pointeurs sont à utiliser dans quelques cas:
* Pointeur générique (base/derived classe)
* Si on veut le setter à NULL
* Si on veut changer la valeur du pointer
