+++
draft = false
title = "Project Structure"

+++

One role, one file, rule. Separate all controllers, services/factories, directives into individual files. Don’t add all controllers in one file, you will end up with a huge file that is very difficult to navigate, keeps things encapsulated and bitesize.

Standard Structure :
Description: This is a very typical app structure that you get to see. On the surface, it seems to make a lot of sense and is very similar to a lot of MVC frameworks. We have a separation of concerns, controllers have their own folder, views have their own folder, external libraries have their own folder, etc.


|-- app.js
|-- controllers/
|   |-- MainCtrl.js
|   |-- AnotherCtrl.js
|-- filters/
|   |-- MainFilter.js
|   |-- AnotherFilter.js
|-- services/
|   |-- MainService.js
|   |-- AnotherService.js
|-- directives/
|   |-- MainDirective.js
|   |-- AnotherDirective.js

Drawback: The disadvantage of this structure is when you have more number of controllers, filters, services, views and directives, you are going to have to do a lot of scrolling in your directory tree to find the required files, thus not suitable for larger projects.

Recommended Structure

Description: An ideal AngularJS app structure should be modularized into very specific functions. In the below structure we have separate folder for app specific functionality. This we can modularize the below structure and thus helps in handling large projects.

|-- app.js
|-- dashboard/
|   |-- dashboard.html
|   |-- dashboard.scss
|   |-- dashboard.directive.js
|   |-- dashboard.controller.js
|-- login/
|   |-- login.html
|   |-- login.scss
|   |-- login.directive.js
|   |-- login.controller.js
|-- inbox/
|   |-- inbox.html
|   |-- inbox.scss
|   |-- inbox.directive.js
|   |-- inbox.controller.js


Advantages:
•	CODE MAINTAINABILITY- The above structure will logically compartmentalize apps and will easily be able to locate and edit code.
•	SCALABLE- Code will be much easier to scale. Adding new directives and pages will not add bloat to existing folders.
•	DEBUGGING- Debugging your code will be much easier with this modularized approach to app development. It will be easier to find the offending pieces of code and fix them.
•	TESTING- Writing test scripts and testing modernized apps is a whole lot easier then non-modularized ones.

