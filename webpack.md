# Webpack Knowledge

## Bundling an NPM package w/ webpack for publish & inclusion in another project that is also bundled with webpack

webpack version: 3.7

Terminology:
  - _dependency_ = the bundle that will be included in another webpack build
  - _project_ = the webpack-bundled project that will list the _dependency_ in its package.json

We want to keep the dependency build size as small as possible, for example not bundling react
because the project will already bundle all react / other shit.

In the dependency, list everything you don't want to bundle in package.json `peerDependencies` field.
Then in the webpack config for the dependency, i did something like this:

    const { peerDependencies } = require('../package.json')
    ...
    module.exports = {
      ...
      externals: Object.keys(peerDependencies)
    }

you should list externals as peer deps so that when the user installs you
dependency in their project, if they don't have the dependency's peer deps
they will get yelled at.


Another problem is telling webpack how to expose your dependency to the project.
The documentation has a lot of misleading information about the `library` / `libraryTarget` / `libraryExport` fields.
These fields are useful if your project code wants to reference the dependency in a non-imported way.
for example, you might want to bundle jQuery with webpack and expose it as the global variable `$`. You could use the
`library` / `libraryTarget` fields to accomplish this.

For me, I want the dependency to expose itself as a module that can be imported.
After a lot of tinkering i got it to work by **ONLY SETTING THE** `libraryTarget` **FIELD** to `'commonjs'`.

So in the depenency webpack config, i have this:

    module.exports = {
      ...
      output: {
        path: <outputpath>, // The main field of your dependency's package.json
        filename: <fname>,   // should be <outputpath>/<fname> so it can be resolved by NPM
        libraryTarget: 'commonjs' // this is required to expose the final bundle as a thing that can be required.
      },
      ...
    }

