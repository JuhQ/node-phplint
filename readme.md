## node-phplint

[![npm](http://img.shields.io/npm/v/phplint.svg?style=flat)](https://www.npmjs.com/package/phplint)
[![Build Status](https://travis-ci.org/wayneashleyberry/node-phplint.svg?branch=master)](https://travis-ci.org/wayneashleyberry/node-phplint)
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat)](https://github.com/feross/standard)
[![Dependency Status](https://david-dm.org/wayneashleyberry/node-phplint/status.svg?style=flat)](https://david-dm.org/wayneashleyberry/node-phplint#info=dependencies)
[![devDependency Status](https://david-dm.org/wayneashleyberry/node-phplint/dev-status.svg?style=flat)](https://david-dm.org/wayneashleyberry/node-phplint#info=devDependencies)

A simple node wrapper to run `php -l` in parallel.

Running `find . -type f -name "*.php" | xargs -n 1 php -l` in a brand new
Laravel project (with dependencies) took _1m 32s_ to find its first error. Running `phplint
'**/*.php'` in the same project took _37s_ to find the same error.

### CLI

```sh
$ npm install --global phplint
$ phplint '**/*.php'
```

node-phplint uses [globby](https://github.com/sindresorhus/globby) for globbing filenames, so the following would work as well:

```sh
$ phplint '**/*.php' '!vendor/**'
```

### Node

```js
var phplint = require('phplint').lint

lint(['src/**/*.php'], function (err, stdout, stderr) {
  if (err) throw new Error(err)

  process.stdout.write(stdout)
  process.stderr.write(stderr)

  // success!
})
```

### NPM

```json
{
  "scripts": {
    "pretest": "phplint 'src/**/*.php'"
  },
  "devDependencies": {
    "phplint": "~1.0.0"
  }
}
```

```sh
$ npm test
```

### Grunt

#### Configure cache directories

This module uses the `cache-swap` module to cache already linted files.
You can configure the cache directories via the `cacheDirName` and the `tmpDir` options.
Both options will be passed to the CacheSwap instance when created.

```js
module.exports = function (grunt) {
  require('phplint').gruntPlugin(grunt)

  grunt.initConfig({
    phplint: {
      options: {
        limit: 10,
        phpCmd: '/home/scripts/php', // Defaults to php
        stdout: true,
        stderr: true,
        tmpDir: '/custom/root/folder', // Defaults to os.tmpDir()
        cacheDirName: 'custom/subfolder' // Defaults to php-lint
      },
      files: 'src/**/*.php'
    }
  })

  grunt.registerTask('test', ['phplint'])

}
```

```sh
$ grunt test
```

### Gulp

```js
var gulp = require('gulp')
var phplint = require('phplint').lint

gulp.task('phplint', function (cb) {
  phplint(['src/**/*.php'], {limit: 10}, function (err, stdout, stderr) {
    if (err) {
      cb(err)
      process.exit(1)
    }
    cb()
  })
})

gulp.task('test', ['phplint'])
```

```sh
$ gulp test
```

#### License

[MIT](http://opensource.org/licenses/MIT) © [Wayne Ashley Berry](https://twitter.com/waynethebrain)
