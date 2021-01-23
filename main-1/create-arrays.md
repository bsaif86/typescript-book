# Create Arrays

Creating an empty array is super easy:

```typescript
const foo:string[] = [];
```

If you want to create an array pre-filled with some content use the ES6 `Array.prototype.fill`:

```typescript
const foo:string[] = new Array(3).fill('');
console.log(foo); // ['','',''];
```

