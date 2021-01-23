# StyleGuide

> An unofficial TypeScript StyleGuide

People have asked me for my opinions on this. Personally I don't enforce these a lot on my teams and projects but it does help to have these mentioned as a tie breaker when someone feels the need to have such strong consistency. There are other things that I feel much more strongly about and those are covered in the [tips chapter](main-1/) \(e.g. type assertion is bad, property setters are bad\) 🌹.

Key Sections:

* [Variable](styleguide.md#variable-and-function)
* [Class](styleguide.md#class)
* [Interface](styleguide.md#interface)
* [Type](styleguide.md#type)
* [Namespace](styleguide.md#namespace)
* [Enum](styleguide.md#enum)
* [`null` vs. `undefined`](styleguide.md#null-vs-undefined)
* [Formatting](styleguide.md#formatting)
* [Single vs. Double Quotes](styleguide.md#quotes)
* [Tabs vs. Spaces](styleguide.md#spaces)
* [Use semicolons](styleguide.md#semicolons)
* [Annotate Arrays as `Type[]`](styleguide.md#array)
* [File Names](styleguide.md#filename)
* [`type` vs `interface`](styleguide.md#type-vs-interface)

## Variable and Function

* Use `camelCase` for variable and function names

> Reason: Conventional JavaScript

**Bad**

```typescript
var FooVar;
function BarFunc() { }
```

**Good**

```typescript
var fooVar;
function barFunc() { }
```

## Class

* Use `PascalCase` for class names.

> Reason: This is actually fairly conventional in standard JavaScript.

**Bad**

```typescript
class foo { }
```

**Good**

```typescript
class Foo { }
```

* Use `camelCase` of class members and methods

> Reason: Naturally follows from variable and function naming convention.

**Bad**

```typescript
class Foo {
    Bar: number;
    Baz() { }
}
```

**Good**

```typescript
class Foo {
    bar: number;
    baz() { }
}
```

## Interface

* Use `PascalCase` for name.

> Reason: Similar to class

* Use `camelCase` for members.

> Reason: Similar to class

* **Don't** prefix with `I`

> Reason: Unconventional. `lib.d.ts` defines important interfaces without an `I` \(e.g. Window, Document etc\).

**Bad**

```typescript
interface IFoo {
}
```

**Good**

```typescript
interface Foo {
}
```

## Type

* Use `PascalCase` for name.

> Reason: Similar to class

* Use `camelCase` for members.

> Reason: Similar to class

## Namespace

* Use `PascalCase` for names

> Reason: Convention followed by the TypeScript team. Namespaces are effectively just a class with static members. Class names are `PascalCase` =&gt; Namespace names are `PascalCase`

**Bad**

```typescript
namespace foo {
}
```

**Good**

```typescript
namespace Foo {
}
```

## Enum

* Use `PascalCase` for enum names

> Reason: Similar to Class. Is a Type.

**Bad**

```typescript
enum color {
}
```

**Good**

```typescript
enum Color {
}
```

* Use `PascalCase` for enum member

> Reason: Convention followed by TypeScript team i.e. the language creators e.g `SyntaxKind.StringLiteral`. Also helps with translation \(code generation\) of other languages into TypeScript.

**Bad**

```typescript
enum Color {
    red
}
```

**Good**

```typescript
enum Color {
    Red
}
```

## Null vs. Undefined

* Prefer not to use either for explicit unavailability

> Reason: these values are commonly used to keep a consistent structure between values. In TypeScript you use _types_ to denote the structure

**Bad**

```typescript
let foo = {x:123,y:undefined};
```

**Good**

```typescript
let foo:{x:number,y?:number} = {x:123};
```

* Use `undefined` in general \(do consider returning an object like `{valid:boolean,value?:Foo}` instead\)

_**Bad**_

```typescript
return null;
```

_**Good**_

```typescript
return undefined;
```

* Use `null` where its a part of the API or conventional

> Reason: It is conventional in Node.js e.g. `error` is `null` for NodeBack style callbacks.

**Bad**

```typescript
cb(undefined)
```

**Good**

```typescript
cb(null)
```

* Use _truthy_ check for **objects** being `null` or `undefined`

**Bad**

```typescript
if (error === null)
```

**Good**

```typescript
if (error)
```

* Use `== undefined` / `!= undefined` \(not `===` / `!==`\) to check for `null` / `undefined` on primitives as it works for both `null`/`undefined` but not other falsy values \(like `''`,`0`,`false`\) e.g.

**Bad**

```typescript
if (error !== null)
```

**Good**

```typescript
if (error != undefined)
```

## Formatting

The TypeScript compiler ships with a very nice formatting language service. Whatever output it gives by default is good enough to reduce the cognitive overload on the team.

Use [`tsfmt`](https://github.com/vvakame/typescript-formatter) to automatically format your code on the command line. Also your IDE \(atom/vscode/vs/sublime\) already has formatting support built-in.

Examples:

```typescript
// Space before type i.e. foo:<space>string
const foo: string = "hello";
```

## Quotes

* Prefer single quotes \(`'`\) unless escaping.

> Reason: More JavaScript teams do this \(e.g. [airbnb](https://github.com/airbnb/javascript), [standard](https://github.com/feross/standard), [npm](https://github.com/npm/npm), [node](https://github.com/nodejs/node), [google/angular](https://github.com/angular/angular/), [facebook/react](https://github.com/facebook/react)\). Its easier to type \(no shift needed on most keyboards\). [Prettier team recommends single quotes as well](https://github.com/prettier/prettier/issues/1105)
>
> Double quotes are not without merit: Allows easier copy paste of objects into JSON. Allows people to use other languages to work without changing their quote character. Allows you to use apostrophes e.g. `He's not going.`. But I'd rather not deviate from where the JS Community is fairly decided.

* When you can't use double quotes, try using back ticks \(\`\).

> Reason: These generally represent the intent of complex enough strings.

## Spaces

* Use `2` spaces. Not tabs.

> Reason: More JavaScript teams do this \(e.g. [airbnb](https://github.com/airbnb/javascript), [idiomatic](https://github.com/rwaldron/idiomatic.js), [standard](https://github.com/feross/standard), [npm](https://github.com/npm/npm), [node](https://github.com/nodejs/node), [google/angular](https://github.com/angular/angular/), [facebook/react](https://github.com/facebook/react)\). The TypeScript/VSCode teams use 4 spaces but are definitely the exception in the ecosystem.

## Semicolons

* Use semicolons.

> Reasons: Explicit semicolons helps language formatting tools give consistent results. Missing ASI \(automatic semicolon insertion\) can trip new devs e.g. `foo() \n (function(){})` will be a single statement \(not two\). TC39 [warning on this as well](https://github.com/tc39/ecma262/pull/1062). Example teams: [airbnb](https://github.com/airbnb/javascript), [idiomatic](https://github.com/rwaldron/idiomatic.js), [google/angular](https://github.com/angular/angular/), [facebook/react](https://github.com/facebook/react), [Microsoft/TypeScript](https://github.com/Microsoft/TypeScript/).

## Array

* Annotate arrays as `foos:Foo[]` instead of `foos:Array<Foo>`.

> Reasons: Its easier to read. Its used by the TypeScript team. Makes easier to know something is an array as the mind is trained to detect `[]`.

## Filename

Name files with `camelCase`. E.g. `accordian.tsx`, `myControl.tsx`, `utils.ts`, `map.ts` etc.

> Reason: Conventional across many JS teams.

## type vs. interface

* Use `type` when you _might_ need a union or intersection:

```text
type Foo = number | { someProperty: number }
```

* Use `interface` when you want `extends` or `implements` e.g

```text
interface Foo {
  foo: string;
}
interface FooBar extends Foo {
  bar: string;
}
class X implements FooBar {
  foo: string;
  bar: string;
}
```

* Otherwise use whatever makes you happy that day.

