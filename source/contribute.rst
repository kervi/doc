=================================
Contribute
=================================

The kervi project is hosted on github and consist of several python packages.
Most of the projects are clean python projects except from the web interface project that is implemented as an Angular 4 application. 

Development environment
======================

`GIT <https://git-scm.com/downloads>`_ is used to handle all source code via github.com.
If you want to participate install GIT and create an account on github.com 

`Visual Studio Code <https://code.visualstudio.com/download>`_ is used for all projects and VS code settings for each project are stored in the github projects.
For python programming the VS Code Python extension by Don Jayamanne is used for lintning and debuggin. 

`NPM <https://docs.npmjs.com/>`_ is used to handle packages for the web interface if you want to contribute to the web part you need to install NPM.


Get and prepare source
====================== 

Github projects
---------------

Create a development root folder for the kervi projects and clone the following projects from github.

::

    git clone https://github.com/kervi/kervi.git
    

Install python packages
----------------

The cloned git projects above should be installed as python development packages.

run the install_dev.bat (windows) or install_dev.sh script. This script will prepare 
your kervi installation for development. 

Preparing web development
-------------------------

The web ui is an Angular 7+ application.
The angular project is located in "kervi-ui/kervi_ui/web".

You need to update the npm packages in the web folder by calling::

    >npm update

Next you have to install the angular cli tool::

    >npm install -g @angular/cli

Projects
========

Kervi core
-----------

The core part of the framework. This project is project is expected to be a base between Kervi and "Kervi for micropython"
 
Kervi
-----

This project is the actual Kervi project that uses the core features and extends with functionality like camera, mail and message plugins.


Platform drivers
---------------------

There is a project for each supported platform.
A platform driver exposes the platforms hardware like gpio, i2c, cameras.
For hardware platforms that are not supported a generic driver is used. 

Kervi device library
--------------------------

This project holds drivers for specific hardware and dummy drivers for test.

Kervi UI (Web dashboards)
-------------------------

The source for the web ui is located in "kervi-ui/kervi_ui/web".

To start the Angular development server use::

    >ng serve webApp

This starts a web server that runs the Kervi angular app directly from source.

The development version listen on local host and you need to set that address in your kervi app settings.
Below is a sample app settings that show how to set the settings IPAddress and IPRootAddress to localhost.

.. code:: python

    from kervi.application import Application
    
    if __name__ == '__main__':
        APP = Application({
            "network":{
                "ip": "127.0.0.1",
            },
        })

        APP.run()

If you want to test your changes in the Kervi web server you need to build the ui app via.

::

    ng build webApp

Go to the web address that is displayed when the kervi app is started.


-------------------------------------
Test apps
-------------------------------------

There is a test project for Kervi that is located  https://github.com/kervi/kervi-test.git

This projects is used for integration test and demonstrates how the framework is used.

Documentation
=============

This site (kervi.org) is created with the python tool sphinx.   

Run these commands to install sphinx ::

    pip install sphinx
    pip install sphinx_rtd_theme


Clone the documentation source from github.

::

    git clone https://github.com/kervi/kervi-doc.git

Source files are located in the "source" folder.

You need to call the sphinx builder in order to generate the html output.
Run the command below in the root of the kervi-doc project::

    make html

Open kervi-doc/build_output/html/index.html in a browser and you should see the finished documentation.

Make a pull request on github when your are ready to publish your changes.
 