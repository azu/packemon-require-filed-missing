```js
require("@file-cache/core")
```

throw following error:

```
node:internal/modules/cjs/loader:488
      throw e;
      ^

Error [ERR_PACKAGE_PATH_NOT_EXPORTED]: No "exports" main defined in /Users/azu/ghq/github.com/azu/packemon-require-filed-missing/node_modules/@file-cache/core/package.json
    at new NodeError (node:internal/errors:372:5)
    at throwExportsNotFound (node:internal/modules/esm/resolve:472:9)
    at packageExportsResolve (node:internal/modules/esm/resolve:693:7)
    at resolveExports (node:internal/modules/cjs/loader:482:36)
    at Function.Module._findPath (node:internal/modules/cjs/loader:522:31)
    at Function.Module._resolveFilename (node:internal/modules/cjs/loader:919:27)
    at Function.Module._load (node:internal/modules/cjs/loader:778:27)
    at Module.require (node:internal/modules/cjs/loader:1005:19)
    at require (node:internal/modules/cjs/helpers:102:18)
    at Object.<anonymous> (/Users/azu/ghq/github.com/azu/packemon-require-filed-missing/index.js:1:1) {
  code: 'ERR_PACKAGE_PATH_NOT_EXPORTED'
}

```

`@file-cache/core/package.json` is following:


```json5
  "types": "./dts/index.d.ts",
  "main": "./cjs/index.cjs",
  "dependencies": {
    "pkg-dir": "^6.0.1"
  },
  "devDependencies": {
    "mocha": "^10.0.0",
    "ts-node": "^10.8.2"
  },
  "scripts": {
    "test": "mocha \"tests/**/*.test.ts\""
  },
  "publishConfig": {
    "access": "public"
  },
  "exports": {
    "./package.json": "./package.json",
    "./*": {
      "types": "./dts/*.d.ts",
      "node": {
        "import": "./mjs/*.mjs"
      }
    },
    ".": {
      "types": "./dts/index.d.ts",
      "node": {
        "import": "./mjs/index.mjs"
      }
    }
  }
```

Workaround: add patch to the package.json

```diff
  "types": "./dts/index.d.ts",
  "main": "./cjs/index.cjs",
  "dependencies": {
    "pkg-dir": "^6.0.1"
  },
  "devDependencies": {
    "mocha": "^10.0.0",
    "ts-node": "^10.8.2"
  },
  "scripts": {
    "test": "mocha \"tests/**/*.test.ts\""
  },
  "publishConfig": {
    "access": "public"
  },
  "exports": {
    "./package.json": "./package.json",
    "./*": {
      "types": "./dts/*.d.ts",
      "node": {
        "import": "./mjs/*.mjs"
      }
    },
    ".": {
      "types": "./dts/index.d.ts",
      "node": {
        "import": "./mjs/index.mjs"
+       "require": "./cjs/index.cjs"
      }
    }
  }
```