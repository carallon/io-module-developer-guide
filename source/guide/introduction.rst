Introduction
############

Interfacing with other systems is at the heart of the |Vendor| feature set but it is not practical to add ideal support for every possible interface to the core software. Triggers and actions provide support for Ethernet, serial, MIDI, etc. but building 2-way interactions with third-party devices often requires many triggers and leaves projects with a difficult maintenance legacy. Importantly, we recognise that customers will often work with a third-party device on more than one project; building integration support with triggers and actions alone makes it hard to reuse work.

The IO Module framework allows separately downloadable plugin modules to be added to Designer projects, where each module adds support for a particular function, interface, protocol or device right into the |Vendor| trigger engine and user interface. A module may be as simple as a timer to repeatedly trigger an action or may add full support for a building management protocol.

Once a module is added to a project, the Designer user interface will present additional triggers, conditions and actions defined by the module. Modules always have at least one *instance* in a project --- creating additional instances of a module to support a device over a LAN would allow multiple devices to be integrated with one controller, with each having their own properties in the project, such as their IP address.

The functional part of modules is written in `Lua <https://www.lua.org/>`_. `JSON <http://www.json.org/>`_ is used to define the module package contents and the Designer user interface elements that the module exposes. Module documentation is written in `CommonMark <http://commonmark.org/>`_ (a strongly defined specification of the original `Markdown <https://daringfireball.net/projects/markdown/>`_).
