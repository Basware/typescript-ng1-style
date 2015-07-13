# Further tips and topics

This page goes deeper on some of the topics omitted from the main style guide.

## Angular modules

Some developers prefer exposing the angular components in SomeFeatureModule.ts.

This has some advantages, especially when the module name needs to be changed - 
you would only change the module name in one place.

On contrast, this style guide recommends the approach where the component is exposed
to angular right after it has been declared (we have tried both and agreed on this approach). 
Which one is better is a matter of taste.

## DI in directives

If you need to inject some features in directive, as opposed to just the controller
associated with the directive (e.g. to use in link() function), you need to add a constructor()
with the features used in argument list and adjust the instantiation like so:

```typescript
angular.module("fbSomeFeature").directive("fbFancyButton", ['$http', 
	($http) => new FancyButtonDirective($http)]);
```

This is a bit messy, and should be rarely needed; therefore, it's not shown in main example.

## Angular tricks

- Since you are using controllerAs throughout, a handy way to get a reference to 
  parent controller (somewhere up the dom tree) with
  ```typescript
  this.containerController : FancyButtonContainerController = this.$scope.$parent.buttonContainer
  ```
  (where buttonContainer is the 'controllerAs' name of the FancyButtonContainerController).
  This works even when you have isolated scope.
  
## TypeScript tips

- Use 'const' instead of 'let' and 'var' freely (you only need let/var if binding changes mid-function).
  const *does not mean* 'constant literal' despite the name.
  
- Use 'any' type freely. You can always add typing on later iterations.

- Don't go overboard with ES6-experimental features like @decorators.

- Don't use the old TS module syntax (pre-1.5, 'module { }'), real ES6 style modules are the preferrend syntax.
