================
Commandline tool
================

Kervi comes with the command line tool *kervi* that helps scaffold applications
and detects running kervi application on the local network.

You can apply the --help option at any level to see possibilities

.. code::
    
    >kervi --help
    Usage: kervi [OPTIONS] COMMAND [ARGS]...

    Options:
    --help  Show this message and exit.

    Commands:
    create  Create and scaffold applications and modules
    detect  Detect devices and applications


Scaffold applications
=====================

Use command *create* when you need to start a kervi project.

.. code::

    >kervi create application my_app "My application"

This will create a project with a main dashboard and a system dashboard. 
The system dashboard shows CPU use, temperature and memory use.

if you want to add camera code you use --add-camera option. 
This is only possible on Raspberry Pi 

.. code::

    >kervi create application my_app "My application" --add-camera

If you want to create a single file application use the --single-file-app option

.. code::

    >kervi create application my_app "My application" --single-file-app

Detect applications
===================

You can use the detect command to scan the local network for running kervi applications.

.. code::

    >kervi detect applications

This will broadcast discovery requests on the local network. 
The result is a list with running kervi applications that are configured for discovery. 

Detect hardware and devices
===========================

You can use the detect command to scan for hardware and connected devices

.. code::

    >kervi detect devices

This will load the platform driver that scans for hardware configuration and connected devices 