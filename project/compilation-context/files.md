# Which Files?

Use `include` and `exclude` to specify files / folders / globs. E.g.:

```javascript
{
    "include":[
        "./folder"
    ],
    "exclude":[
        "./folder/**/*.spec.ts",
        "./folder/someSubFolder"
    ]
}
```

## Globs

* For globs : `**/*` \(e.g. sample usage `somefolder/**/*`\) means all folder and any files \(the extensions `.ts`/`.tsx` will be assumed and if `allowJs:true` so will `.js`/`.jsx`\)

## `files` option

You can either use `files` to be explicit.

```javascript
{
    "files":[
        "./some/file.ts"
    ]
}
```

But it is not recommended as you have to keep updating it. Instead use `include` to just add the containing folder.

