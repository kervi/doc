===========
Hello world
===========

The hart of a kervi application is the Application class.   
You must create an Application instance as the very first  operation 
in your main application script. 

Create a python file my_app.py with the content below.

.. code:: Python

    if __name__ == '__main__': # this line is important in windows
        from kervi.application import Application
        
        app = Application()
        app.run()

When you run the python script in a terminal you should see the following output. 

.. role:: green

.. raw:: html

    <style> .green {color:green} </style>

::

    >python3 my_app.py
    Starting kervi application
    IPC address: tcp://192.168.43.21:9500
    loaded plugin: kervi.plugin.storage.sqlite_temp
    loaded plugin: kervi.plugin.storage.sqlite
    platform driver: kervi.platforms.windows
    load interprocess communication
    start websocket:  192.168.43.21 9000
    Listining for discovery requests on port: 9434
    :green:`Your Kervi application is ready at http://192.168.43.21:8080`
    Press ctrl + c to stop your application


That is a lot of output for two lines of code.
The Application class prepares your application upon instantiation. It figures out
if you are using a platform like Raspberry Pi and loads required
drivers and plugins. 

The important line is "Your application is ready at http://192.168.43.21:8080".
Open a browser and go to that address

.. image:: ../images/hello_world.png

Not the most inspiring application but now you are started. 
Proceed to the next section and see how you add some content. 