# Node.js vm module
> The vm module provides APIs for compiling and running code within V8 Virtual Machine contexts. The vm module is not a security mechanism. Do not use it to run untrusted code. The term "sandbox" is used throughout these docs simply to refer to a separate context, and does not confer any security guarantees.

## API
* vm.createContext
> If given a sandbox object, the vm.createContext() method will prepare that sandbox so that it can be used in calls to vm.runInContext() or script.runInContext(). Inside such scripts, the sandbox object will be the global object, retaining all of its existing properties but also having the built-in objects and functions any standard global object has. Outside of scripts run by the vm module, global variables will remain unchanged.
* vm.runInContext

* vm.runInNewContext
> The vm.runInNewContext() first contextifies the given sandbox object (or creates a new sandbox if passed as undefined), compiles the code, runs it within the context of the created context, then returns the result. Running code does not have access to the local scope.
``` JavaScript
const util = require('util');
const vm = require('vm');

const sandbox = {
  animal: 'cat',
  count: 2
};

vm.runInNewContext('count += 1; name = "kitty"', sandbox);
console.log(util.inspect(sandbox));

// { animal: 'cat', count: 3, name: 'kitty' }
```
* vm.runInThisContext
> vm.runInThisContext() compiles code, runs it within the context of the current global and returns the result. Running code does not have access to local scope, but does have access to the current global object.
```JavaScript
const vm = require('vm');
let localVar = 'initial value';

const vmResult = vm.runInThisContext('localVar = "vm";');
console.log('vmResult:', vmResult);
console.log('localVar:', localVar);
// vmResult: 'vm', localVar: 'initial value'
```

* script.runInContext
```JavaScript
const vm = require('vm');

const sandbox = {x: 1};
const ctx1 = vm.createContext(sandbox); // ctx1 === sandbox
const code = 'y = 20; ({"aFunc": function() { ++x;}})';

// using vm.runInContext
aObj = vm.runInContext(code, ctx1);
console.log(aObj); // ==> { aFunc: [Function: aFunc] }
console.log(aObj.aFunc.toString()); // ==> function() { ++x;}
console.log(ctx1); // ==> {x: 1, y: 20}

aObj.aFunc();
console.log(ctx1); // ==> {x: 2, y: 20}

// 
```
