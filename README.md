# es6-lib

This project illustrates how a library written in es6 can be published supporting [tree shaking](https://webpack.js.org/guides/tree-shaking/). I intentionally created a minimal React component library as it involves some extra configuration than a simple tiny module.

The library in the [src](./src) directory is written in es6 using the [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) and [`export`](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export) syntax. The project that uses this library can be found in the [test](./test) directory.

The library is bundled with [rollup](https://rollupjs.org/guide/en). The [rollup configuration](./rollup.config.js) uses babel to create an es module using the `import` and `export` syntax.

The project is bundled with [webpack](https://webpack.js.org/) as it is meant to run as a stand-alone application in the browser. The [webpack configuration](./test/webpack.config.js) has a specific optimization configuration to create separate bundles for vendor libraries and webpack runtime code. This optimization configuration is not needed, and it is only used in this case to isolate the code that is produced for the `index.js` file.

## How to test it yourself

Clone the project and install and build the library:

```
yarn install && yarn build
```

Then navigate to the [test](./test) directory and install and build the project:

```
cd test && yarn install && yarn build
```

Notice that the webpack output will contain something like the following:

```
index.js  221 bytes       0  [emitted]  index
```

Now if we import and use the `Input` component in [./test/src/index](./test/src/index.js):

```jsx
import React from 'react';
import { render } from 'react-dom';
import { Button, Input } from 'es6-lib';

render(
  <div>
    <Button />
    <Input />
  </div>,
  document.getElementById('root')
);
```

and run `yarn build` again, we'll notice that the size of the `index.js` file increased.

```
index.js  279 bytes       0  [emitted]  index
```

That is because `webpack` now imported the code for the `Input` component as well. In the previous build, `webpack` didn't include the code for the `Input` component from the `es6-lib` package, as we were not using it.
