# Hooks

Hooks are a way for one piece of code to interact/modify another piece of code at specific, pre-defined spots. They make up the foundation for how plugins and themes interact with WordPress Core, but they’re also used extensively by Core itself.

There are two types of hooks: [Actions](https://developer.wordpress.org/plugins/hooks/actions/) and [Filters](https://developer.wordpress.org/plugins/hooks/filters/). To use either, you need to write a custom function known as a `Callback`, and then register it with a WordPress hook for a specific action or filter.

[Actions](https://developer.wordpress.org/plugins/hooks/actions/) allow you to add data or change how WordPress operates. Actions will run at a specific point in the execution of WordPress Core, plugins, and themes. Callback functions for Actions can perform some kind of a task, like echoing output to the user or inserting something into the database. Callback functions for an Action do not return anything back to the calling Action hook.

[Filters](https://developer.wordpress.org/plugins/hooks/filters/) give you the ability to change data during the execution of WordPress Core, plugins, and themes. Callback functions for Filters will accept a variable, modify it, and return it. They are meant to work in an isolated manner, and should never have [side effects](https://en.wikipedia.org/wiki/Side_effect_(computer_science)) such as affecting global variables and output. Filters expect to have something returned back to them.

WordPress provides many hooks that you can use, but you can also [create your own](https://developer.wordpress.org/plugins/hooks/custom-hooks/) so that other developers can extend and modify your plugin or theme.

## Actions vs. Filters

The main difference between an action and a filter can be summed up like this:

- An action takes the info it receives, does something with it, and returns nothing. In other words: it _acts_ on something and then exits, returning nothing back to the calling hook.
- A filter takes the info it receives, modifies it somehow, and returns it. In other words: it _filters_ something and passes it back to the hook for further use.

Said another way:

- An action interrupts the code flow to do something, and then returns back to the normal flow without modifying anything;
- A filter is used to modify something in a specific way so that the modification is then used by code later on.

The _something_ referred to is the parameter list sent via the hook definition. More on this in later sections.

## More Resources

- [Filter Reference](https://codex.wordpress.org/Plugin_API/Filter_Reference)
- [Action Reference](https://codex.wordpress.org/Plugin_API/Action_Reference)