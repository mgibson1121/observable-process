# Node.js Child Process Observer

[![Circle CI](https://circleci.com/gh/Originate/observable-process.svg?style=shield)](https://circleci.com/gh/Originate/observable-process)
[![Dependency Status](https://david-dm.org/originate/observable-process.svg)](https://david-dm.org/originate/observable-process)
[![devDependency Status](https://david-dm.org/originate/observable-process/dev-status.svg)](https://david-dm.org/originate/observable-process#info=devDependencies)


High-level support for running, observing, and interacting with child processes
in Node.js


```javascript
ObservableProcess = require('observable-process');

observer = new ObservableProcess('my-server --port 3000');
observer.wait('listening on port 3000', function() {
  // the child process says it is ready now
});
```

You can also provide the process to run as an argv array:

```javascript
observer = new ObservableProcess(['my-server', '--port', '3000']);
```

More details around waiting for output of running processes are
in the [spec](features/observable-process.feature)
and its [implementation](features/steps/steps.ls).


## Logging the output

* by default the output of the observed process is hidden
* provide the option `verbose: true` to log the output on stdio
* provide custom console streams via the option `console`

```javascript
observer = new ObservableProcess('my-server', { verbose: true });
// output goes to your terminal

observer = new ObservableProcess('my-server', { verbose: true, console: myConsole });
// output goes to your myConsole object
```

More details around logging options in the [spec](features/verbose.feature)
and its [implementation](features/steps/steps.ls).


## Killing the process

If the process is running, you can kill it via:

```javascript
observer.kill()
```

To let ObservableProcess notify you when a process ended,
subscribe to the `ended` event:

```javascript
observer = new ObservableProcess('my-server')
observer.on 'ended', (err, exitCode) => {
  // the process has ended here
  // you can also access the exit code via observer.exitCode
}
```


## related libraries

* [nexpect](https://github.com/nodejitsu/nexpect):
  Allows to define expectations on command output,
  and send it input,
  but doesn't allow to add more listeners to existing long-running processes,
  which makes declarative testing hard.
