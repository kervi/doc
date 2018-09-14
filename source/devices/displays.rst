========
Displays
========

This section shows how to initialize and link displays in the kervi device library.
All samples links to a text box in the ui where it is possible to enter the text that 
should be displayed. 

See :ref:`Displays <device_displays>` for further details on display programming.

SSD1306(I2C)
-----------

.. code:: python

    from kervi.devices.displays.SSD1306 import SSD1306DeviceDriver
    SSD1306DeviceDriver(
        height, 
        width = 128, 
        rst = None, 
        i2c_bus=None, 
        i2c_address=SSD1306_I2C_ADDRESS
    )
    
:height: Height of display.
:width: Width of display.
:rst: gpio to use as as hardware reset. 
:i2c_bus: I2C bus. If 0 or None default bus for platform is selected.
:i2c_address: I2C address. Default is 0x3C.


.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.displays import Display
    from kervi.devices.displays.SSD1306 import SSD1306DeviceDriver
    display = Display("d_1", "Display", SSD1306DeviceDriver(32))
    display.text.value = "test"
    display.text.link_to_dashboard()

    APP.run()


HD44780(I2C, PCF8574)
-----------

This device driver supports HD44780 displays that are controlled via the PCF8574 gpio i2c chip.

.. code:: python

    from kervi.devices.displays.HD44780_PCF8574 import HD44780_PCF8574_DeviceDriver
    HD44780_PCF8574_DeviceDriver(
        pin_map=0, 
        cols=16, 
        lines=2, 
        address=0x3f, 
        busnum=0
    ):
    
:pin_map: 
    There are several products that uses PCF8574 to control the display but have different pin setup.
    To handle this situation the driver has different pin_maps to use.
    Use values 0-3. 

:cols: Number of characters in a line.
:lines: Number of lines. 
:address: I2C address. Default is 0x3C.
:bus: I2C bus. If 0 or None default bus for platform is selected.


.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.displays import Display
    from kervi.devices.displays.HD44780_PCF8574 import HD44780_PCF8574_DeviceDriver
    display = Display("d_1", "Display", HD44780_PCF8574_DeviceDriver(0))
    display.text.value = "test"
    display.text.link_to_dashboard()

    APP.run()


HD44780(I2C, MCP23017)
-----------

This device driver supports HD44780 displays that are controlled via the MCP23017 gpio i2c chip.

.. code:: python

    from kervi.devices.displays.HD44780_MCP23017 import HD44780_MCP23017_DeviceDriver
    HD44780_MCP23017_DeviceDriver(
        cols=16, 
        lines=2, 
        address=0x20, 
        busnum=0
    ):
    
:cols: Number of characters in a line.
:lines: Number of lines. 
:address: I2C address. Default is 0x20.
:bus: I2C bus. If 0 or None default bus for platform is selected.


.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.displays import Display
    from kervi.devices.displays.HD44780_MCP23017 import HD44780_MCP23017_DeviceDriver
    display = Display("d_1", "Display", HD44780_MCP23017_DeviceDriver())
    display.text.value = "test"
    display.text.link_to_dashboard()

    APP.run()


HD44780
-----------

This device driver supports HD44780 displays that is controlled via gpio.

.. code:: python

    from kervi.devices.displays.HD44780_MCP23017 import HD44780_MCP23017_DeviceDriver
    HD44780DeviceDriver(
        rs, en, 
        d4, d5, d6, d7,
        cols= 16, 
        lines= 2,
        backlight=None,
        invert_polarity=True,
        enable_pwm=False,
        initial_backlight=1.0
    ):
    
:rs, en, d4, d5, d6, d7: 
    RS, EN, and D4...D7 parameters should be the pins
    connected to the LCD RS, clock enable, and data line 4 through 7 connections.
    The LCD will be used in its 4-bit mode so these 6 lines are the only ones
    required to use the LCD.

:cols: Number of columns in the display
:lines: Number of lines in the display. 
:backlight, invert_polarity: 
    If you would like to control the backlight, pass in the pin connected to
    the backlight with the backlight parameter.  The invert_polarity boolean
    controls if the backlight is one with a LOW signal or HIGH signal.  The 
    default invert_polarity value is True, i.e. the backlight is on with a
    LOW signal.

:enable_pwm, initial_backlight: 
    You can enable PWM of the backlight pin to have finer control on the 
    brightness.  To enable PWM make sure your hardware supports PWM on the 
    provided backlight pin and set enable_pwm to True (the default is False).
    The appropriate PWM library will be used depending on the platform, but
    you can provide an explicit one with the pwm parameter.
    The initial state of the backlight is ON, but you can set it to an 
    explicit initial state with the initial_backlight parameter (0 is off,
    1 is on/full bright).

.. code:: python

    from kervi.application import Application
    APP = Application()
    
    from kervi.hal.gpio import GPIO
    from kervi.displays import Display
    from kervi.devices.displays.HD44780_MCP23017 import HD44780_MCP23017_DeviceDriver
    
    display = Display(
        "d_1",
        "Display", 
        HD44780DeviceDriver(
            GPIO[15], GPIO[14], GPIO[13], GPIO[12],
            GPIO[13], GPIO[12], GPIO[11], GPIO[10]
        ))
    display.text.value = "test"
    display.text.link_to_dashboard()

    APP.run()




