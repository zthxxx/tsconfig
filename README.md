# Zthxxx's base tsconfig

## Files

### `tsconfig.json`

The basic tsconfig. Designed to be **inherited** by package root `tsconfig.json`, used for IDE type inference.

### `build.esm.json` / `build.cjs.json`

Designed to be as **symlink** / **copied** beside package root `tsconfig.json`, use for build `esm`/`cjs` and `.d.ts` files.

### `build.types.json`

Designed to be as **symlink** / **copied** beside package root `tsconfig.json`, use for build ONLY `.d.ts` files.

## Usage

install

```bash
npm i -D @zthxxx/tsconfig
```

Example in package root `tsconfig.json`:

```json
{
  "extends": "@zthxxx/tsconfig/tsconfig.json",
  "compilerOptions": {
    "rootDir": "src",
    "outDir": "es",
    "baseUrl": ".",
    
    // some external types
    "types": [
      "node",
    ],
  },
  "include": [
    "src",
  ],
  "references": [
    // relative path reference to other monorepo package
    // use for IDE `Go to Source Definition`
    { "path": "..." },
  ],
}
```


## `composite` and `references`

Notice that I don't use `composite:true` for monorepo, even if use `references` with each other,

since `composite` will make `tsc` compile all of `references packages` at once 
and depend on their source code, 

but in monorepo usually it's wrong,

I want compile packages independent and every package in compiling should use output `.d.ts` with others.

<br/>

`references` without `composite:true` maybe cause IDE warn you some error,

like `Referenced project '...' must have setting "composite": true.`,

DON'T worry, it makes no influence to `tsc` or any other compiler or bundler.

<br/>

`references` is useful in IDE for the function `Go to Source Definition` in monorepo,

like via Ctrl/Command + Click, will jump to source code file with correct line.

That also needs set `outDir` path point to `types` field paths in `package.json`.

Otherwise without `references`,
the `Go to Source Definition` only jump to `node_modules/`
or compiled output file `.d.ts` in such as `dist/` or `es/`.



## `references` or `paths`

Although use `paths` in tsconfig also support IDE `Go to Source Definition` to monorepo source files,

but it's just make other package's source code files as source files in the current package, NOT ONLY type declaration.

So while `tsc` compiling, it will also output product of compilation(`.d.ts`/`.js`)
beside their source files in other packages
