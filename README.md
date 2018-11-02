# react-ecmascript

ECMAScript module versions of
- react.production.min.js
- react-dom.production.min.js
- react.development.js
- react-dom.development.js

with the right exports and imports to use them directly in [browsers which support ECMAScript modules and module loading](https://caniuse.com/#feat=es6-module).

react and react-dom version: 16.6.0

## usage

### pkg.module
The module property in package.json only supports one file, and this creates four modules to use. Therefor the pkg.module isn't used and you need to copy the files yourself. Or generate them before you use them in development or during your build step.

### during development import them directly from unpkg.com
```js
import React from 'https://unpkg.com/react-ecmascript@1.2.0/react.development.mjs';
import ReactDOM from 'https://unpkg.com/react-ecmascript@1.2.0/react-dom.development.mjs';
```

#### copy paste
In the root of this repo are the four script files.
- react.development.mjs
- react-dom.development.mjs
- react.production.min.mjs
- react-dom.production.min.mjs

You can copy paste the relevant files and upload them to wherever you want. They should work right away because the module specifier is a relative one.

#### As an NPM module in node
Install
```bash
npm i react-ecmascript
```

Than generate the sources in Node
```js
const reactEcmascript = require('react-ecmascript');

reactEcmascript('someUrl') // optional uri as argument, pass in the folder. the correct filename will get appended
  .then(sources => {
    console.log(sources);
    /*
    {
      'react.production.min.mjs': 'content as string',
      'react-dom.production.min.mjs': 'content as string',
      'react.development.mjs': 'content as string',
      'react-dom.development.mjs': 'content as string'
    }
    */

    // do something with these sources, like saving them to disk to use them in your project
  });
```


#### Replacing the import uri for react automatically
The optional argument that can be passed to the function exported by the react-ecmascript module is the directory where these files will be made available to be used in a browser. The relevant filename to import will get appended automatically (production.min or development). In most cases this will not be needed though.

Either use ```--uri``` or ```-u``` followed by the uri. All the CLI ways (via npm run, node of execting will overwrite the four files in the root.

You can also pass the uri argument through the CLI, like this
```bash
npm run build -- --uri https://some-uri.com/vendor/
```
(mention the extra -- to have npm pass the arguments down to the script)

Or when using the Node script directly
```bash
node src/index.js --uri https://some-other-uri.com/
```
or
```bash
node src/index.js -u /some/dir/
```

or even when the src/index.js file is made executable you can execute it from the CLI
```bash
./src/index.js -u /
```

### Usage with Rollup
The files generated by this script can be used directly in Rollup. To use these files through Rollup you will need to add the correct settings to the Rollup config.

Add react and react-dom to the output.paths object so Rollup knows with what to replace the module specifier in the files that import react and react-dom:
```js
{
  react: urlWhereTheReactFileWillBeDeployed,
  'react-dom': urlWhereTheReactDOMFileWillBeDeployed
}
```
and also flag them as being external in the external array to prevent unfound modules messages:
```js
[
  'react',
  'react-dom',
  `${directoryWhereTheFilesWillBeDeployed}react.development.mjs`,
  `${directoryWhereTheFilesWillBeDeployed}react-dom.development.mjs`,
  `${directoryWhereTheFilesWillBeDeployed}react.production.min.mjs`,
  `${directoryWhereTheFilesWillBeDeployed}react-dom.production.min.mjs`
]

```

You can also reference the files directly from the node_modules directory in Rollup if you're using the ```'rollup-plugin-node-resolve'``` plugin.


# Notice
When [react and react-dom have a module entry in their package.json](https://github.com/facebook/react/issues/10021) and they will have [formalized the top-level ES exports](https://github.com/facebook/react/issues/11503) this module will be deprecated. Untill then this is the next best thing if you want to use react and react-dom as ECMAScript modules in a browser.
