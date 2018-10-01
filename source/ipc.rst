==========================
Interprocess communication
==========================

A core feature of the kervi framework is its interprocess communication module called *the spine*.
Information that are transmitted over the spine is handled transparently regarding process-, device- and network boundaries.

You seldom need to work with the spine module directly because of the linking capabilities and action decorators that are build into the framework.

It is possible to send queries, commands and trigger events over the spine.

spine class
===========

The spine module is ready to use after the Application class has been initialized in a single file application.
If you are using the :ref:`multi process <multi_process>` capabilities the spine is ready within the __init__.py and modules loaded in __init__.py.

To use the spine import the Spine class:

.. code:: python

    from kervi.spine import Spine

Commands
========

Commands are a kind of distributed void functions. 

.. code:: python

    
    from kervi.application import Application
    from kervi.actions import action

    APP = Application()    
    import kervi.spine import Spine
    spine = Spine()
        
    #command handler function
    def my_command_handler(param1, param2):
        print("my_handler called:", param1, param2)
    
    
    spine.register_command_handler("myCommand", my_command_handler)
    
    @action
    def app.start():
        #when the app is started this action is called
        spine.send_command("myCommand", "p1", 2)

    app.run()

Queries
=======

With queries your are able to send 

.. code:: python

    
    from kervi.application import Application
    from kervi.actions import action

    APP = Application()    
    import kervi.spine import Spine
    spine = Spine()
        
    #query handler function
    def my_query_handler(param1, param2):
        print("my_handler called:", param1, param2)
        return "A query result:" + param1
    
    spine.register_query_handler("myQuery", my_query_handler)
    
    @action
    def app.start():
        #when the app is started this action is called
        result = spine.send_query("myCommand", "p1", 2)
        print("Query result", result)

    app.run()

Events
======

