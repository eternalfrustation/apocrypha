# Plugin System

There are multiple ways to implement a plugin system, the following are the ones i have worked on:

- Global Registry Based.
- Plugin struct based.
- Central registry patcher based.

## Global Registry Based

Here, we define multiple mutable global objects, which are used by a run function at the end to pull different values from. Honestly the worst version of a plugin system. 

Enforcing dependencies is a PITA, and you need to have a convention for running code anyway, just that the code necessarily has to mutate the global state.

You have to magically know what registries are present and where they are writted if you want to add things. This can be remedied if you instead just store a single registry with plugin structs, but global state means that tracing what is in the registry at any given time is hard to track.

## Plugin Struct based.

Here, we define a way to get all the capabilities and properties exposed by a plugin, this can be a subclass, interface instance or just a function that returns a struct. The plugin system then uses these structs to configure and run the application.

This is fine, but replacing and removing items involves hacking around things, like either allowing for mutating global state, or providing separate fields in the plugin struct to allow for hooks that remove or replace items.

## Central Registry patcher.

Here, each plugin defines a function that takes in the app state, and returns a new app state. This allows for easily adding, removing and replacing items, while also allowing for easily tracking the state of the app in any given plugin. So far, there aren't any cons i have found.
