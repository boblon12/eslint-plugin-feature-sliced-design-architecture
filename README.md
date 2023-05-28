# eslint-plugin-ulbi-tv-plugin

Custom eslint plugin to follow FSD (Feature Sliced Design) frontend architecture rules.
[FSD Architecture documentation](https://feature-sliced.design/docs/get-started/overview)

## Installation

You'll first need to install [ESLint](https://eslint.org/):

```sh
npm i eslint --save-dev
```

Next, install `eslint-plugin-ulbi-tv-plugin`:

```sh
npm install eslint-plugin-ulbi-tv-plugin --save-dev
```

## Usage

Add `feature-sliced-design-architecture-plugin` to the plugins section of your `.eslintrc` configuration file. You can omit the `eslint-plugin-` prefix:

```json
{
    "plugins": [
        "feature-sliced-design-architecture"
    ]
}
```


Then configure the rules you want to use under the rules section.

```json
{
    "rules": {
        "feature-sliced-design-architecture/rule-name": 2
    }
}
```

## Supported Rules

# Rule for check imports from layers structure of FSD architecture (`feature-sliced-design-architecture/layer-imports`)

<!-- end auto-generated rule header -->

## Rule Details

Rule for check imports from layers structure of FSD architecture

### Options

| Name                     | Description                                    |
| :----------------------- | :--------------------------------------------- |
| alias                    | The alias with which the import path begins    |
| ignoreImportPatterns     | Array of glob path templates to ignore of rule |

### Examples options settings

```js
'feature-sliced-design-architecture/layer-imports': ['error', {
  alias: '@', // Example import path with alias - @/shared/...
  ignoreImportPatterns: ['**/StoreProvider', '**/*.test.*'],
}],
```

### Examples

Examples of **correct** code for this rule:

```js
// File path C:/Users/project/src/features/Component
import Button from 'shared/Button.tsx'
import Post from 'entities/Post.tsx'

// With option: alias: '@'
// File path C:/Users/project/src/features/Component
import Button from '@/shared/Button.tsx'
import Post from '@/entities/Post.tsx'

// With option: ignoreImportPatterns: ['**/StoreProvider']
// File path C:/Users/project/src/features/Component
import { StateSchema } from 'app/providers/StoreProvider'
```

Examples of **incorrect** code for this rule:

```js
// File path C:/Users/project/src/entities/Component
import AddComment from '@/features/AddComment' // Error: A layer can only import the underlying layers into itself. (app > pages > widgets > features > entities > shared)
```

# Checking imports against FSD architecture rules (`feature-sliced-design-architecture/import-path-check`)

<!-- end auto-generated rule header -->

## Rule Details

Checking imports against FSD architecture rules

### Options

| Name                     | Description                                    |
| :----------------------- | :--------------------------------------------- |
| alias                    | The alias with which the import path begins    |

### Examples options settings

```js
'feature-sliced-design-architecture/import-path-check': ['error', {
  alias: '@', // Example import path with alias - @/shared/...
}],
```

### Examples

Examples of **correct** code for this rule:

```js
// File path C:/Users/project/src/features/AddComment
import Button from 'shared/Button.tsx'
import { getCommentStatus } from '../../model/selectors/getCommentStatus'

// With option: alias: '@'
// File path C:/Users/project/src/features/AddComment
import Button from '@/shared/Button.tsx'
import { getCommentStatus } from '../../model/selectors/getCommentStatus'
```

Examples of **incorrect** code for this rule:

```js
// File path C:/Users/project/src/features/AddComment
import { getCommentStatus } from 'features/model/selectors/getCommentStatus'
// Error: Within one slice, all import paths must be relative

// With option: alias: '@'
// File path C:/Users/project/src/features/AddComment
import { getCommentStatus } from '@/features/model/selectors/getCommentStatus'
// Error: Within one slice, all import paths must be relative

// File path C:/Users/project/src/features/AddComment
import { Currency } from '../shared/const/currency'
// Error: Shared layer import must be absolute path
```

# FSD Architecture rule for public api imports (`feature-sliced-design-architecture/public-api-imports`)

<!-- end auto-generated rule header -->

## Rule Details

FSD Architecture rule for public api imports

### Options

| Name                     | Description                                                                                  |
| :----------------------- | :---------------------------------------------                                               |
| alias                    | The alias with which the import path begins                                                  |
| testFilesPatterns        | Array of glob path templates to need import files from testing public API from testing.js/ts |

### Examples options settings

```js
'feature-sliced-design-architecture/public-api-imports': ['error', {
  alias: '@', // Example import path with alias - @/shared/...
  ignoreImportPatterns: ['**/StoreProvider', '**/*.test.*'],
}],
```

### Examples

Examples of **correct** code for this rule:

```js
import { getData } from '../../model/selectors/getData'
import { Component } from 'entities/Component'

// With option: alias: '@'
import { Component } from '@/entities/Component'

// With option: testFilesPatterns: ['**/*.test.*']
// File path C:/Users/project/src/entities/Component/file.test.ts
import { Component } from 'entities/Component/testing'

// File path 'C:/Users/project/src/widgets/Profile/model/selectors/getUserProfile.ts',
import { getUser } from 'entities/User'

// With option: alias: '@'
// File path 'C:/Users/project/src/widgets/Profile/model/selectors/getUserProfile.ts',
import { getUser } from '@/entities/User'
```

Examples of **incorrect** code for this rule:

```js
import { Some } from 'entities/Component/model/some.ts'
// Error: Absolute import is allowed only from public API (index.js/ts)

// With option: alias: '@'
import { Some } from '@/entities/Component/model/some.ts'
// Error: Absolute import is allowed only from public API (index.js/ts)

// With option: testFilesPatterns: ['**/*.test.*']
// File path C:/Users/project/src/entities/Component/file.test.ts
import { Some } from 'entities/Component/testing/some.ts'
// Error: Absolute import is allowed only from public API (index.js/ts)

// With option: testFilesPatterns: ['**/*.test.*']
// File path C:/Users/project/src/entities/Component/forbidden.ts
import { Some } from 'entities/Component/testing'
// Error: Test data need import from public API file for tests (testing.js/ts) only in files from testFilesPatterns option

// File path 'C:/Users/project/src/widgets/Profile/model/selectors/getUserProfile.ts',
import { getUser } from '../../../../entities/User/model/selectors/getUser/getUser'
// Error: Must be imported from a public API file (index.js/ts)
```


