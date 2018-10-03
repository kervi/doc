===========
Get started
===========

This section shows some of the core concepts in the Kervi framework.
Most examples runs on any platform supporting Python.  

Hello world
===========

Any Kervi application starts with the Application class.

create a python file my_app.py with the content below.

.. code:: Python

    from kervi.application import Application
    
    app = Application()
    app.run()


Run the file 

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
The important line is "Your application is ready at http://192.168.43.21:8080"
Open a browser and go to that address

.. image:: images/hello_world.png
    :width:  50%
