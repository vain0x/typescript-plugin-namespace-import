# typescript-plugin-namespace-import

A [TypeScript Language Service Plugin](https://github.com/microsoft/TypeScript/wiki/Writing-a-Language-Service-Plugin) to insert [Namespace Import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#import_an_entire_modules_contents) automatically.

## Motivation

We often use an object as a namespace.

```typescript
// someLogics.ts
export const someLogics = {
  calculate() { ... },
  print() { ... },
};

// main.ts
import { someLogics } from "./someLogics.ts";

someLogics.calculate()
// `someLogics` is auto-completable without import!
```

This is good way in developer experience, but it obstruct tree-shaking. In this case, `someLogics.print` will be included in bundle although it's not used.

To keep tree-shaking working, we can use [Namespace Import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#import_an_entire_modules_contents).

```typescript
// someLogics.ts
export function calculate() { ... }
export function print() { ... }

// main.ts
import * as someLogics from "./someLogics.ts";

someLogics.calculate()
// `someLogics` is NOT auto-completable without import :(
```

Now we can tree-shake `someLogics.print`. However, developer experience get worse because we can't auto-complete `someLogics` without import statement. We need to write import statement by ourselves.

typescript-plugin-namespace-import resolves this problem by making Namespace Import auto-completable.

## Installation

Install with npm/yarn.

```sh
npm install -D typescript-plugin-namespace-import
# or yarn add -D typescript-plugin-namespace-import
```

Then add this plugin in `tsconfig.json`.

```json
{
  "compilerOptions": {
    "plugins": [
      {
        "name": "typescript-plugin-namespace-import",
        "options": {
          "paths": ["src/logics"]
        }
      }
    ]
  }
}
```

`paths` option is required. See below for detail.

## Options

### paths (required)

Specify directory in relative path to the project's root (`tsconfig.json`'s dir). All `.ts` or `.js` files in the directories can be Namespace Imported with auto-completion.
