.. _plugins:

Plugins
=======

Most part of the kervi framework is made by plugins. 
This makes it easy to extend or replace the functionality of the framework.
You can use existing plugin base classes for  for common task like storage, messaging and routing
Plugins that do not fit any of theres categories can be developed as general plugins.

There are two kinds of plugins managed or unmanaged. 
Unmanaged plugins are loaded into its own process 
where as managed plugins are loaded into a shared process via a manager.

You configure plugins via the config "plugin" section. 



.. toctree::
   :maxdepth: 1
   :glob:
   
   *


