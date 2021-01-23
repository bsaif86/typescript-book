# globals.d.ts

We discussed _global_ vs. _file_ modules when covering [projects](./) and recommended using file based modules and not polluting the global namespace.

Nevertheless, if you have beginning TypeScript developers you can give them a `globals.d.ts` file to put interfaces / types in the global namespace to make it easy to have some _types_ just _magically_ available for consumption in _all_ your TypeScript code.

> For any code that is going to generate _JavaScript_ we highly recommend using _file modules_.

* `globals.d.ts` is great for adding extensions to `lib.d.ts` if you need to.
* It's good for quick `declare module "some-library-you-dont-care-to-get-defs-for";` when doing JS to TS migrations.

