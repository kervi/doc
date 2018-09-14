.. _device_displays:
========
Displays
========

Kervi has support for displaying text and values on displays.
This is handled via the Display class.
The Display class recognize the type of the connected display and handles the differences independently.
This makes it easy to change the display driver with very few changes to the code.

The Display class also supports text templates where it is possible to specify and link sensors and other kervi values.

Below is an example that show how to connect a display to a text box in the web ui.
Any text that is entered in the text box is displayed in the display.

.. code:: python

    from kervi.application import Application
    APP = Application()

    from kervi.displays import Display
    from kervi.devices.displays.SSD1306 import SSD1306DeviceDriver

    display = Display("d1", "Display", SSD1306DeviceDriver(128, 32))
    display.text.link_to_dashboard()

    APP.run()

Below is an example where cpu load and temp is displayed dynamically.
This is done via DisplayPages.
A DisplayPage holds an text template and dynamic links to values that should be displayed.
When the values of the linked values changes the display is updated.

A display can have multiple DisplayPages and cycle thru them on time interval or via change_page action.

.. code:: python

    from kervi.application import Application
    APP = Application()


    from kervi.sensors.sensor import Sensor
    from kervi.devices.platforms.common.sensors.cpu_use import CPULoadSensorDeviceDriver
    from kervi.devices.platforms.common.sensors.cpu_temp import CPUTempSensorDeviceDriver
    
    #build in sensor that measures cpu use
    SENSOR_CPU_LOAD = Sensor("CPULoadSensor", "CPU", CPULoadSensorDeviceDriver())
    
    #build in sensor that measures cpu temperature
    SENSOR_CPU_TEMP = Sensor("CPUTempSensor", "", CPUTempSensorDeviceDriver())

    from kervi.displays import Display, DisplayPage
    from kervi.devices.displays.SSD1306 import SSD1306DeviceDriver

    #create a display page
    cpu_page = DisplayPage("cpu")
    cpu_page.template = "CPU : {CPULoadSensor}%\ntemp: {CPUTempSensor} \u00B0C"
    #link values and enter value text format
    cpu_page.link_value(SENSOR_CPU_LOAD, "2.0f")
    cpu_page.link_value(SENSOR_CPU_TEMP, "2.0f")

    display = Display("d1", "Display", SSD1306DeviceDriver(128, 32))
    display.add_page(cpu_page)

    APP.run()