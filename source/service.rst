===============
Run as service
===============

It is possible to run your kervi application as an service that starts with your device (Raspberry Pi).

Install service
===============

To install your application as a service use the --install-service as a parameter.

.. code:: python

    >python3 my_app.py --install-service


Start, stop, restart
====================

You need to stop your application if it is started as a service before you are able to update the code.
You can start, stop and restart by using the parameters --start-service, --stop-service and --restart-service

.. code:: python

    >python3 my_app.py --stop-service

Uninstall service
=================

Use the parameter --uninstall-service if you want to stop running your application as service.

.. code:: python

    >python3 my_app.py --uninstall-service


Detect running services
=======================

You can use the Kervi cli tool to detect running kervi applications if you are unsure whether or not a service is running.

.. code:: python

    >kervi detect applications

This will list all running kervi applications on the local network.

