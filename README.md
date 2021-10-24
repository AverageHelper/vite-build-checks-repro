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

Run the following command:

```sh
$ vue-tsc --noEmit && vite build
```

### Expected Behavior

The above command should finish with a nonzero exit code due to a TypeScript compiler error.

We have defined a `Thing` interface in [src/@types/global.d.ts](src/@types/global.d.ts).

We use the `Thing` interface in [src/components/HelloWorld.vue](src/components/HelloWorld.vue) to annotate the type of a computed property that defines an object literal. VSCode reports this error inline, but `vue-tsc` does not seem to notice the error at all.

### Actual Behavior

The build completes without apparent issue. TypeErrors may throw at runtime, because the errors have not been caught by the CLI.

## The Workaround (See [volar#630](https://github.com/johnsoncodehk/volar/issues/630#issuecomment-950391747))

This issue has to do with the way we use multiple tsconfig files. [`vue-tsc` does not support the `composite` option (yet).](https://github.com/johnsoncodehk/volar/issues/630#issuecomment-950391747)

1. Remove the `composite` property from all tsconfig files.
2. Select the appropriate tsconfig file when running `vue-tsc`:

```sh
$ vue-tsc --noEmit --project tsconfig.prod.json && vite build
```

## Why Fixing this Matters

### CLI Typechecks Are Important

Evan You [suggests](https://github.com/vitejs/vite/issues/2539#issuecomment-800291208) that developers should lean on their IDE to report type checks. This is all well and good when working with types that are defined and used in the same file. However, in a production environment, there are likely to be dozens of moving parts, many of which are abstracted into their own modules for ease of maintenance.

Consider this repository, where a developer has modified the `Thing` interface in [src/@types/global.d.ts](src/@types/global.d.ts) to add a `note` property. The `HelloWorld` component creates an object that implements the `Thing` interface, but has not been updated to add the new property. In fact, when that file is not open in VSCode, the IDE reports no errors from that file. My team expects the CI/CD pipeline, especially `vue-tsc`, to report the error before this broken code hits production.

I cannot expect nor trust my team to open every file in a large project just so that VSCode can report errors inline. We must have these errors exposed in CI/CD.

### TypeScript Project References Are Important

TypeScript added this feature for a reason, and since Vue 3 is written in TypeScript, `vue-tsc` should support it, or document that it won't support it.
