# HardSourceWebpackPlugin

`HardSourceWebpackPlugin` is a plugin for webpack to cache an intermediate step modules reach during a webpack. Run webpack twice with this plugin, the first build will take the normal amount of time. The second time, the build time will be greatly reduced.

Please note this plugin cannot track all possible changes that may invalidate a member of the cache. If you make a change outside your project's source code like updating a depending loader or webpack plugin or other step to your build process, you may need to delete the existing hard source cache. HardSourceWebpackPlugin can detect when the original content for a module has changed thanks to webpack's normal facilities used in watch-mode rebuilds. HardSourceWebpackPlugin can not guarantee it will detect when loaders and plugins that modify your code have changed. You may need to delete your current HardSource cache when you modify your build process by adding, removing, updating loaders and plugins or changing any part of your build configuration.

HardSourceWebpackPlugin makes the assumption that any cacheable module is deterministic between builds, or that it will not change. Loaders already help determine this by setting a cacheable flag when they operate on a module. After the loaders execute webpack's mangling of module loading statements is the only common thing left to make deterministic. webpack does this through ID records. Writing these records to the file system must be turned on. In use in conjuction with webpack-dev-server, this plugin will ensure the records are written to the file system, as the dev-server writes them to memory.

A webpack config with HardSourceWebpackPlugin may look like

```js
var HardSourceWebpackPlugin = require('hard-source-webpack-plugin');

module.exports = {
  context: // ...
  entry: // ...
  output: // ...
  // Can be constructed with __dirname and path.join.
  recordsPath: '/absolute/path/to/records.json',
  plugins: {
    new HardSourceWebpackPlugin({
      // Either an absolute path or relative to output.path.
      cacheDirectory: 'path/to/cache',
    }),
  },
};
```

## Please contribute!

This plugin is pretty young and we don't know what it has trouble with yet. Trying the plugin in a project and its creating errors or doesn't notice changes to original source, let us know your loaders and plugins or other build process details in an issue. Have a fix for something you've encountered, send us a fix.