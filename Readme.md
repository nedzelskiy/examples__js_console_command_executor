##### Review:

This is a script that allows you wrote command and execute it while this script will be listening stdin!
You can add any handlers for any keys and any commands for run as written below. Enjoy!

##### Usage:

````
const availableCommands = {
    'k': {
        run: function() {
            console.log('kill process!');
        }
    },
    'exit': {
        run: function() {
            console.log('exit process!');
        }
    }
};
````
adding help for each commands
````
availableCommands.k.usage =     'k [PID, [SIGNAL]]               kill process by its PID';
availableCommands.exit.usage =  'exit                            stop watching for commands and exit script';
````
````
const execConsole = require('./index')(availableCommands);
````
adding new key "Cntr + q"
````
execConsole.keys['\u0011'] = (controls, commands) => {
    console.log('This is handler for Cntrl + q! Another exit action!');
};
````
adding action move cursor left for two position for key "{"
````
execConsole.keys['\u007B'] = (controls, commands) => {
    if (controls.cursorPosition > 1) {
        controls.cursorPosition = controls.cursorPosition - 2;
        process.stdout.cursorTo(controls.cursorPosition);
    }
};
````
rebind standard handler "combineActionsForEnterHandle"
````
execConsole.actions.combineActionsForEnterHandle = function() {
    console.log("\r\n\r\nThis is a logger! From rebind standard handler \"combineActionsForEnterHandle\"!");
    this.enterActionHandle(execConsole.controls);
    this.executeCommand(execConsole.controls, execConsole.commands);
};
````

run script
````
execConsole();
````
and even add a new command after script run!
````
execConsole.commands['n'] = {
    run: function() {
        console.log('I\'m a new command added after running script!');
    },
    usage: 'n                               just a new added command!'
};

````

Try use it! For more example run:
````
node ./example_runing.js
````