# Emitter

There are two `emitters` provided with the TypeScript compiler:

* `emitter.ts`: this is the emitter you are most likely to be interested in. Its the TS -&gt; JavaScript emitter.
* `declarationEmitter.ts`: this is the emitter used to create a _declaration file_ \(a `.d.ts`\) for a _TypeScript source file_ \(a `.ts` file\).

We will look at `emitter.ts` in this section.

## Usage by `program`

Program provides an `emit` function. This function primarily delegates to `emitFiles` function in `emitter.ts`. Here is the call stack:

```text
Program.emit ->
    `emitWorker` (local in program.ts createProgram) ->
        `emitFiles` (function in emitter.ts)
```

One thing that the `emitWorker` provides to the emitter \(via an argument to `emitFiles`\) is an `EmitResolver`. `EmitResolver` is provided by the program's TypeChecker, basically it is a subset of _local_ functions from `createChecker`.

