# Brainstorming

Welcome to Muon! Muon is my dream for a modal _development environment_. That is, not just a modal text editor, but an entire environment designed around modal control. Current solutions for modal editing usually involve a terminal-based text editor. While this is great for editing text, a developer has a lot more to do than text editing - source control, running tasks, managing files, etc. Terminal-based modal text editors  only edit text, requiring the developer to either install a ton of plugins, or use other programs for these other tasks.

Muon is _not_ just a text editor - it is a modal framework and window manager for development "widgets". Hence the tagline "a modal _development environment_", not "a modal _text editor_". With Muon, you don't need to context switch nearly as often; your text editor, file manager, source control, and terminal emulator are all in one place, and can all be controlled from the keyboard. And of course, plugins can contribute their own new "widgets" to accomplish a variety of tasks.

## Modal control

Muon will use a modal control scheme based on [Kakoune](https://kakoune.org/). While this will be an enormous turn-off to Vim and Emacs users, Kakoune's control scheme is designed around consistency, and has numerous advantages over Vim and Emacs' control schemes. The Kakoune author wrote an [amazing piece](https://kakoune.org/why-kakoune/why-kakoune.html) on the philosophy behind its control scheme. Investing a few weeks into learning Kakoune's control scheme is definitely worth the effort.

## Widgets

Muon is based on the concept of "widgets". A widget is the receiver of your command inputs and is what is displayed on screen. Muon receives and processes your control commands, and if applicable, will pass the command to the active widget. The widget is expected to perform an action consistent with the purpose of that command - for example, pressing `j` will move down a line in the text editor, or down a row in the file manager. The exact implementation details are left up to each widget, but each widget is expected to be consistent with other widgets when it comes to basic commands (to be defined in detail later).

Not all widgets will support all commands. For example, a command to change a selection's surrounding braces to quotes only makes sense in the context of a text editor. To this end, Muon will define a list of "universal commands" - such as basic movement, registers, go-to, and view commands - that all widgets must implement, or have a very good excuse not to implement. More specialized commands, such as the aforementioned surround commands, may be implemented on a per-widget basis. It will be left to the plugin developers to remain consistent with one another on these special commands.

## Plugins

One of the central goals for Muon will be to be as extensible as possible. The Muon binary itself will contain the framework for the modal control scheme, as well as the window manager. Widgets will all be implemented as core plugins, and may be added and removed as the developer wishes.

Plugins may contribute new widgets, or provide specific functionality to an existing widget. For example, a plugin to edit surrounding braces only makes sense in the context of a text editor, and so will have a dependency on a text editor widget.

Plugins will be implemented using Muon's Lua API. Details of this API will need to be ironed out in the future. For plugins that are contributing a widget, the plugin defines an interaction model in Lua (or JSON?) that links Muon's commands to an external program, and allows the external program to return output to Muon. This will _definitely_ require more research as to the exact implementation details.

Muon will ship with a built-in plugin manager that will pull plugins off of GitHub. There will be no official plugin store or moderation of plugins - the user is expected to be responsible enough not to download malware. Of course, since the plugin manager will be itself a plugin, corporations may write their own privately distributed plugins with a custom plugin manager if they so wish.

## Everything asynchronous

Modern machines are stuffed full of computing cores, so Muon will be designed to take advantage of this! This is one of the primary reasons for writing Muon in Rust. A user should expect to be able to clone a new git repository, compile their project on release mode, and navigate their file tree without a hiccup. Time is money, and time spent waiting for a git fetch to complete is time wasted!

## The plan

I am severely inexperienced to pull this off. I will not pull any punches on that fact. In fact, this might not even be a feasible project! But I intend to work on this as hard as I can to try and make it a reality. There are many implementation details to work through, and many technologies to research and learn. There are trials to overcome and mistakes to be made. There is code to refactor ten times before it actually works well. All of this, and more, is Muon's future. Hopefully vaporware is not on that list.