# Another typescript style guide for Angular 1.x applications

## Introduction

This is a style guide for fictional company "FooBar", i.e. "fb" prefix represents
it where relevant.

Goals:
 - Quick reference on how to implement Angular 1.x elements in TypeScript
 - As little text as possible. If you feel like reading more, see [further topics](https://github.com/vivainio/typescript-ng1-style/blob/master/further_topics.md) as well.

Non-goals:
 - Teaching TypeScript or AngularJS
 - Lint ruleset
 - Code review guidance

Tooling workflow we are using:
 - TypeScript 1.6+
 - Browserify, tsify and watchify (because tsc doesn't support bundling yet)
 - Visual Studio Code
 - All third party libs (including angular) dumb-concatenated to lib.js (not exposed to browserify),
   application built with browserify to main.js. "angular" is globally visible.
 - Strict DI used
 - Gulp (but it doesn't matter, as heavy lifting done by browserify)
 - TSD package manager is used to install and bundle TypeScript definition files for improved intellisense.

## Controllers

```typescript

import {MyService} from './MyService';

export class MyController {
	untypedAttribute;
	stringAttribute: string;

	static $inject = ['$http', 'MyService'];
	contructor(private $http: ng.IHttpService, private MyService: MyService) {
	}

	someMethod() {
		this.MyService.someMethodOnService();
	}
}

angular.module('fbSomeFeature').controller('MyController', MyController);

```

Notable items:
 - MyService 'import'-ed just for type information

## Services

```typescript

export class MyService {

	static $inject = ['$http'];
	constructor(private $http: ng.IHttpService) {
	}

	someMethod() {
		// this.$http.get(...)
	}
}

angular.module('fbSomeFeature').service('MyService', MyService);
```

Notable items:
 - Use service() instead of factory
 - class exported to allow importing the type (as in MyController).

## Directives

```typescript

class FancyButtonController {
    // ... see MyController above
}

angular.module('fbSomeFeature')
	.directive('fbFancyButton', function FancyButtonDirective(): ng.IDirective) {
	return {
	    restrict: 'EA',
	    templateUrl: 'Features/SomeFeature/FancyButton.html',
	    controller: FancyButtonController,
	    controllerAs: 'fancyButton',
	    scope: { text:  '@' },
	    bindToController: true,
	    link: (scope: ng.IScope, element: ng.IAugmentedJQuery,
	            attrs: ng.IAttributes, controller: FancyButtonController) => {
	        element.find('#main-title').text(controller.text);
	    }
	}
}


```

Notable items:

 - controllerAs and bindToController
 - Relevant file names: FancyButtonDirective.ts, FancyButton.html
 - DDO is in the same file as controller (as controller is tightly coupled to it)
 - There is no separate class for directive. TypeScript 1.6 has strict object literal checking,
   so with this "old style" function based syntax you get proper compiler warnings for unknown
   DDO attributes.
 - The 'anonymous' function has a name to make it stand out in debugger.

## Angular Modules

```typescript

// this is feature specific "root" module, SomeFeatureModule.ts

// create agular module here
angular.module('fbSomeFeature', ['dep1', 'dep2']);

// add things to the created angular module here
import './MyService'
import './FancyButton'

```

And in root app.ts:

```typescript

import './SomeFeature/SomeFeatureModule';

```

Notable items:

- Angular modules have limited utility compared to standard ES6
  modules (most useful for reusable third party libraries).
  Don't overdesign them in your app.
