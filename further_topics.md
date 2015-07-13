# Further tips and topics

This page goes deeper on some of the topics omitted from the [main style guide](https://github.com/vivainio/typescript-ng1-style).
It should be considered optional reading if you have some concerns left after reading the main guide.

## Angular modules

Some developers prefer exposing the angular components in SomeFeatureModule.ts.

This has some advantages, especially when the module name needs to be changed - 
you would only change the module name in one place.

In contrast, this style guide recommends the approach where the component is exposed
to angular right after it has been declared (we have tried both and agreed on this approach). 
Which one is better is a matter of taste.

Some developers only have one angular module (e.g. 'fbApp') instead of finer grained module distribution  
and are perfectly happy with that. Consider that approach as well.

## DI in directives

If you need to inject some features in directive, as opposed to just the controller
associated with the directive (e.g. to use in link() function), you need to add a constructor()
with the features used in argument list and adjust the instantiation like so:

```typescript
angular.module("fbSomeFeature").directive("fbFancyButton", ['$http', 
	($http) => new FancyButtonDirective($http)]);
```

This is a bit messy, and should be rarely needed; therefore, it's not shown in main page.

(TODO: add full example)

## Automatic DI

There are tools like [ng-annotate](https://github.com/olov/ng-annotate) that automatically insert $inject annotations to
your source code. We have noted that they can cause hard-to-debug bugs (as they work
by text scanning), therefore we don't advice using them and opt for exlicit 'static $inject'
style annotations everywhere.

## Angular tips

- Since you are using controllerAs throughout, a handy way to get a reference to 
  parent controller (somewhere up the DOM tree) with
  ```typescript
  this.containerController : FancyButtonContainerController = this.$scope.$parent.buttonContainer
  ```
  (where buttonContainer is the 'controllerAs' name of the FancyButtonContainerController).
  This works even when you have isolated scope.
- ng-controller is out. Don't use it.
- ng-init is a good way to shorten your template code, and can be used in any node. Don't shy away from it.
- Use {{::oneTimeBindings}} everywhere you can.
  
## TypeScript tips

- Use 'const' instead of 'let' and 'var' freely (you only need let/var if binding changes mid-function).
  const *does not mean* 'constant literal' despite the name. 
  
- Use 'any' type freely. You can always add typing on later iterations. The more you add types, the more mileage
  you get from compiler and IDE though.

- Don't go overboard with ES6-experimental features like @decorators.

- Don't use the old TS module syntax (pre-1.5, 'module { }'), real ES6 style modules are the preferrend syntax.
