+++
draft = false
title = "Commenting"

+++

If planning to produce documentation, use jsDoc syntax to document function names, description, params and returns. Use @namespace and @memberOf to match your app structure.  

  1.	You can generate (and regenerate) documentation from your code, instead of writing it from scratch
  2.	Provides consistency using a common industry tool.

```javascript

/**
 * Logger Factory
 * @namespace Factories
 */
(function() {
  angular
      .module('app')
      .factory('logger', logger);

  /**
   * @namespace Logger
   * @desc Application wide logger
   * @memberOf Factories
   */
  function logger($log) {
      var service = {
         logError: logError
      };
      return service;

      ////////////

      /**
       * @name logError
       * @desc Logs errors
       * @param {String} msg Message to log
       * @returns {String}
       * @memberOf Factories.Logger
       */
      function logError(msg) {
          var loggedMsg = 'Error: ' + msg;
          $log.error(loggedMsg);
          return loggedMsg;
      };
  }
})();

```
