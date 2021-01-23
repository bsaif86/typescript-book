# static constructors

TypeScript `class` \(like JavaScript `class`\) cannot have a static constructor. However, you can get the same effect quite easily by just calling it yourself:

```typescript
class MyClass {
    static initialize() {
        // Initialization
    }
}
MyClass.initialize();
```

