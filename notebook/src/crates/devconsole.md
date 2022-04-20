# Dev Console

Crate offering the ability to define arbitrary "console commands" that can
be executed at runtime.

## Commands Implementations

Each command can either be a regular bevy system or an exclusive system.

It receives an "argv" array of arguments as input, similar to typical CLI.

It can do whatever it wants.

## Commands execution

Custom stage (or exclusive system in stageless) to execute the commands.

User adds it to their schedule.

Commands are to be triggered via Bevy events. Any system can send an event
with a command+arguments, and when the stage next runs, it will drain those
events and execute every command in order.

Optional: if the command impl is a parallel system, we could be smart and
use parallel execution based on data access conflicts, just like bevy.

## Commands Registration

There could be a "dev commands registry" resource, which is basically a
hash map of all the known commands. It can be modified at any time, to add
or remove commands.

The stage, when executing commands, looks for the command from each incoming
event in this registry, to map it to its implementation.

Can also register a default/fallback handler, for any command not found in
the hash map.

### Optional: pluggable arg-parser impls

The events could contain the whole commandline as a single string, and the
exact function for breaking it up into "command name" + "arguments" could
be made replaceable.

This could let users to replace the default with their own, for custom
syntax handling.

The default behavior should be simply to trim, split on space, remove empty
substrings, and simple quoting behavior.

## Optional: UI

We could provide a sample UI for offering text input to type commands, and
sending the event to execute the command.

## Optional: Scripting

No reason why we can't implement the "shell script" concept, and provide
an abstraction for simply reading a list of commands and executing them all.

### Optional: Glorify!

It could be enhanced with syntax for variables and stuff ... turning it into
a "real scripting language".

This could be built on the "pluggable arg-parser impls" idea. If the arg
parsers are stateful, they could do string interpolation from variables.
