+++
draft = false
title = "Exception Handling"

+++

Angular’s built-in $exceptionHandler handles Angular expression exceptions that are not caught anywhere else within the application. By default, it will simply delegate uncaught exception messages to $log.error. You can also send custom exception messages to$exceptionHandler.  You should do so if you cannot handle the exception directly where it occurred, and you want it to be handled using the default exception handler. Also, rather than using$log.error to log exceptions within your application’s code, it is advisable to pass the messages to $exceptionHandler instead, since it provides a more consistent location to handle exceptions. The following is an example of how to send a custom error message to the default exception handler.

<b>Example and Reference</b>

[Exception Handling in Angular](http://www.tutorialsteacher.com/codeeditor?cid=ng-161)

[Angular Exception logging made simple](https://www.loggly.com/blog/angularjs-exception-logging-made-simple/)
