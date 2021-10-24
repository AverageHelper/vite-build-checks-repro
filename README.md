# Vite Build Process Error

This repository is designed to reproduce an issue where `vue-tsc` would fail to catch TypeScript type errors at build time.

[Vite's documentation](https://vitejs.dev/guide/features.html#typescript) states that "\[Vite\] assumes type checking is taken care of by your IDE and build process", then explains that one can "install `vue-tsc` and run `vue-tsc --noEmit` to also type check your \*.vue files". This appears to not be the case.

## Recommended IDE Setup

- [VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=johnsoncodehk.volar)

```sh
$ npm install
$ npm start
```

## The Issue

```sh
$ vue-tsc --noEmit && vite build
```

### Expected Behavior

The above command should finish with a nonzero exit code due to a TypeScript compiler error.

### Actual Behavior

The build completes without apparent issue. TypeErrors may throw at runtime, because the errors have not been caught by the CLI.
