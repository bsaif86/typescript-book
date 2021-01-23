# TIP: SyntaxKind enum

`SyntaxKind` is defined as a `const enum`, here is a sample:

```typescript
export const enum SyntaxKind {
    Unknown,
    EndOfFileToken,
    SingleLineCommentTrivia,
    // ... LOTS more
```

It's a `const enum` \(a concept [we covered previously](../../type-system/enums.md)\) so that it gets _inlined_ \(e.g. `ts.SyntaxKind.EndOfFileToken` becomes `1`\) and we don't get a dereferencing cost when working with the AST. However, the compiler is compiled with `--preserveConstEnums` compiler flag so that the enum _is still available at runtime_. So in JavaScript you can use `ts.SyntaxKind.EndOfFileToken` if you want. Additionally you can convert these enum members to display strings using the following function:

```typescript
export function syntaxKindToName(kind: ts.SyntaxKind) {
    return (<any>ts).SyntaxKind[kind];
}
```

