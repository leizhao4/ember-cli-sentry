ember-cli-sentry
================

An ember-cli addon adding [Sentry](https://www.getsentry.com) support.

Docs are available [here](http://damiencaselli.github.io/ember-cli-sentry/).

## What it does (and does not)

This addon does:

* Enable safe use of Raven.js whether you are in development mode or not.
* Inject a logging service to routes, components, controllers and models to access Raven object.
* Provide a default logger generator that should work for the vast majority of people.
* Provide `raven-js` in browser.
* Provide rather complete customization.

This addon does **not**:

* Generate the logging service for you.
* Provide a Sentry key for testing.

## Important notice if you migrate from 2.x.x to 3.x

Please have a look at [this wiki entry](https://github.com/damiencaselli/ember-cli-sentry/wiki/Migration-from-2.x.x-to-3.x) if you upgrade major version of ember-cli-sentry.

## Install

From any ember-cli application, run `ember install ember-cli-sentry`.

_Note: Since **v3.0.0**, `raven-js` package is automatically included by this addon._

The addon will assume there is an available service that proxies Raven, which is not the case unless you already did the install.

The easiest way of doing it is to create a service only extending `ember-cli-sentry/services/raven`:

```js
// your-app/services/custom-logger.js
export { default } from 'ember-cli-sentry/services/raven';
```

You can also use a generator `ember g logger <logger-name>`, which will generate a service called `<logger-name>` extending `ember-cli-sentry/services/raven` and exposing its methods and properties.  
If you create the logger service that way, note that you must use the same name for the `serviceName` property.


Now that you have a dedicated service for Raven.js, let's configure it.

## Configuration

### TLDR

You already have a service named `raven` that extends `ember-cli-sentry/services/raven`.

```js
// config/environment.js

module.exports = function(environment) {
  var ENV = {

    /* config */

    sentry: {
      dsn: 'https://<dummykey>@app.getsentry.com/<dummyproject>'
    }
  }
}
```

### Complete config

```js
// config/environment.js

module.exports = function(environment) {
  var ENV = {

    /* config */

    sentry: {
      /**
       * The only mandatory parameter.
       *
       * @type {String}
       */
      dsn: 'https://<dummykey>@app.getsentry.com/<dummyproject>',

      /**
       * Sets Raven.debug property when running `Raven.config`.
       *
       * @type {Boolean}
       * @default true
       */
      debug: true,

      /**
       * If set to true, it will prevent Raven.js from being initialized.
       * Errors and logs will be logged to the console (default) instead of
       * being reported by Raven.
       *
       * @type {Boolean}
       * @default undefined
       */
      development: false,

      /**
       * Injects the logging service as this property.
       *
       * @type {String}
       * @default 'raven'
       */
      exposedPropertyName: 'raven',

      /**
       * If set to true, addon will try to have Ember.onerror
       * and Ember.RSVP.on('error') captured by Raven.
       *
       * @type {Boolean}
       * @default true
       */
      globalErrorCatching: true,

      /**
       * Service used to interface with Raven.
       *
       * @type {String}
       * @default 'raven'
       */
      serviceName: 'raven',

      /**
       * Raven.js option.
       *
       * @type {Array}
       * @default []
       */
      includePaths: [],

      /**
       * Raven.js option.
       *
       * @type {Array}
       * @default []
       */
      whitelistUrls: [],

      /**
       * Options to pass directly to Raven.js. Note: whitelistUrls and
       * includePaths in this will take precedence
       * over the above.
       *
       * @default {}
       */
      ravenOptions: {},
    }
  }
}
```

## Content Security Policy

To allow Ravenjs to work properly, you need to add a couple of thing to the content security policy rules:

```
'script-src': "'self' 'unsafe-inline' 'unsafe-eval' cdn.ravenjs.com",
'img-src': "data: app.getsentry.com",
'connect-src': "'self' app.getsentry.com"
```

## Meaningless stack traces?

See [this issue](https://github.com/damiencaselli/ember-cli-sentry/issues/28).

## Example

The dummy application in tests is a working example with a couple of logging here and there, and a default logger.

## Dependencies

[Raven.js](https://github.com/getsentry/raven-js)

## Licence

[MIT](https://raw.githubusercontent.com/damiencaselli/ember-cli-sentry/master/LICENSE.md)
