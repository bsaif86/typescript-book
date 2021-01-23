# singleton pattern

The conventional singleton pattern is really something that is used to overcome the fact that all code must be in a `class`.

```typescript
class Singleton {
    private static instance: Singleton;
    private constructor() {
        // do something construct...
    }
    static getInstance() {
        if (!Singleton.instance) {
            Singleton.instance = new Singleton();
            // ... any one time initialization goes here ...
        }
        return Singleton.instance;
    }
    someMethod() { }
}

let something = new Singleton() // Error: constructor of 'Singleton' is private.

let instance = Singleton.getInstance() // do something with the instance...
```

However, if you don't want lazy initialization you can instead just use a `namespace`:

```typescript
namespace Singleton {
    // ... any one time initialization goes here ...
    export function someMethod() { }
}
// Usage
Singleton.someMethod();
```

> Warning : Singleton is just a fancy name for [global](http://stackoverflow.com/a/142450/390330)

For most projects `namespace` can additionally be replaced by a _module_.

```typescript
// someFile.ts
// ... any one time initialization goes here ...
export function someMethod() { }

// Usage
import {someMethod} from "./someFile";
```

