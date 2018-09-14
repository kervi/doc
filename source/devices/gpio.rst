GPIO
====

MCP23017(I2C)
-----------

This device driver supports the gpio MCP23017 

.. code:: python

    MCP23017DeviceDriver(
        address=0x20, 
        busnum=0
    ):
    
:address: I2C address. Default is 0x20.
:bus: I2C bus. If 0 or None default bus for platform is selected.


.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.gpio.MCP230XX import MCP23017DeviceDriver
    gpio = MCP23017DeviceDriver()
    
    gpio[1].define_as_output()
    #Link to dashboard, shows as a button
    gpio[1].link_to_dashboard()
    
    gpio[2].define_as_input(pull_up=True)
    #Link to dashboard, shows as a read only button
    gpio[2].link_to_dashboard()

    APP.run()

MCP23008(I2C)
-----------

This device driver supports the gpio MCP23008 with 8 general digital io pins 

.. code:: python

    MCP23008DeviceDriver(
        address=0x20, 
        busnum=0
    ):
    
:address: I2C address. Default is 0x20.
:bus: I2C bus. If 0 or None default bus for platform is selected.


.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.gpio.MCP230XX import MCP23008DeviceDriver
    gpio = MCP23008DeviceDriver()
    
    gpio[1].define_as_output()
    #Link to dashboard, shows as a button
    gpio[1].link_to_dashboard()
    
    gpio[2].define_as_input(pull_up=True)
    #Link to dashboard, shows as a read only button
    gpio[2].link_to_dashboard()

    APP.run()

PCF8574, PCF8574A(I2C)
----------------------

This device driver supports the PCF8574 gpio extender with 8 general digital io pins 

.. code:: python

    PCF8574DeviceDriver(
        address=0x27, 
        bus=0
    ):
    
:address: 
    I2C address. Default is 0x27.
    This device comes in two flavors PCF8574 and PCF8574A.
    For PCF8574 the valid addresses should be in the range 0x20 to 0x27.
    For PCF8574A the valid addresses should be in the range 0x38 to 0x3F.

:bus: I2C bus. If 0 or None default bus for platform is selected.

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.gpio.PCF8574 import PCF8574DeviceDriver
    gpio = PCF8574DeviceDriver()
    
    gpio[1].define_as_output()
    #Link to dashboard, shows as a button
    gpio[1].link_to_dashboard()
    
    gpio[2].define_as_input()
    #Link to dashboard, shows as a read only button
    gpio[2].link_to_dashboard()

    APP.run()

PCF859(I2C)
----------------------

This device driver supports the PCF859 gpio extender with 4 analog inputs and one analog output.

.. code:: python

    PCF859DeviceDriver(
        address=0x48, 
        bus=0
    ):
    
:address: I2C address. Default is 0x48.
    
:bus: I2C bus. If 0 or None default bus for platform is selected.

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.devices.gpio.PCF859 import PCF859DeviceDriver
    gpio = PCF859DeviceDriver()
    
    Link to dashboard, shows as a button
    gpio["AIN1"].link_to_dashboard()
    gpio["AOUT"].link_to_dashboard()
    gpio["AOUT"].value = 0.5
    APP.run()
