# Matter In Motion. Units

Units is a simple way to have definable namespaces for application modules and two-step initialization.

More info at [units repository](https://github.com/velocityzen/units)

## Unit

A unit is an interface

```js
const Controller = function () {
  this.db = undefined;
};

Controller.prototype.__init = function (units) {
  // all units are instantiated at this point
  // getting components we're depended on
  this.db = units.require('db');
};
```

### Interface methods and properties

#### __init
Function, unit initialisation

#### __initRequired
Boolean, means that this unit returned inited when required

#### __instance
Function, If this method exists units returns the result of this method when required instead of the unit class itself.

## Usage

Units lets you build very simple, predictable and flexible architecture. Look at the actual resources example:

```
/resources
  /message
    api.js
    controller.js
    index.js
  /user
    api.js
    controller.js
    index.js
index.js
```

With this structure, you can easily have references to any of your classes.

To get reference to message controller from user controller:

```js
User.prototype.__init = function(units) {
  this.message = units.require('message.controller');
  //or
  this.message = units.require('resources.message.controller');
}
```

To get all the resource from somewhere in the app:

```js
Service.prototype.__init = function(units) {
  const resources = units.require('resources');

  //now you can iterate over resources
  for (name of resources) {
    const resource = resources.require(name);
    //do something with the resource
  }

  //or this way
  resources.forEach((unit, name) => {
    console.log(name);
  });

  //or this way
  resources.match('^(.*)\.api$', (unit, name) => this.addResource(name, unit));
}
```

You can expose any data to the units namespaces as well. Learn more about it at [units repository](https://github.com/velocityzen/units)
