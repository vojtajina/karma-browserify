karma-browserify
================

[Browserify](http://browserify.org) for [Karma](http://karma-runner.github.io)

[![NPM version](https://badge.fury.io/js/karma-browserify.png)](http://badge.fury.io/js/karma-browserify)

Browserify is an awesome tool for managing front-end module dependencies with
`require`, just like you would in Node. Karma is a killer JS test runner that's 
super fast and easy. But put them together and you're entering a world of pain.
This plugin aims to make them play nice.

Under most circumstances, Browserify attempts to give you a single monolithic
bundle of all of your JS files and their dependencies. That's perfect for fast
delivery with minimal requests in a browser environment, but sucks hard if 
you're just trying to run your tests (over and over). This plugin hijacks the
Browserify pipeline to produce a single bundle of *all* dependencies found in
your tests (and their dependencies) which is transferred to the browser under
test only once (unless it changes). A separate, minimal bundle is generated for
each test which, aside from the test code, only contains references to 
external dependencies in the main bundle. That way dependencies are updated in 
the browser only when necessary (watching for changes is supported) and your 
tests remain lightning fast.


Requirements
------------

This module currently requires the `canary` version of Karma:

```sh
$ npm install 'karma@canary' --save-dev
```

Note that the Karma configuration file format has changed since `v0.8`. Use 
`karma init` to generate a fresh config.


Installation
------------

Install the plugin from npm:

```sh
$ npm install karma-browserify --save-dev
```

Or from Github:

```sh
$ npm install 'git+https://github.com/xdissent/karma-browserify.git' --save-dev
```

Add `browserify` to the `frameworks` and `preprocessor` keys in your 
Karma configuration:

```coffee
module.exports = (karma) ->
  karma.configure

    # frameworks to use
    frameworks: ['mocha', 'browserify']

    preprocessors:
      '**/*.coffee': 'coffee'
      'my/test/files/*': 'browserify'

    # ...
```


Options
-------

The plugin may be configured using the `browserify` key in your Karma config:

```coffee
module.exports = (karma) ->
  karma.configure

    browserify: 
      extension: ['.coffee']  # This is for future compatibility.
      ignore: [path.join __dirname, 'components/angular-unstable/angular.js']
      transform: ['coffeeify']
      watch: true   # Watches dependencies only (Karma watches the tests)

    # ...
```


Usage
-----

Just `require` modules from within tests as you normally would in Node:

```coffee
something = require '../some/module'

describe 'karma tests with browserify', ->

  it 'should gimme dat module', ->
    something.should.exist()
```

See the [example](https://github.com/xdissent/karma-browserify/tree/master/example)
for a simple working setup.
