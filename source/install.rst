============
Installation
============

prerequisites
=============

Kervi is a python framework and needs python 3.4+ and the python package manager pip. 

The standard Raspberry Pi operating system's comes with python and pip preinstalled. 
If you are able to start your Raspberry Pi you are ready to install Kervi.

If you install Kervi on other platforms like windows and linux you have to install python and pip. Please follow the instructions on
`python.org <https://python.org>` to get python installed. 
Notice there are no native support for hardware when you install on these platforms but you will be able to
create dashboards and let users interact with your python kervi application via a browser.

There is a requirement for libjpeg-dev on certain versions of python on the raspberry pi.

::
    
    sudo apt-get update
    sudo apt-get install libjpeg-dev

Install
========

Open a terminal window and run the pip command below. 

Raspberry Pi::

    sudo pip3 install kervi

Other platforms::

    pip3 install kervi


