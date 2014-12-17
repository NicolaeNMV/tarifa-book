# Anatomy of a tarifa project

This is what a tarifa project for ios and android using the default project template looks like - **bold for editable files** -

<pre>
|-- <b>tarifa.json</b>                     /* project definition */
|-- <b>private.json</b>                    /* private project definition */
|-- app                             /* contains the cordova app */
|   |-- config.xml
|   |-- hooks
|   |-- platforms
|   |-- plugins
|   `-- www                         /* link/copy from `project/www` */
|-- images                          /* icons + splashscreens */
|   |-- android
|   `-- ios
`-- project                         /* front-end project */
    |-- bin
    |   |-- <b>build.js</b>                /* front-end project's build interface to tarifa */
    |   |-- <b>check_android.js</b>        /* add specifics steps to `tarifa check` */
    |   |-- <b>check_ios.js</b>
    |-- node_modules
    |   `-- ...
    |-- <b>package.json</b>
    |-- src
    |   `-- <b>app.js</b>
    `-- www                         /* front-end project's output linked/copied to cordova's www */
        |-- <b>style.css</b>
        `-- <b>index.html</b>
</pre>

The root of a tarifa project is composed of 3 folders:

* `app` contains the cordova app.
* `images` contains all icons and splash screens.
* `project` contains the www project.

and 2 json files:

* `tarifa.json` defines the project and every single configuration.
* `private.json` consists of private data (typically tokens, IDs...) that you
don't want to expose.

### The raw cordova app

The `app` folder contains a regular cordova application. Any cordova app (version 4.x.x)
should work with tarifa, thus you can use an existing cordova app folder in a
tarifa project.

At this time, the tarifa build process performs setting replacements in various
cordova files without undoing them. In a future version of tarifa this won't be
the case anymore.

During the build process and before finishing the [prepare step](../usage/prepare.md), tarifa will copy or link (depending on the OS) the output of the `www` project folder to `app/www`.

Currently, tarifa just wraps cordova plugins without any extra features.

### The www project

The `project` folder is a regular front-end project with the build system of your choice. It must
satisfy the following interface:

* having a `package.json`.
* having a `bin/build.js` node module exposing `build` function starting the build process, `watch` and `close` function to start and stop live reload.
* generating output in a folder named `www` or if available, at the path given by the `project_output` attribute in `tarifa.json`.

More precisely, the `bin/build.js` module must have the following signature:

``` javascript
var Q = require('q');

module.exports.build = function (platform, settings, config) {
    // do async stuff then
    return Q.resolve();
}

module.exports.watch = function watch(f, settings, platform, config) {
    // init front-end watch
    // then call `f('changed file path')`
    // each time the live reload needs to be triggered
}

module.exports.close = function close() {
    // close all watch handler used in `watch()`
    // called by tarifa on ^C
}

```

where

* `platform` is the platform chosen by the user for the current build.
* `settings` is an object containing all the project settings (this object is
  simply the result of merging `tarifa.json` and `private.json`).
* `config` is the name of the configuration chosen by the user.
* `f` is a function which needs to be called each time a file needs to be reload. it takes the path of the changed file as argument.

In the default tarifa project template, *browserify* is used to embed the settings
which are specific to a configuration as a global module - named `settings` - that
you can require in your js code.

See the [default template www project build script](https://github.com/TarifaTools/tarifa/blob/master/template/project/bin/build.js) for a full example.

### `tarifa.json` and `private.json`

These two files wholly describe a tarifa project and contain everything that is
needed for building and managing it. This goes from the definition of the various
configurations to the keystore paths. `tarifa.json` defines most of the data and
`private.json` contains all the private stuff, such as the Apple ID or the hockeyapp
token, that you don't want to share publicly.

A more detailed description may be found in the [Configurations](../configurations/index.md)
chapter.
