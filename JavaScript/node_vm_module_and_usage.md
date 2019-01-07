# Node.js vm module
> The vm module provides APIs for compiling and running code within V8 Virtual Machine contexts. The vm module is not a security mechanism. Do not use it to run untrusted code. The term "sandbox" is used throughout these docs simply to refer to a separate context, and does not confer any security guarantees.

## API
* vm.createContext
> If given a sandbox object, the vm.createContext() method will prepare that sandbox so that it can be used in calls to vm.runInContext() or script.runInContext(). Inside such scripts, the sandbox object will be the global object, retaining all of its existing properties but also having the built-in objects and functions any standard global object has. Outside of scripts run by the vm module, global variables will remain unchanged.
* vm.runInContext
> The vm.runInContext() method compiles code, runs it within the context of the contextifiedSandbox, then returns the result. Running code does not have access to the local scope. The contextifiedSandbox object must have been previously contextified using the vm.createContext() method.
```JavaScript
const vm = require('vm');

const sandbox = {x: 1};
const ctx1 = vm.createContext(sandbox); // ctx1 === sandbox
const code = 'y = 20; ({"aFunc": function() { ++x;}})';

aObj = vm.runInContext(code, ctx1);
console.log(aObj); // ==> { aFunc: [Function: aFunc] }
console.log(aObj.aFunc.toString()); // ==> function() { ++x;}
console.log(ctx1); // ==> {x: 1, y: 20}

aObj.aFunc();
console.log(ctx1); // ==> {x: 2, y: 20}
```
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

* Class:vm.Script
* script.runInContext
```JavaScript
const vm = require('vm');

const sandbox = {x: 1};
const ctx1 = vm.createContext(sandbox); // ctx1 === sandbox
const code = 'y = 20; ({"aFunc": function() { ++x;}})';

// using vm.runInContext
const script = new vm.Script(code);
const aObj = script.runInContext(ctx1);
console.log(aObj); // ==> { aFunc: [Function: aFunc] }
console.log(aObj.aFunc.toString()); // ==> function() { ++x;}
console.log(ctx1); // ==> {x: 1, y: 20}

aObj.aFunc();
console.log(ctx1); // ==> {x: 2, y: 20}
```

see more in [VM (Executing JavaScript)](https://nodejs.org/api/vm.html)

## source internals
* [vm.js](https://github.com/nodejs/node/blob/master/lib/vm.js)
```JavaScript
const {
  ContextifyScript,
  makeContext,
  isContext: _isContext,
  compileFunction: _compileFunction
} = internalBinding('contextify');
```
* node_contextify.[h](https://github.com/nodejs/node/blob/master/src/node_contextify.h)/[cp](https://github.com/nodejs/node/blob/master/src/node_contextify.cc)
```c++
using v8::Context;
using v8::EscapableHandleScope;
using v8::External;
using v8::Function;
using v8::FunctionCallbackInfo;
using v8::FunctionTemplate;
using v8::HandleScope;
using v8::Local;
using v8::Script;
using v8::ScriptCompiler;
using v8::ScriptOrigin;
using v8::UnboundScript;
```

* v8
  - [context.h/ccc](https://chromium.googlesource.com/v8/v8.git/+/master/src/contexts.h)
  - [handles.h/cc](https://chromium.googlesource.com/v8/v8.git/+/master/src/handles.h)
  - [objects.h/cc](https://chromium.googlesource.com/v8/v8.git/+/master/src/objects.h)
```c++
class JSFunction: public JSObject {
 public:
  // [prototype_or_initial_map]:
  DECL_ACCESSORS(prototype_or_initial_map, Object)
  // [shared]: The information about the function that
  // can be shared by instances.
  DECL_ACCESSORS(shared, SharedFunctionInfo)
  // [context]: The context for this function.
  inline Context* context();
  inline void set_context(Object* context);
  inline JSObject* global_proxy();
  inline Context* native_context();
  static Handle<Context> GetFunctionRealm(Handle<JSFunction> function);
  // [code]: The generated code object for this function.  Executed
  // when the function is invoked, e.g. foo() or new foo(). See
  // [[Call]] and [[Construct]] description in ECMA-262, section
  // 8.6.2, page 27.
  inline Code* code();
  ...
}
```

## Usage in Jest
```JavaScript
//simpole.test.js
it('self is a global object', () => {
  console.log(global === self, self); // ==> true, window object
});
```

Jest load and run script with custom window object as global object
``` JavaScript
// jest-environment-jsdom/build/index.js
class JSDOMEnvironment;
// jsdom/lib/api.js
class JSDOM;
// jsdom/lib/jsdom/browser/Window.js
class Window;
// jsdom/lib/jsdom/browser/documentfeatures.js
exports.contextifyWindow = window => {
  if (vm.isContext(window)) {
    return;
  }

  vm.createContext(window);
  const documentImpl = idlUtils.implForWrapper(window._document);
  documentImpl._defaultView = window._globalProxy = vm.runInContext("this", window);
};
// jest-runtime/build/index.js
_execModule(localModule, options, moduleRegistry, from) {
...
const transformedFile = this._scriptTransformer.transform(
      filename,
      {
        collectCoverage: this._coverageOptions.collectCoverage,
        collectCoverageFrom: this._coverageOptions.collectCoverageFrom,
        collectCoverageOnlyFrom: this._coverageOptions.collectCoverageOnlyFrom,
        isInternalModule: isInternalModule
      },
      this._cacheFS[filename]
    );
...
  const runScript = this._environment.runScript(transformedFile.script);
...
}

// jest-runtime/build/script_transformer.js
transform(filename, options, fileSource)
_transformAndBuildScript(filename, options, instrument, fileSource) {
...
return {
        mapCoverage: mapCoverage,
        script: new (_vm || _load_vm()).default.Script(wrappedCode, {
          displayErrors: true,
          filename: isCoreModule ? 'jest-nodejs-core-' + filename : filename
        }),
        sourceMapPath: sourceMapPath
      };
...
}

// jest-environment-jsdom/build/index.js
  runScript(script) {
    if (this.dom) {
      return this.dom.runVMScript(script);
    }
    return null;
  }
  runVMScript(script) {
    if (!vm.isContext(this[window])) {
      throw new TypeError("This jsdom was not configured to allow script running. " +
        "Use the runScripts option during creation.");
    }

    return script.runInContext(this[window]);
  }
```



