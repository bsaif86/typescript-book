# Number

Chaque fois que vous manipulez des nombres dans n'importe quel langage de programmation, vous devez être conscient des particularités de la façon dont le langage gère les nombres. Voici quelques informations essentielles sur les nombres en JavaScript que vous devez connaître.

## Core Type

JavaScript n'a qu'un seul type de nombres. Il s'agit d'un `Number` de 64 bits à double précision. Ci-dessous, nous discutons de ses limites avec une solution recommandée.

## Decimal

Pour ceux qui connaissent le double / float dans d'autres langues, vous savez que les nombres binaires à virgule flottante _ne correspondent pas_ correctement aux nombres décimaux. Un exemple trivial \(et célèbre\) avec les nombres intégrés de JavaScript est illustré ci-dessous :

```javascript
console.log(.1 + .2); // 0.30000000000000004
```

> Pour un vrai calcul décimal, utilisez `big.js` mentionné ci-dessous.

## Integer

Les limites des entiers représentées par le type de nombre intégré sont `Number.MAX_SAFE_INTEGER` et `Number.MIN_SAFE_INTEGER`.

```javascript
console.log({max: Number.MAX_SAFE_INTEGER, min: Number.MIN_SAFE_INTEGER});
// {max: 9007199254740991, min: -9007199254740991}
```

**Safe** dans ce contexte fait référence au fait que la valeur _ne peut pas être le résultat d'une erreur d'arrondi_.

Les valeurs dangereuses sont à `+1 / -1` de ces valeurs sûres et toute quantité d'addition / soustraction _arrondira_ le résultat.

```javascript
console.log(Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2); // true!
console.log(Number.MIN_SAFE_INTEGER - 1 === Number.MIN_SAFE_INTEGER - 2); // true!

console.log(Number.MAX_SAFE_INTEGER);      // 9007199254740991
console.log(Number.MAX_SAFE_INTEGER + 1);  // 9007199254740992 - Correct
console.log(Number.MAX_SAFE_INTEGER + 2);  // 9007199254740992 - Arrondi !
console.log(Number.MAX_SAFE_INTEGER + 3);  // 9007199254740994 - Arrondi - correct by luck
console.log(Number.MAX_SAFE_INTEGER + 4);  // 9007199254740996 - Arrondi !
```

Pour vérifier la sécurité, vous pouvez utiliser ES6 `Number.isSafeInteger` :

```javascript
// Valeur sûre
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER)); // true

// Valeur non sûre
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1)); // false

// Parce qu'elle pourrait y avoir été arrondie en raison d'un débordement
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 10)); // false
```

> javascript a récemment obtenu le support de [bigint](https://javascript.info/bigint) support. Mais, vous pouvez également, si vous voulez des mathématiques entières de précision arbitraire, utiliser `big.js` mentionné ci-dessous.

## big.js

chaque fois que vous utilisez les mathématiques pour les calculs financiers \(par exemple, calcul gst, argent avec cents, ajout, etc.\) utilisez une bibliothèque comme [big.js](https://github.com/mikemcl/big.js/) qui est conçu pour

* mathématiques décimales parfaites
* valeurs hors limites sûres

l'installation est simple :

```bash
npm install big.js @types/big.js
```

exemple d'utilisation rapide :

```javascript
import { big } from 'big.js';

export const foo = new big('111.11111111111111111111');
export const bar = foo.plus(new big('0.00000000000000000001'));

// pour obtenir un nombre :
const x: number = number(bar.tostring()); // perd la précision
```

> N'utilisez pas cette bibliothèque pour les mathématiques utilisées à des fins intensives d'interface utilisateur / performances, par exemple des graphiques, des dessins sur toile, etc.

## nan

lorsqu'un calcul de nombre n'est pas représentable par un nombre valide, javaScript renvoie une valeur spéciale `NaN`. Un exemple classique est les nombres imaginaires :

```javascript
console.log(Math.sqrt(-1)); // NaN
```

Remarque : les contrôles d'égalité **ne fonctionnent pas** sur les valeurs `NaN`. Utilisez plutôt `Number.isNaN` :

```javascript
// Don't do this
console.log(NaN === NaN); // false!!

// Do this
console.log(Number.isNaN(NaN)); // true
```

## Infinity

Les limites extérieures des valeurs représentables dans Number sont disponibles en tant que valeurs statiques `Number.MAX_VALUE` et `-Number.MAX_VALUE`.

```javascript
console.log(Number.MAX_VALUE);  // 1.7976931348623157e+308
console.log(-Number.MAX_VALUE); // -1.7976931348623157e+308
```

Les valeurs en dehors de la plage où la précision n'est pas modifiée sont fixées à ces limites, par exemple :

```javascript
console.log(Number.MAX_VALUE + 1 == Number.MAX_VALUE);   // true!
console.log(-Number.MAX_VALUE - 1 == -Number.MAX_VALUE); // true!
```

Les valeurs en dehors de la plage où la précision est modifiée sont converties en valeurs spéciales `Infinity` / `-Infinity` par exemple.

```javascript
console.log(Number.MAX_VALUE + 10**1000);  // Infinity
console.log(-Number.MAX_VALUE - 10**1000); // -Infinity
```

Bien sûr, ces valeurs d'infini spéciales apparaissent également avec une arithmétique qui l'exige, par exemple :

```javascript
console.log( 1 / 0); // Infinity
console.log(-1 / 0); // -Infinity
```

Vous pouvez utiliser ces valeurs `Infinity` manuellement ou en utilisant des membres statiques de la classe `Number` comme indiqué ci-dessous :

```javascript
console.log(Number.POSITIVE_INFINITY === Infinity);  // true
console.log(Number.NEGATIVE_INFINITY === -Infinity); // true
```

Heureusement, les opérateurs de comparaison \(`<` / `>`\) fonctionnent de manière fiable sur les valeurs de l'infini :

```javascript
console.log( Infinity >  1); // true
console.log(-Infinity < -1); // true
```

## Infinitesimal

La plus petite valeur non nulle représentable dans Number est disponible en tant que `Number.MIN_VALUE` statique.

```javascript
console.log(Number.MIN_VALUE);  // 5e-324
```

Les valeurs inférieures à `MIN_VALUE` \("valeurs de sous-dépassement"\) sont converties en 0.

```javascript
console.log(Number.MIN_VALUE / 10);  // 0
```

> Intuition supplémentaire : tout comme les valeurs supérieures à `Number.MAX_VALUE` sont fixées à INFINITY, les valeurs inférieures à `Number.MIN_VALUE` sont limitées à `0`.

